---
tags: [trigger, diagnose, 验收阶段, 上线前]
model: GPT5.5 (Codex)
skill: diagnose
upstream: "[[04-代码审查-code-review]]"
---

# 05 — 最终验收

## 这个文件干什么

所有 commit 写完了、审完了，最后跑一次全链路验收。按真实使用场景一步步测，确认功能完整、性能达标。

## 什么时候用

- 所有 commit 通过 review
- 准备上线/合并之前
- 需要确认"这东西真的能用"

## 用什么模型

**GPT 5.5（Codex）**——验收需要全局视角，不是修 bug。

## 怎么用

在 Codex 或 Claude Code 里输入：

```
/diagnose：全链路验收 [功能名称]。
复现路径：
  1) [步骤1] → 预期 [结果]
  2) [步骤2] → 预期 [结果]
  3) [错误路径1] → 预期 [错误码]
性能基线：[P50/P95 目标]。
```

## 举个例子

```
/diagnose：全链路验收 agri-zone forecast 查询。
复现路径：
  1) GET /api/agri-zones → 确认返回 zone 列表
  2) GET /api/agri-zones/feicheng → 确认能力摘要
  3) GET .../forecast?source=gfs&variable=tp&lead_hours_max=24 → cells+weights+series
  4) 传不存在的变量 → 确认 422
  5) 传不存在的 zone → 确认 404
  6) 传两个 source → 确认都能返回
  7) 一个 source 没数据 → 确认 200 + unavailable
性能基线：单 source/单变量/24h < 1s。
```

## 会得到什么

- 每个步骤的通过/失败
- 性能数据
- 漏掉的边界情况
- 能不能上线

## 没过怎么办

- 如果是 bug → 回到 [[03-TDD实现-tdd-hub]] 修复
- 如果是设计问题 → 回到 [[01-需求夯实-grill-me]] 重来

## 下一步

验收通过后 → [[10-PR合并收尾-pr-merge-closeout]]
