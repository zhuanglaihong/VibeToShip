---
tags: [trigger, review, 审查阶段, 质量]
model: GPT5.5 (Codex)
skill: review
upstream: "[[03-TDD实现-tdd-hub]]"
---

# 04 — 代码审查

## 这个文件干什么

TDD 写完了、测试全绿了，用**另一个模型**来审查代码质量。绝不能用写代码的模型审自己写的东西——DeepSeek 写的代码，GPT 来审。

## 什么时候用

- 一个 commit 写完了、测试全绿
- 准备合并之前

## 用什么模型

**GPT 5.5（Codex）**——用它审 DeepSeek 写的代码，不容易放过自己。

## 怎么用

在 Codex 或 Claude Code 里输入：

```
/review：审查 Commit [编号] 的改动。
对比 [需求文档/RFC] 检查有没有偏差。
检查点：[正确性、性能、安全、测试覆盖]。
有问题直接给修复建议。
```

## 举个例子

```
/review：审查 Commit 4 的改动（单 source zone forecast 端到端）。
对比 docs/issue/05-agri-zone-forecast-api.md §8.3 的响应结构。
检查点：cells[] 和 values[] 顺序是否对齐、404/409/422 错误码是否和 RFC 一致、变量 unit 和 value_semantics 映射是否正确。
```

## 会得到什么

- 发现的问题（致命/重要/建议）
- 和原始需求对比的偏差
- 可以直接粘贴的修复代码

## 没过怎么办

不通过 → 回到 [[03-TDD实现-tdd-hub]] 修复，修完再审。

## 下一步

全部 commit 通过后 → [[05-最终验收-diagnose]]
