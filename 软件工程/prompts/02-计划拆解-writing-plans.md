---
tags: [trigger, plan, 设计阶段, 计划]
model: GPT5.5 (Codex)
skill: writing-plans
upstream: "[[01-需求夯实-grill-me]]"
---

# 02 — 计划拆解

## 这个文件干什么

把上一步确定的需求，拆成一个个小 commit（或 PR）。每个 commit 独立可验证、可以单独回滚。拆完之后就知道先做什么、后做什么。

## 什么时候用

- 需求已经通过 grill 阶段，有了明确边界
- 需要排开发顺序
- 一个功能太大不知道从哪里下手

## 用什么模型

**GPT 5.5（Codex）**——它擅长工程拆解和依赖分析。

## 怎么用

在 Codex 或 Claude Code 里输入：

```
/writing-plans：把 [需求名 / Issue 编号] 拆成可独立交付的小 commit。
每个 commit 告诉我：改哪些文件、写哪些测试、跑什么命令算通过。
列出依赖关系和建议的起手顺序。
```

## 举个例子

```
/writing-plans：把 Issue #05 的 Phase 1 拆成 9 个 commit。
Commit 1：PostGIS 客户端骨架（config + 连接池 + 单测）
Commit 2：zone 只读仓储 + list/detail 接口
Commit 3：forecast_index 路由查询
Commit 4：单 source zone forecast 端到端
建议优先做 1→2→3→4 拿到第一条完整链路，再补 5-9。
```

## 会得到什么

- 编号的 commit 清单（标题 + 文件列表）
- 每个 commit 的测试命令和通过标准
- 依赖关系（哪些必须先做）
- 风险排序（哪个 commit 风险最大）

## 下一步

每个 commit 逐个 → [[03-TDD实现-tdd-hub]]
