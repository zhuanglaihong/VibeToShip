---
tags: [rules, 模型, 分工]
---

# 模型分工

## 这个文件干什么

GPT 5.5 和 DeepSeek 各自该干什么。**模型选择要匹配任务类型，避免成本和质量失衡**。

## 分工表

| 阶段 | 模型 | 原因 |
|------|------|------|
| 需求分析 (Grill) | GPT 5.5 (Codex) | 擅长推理和架构 |
| 拆计划 (Plan) | GPT 5.5 (Codex) | 需要理解全局依赖 |
| 写代码 (TDD) | DeepSeek (Claude Code) | 执行效率高，输出简洁 |
| 代码审查 (Review) | GPT 5.5 (Codex) | 不能自己审自己 |
| 紧急修 Bug | DeepSeek (Claude Code) | 速度优先 |
| 重构 (Refactor) | GPT 5.5 (Codex) | 需要全局视角 |
| 最终验收 (Diagnose) | GPT 5.5 (Codex) | 验收需要权威判断 |

## 黄金法则

**GPT 想 → DeepSeek 做 → GPT 审**

三步两模型，缺一不可。
