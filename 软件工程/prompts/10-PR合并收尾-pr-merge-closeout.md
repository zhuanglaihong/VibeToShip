---
tags: [trigger, pr, merge, 收尾, 验收]
model: GPT5.5 (Codex)
skill: review
upstream: "[[05-最终验收-diagnose]]"
---

# 10 — PR / 合并收尾

## 这个文件干什么

功能已经实现、审查和验收都过了，准备提交、开 PR 或合并前做最后一次收口。重点是确认完成定义、测试结果、文档同步和回滚方式。

## 什么时候用

- 准备创建 commit
- 准备开 PR
- PR 合并前最后检查
- 需要写 PR body 或交付说明

## 用什么模型

用 **GPT 5.5（Codex）**——收尾需要全局检查，不能只看最后一个文件。

## 怎么用

在 Codex 或 Claude Code 里输入：

```
请按 PR / 合并收尾检查当前改动。
范围：[分支 / Issue / Commit 列表]。
对照完成定义、文档同步矩阵、测试结果和回滚方式。
输出可直接用于 PR body 的内容。
```

## 检查清单

- [ ] 对照 [[../specs/quality/完成定义-definition-of-done]]
- [ ] 对照 [[../rules/文档同步矩阵-doc-sync-matrix]]
- [ ] 确认 review 阻塞问题已修复
- [ ] 确认 diagnose 验收路径已通过
- [ ] 确认目标测试和最小回归已通过
- [ ] Issue/行为变更已通过 `bash scripts/guardrails/check_tdd_template.sh`
- [ ] PR body 写清楚改动摘要、测试结果、关联 Issue、回滚方式

## PR Body 模板

```markdown
## 改动摘要
- xxx

## 测试结果
/Users/gaoyu/miniconda3/envs/met-py311/bin/python -m pytest tests/unit/test_xxx.py -v  # N passed
bash scripts/guardrails/check_tdd_template.sh  # passed

## 文档同步
- 已同步：docs/reference/API.md
- 无需同步：未改变 API/数据流/issue 状态

## 关联 Issue
Closes #NN

## 回滚方式
git revert <commit>
```

## 没通过怎么办

- 测试失败 → 回 [[03-TDD实现-tdd-hub]]
- review 阻塞 → 回 [[04-代码审查-code-review]] 和 [[03-TDD实现-tdd-hub]]
- 文档缺失 → 按 [[../rules/文档同步矩阵-doc-sync-matrix]] 补齐
- 环境异常 → 回 [[12-外部依赖环境排障-env-troubleshooting]]

## 下一步

合并后 → [[11-复盘流程改进-retro]]
