# Deliverable Mapping 检查清单

使用此清单确保生成输出满足通用后端测试交付要求。

## 必需的工具输入

- 接受 requirement 文档输入。
- 接受 backend 代码仓输入。
- 支持单独输入任一项，或两者同时输入。

## 必需的工具产物

- 用于生成的 Prompt 文本。
- Model 推荐（primary + fallback）。
- 运行时实际使用模型（`model_used`）与版本/日期。
- Prompt 版本与文件路径（`prompt_artifacts[]`），确保可复现。
- 生成的 strategy、test cases 与 release package。
- `structured_result`（JSON）与 `review_report`（自然语言）双层输出。

## 必需的生成输出

- Test strategy 与 scope。
- 带 coverage 意图的 test case 集合。
- Defect report template 与 sample findings。
- Risk 与 release recommendation。
- `gate_evaluation`（profile_id、rules_hit、final_decision、rationale）。
- 若无用户配置，显式标注 `using_default_profile=true`。

## 必需的实验分析字段

- Accuracy（defect 相关性与可行动性）。
- Coverage（AC 覆盖与 risk-scenario 覆盖）。
- Generalization（跨 backend 语言/框架）。
- 建议至少提供：`precision`、`recall`、`f1`、`validation_sample_size`。
- 与传统非 AI testing 的对比。
- Quantitative gate 命中情况与判定一致性（schema 与报告一致）。

## 必需的报告章节

- 相较传统 testing workflow 的优缺点。
- AI 在实践中的限制。
- 下一轮 prompt 迭代的改进策略。
