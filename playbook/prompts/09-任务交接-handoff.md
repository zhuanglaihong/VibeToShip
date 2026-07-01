---
tags: [trigger, handoff, 协作, 交接]
model: GPT5.5 (Codex)
skill: handoff
---

# 09 — 任务交接

## 这个文件干什么

任务做到一半需要他人接手时，让 AI 自动生成交接文档——已完成什么、卡在哪里、下一步做什么。**接手人不用猜，看完就能继续**。

## 什么时候用

- 需要团队成员接手继续推进
- 遇到阻塞，换人解决
- 临时请假，需要别人顶

## 用什么模型

**GPT 5.5（Codex）**——交接需要清晰的结构化输出。

## 怎么用

在 Claude Code 里输入：

```
/handoff：交接 [分支名 / Issue 编号]。
当前状态：[做到哪了]。
卡在哪里：[阻塞点]。
需要接手人做的事：[具体列表]。
```

## 举个例子

```
/handoff：交接 Issue #05 Commit 4。
已完成：zone forecast 端到端服务写完了，路由和测试 26 个全绿。
卡在：多 source shared-grid 校验还没加。
接手人要做：
  1. 在 agri_zone_forecast_service.py 加 _validate_source_grids()
  2. 在 routes.py 加 source_grid_mismatch → 422
  3. 写多 source 的测试
  4. 跑 /Users/gaoyu/miniconda3/envs/met-py311/bin/python -m pytest tests/unit/test_issue05_agri_zone_forecast_service.py
参考：RFC §9。
```

## 会得到什么

- 已完成清单 + 阻塞点
- 下一步要做什么（明确可执行）
- 要跑的测试命令
- 参考文档链接

## 下一步

接手人完成任务后 → [[10-PR合并收尾-pr-merge-closeout]]
