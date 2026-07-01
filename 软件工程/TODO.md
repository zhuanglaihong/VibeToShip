---
tags: [todo, skill, codex, claude]
---

# TODO

## Skill 入口对齐

- [ ] 核对 Obsidian 流程文档里的 skill/trigger 是否在项目 `.codex/.claude` 中存在。
  - 当前发现：`grill-me`、`writing-plans`、`diagnose`、`handoff` 在 Obsidian 流程里被当成项目 skill/trigger 使用，但当前项目 `.codex/.claude` 里没有这些 skill，只有全局 `.claude` 里可能存在。
  - 安装来源：Claude Code 可用 `npx skills@latest add mattpocock/skills` 安装这些 skill。
  - Codex 现状：该安装方式只支持 Claude Code；Codex 需要从 Claude Code 安装结果中拷贝一份到项目 `.codex/skills/`，并检查 frontmatter、路径引用和工具名是否可用。
  - 风险：换到 Codex 项目上下文或只加载项目 skill 时，入口可能失效。
  - 待定处理：把这些 skill 补进项目 `.codex/.claude`，或在 Obsidian 文档里标注“依赖全局 Claude Code skill”并给 Codex fallback 指令。
