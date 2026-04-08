# Assignment 1 提交包（可直接交付版）

本文件用于将当前仓库内容对齐 `Assignment 1.zh.md` 的提交要求。  
使用方式：保留结构，替换 `TODO` 项并补齐附件。

## 0. 项目概述

- 项目主题：基于 LLM 的后端测试工具（生成型 + 增量型）
- 工具形态：
  - `skills/generative-testing`：从需求/代码生成测试资产
  - `skills/progressive-test-integration`：基于改动维护增量测试与门禁
- 覆盖测试技术：
  - 黑盒：AC 映射、边界/异常场景、输入组合
  - 白盒：代码路径信号（分支/错误处理）驱动测试补充
  - 静态风险：配置/依赖/错误处理类风险检查（以测试策略形式体现）

## 1. 输入（对应作业要求 2.1）

- 需求文档输入：支持
- 代码仓输入：支持
- 混合输入：支持
- 证据位置：
  - [skills/generative-testing/SKILL.md](/Users/qiuqiu/Desktop/test-skills/skills/generative-testing/SKILL.md)
  - [skills/progressive-test-integration/SKILL.md](/Users/qiuqiu/Desktop/test-skills/skills/progressive-test-integration/SKILL.md)

## 2. 工具产物（对应作业要求 2.2）

### 2.1 Prompts

- 统一门禁/调度提示： [agent4.md](/Users/qiuqiu/Desktop/test-skills/agent4.md)
- Skill 默认入口 prompt：
  - [skills/generative-testing/agents/openai.yaml](/Users/qiuqiu/Desktop/test-skills/skills/generative-testing/agents/openai.yaml)
  - [skills/progressive-test-integration/agents/openai.yaml](/Users/qiuqiu/Desktop/test-skills/skills/progressive-test-integration/agents/openai.yaml)
- `TODO`：补充本次实验实际 task prompt 文本与版本号（建议附录 A）。

### 2.2 Models

- 推荐：`primary=gpt-5.4`，`fallback=gpt-5.4-mini`
- 已写入输出 schema 合同，支持产物追踪
- `TODO`：填写你本次实验“实际使用模型 + 日期”（例如 `gpt-5.4, 2026-04-08`）。

### 2.3 模型生成代码/测试资产

- 产物类型：AC mapping、test case package、impact matrix、registry delta、release gate decision
- 证据位置：
  - [skills/generative-testing/references/schema-and-gate-profile.md](/Users/qiuqiu/Desktop/test-skills/skills/generative-testing/references/schema-and-gate-profile.md)
  - [skills/progressive-test-integration/references/schema-and-gate-profile.md](/Users/qiuqiu/Desktop/test-skills/skills/progressive-test-integration/references/schema-and-gate-profile.md)

## 3. 生成输出（对应作业要求 2.3）

- 黑盒/白盒分析产出：测试用例（已覆盖）
- 静态风险相关输出：风险与缺陷样例（`sample_findings`/`defects` 字段）
- 输出格式：
  - Section A: `structured_result`（JSON）
  - Section B: `review_report`（自然语言）

## 4. 实验分析（对应作业要求 2.4）

本仓库已要求输出以下分析维度：
- Accuracy
- Coverage
- Generalization
- Limitations

并补充了最小量化指标：
- 生成型：`precision`、`recall`、`f1`、`validation_sample_size`
- 增量型：`impact_precision`、`defect_precision`、`validation_sample_size`

`TODO`：填写真实实验数据（建议至少 2 个项目或 2 轮 prompt 对比）。

## 5. 项目报告（对应作业要求 2.5）

必须包含：
- 与传统非 AI 测试方法的对比、优缺点
- 实践中遇到的 AI 局限
- 改进策略与总结

可直接复用章节来源：
- [skills/generative-testing/references/deliverable-mapping.md](/Users/qiuqiu/Desktop/test-skills/skills/generative-testing/references/deliverable-mapping.md)
- [skills/progressive-test-integration/references/deliverable-mapping.md](/Users/qiuqiu/Desktop/test-skills/skills/progressive-test-integration/references/deliverable-mapping.md)

## 6. 评分标准对齐说明（快速自检）

- 概念理解（10%）：已覆盖三类测试思想及后端适用边界
- 设计实现连贯性（20%）：输入 -> 策略 -> 用例/增量决策 -> 门禁
- 覆盖与效果（40%）：AC/risk/critical path + 可量化门禁
- 深度分析（20%）：generalization + limitations + prompt 迭代
- 展示表达（10%）：建议按本文件 0-5 节制作 PPT

## 7. 提交前检查清单

- [ ] 报告 PDF 已包含团队 ID、姓名、学号
- [ ] PPT PDF 首页信息完整
- [ ] 测试脚本与样例输出已打包压缩
- [ ] 本文件所有 `TODO` 已替换为真实信息
- [ ] 最终结论与证据一致（schema 与报告不冲突）
