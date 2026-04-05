# Deliverable Mapping 检查清单

使用此清单确保输出满足通用后端测试交付要求。

## 必需的工具输入

- 接受需求文档和/或代码仓库输入。
- Incremental mode 额外需要变更上下文（`git diff`、commit notes、PR summary）。

## 必需的工具产物

- 本次运行使用的 Prompt。
- Model 推荐（primary + fallback）。
- 生成的 testing artifacts（impact matrix、regression plan、registry delta、defects）。
- `structured_result`（JSON）与 `review_report`（自然语言）双层输出。

## 必需的生成输出

- Incremental test decisions：add/update/deprecate/keep。
- 带可复现证据的 defect 列表。
- 带 rationale 的 release recommendation。
- `gate_evaluation`（profile_id、rules_hit、final_decision、rationale）。
- 若无用户配置，显式标注 `using_default_profile=true`。

## 必需的实验分析字段

- Accuracy（impact 与 defect 预测质量）。
- Coverage（AC 与 risk-scenario 覆盖）。
- Generalization（跨不同 backend stack 的表现）。
- 与传统非 AI testing 的对比。
- Quantitative gate 命中情况与判定一致性（schema 与报告一致）。

## 必需的报告章节

- 相比传统 workflow 的优势与劣势。
- 实践中遇到的限制。
- prompt 或 workflow 迭代的改进计划。
