---
tags: [rules, git, 规范]
---

# Git 规范

## 这个文件干什么

commit 怎么写、分支怎么管。**以后翻 git log 能看懂谁干了啥**。

## Commit 格式

```
<type>: <subject> (<scope>)
```

type：`feat` `fix` `refactor` `docs` `test` `chore`

例：
```
feat: add PostGIS client skeleton (Issue #05 Commit 1)
fix: sanitize nan/inf in Feicheng trend response
docs: update API.md with agri-zone endpoints
```

## 分支规则

- `main` —— 稳定分支，默认只通过 PR 合入
- 小改动也先开短分支，命名用 `fix/xxx`、`docs/xxx` 或 `chore/xxx`
- 大功能开 `feature/xxx` 分支
- 只有个人本地草稿可以临时在 `main` 上试验；形成可提交改动前先切分支

## 推送前

```bash
/Users/gaoyu/miniconda3/envs/met-py311/bin/python -m pytest tests/unit/ -q
bash scripts/guardrails/check_tdd_template.sh
git diff --stat
```
