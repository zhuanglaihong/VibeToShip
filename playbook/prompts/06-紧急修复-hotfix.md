---
tags: [trigger, hotfix, 紧急, 跳过流程]
model: DeepSeek (Claude Code)
skill: systematic-debugging
---

# 06 — 紧急修复（线上故障，先恢复服务）

## 这个文件干什么

线上服务不可用、接口 500、数据对不上——需要立刻定位根因、最小修复。**先恢复服务，不做大范围重构**。进入 PR/合并收尾前，仍然要补齐必要测试和门禁。

## 什么时候用

- 线上告警（500、超时、数据异常）
- 需要几分钟内出修复
- 不是规划好的开发任务

## 用什么模型

**DeepSeek（Claude Code）**——速度优先，适合快速复现和最小修复。

## 怎么用

在 Claude Code 里输入：

```
Bug：[现象]，[复现步骤]。只定位根因，加最小修复，单测验证。不改架构。
```

## 举个例子

```
Bug：GET /api/feicheng/grids/g_4_4/trend 返回 500。
日志：json.dumps 报 Out of range float values。
只定位哪里产生 nan/inf，加数学检查，不改架构。
```

## 会得到什么

- 根因一句话
- 最小 diff（通常几行）
- 回归测试命令

## 注意

这个 trigger 可以先跳过 01-04 的完整规划流程，但不能跳过最终质量门禁：

- 如果改了业务行为，进入 [[10-PR合并收尾-pr-merge-closeout]] 前必须补测试。
- Issue/行为变更仍需通过 `bash scripts/guardrails/check_tdd_template.sh`。
- 如果修复扩大成设计调整，回到 [[01-需求夯实-grill-me]] 或 [[02-计划拆解-writing-plans]]。
