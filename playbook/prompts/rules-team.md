---
tags: [rules, 规范, 团队]
---

# 团队规范

## 这个文件干什么

团队共享的编码规范。commit 格式、PR 模板、提交前检查清单、GPT 和 DeepSeek 各自干什么。**不是提示词，是规则——每条都要遵守**。

---

## 流程入口

- 不知道用哪个 prompt → [[00-任务分流-router]]
- 正常功能开发 → [[01-需求夯实-grill-me]] → [[02-计划拆解-writing-plans]] → [[03-TDD实现-tdd-hub]] → [[04-代码审查-code-review]] → [[05-最终验收-diagnose]] → [[10-PR合并收尾-pr-merge-closeout]]
- 线上故障 → [[06-紧急修复-hotfix]]
- 纯重构 → [[07-代码重构-refactor]]
- 新项目 → [[08-新项目脚手架-new-project]]
- 任务交接 → [[09-任务交接-handoff]]
- 合并后复盘 → [[11-复盘流程改进-retro]]
- 环境/依赖问题 → [[12-外部依赖环境排障-env-troubleshooting]]

## Commit 格式

```
<type>: <subject> (<scope>)

例：
feat: add PostGIS client skeleton (Issue #05 Commit 1)
fix: sanitize nan/inf in Feicheng trend response
refactor: consolidate error handler with dict mapping
docs: update API.md with agri-zone endpoints
```

type 只能是这些：`feat` / `fix` / `refactor` / `docs` / `test` / `chore`

## PR Body 模板

统一使用 [[10-PR合并收尾-pr-merge-closeout]] 中的 PR Body 模板，必须包含改动摘要、测试结果、文档同步、关联 Issue 和回滚方式。

## 提交前必须做的事

- [ ] 新增/更新了测试
- [ ] 相关测试全绿
- [ ] Issue/行为变更已通过 `bash scripts/guardrails/check_tdd_template.sh`
- [ ] 相关文档同步了（API.md 等）
- [ ] 没有残留 print/调试日志
- [ ] 没有残留 TODO（除非带 Issue 编号）

详细标准见：
- [[../specs/quality/完成定义-definition-of-done]]
- [[../rules/文档同步矩阵-doc-sync-matrix]]
- [[../rules/失败处理规则-failure-handling]]
- [[../rules/能力入口索引-skill-entry-index]]

## GPT 和 DeepSeek 怎么分工

| 阶段 | 用哪个 | 为什么 |
|------|--------|--------|
| 需求分析、拆计划 | GPT 5.5 (Codex) | 擅长推理和架构 |
| 写代码（TDD） | DeepSeek (Claude Code) | 执行效率高，适合实现 |
| 代码审查 | GPT 5.5 (Codex) | 不能自己审自己 |
| 紧急修 Bug | DeepSeek (Claude Code) | 速度优先 |
| 重构、验收 | GPT 5.5 (Codex) | 需要全局视角 |

## 基本原则

- **GPT 想、DeepSeek 做、GPT 审**——三个步骤用两个模型
- **每个 commit 独立**——可以单独回滚
- **测试先于代码**——TDD 不倒着走
- **文档跟着代码改**——API 变了文档就要变
