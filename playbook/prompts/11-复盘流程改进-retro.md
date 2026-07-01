---
tags: [trigger, retro, 复盘, 改进]
model: GPT5.5 (Codex)
skill: review
upstream: "[[10-PR合并收尾-pr-merge-closeout]]"
---

# 11 — 复盘 / 流程改进

## 这个文件干什么

一个 Issue、PR、事故或阶段性任务结束后，记录这次哪些地方顺、哪些地方卡、哪些 prompt/rule/skill 需要改。目标是让流程越用越准。

## 什么时候用

- Issue 关闭后
- PR 合并后
- 线上事故修完后
- 一个阶段做完，发现流程有重复劳动或漏项
- 某个 prompt 不好用，需要沉淀改进

## 用什么模型

用 **GPT 5.5（Codex）**——复盘需要抽象经验和更新流程。

## 怎么用

在 Codex 或 Claude Code 里输入：

```
请复盘 [Issue/PR/事故/任务名]。
输入材料：[需求文档、PR、测试结果、review finding、事故日志]。
输出：做得好的、踩坑的、流程缺口、需要更新的 Obsidian 文档和项目 skill/rule。
```

## 复盘模板

```markdown
## 背景
- 任务：
- 时间：
- 参与角色：

## 结果
- 是否完成：
- 关键测试：
- 是否上线/合并：

## 做得好的
- xxx

## 踩坑和根因
- 现象：
- 根因：
- 如何避免：

## 流程缺口
- 缺哪个 prompt：
- 哪条 rule 不清楚：
- 哪个 skill/agent 缺失：

## 后续 TODO
- [ ] 更新 Obsidian 文档：
- [ ] 更新项目 `.claude/.codex`：
- [ ] 补测试或门禁：
```

## 输出要求

- 只记录可复用经验，不写情绪化评价。
- 每个改进项都要有落点：Obsidian 文档、项目 rule、skill、agent、测试或脚本。
- 如果发现 skill 入口不一致，更新 [[../TODO]] 和 [[../rules/能力入口索引-skill-entry-index]]。

## 下一步

有流程改动 → 更新对应文档；没有改动 → 归档复盘结论。
