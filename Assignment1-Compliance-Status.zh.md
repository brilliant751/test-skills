# Assignment 1 要求对照状态（当前版本）

对照来源：`Assignment 1.zh.md`  
评估日期：2026-04-08

## 一、已达到的要求（文档与规范层）

1. 已覆盖作业要求的测试方向与工具定位  
- 状态：已达到  
- 说明：仓库已定义生成型与增量型后端测试流程，覆盖黑盒/白盒思路与静态风险检查视角。  
- 证据：  
  - [skills/generative-testing/SKILL.md](/Users/qiuqiu/Desktop/test-skills/skills/generative-testing/SKILL.md)  
  - [skills/progressive-test-integration/SKILL.md](/Users/qiuqiu/Desktop/test-skills/skills/progressive-test-integration/SKILL.md)

2. 已支持作业要求的两类输入  
- 状态：已达到  
- 说明：支持需求文档输入、代码仓输入以及混合输入。  
- 证据：  
  - [skills/generative-testing/SKILL.md](/Users/qiuqiu/Desktop/test-skills/skills/generative-testing/SKILL.md)  
  - [skills/progressive-test-integration/SKILL.md](/Users/qiuqiu/Desktop/test-skills/skills/progressive-test-integration/SKILL.md)

3. 已具备工具产物框架（Prompt/模型/输出契约）  
- 状态：已达到  
- 说明：已定义统一门禁提示词、Skill 默认 prompt、结构化输出 schema 与门禁规则。  
- 证据：  
  - [agent4.md](/Users/qiuqiu/Desktop/test-skills/agent4.md)  
  - [skills/generative-testing/agents/openai.yaml](/Users/qiuqiu/Desktop/test-skills/skills/generative-testing/agents/openai.yaml)  
  - [skills/progressive-test-integration/agents/openai.yaml](/Users/qiuqiu/Desktop/test-skills/skills/progressive-test-integration/agents/openai.yaml)  
  - [skills/generative-testing/references/schema-and-gate-profile.md](/Users/qiuqiu/Desktop/test-skills/skills/generative-testing/references/schema-and-gate-profile.md)  
  - [skills/progressive-test-integration/references/schema-and-gate-profile.md](/Users/qiuqiu/Desktop/test-skills/skills/progressive-test-integration/references/schema-and-gate-profile.md)

4. 已覆盖实验分析所需维度  
- 状态：已达到  
- 说明：已在规范中纳入 accuracy、coverage、generalization、limitations，并要求量化指标字段。  
- 证据：  
  - [skills/generative-testing/SKILL.md](/Users/qiuqiu/Desktop/test-skills/skills/generative-testing/SKILL.md)  
  - [skills/progressive-test-integration/SKILL.md](/Users/qiuqiu/Desktop/test-skills/skills/progressive-test-integration/SKILL.md)  
  - [skills/generative-testing/references/deliverable-mapping.md](/Users/qiuqiu/Desktop/test-skills/skills/generative-testing/references/deliverable-mapping.md)  
  - [skills/progressive-test-integration/references/deliverable-mapping.md](/Users/qiuqiu/Desktop/test-skills/skills/progressive-test-integration/references/deliverable-mapping.md)

5. 已补齐作业提交映射模板  
- 状态：已达到  
- 说明：已提供可直接填写的提交包文档（含 TODO 与提交前清单）。  
- 证据：  
  - [submission/Assignment1-Submission-Pack.zh.md](/Users/qiuqiu/Desktop/test-skills/submission/Assignment1-Submission-Pack.zh.md)  
  - [README.md](/Users/qiuqiu/Desktop/test-skills/README.md)

## 二、未达到的要求（提交与实证层）

1. 真实实验数据未补齐  
- 状态：未达到  
- 缺口：实际模型版本/日期、真实 precision/recall/F1（或 impact_precision/defect_precision）、样本量、跨项目泛化结果仍是待填。  
- 建议补位位置：  
  - [submission/Assignment1-Submission-Pack.zh.md](/Users/qiuqiu/Desktop/test-skills/submission/Assignment1-Submission-Pack.zh.md)

2. 本次实验的真实 Prompt 与输出样例未归档  
- 状态：未达到  
- 缺口：当前以规范与示例为主，尚缺“本轮实验实际使用”的 prompt 文件和输出结果附件。

3. 项目报告成稿（PDF）未形成  
- 状态：未达到  
- 缺口：需提交包含对比传统方法、AI 局限、改进策略、总结的最终报告 PDF。

4. 英文展示 PPT（PDF）未形成  
- 状态：未达到  
- 缺口：需提供 15 分钟汇报材料，并包含团队 ID、姓名、学号。

5. 测试脚本压缩包未提供  
- 状态：未达到  
- 缺口：作业要求提交测试脚本压缩文件，当前仓库尚无对应打包产物。

## 三、结论

当前仓库已完成“方法设计与交付模板”层面的对齐，可支撑评分中的概念、设计连贯性与表达结构。  
距离最终可提交版本，仍需补齐“真实实验证据与正式提交件”（报告 PDF、PPT PDF、脚本压缩包、实测数据）。

## 四、本次修改记录（2026-04-08）

1. 新增作业提交总包模板  
- 文件：  
  - [submission/Assignment1-Submission-Pack.zh.md](/Users/qiuqiu/Desktop/test-skills/submission/Assignment1-Submission-Pack.zh.md)  
- 目的：把作业要求 2.1-2.5 直接映射到可填写结构，降低漏项风险。

2. 更新 README，加入作业提交映射入口  
- 文件：  
  - [README.md](/Users/qiuqiu/Desktop/test-skills/README.md)  
- 目的：让仓库首页直接指向可交付文档。

3. 增强 `generative-testing` 输出契约字段  
- 文件：  
  - [skills/generative-testing/SKILL.md](/Users/qiuqiu/Desktop/test-skills/skills/generative-testing/SKILL.md)  
  - [skills/generative-testing/references/schema-and-gate-profile.md](/Users/qiuqiu/Desktop/test-skills/skills/generative-testing/references/schema-and-gate-profile.md)  
  - [skills/generative-testing/references/deliverable-mapping.md](/Users/qiuqiu/Desktop/test-skills/skills/generative-testing/references/deliverable-mapping.md)  
- 关键新增：  
  - `meta.models{primary,fallback}`  
  - `meta.prompt_artifacts[]`  
  - `analysis_metrics`（`precision/recall/f1/validation_sample_size` 等）
- 目的：满足“所用模型、prompts、实验分析可量化”要求。

4. 增强 `progressive-test-integration` 输出契约字段  
- 文件：  
  - [skills/progressive-test-integration/SKILL.md](/Users/qiuqiu/Desktop/test-skills/skills/progressive-test-integration/SKILL.md)  
  - [skills/progressive-test-integration/references/schema-and-gate-profile.md](/Users/qiuqiu/Desktop/test-skills/skills/progressive-test-integration/references/schema-and-gate-profile.md)  
  - [skills/progressive-test-integration/references/deliverable-mapping.md](/Users/qiuqiu/Desktop/test-skills/skills/progressive-test-integration/references/deliverable-mapping.md)  
- 关键新增：  
  - `meta.models{primary,fallback}`  
  - `meta.prompt_artifacts[]`  
  - `analysis_metrics`（`impact_precision/defect_precision/validation_sample_size` 等）
- 目的：满足增量测试场景下的可追踪与可复现要求。

5. 新增本合规状态文件  
- 文件：  
  - [submission/Assignment1-Compliance-Status.zh.md](/test-skills/submission/Assignment1-Compliance-Status.zh.md)  
- 目的：形成“已达到/未达到”对照与缺口清单，支持答辩和提交前自检。
