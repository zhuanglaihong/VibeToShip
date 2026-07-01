---
tags: [rules, skill, codex, claude, 入口]
---

# 能力入口索引

## 这个文件干什么

说明 Obsidian 流程里用到的 skill/trigger 到底来自哪里。先查这里，再决定去 Claude Code、Codex 还是项目内 `.claude/.codex` 执行。

## 项目内置入口

这些入口应优先放在项目 `.claude/skills/` 和 `.codex/skills/` 中，保证换环境也能跑。

| 入口 | 用途 | 期望位置 |
|------|------|----------|
| `tdd-hub` | Issue/行为变更 TDD 实现 | `.claude/skills/tdd-hub/` + `.codex/skills/tdd-hub/` |
| `review` | 代码审查 | `.claude/skills/review/` + `.codex/skills/review/` |
| `refactor` | 行为不变的重构分析 | `.claude/skills/refactor/` + `.codex/skills/refactor/` |
| `testing` | pytest 执行与失败定位 | `.claude/skills/testing/` + `.codex/skills/testing/` |
| `systematic-debugging` | 系统化复现、定位、最小修复 | 全局 Claude Code skill；Codex 需要拷贝到 `.codex/skills/` 或按 fallback 执行 |

## 外部安装入口

这些入口来自 `mattpocock/skills`。Claude Code 可直接安装，Codex 需要人工拷贝或维护项目镜像。

```bash
npx skills@latest add mattpocock/skills
```

| 入口 | 用途 | Claude Code | Codex |
|------|------|-------------|-------|
| `grill-me` | 需求夯实、追问边界 | 可通过 `npx skills` 安装 | 需要拷贝到 `.codex/skills/` 或写 fallback |
| `writing-plans` | 拆计划、拆 commit/PR | 可通过 `npx skills` 安装 | 需要拷贝到 `.codex/skills/` 或写 fallback |
| `diagnose` | 复现、验收、定位根因 | 可通过 `npx skills` 安装 | 需要拷贝到 `.codex/skills/` 或写 fallback |
| `handoff` | 任务交接 | 可通过 `npx skills` 安装 | 需要拷贝到 `.codex/skills/` 或写 fallback |

## Fallback 规则

如果当前环境没有对应 skill，不要假装已触发。按下面方式处理：

1. 先说明入口缺失。
2. 读取本 vault 对应 prompt 文档。
3. 按 prompt 的目标手动执行同等流程。
4. 把缺失入口记录到 [[../TODO]]。

## 维护规则

- 新增 Obsidian prompt 时，同步在本索引登记入口来源。
- 从 Claude Code 拷贝到 Codex 后，检查 frontmatter 的 `name` 和 `description`。
- 三端配置变化时，对照项目 `.claude/`、`.codex/`、`.cursor/` 保持语义一致。
