---
tags: [trigger, env, dependencies, 排障, 外部依赖]
model: DeepSeek (Claude Code)
skill: systematic-debugging
---

# 12 — 外部依赖 / 环境排障

## 这个文件干什么

代码逻辑没法继续推进时，先排环境和外部依赖。比如 conda、Python 解释器、Docker、MinIO、PostGIS、DuckDB 文件锁、Claude/Codex skill 缺失。

## 什么时候用

- 测试不是因为断言失败，而是环境起不来
- 依赖安装失败、解释器不对、命令不存在
- Docker/MinIO/PostGIS/服务端口异常
- DuckDB 文件锁、权限、路径错误
- Claude Code 有 skill，Codex 没有对应入口
- 外部 API 或账号认证异常

## 用什么模型

用 **DeepSeek（Claude Code）**——排障优先快速复现和收集证据。复杂根因或跨系统设计问题再交给 GPT 5.5。

## 怎么用

在 Claude Code 或 Codex 里输入：

```
请排查环境/外部依赖问题。
现象：[报错/命令输出/截图描述]。
期望：[原本应该发生什么]。
约束：先复现、再定位，不改业务逻辑。
```

## 排查顺序

1. 确认当前工作目录和分支。
2. 确认解释器和依赖版本。
3. 复现最小失败命令。
4. 区分是环境问题、外部服务问题还是业务代码问题。
5. 只做最小修复或给出明确操作。
6. 如果变成业务 bug，回 [[03-TDD实现-tdd-hub]]。

## 常用检查

```bash
/Users/gaoyu/miniconda3/envs/met-py311/bin/python --version
/Users/gaoyu/miniconda3/envs/met-py311/bin/python -m pytest tests/unit/<target>.py -v
docker ps
git status --short
bash scripts/guardrails/check_tdd_template.sh
```

## Skill 缺失处理

如果问题是 Claude Code 有 skill、Codex 没入口：

1. 查 [[../rules/能力入口索引-skill-entry-index]]。
2. 确认是否来自 `mattpocock/skills`。
3. Claude Code 可用：

```bash
npx skills@latest add mattpocock/skills
```

4. Codex 需要拷贝到项目 `.codex/skills/`，并检查 frontmatter、路径引用、工具名。
5. 把缺口记录到 [[../TODO]]。

## 会得到什么

- 最小复现命令
- 根因分类
- 修复或绕过步骤
- 是否需要回到 TDD、hotfix 或 PR 收尾流程

## 没通过怎么办

- 线上故障 → [[06-紧急修复-hotfix]]
- 业务行为错误 → [[03-TDD实现-tdd-hub]]
- 设计前提错误 → [[01-需求夯实-grill-me]]
- 只是缺文档 → [[../rules/文档同步矩阵-doc-sync-matrix]]
