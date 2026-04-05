---
name: generative-testing
description: 基于自然语言需求和/或后端代码库上下文，跨语言与框架生成 backend 测试资产。适用于需要 test strategy、AC mapping、test case 设计、执行规划、缺陷报告模板与发布建议，且无需 YAML 配置的场景。
---

# Generative Testing

使用此 skill 从需求与代码仓上下文生成 backend 测试资产。

优先使用自然语言输入。除非用户明确要求，否则不要强制使用 YAML。
输出采用双层模式：先给结构化事实，再给人类可读报告。

## Quick Start

收集以下五个自然语言信息块：
1. Requirements 与 acceptance criteria。
2. Repository 或 module 上下文（若无代码，则提供仅需求上下文）。
3. Runtime 与 test environment。
4. 已知业务风险与约束。
5. Quality gate policy。

随后一次性生成测试产物。

自动上下文收集规则：
- 若用户提供了需求文档或代码路径，优先使用用户指定来源。
- 若用户未提供，自动扫描 `doc/` 或 `docs/` 目录、文件名包含 `PRD` 的文档、以及 `README*`，用于定位需求与约束信息。
- 后端代码目录不做硬编码：优先使用用户指定目录；否则基于 `README`/其他文档中的项目结构说明自动识别，并在输出中声明识别依据与目标目录。

## Workflow

### Step 1: Build test scope from spec docs or code

接受以下任一种输入：
- 仅产品/需求文档输入。
- Codebase/module 输入。
- 产品/需求文档与代码同时输入。

将 AC 映射为可验证的 test objectives。若 AC 不完整，做最小化假设并清晰标注。

### Step 2: Design mixed testing strategy

在有帮助时结合 static/black-box/white-box 思路，但输出应保持为一个 backend-focused 测试包。

始终覆盖：
- 正常流程。
- 异常/错误路径。
- 边界值。
- Authorization 与 access control。
- Concurrency 与 race conditions。
- Transaction consistency 与 rollback。
- Idempotency。
- 外部依赖失败。
- Misconfiguration/environment drift。

### Step 3: Generate executable test asset package

产出：
- AC mapping matrix。
- 按优先级排序的 test case 列表。
- 按语言/框架给出的测试实现建议。
- 执行顺序与 observability 点（API、DB、logs、events）。

### Step 4: Produce defect and release templates

生成可复用 defect template：
- Preconditions。
- Steps。
- Expected vs actual。
- Reproduction logs。
- Severity: Blocker/Critical/Major/Minor。

生成 release recommendation：
- `GO`, `GO_WITH_CONDITIONS`, `NO_GO`.
- 由 coverage、defects、residual risk 组成的 evidence chain。

### Step 5: Produce analysis blocks

包含：
- 与传统非 AI backend testing 的对比。
- Accuracy/coverage/generalization 分析建议。
- 局限性与 prompt 迭代改进点。

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
- `scope_and_strategy`: `scope_statement`, `test_objectives[]`, `risk_tiers[]`
- `ac_mapping_matrix[]`: `ac_id`, `objective`, `test_candidates[]`, `coverage_intent`
- `test_case_package[]`: `test_id`, `priority`, `type`, `target_layer`, `reason`, `evidence_ref[]`
- `execution_plan`: `sequence[]`, `observability_points[]`
- `sample_findings[]`: `id`, `severity(P0-P3)`, `status(open/closed)`, `reproducible`, `evidence_ref[]`
- `coverage_metrics`: `ac_coverage`, `risk_coverage`, `blocked_tests`
- `analysis_checklist`: `accuracy_notes[]`, `coverage_notes[]`, `generalization_notes[]`, `limitations[]`
- `assumptions[]`, `unknowns[]`, `blockers[]`
- `gate_evaluation`: `profile_id`, `rules_hit[]`, `final_decision(GO/GO_WITH_CONDITIONS/NO_GO)`, `rationale`

若输入信息不足，仍需输出完整 schema；在 `unknowns[]` 与 `blockers[]` 量化暴露不确定性。

### Section B: `review_report` (Natural Language)

保持以下 7 个章节（顺序固定）：
1. Scope and strategy。
2. AC mapping matrix。
3. Test case package（prioritized）。
4. Execution plan and observability。
5. Defect report template 与 sample findings。
6. Risk assessment 与 release recommendation。
7. Experiment-analysis checklist（accuracy、coverage、generalization、limitations）。

报告内容必须能回溯到 Section A 字段，且末尾必须追加 `Decision Trace`。

仅使用 backend 范围。对 frontend/UI testing 请求需明确拒绝。

## Non-goals

- 不自动实现 CI/CD pipelines。
- 不执行 production-side 测试。
- 未提供环境命令时，不保证 framework-specific tooling 可直接落地。

## Model and evidence guidance

- 为可复现性推荐一个 primary LLM 与一个 fallback LLM。
- 保留 prompt 与输出产物，作为可审计证据。
- 优先先输出 `structured_result`，再基于其渲染 `review_report`。

## References

- 跨语言 backend 测试示例与映射指导见 [references/backend-testing-examples.md](references/backend-testing-examples.md)。
- 通用交付清单见 [references/deliverable-mapping.md](references/deliverable-mapping.md)。
- 双层输出 schema 与量化门禁模板见 [references/schema-and-gate-profile.md](references/schema-and-gate-profile.md)。
