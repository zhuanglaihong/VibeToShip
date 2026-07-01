---
tags: [trigger, tdd, 实现阶段, 写代码]
model: DeepSeek (Claude Code)
skill: tdd-hub
upstream: "[[02-计划拆解-writing-plans]]"
---

# 03 — TDD 实现（一个一个 Commit 写代码）

## 这个文件干什么

拿到上一步拆好的 commit 清单，从第一个开始，按 TDD 方式写代码：**先写测试 → 再写实现 → 最后清理**。这是整个流程里最耗时间的步骤。

## 什么时候用

- 计划已经拆好，知道要改哪些文件
- 准备开始写代码，不是聊需求

## 用什么模型

用 **DeepSeek（Claude Code）**——执行效率高、输出简洁，适合按计划完成实现。

## 怎么用

在 Claude Code 里输入：

```
/tdd-hub：实现 Commit [编号] — [标题]。
Red：先写测试 [测试文件路径]，覆盖 [场景]。
Green：最小实现 [改动文件路径]。
Refactor：对齐现有代码风格，不改行为。
跑完 [目标测试命令] 和 `bash scripts/guardrails/check_tdd_template.sh` 后告诉我结果。
```

## 举个例子

```
/tdd-hub：实现 Commit 1 — PostGIS 配置与客户端骨架。
Red：写 tests/unit/test_issue05_postgis_client.py，17 个用例。
Green：写 services/postgis_client.py（连接池 + context manager）。
Refactor：对齐 MinIO/DuckDB 客户端模式。
跑完 /Users/gaoyu/miniconda3/envs/met-py311/bin/python -m pytest tests/unit/test_issue05_postgis_client.py -v 和 bash scripts/guardrails/check_tdd_template.sh 后告诉我结果。
```

## 会得到什么

- 测试文件 + 实现代码
- 测试跑完的结果（必须全绿）
- 没通过 AI 会自己修

## 没过怎么办

**没通过就重来，不要跳到下一步。** TDD 是单行道，红了不能走。

## 下一步

每个 commit 通过后 → [[04-代码审查-code-review]]
