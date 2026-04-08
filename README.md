# Backend Testing Skills Repository

这个仓库提供两套可复用的后端测试 Skill：
- `generative-testing`：从需求/代码上下文生成测试策略与测试资产
- `progressive-test-integration`：基于改动上下文做增量测试维护与放行门禁判断

## 仓库结构

```text
.
├── agent4.md
├── requirement/
│   └── Assignment.zh.md
└── skills/
    ├── generative-testing/
    │   ├── SKILL.md
    │   ├── agents/openai.yaml
    │   └── references/
    │       ├── backend-testing-examples.md
    │       ├── deliverable-mapping.md
    │       └── schema-and-gate-profile.md
    └── progressive-test-integration/
        ├── SKILL.md
        ├── agents/openai.yaml
        └── references/
            ├── backend-testing-examples.md
            ├── deliverable-mapping.md
            └── schema-and-gate-profile.md
```

## 各文件用途

- `agent4.md`
  - 测试 Agent 的统一门禁提示词（技能选择规则、通用后端测试规则、默认放行门禁、输出约束）。
- `skills/*/SKILL.md`
  - 每个 Skill 的主规范：输入、工作流、输出契约、限制与引用资料。
- `skills/*/agents/openai.yaml`
  - 该 Skill 的默认入口提示（default prompt）。
- `skills/*/references/*.md`
  - 参考资料，包括测试示例、交付检查清单、schema 与门禁模板。

## 如何使用

### 1) 选择 Skill

- 需要“生成完整测试方案/测试包”时，用 `generative-testing`
- 需要“基于本次改动做增量决策”时，用 `progressive-test-integration`

### 2) 在对话中触发

推荐显式触发（最稳定）：

```text
使用 $generative-testing。
需求在 docs/PRD-order.md，代码在 services/order。
请输出 structured_result + review_report，并给出 release decision。
```

```text
使用 $progressive-test-integration。
基于这次 git diff 和 AC 做增量测试决策，输出 ADD/UPDATE/DEPRECATE/KEEP，
并给出门禁结论与 Decision Trace。
```

### 3) 默认行为

两个 Skill 默认都支持：
- 自然语言输入（无需 YAML）
- 双层输出：
  - `Section A: structured_result`（JSON，事实源）
  - `Section B: review_report`（自然语言报告）
- 默认量化门禁 `default-profile-v1`（开箱即用）

如需覆盖阈值，可提供 `gate_overrides`；覆盖仅影响阈值，不改变 schema 结构。

## 推荐工作流

1. 先用 `generative-testing` 建立基线测试资产。
2. 每次代码变更后用 `progressive-test-integration` 做增量维护与门禁判定。
3. 将 `structured_result` 作为审计与自动化集成输入，将 `review_report` 作为人工评审输出。

## 注意事项

- 当前能力范围是 backend testing，不包含 UI/E2E 自动化。
- 输入信息不完整时，Skill 会 best-effort 输出，并在 `assumptions/unknowns/blockers` 标注不确定性。

## 作业提交映射

为对齐 `Assignment 1.zh.md` 的提交要求，新增可直接填写的提交包模板：

- [submission/Assignment1-Submission-Pack.zh.md](/Users/qiuqiu/Desktop/test-skills/submission/Assignment1-Submission-Pack.zh.md)
