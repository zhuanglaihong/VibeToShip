---
tags: [trigger, refactor, 清理, 技术债]
model: GPT5.5 (Codex)
skill: refactor
---

# 07 — 代码重构（整理，不改行为）

## 这个文件干什么

代码能跑但太丑——40 行 if/elif、重复逻辑、命名混乱。把代码整理干净，**所有测试不能变、接口不能变、行为不能变**。

## 什么时候用

- 代码能跑但是看着难受
- 后续改动前先整理地基
- 没有新功能，纯粹清理

## 用什么模型

**GPT 5.5（Codex）**——重构需要理解现有模式，GPT 比 DeepSeek 更擅长。

## 怎么用

在 Claude Code 里输入：

```
/refactor：[文件名]，[问题描述]。
约束：所有测试不动、响应体不变、行为不变。
```

## 举个例子

```
/refactor：services/routes.py 的 agri-zone 错误处理。
问题：40 行 if/elif 链，每个错误码都重复一遍。
目标：收敛为 dict 映射 {code: status}，5 行解决。
约束：所有 test_issue05_agri_zone_routes.py 测试不动。
```

## 会得到什么

- 重构了哪些文件
- 跑什么测试验证行为不变
- 前后代码行数对比
