---
name: progressive-test-integration
description: 基于自然语言变更上下文、git diff 摘要与 acceptance criteria，执行增量 backend test integration。适用于 backend 代码变更后，需要 impact analysis、test case add/update/deprecate 决策、分层 regression 范围、test debt 跟踪与发布门禁证据，且无需 YAML 输入的场景。
---

# Progressive Test Integration

使用此 skill 在每次代码变更后持续维护 backend 测试。

优先以自然语言交互。除非用户明确要求结构化模式，否则不要要求其提供 YAML。
输出采用双层模式：先给结构化事实，再给人类可读报告。

## Quick Start

请用户用自然语言提供五块信息：
1. Requirements 与 AC。
2. 变更摘要（`git diff`、commit notes 或 PR summary）。
3. Repository 与 module 上下文。
4. Test/runtime environment。
5. Quality gate 与 release policy。

若信息缺失，做最小化推断，声明假设，并继续给出 best-effort 增量计划。

自动上下文收集规则：
- 若用户指定了需求文档或代码路径，优先使用用户指定路径。
- 若用户未指定，自动扫描 `doc/` 或 `docs/` 目录、文件名包含 `PRD` 的文档、以及 `README*`，用于定位需求与约束信息。
- 后端代码目录不做硬编码：优先使用用户指定目录；否则基于 `README`/其他文档中的项目结构说明自动识别，并在输出中声明识别依据与目标目录。

## Workflow

### Step 1: Parse change context and classify risk

提取：
- 变更文件、functions、APIs、config、SQL/DDL。
- 每项变更对应的 risk type：functionality、compatibility、security、performance、consistency。

构建 impact chain：
`diff -> file/function -> capability -> affected tests`.

### Step 2: Decide incremental test actions

针对每个受影响测试域，输出一个决策：
- `ADD`: missing tests required by new behavior.
- `UPDATE`: existing tests need adjustment.
- `DEPRECATE`: obsolete tests to retire.
- `KEEP`: existing tests remain valid.

始终附带 reason 与 evidence source（AC、diff fragment、behavior change）。

### Step 3: Build layered regression plan

产出三层回归计划：
1. Minimal regression set（仅直接影响路径）。
2. High-risk set（auth、transactions、concurrency、external dependencies）。
3. Full regression 触发条件（contract break、core refactor、cross-module transaction change）。

### Step 4: Maintain test registry

将 test registry 维护视为必选项：
- 输出包含 `ADD/UPDATE/DEPRECATE` 的 registry delta 表。
- 包含 test ID、title、owner/module、priority、automation level、reason。
- 若不确定，标记为 `BLOCKED` 并列出缺失数据。

### Step 5: Produce release decision package

输出：
- 带可复现证据的 defect 列表。
- Test debt 列表（未覆盖风险、延期测试、blockers）。
- Release decision：`GO`、`GO_WITH_CONDITIONS` 或 `NO_GO`。
- 决策 evidence chain（coverage 状态、defect severity 统计、residual risks）。

### Step 6: Evaluate quantitative gate profile

默认使用 `default-profile-v1`，无需用户预先配置：
- `P0 open > 0 => NO_GO`
- `P1 open > 3 => NO_GO`
- `ac_coverage < 0.85 => NO_GO`
- `0.85 <= ac_coverage < 0.95 => GO_WITH_CONDITIONS`
- `risk_coverage < 0.80 => GO_WITH_CONDITIONS`
- `blocked_tests > 0` 且命中核心链路 => `NO_GO`，否则 `GO_WITH_CONDITIONS`
- 其余满足 => `GO`

当用户提供 `gate_overrides` 时：
- 仅覆盖阈值，不改变 output schema。
- 在输出中标注 `using_default_profile=false` 与 `profile_id=custom`。
- 若用户未提供覆盖，必须标注 `using_default_profile=true` 与 `profile_id=default-profile-v1`。

### Step 7: Render human-readable review from schema

- 先完成 `structured_result`（唯一事实源）。
- 再渲染 `review_report`（仅可引用 `structured_result` 中已有事实）。
- 禁止在报告中新增 schema 中不存在的 defect 或 decision。
- 在报告末尾追加 3-5 行 `Decision Trace`，列出命中规则与最终判定。

## Output Contract

按固定顺序返回两段，不可交换：

### Section A: `structured_result` (JSON)

必须包含以下字段：
- `meta`: `schema_version`, `skill_version`, `generated_at`, `evidence_sources[]`, `using_default_profile`
- `change_summary`: `changed_components[]`, `risk_tiers[]`
- `impact_matrix[]`: `diff_item`, `capability`, `affected_tests[]`, `risk_type`
- `incremental_decisions[]`: `action(ADD/UPDATE/DEPRECATE/KEEP)`, `test_id`, `reason`, `evidence_ref[]`
- `defects[]`: `id`, `severity(P0-P3)`, `status(open/closed)`, `reproducible`, `evidence_ref[]`
- `coverage_metrics`: `ac_coverage`, `risk_coverage`, `blocked_tests`
- `test_debt[]`: `item`, `risk_level`, `owner`, `eta`, `blocking_release`
- `assumptions[]`, `unknowns[]`, `blockers[]`
- `gate_evaluation`: `profile_id`, `rules_hit[]`, `final_decision(GO/GO_WITH_CONDITIONS/NO_GO)`, `rationale`

若输入信息不足，仍需输出完整 schema；在 `unknowns[]` 与 `blockers[]` 量化暴露不确定性。

### Section B: `review_report` (Natural Language)

保持以下 8 个章节（顺序固定）：
1. Change summary 与 risk tiers。
2. Impact matrix（`diff -> capability -> tests`）。
3. Incremental test decisions（`ADD/UPDATE/DEPRECATE/KEEP`）。
4. Regression plan（minimal/high-risk/full trigger）。
5. Defects 与 blockers。
6. Test registry delta。
7. Test debt。
8. 带 evidence 的 release decision。

报告内容必须能回溯到 Section A 字段，且末尾必须追加 `Decision Trace`。

仅使用 backend 范围。对 frontend/UI test 请求需明确拒绝。

## Non-goals

- 不声称提供 end-to-end UI automation。
- 不声称在 production 执行测试。
- 不承诺落地 performance benchmarking 实现（仅提供规划与测试建议）。

## Model and evidence guidance

- 为可复现性推荐一个 primary LLM 与一个 fallback LLM。
- 保持 prompt artifacts 与生成输出可追踪，便于审计与复盘。
- 优先先输出 `structured_result`，再基于其渲染 `review_report`。

## References

- 语言相关测试示例与 mapping 指导见 [references/backend-testing-examples.md](references/backend-testing-examples.md)。
- 通用交付输出检查清单见 [references/deliverable-mapping.md](references/deliverable-mapping.md)。
- 双层输出 schema 与量化门禁模板见 [references/schema-and-gate-profile.md](references/schema-and-gate-profile.md)。
