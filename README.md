<p align="center">
  <br>
  <h1 align="center">VibeToShip · AI 辅助软件工程工作流</h1>
  <p align="center">
    <i>把模糊想法沿着可审查、可验证的工程路径推进到交付<br>面向 Claude Code/ Codex/ Cursor，适合个人开发者和小团队直接使用</i>
  </p>
</p>

<p align="center">
  <a href="#why"><img src="https://img.shields.io/badge/导航-目录-blue?style=flat-square"></a>
  <a href="#quickstart"><img src="https://img.shields.io/badge/快速开始-30s-10B981?style=flat-square"></a>
  <a href="#core-design"><img src="https://img.shields.io/badge/核心-模型分工-F59E0B?style=flat-square"></a>
  <a href="#pipeline"><img src="https://img.shields.io/badge/流程-12阶段-8B5CF6?style=flat-square"></a>
  <a href="#skills"><img src="https://img.shields.io/badge/Skill-16个-EC4899?style=flat-square"></a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Obsidian-Optional-483699?style=for-the-badge&logo=obsidian&logoColor=white" alt="Obsidian optional">
  <img src="https://img.shields.io/badge/Claude_Code-Compatible-2563EB?style=for-the-badge" alt="Claude Code">
  <img src="https://img.shields.io/badge/Codex-Compatible-10B981?style=for-the-badge" alt="Codex">
  <img src="https://img.shields.io/badge/license-MIT-green?style=for-the-badge" alt="License">
</p>

<br>

---

## 📑 导航

- [💡 为什么你需要这个](#why)
- [⚡ 快速开始](#quickstart)
- [🧠 核心设计：模型分工体系](#core-design)
- [🔄 全流程总览](#pipeline)
- [📦 项目结构](#structure)
- [🔧 配套 Skill](#skills)
- [🛡️ 配套体系](#quality)
- [👤 创作者](#author)

---

<a id="why"></a>

## 💡 为什么你需要这个

<p align="center">
  <br>
  <b>你的 AI 编程助手很强，但你的工程流程还是散装的。</b>
  <br><br>
</p>

| 😩 痛点 | ✅ VibeToShip 的做法 |
|----------|---------------------|
| prompt 散落在聊天记录里，每次重新粘贴 | 12 个阶段入口和工程规则保存在仓库中，任何 Agent 都能按路径继续 |
| AI 写完没人审，质量靠运气 | 代码审查清单逐项检查，最终验收全链路验证 |
| 推理模型写代码、执行模型做架构，角色错配 | **推理模型想 → 执行模型做 → 推理模型审**，各司其职 |
| 流程断在"代码写完" | 从需求到复盘 12 阶段闭环，每个阶段都有质量门禁 |
| 团队协作没有统一规范 | DoD、Commit 格式、PR 模板、分支规则全部内嵌 |
| Claude Code 和 Codex 各管各的 | skill 入口索引统一管理多工具链 |

> **一句话：** 从 Vibe 到 Ship——让 AI 不只负责写代码，也沿着完整工程流程把软件可靠地交付出去。

<br>

---

<a id="quickstart"></a>

## ⚡ 快速开始

```bash
git clone https://github.com/zhuanglaihong/VibeToShip.git
```

### 方式一：直接交给编程 Agent（推荐）

不需要安装 Obsidian，也不需要记住阶段编号。将 VibeToShip 和你的目标项目同时打开，或在对话中给出 VibeToShip 的本地路径，然后告诉 Claude Code、Codex、Cursor 或其他能读取本地文件的编程 Agent：

```text
请使用 VibeToShip 开发当前项目。
先阅读 VibeToShip/playbook/prompts/00-任务分流-router.md、
playbook/rules/模型分工-model-roles.md 和
playbook/rules/能力入口索引-skill-entry-index.md，
再按任务类型选择最小充分流程。
```

例如：

```text
使用 VibeToShip，为当前项目增加订单失败自动重试功能。
```

Agent 会先分流任务，再按需要进入需求澄清、计划、TDD、审查、验收或交接流程。

### 方式二：用 Obsidian 浏览工作流（可选）

如果你喜欢可视化浏览和笔记链接，将本仓库作为 Obsidian Vault 打开即可。Obsidian 只负责阅读和导航，不是使用 VibeToShip 的前提。

### 方式三：按需安装配套 Skills（可选）

本仓库自带 16 个通用工程 Skill（位于 `skills/`）。你可以直接让 Agent 阅读对应的 `SKILL.md`，或按所用工具的安装方式把需要的 Skill 加入本地环境；不必一次安装全部。另有可选的公共 Skill：

```bash
# 公共 skill（grill-me、tdd、handoff、to-issues 等）
npx skills@latest add mattpocock/skills
```

> 各工具的能力映射见 `playbook/rules/能力入口索引-skill-entry-index.md`。

<br>

---

<a id="core-design"></a>

## 🧠 核心设计：模型分工体系

<p align="center">
  <br>
  <b>AI 写代码最大的浪费：所有任务用同一个模型。</b><br>
  强推理模型擅长架构和审查，但执行效率低、成本高。<br>
  高效率模型写代码飞快，但让它做架构设计，欠考虑。
  <br><br>
</p>

<p align="center">
  <b><code>推理模型想 → 执行模型做 → 推理模型审</code></b>
  <br><br>
  <i>三步两模型，缺一不可。不是建议——是写进工作流规则里的。</i>
</p>

<br>

| 阶段 | 模型类型 | 定位 |
|------|---------|------|
| 🧭 需求分析 · 拆计划 · 审查 · 验收 · 复盘 | **强推理模型** | 擅长推理和架构，负责"想"和"审" |
| ⚡ 代码实现 · TDD · 紧急修复 | **高效率模型** | 执行效率高、输出简洁，负责"做" |

> 📌 VibeToShip 不绑定模型或工具。你可以替换为惯用的任意组合，只要保持“强推理 → 高效率 → 强推理”的分工逻辑即可。

<br>

---

<a id="pipeline"></a>

## 🔄 全流程总览

**12 个阶段，覆盖软件工程完整生命周期。**

| #   | 阶段      | 一句话                    | 推荐模型 |
| --- | ------- | ---------------------- | ---- |
| 00  | 任务分流    | 不知道该干啥？先来这里            | 推理   |
| 01  | 需求夯实    | AI 反问你，把模糊需求问清楚        | 推理   |
| 02  | 计划拆解    | 拆成独立 commit，逐个击破       | 推理   |
| 03  | TDD 实现  | Red → Green → Refactor | 执行   |
| 04  | 代码审查    | 正确性 · 安全 · 性能 · 覆盖     | 推理   |
| 05  | 最终验收    | 全链路验证，确认能上线            | 推理   |
| 06  | 紧急修复    | 线上炸了？先止血再复盘            | 执行   |
| 07  | 代码重构    | 行为不变，只清理结构             | 推理   |
| 08  | 项目脚手架   | 新项目从 0 到 1             | 推理   |
| 09  | 任务交接    | 做到一半给别人接               | 推理   |
| 10  | PR 合并收尾 | PR 模板、测试结果、回滚方式        | 推理   |
| 11  | 复盘改进    | 事故/Issue 结束后总结         | 推理   |
| 12  | 环境排障    | 依赖、服务、外部异常             | 推理   |

### 典型快乐路径

```
00 分流 → 01 夯实 → 02 拆计划 → 03 TDD → 04 审查 → 05 验收 → 10 PR收尾 → 11 复盘
```

出现故障时分支到 `06 紧急修复`；只清理结构走 `07 重构`；新项目走 `08 脚手架`。

> 📌 每个 prompt 通过 `下一步 →` 链接指向后续阶段——不需要背流程。

<br>

---

<a id="structure"></a>

## 📦 项目结构

```
playbook/                       ← 工程方法论核心
│
├── prompts/                    ← 提示词：12 个阶段 + 团队规范，每个文件标注模型、下一步
│   ├── 00-任务分流-router.md           ← 入口：不知道该干什么，先来这里
│   ├── 01-需求夯实-grill-me.md         ← AI 反问，把模糊需求问清楚
│   ├── 02-计划拆解-writing-plans.md    ← 拆成独立 commit，逐个击破
│   ├── 03-TDD实现-tdd-hub.md           ← Red → Green → Refactor
│   ├── 04-代码审查-code-review.md      ← 正确性 · 安全 · 性能 · 覆盖
│   ├── 05-最终验收-diagnose.md         ← 全链路验证
│   ├── 06-紧急修复-hotfix.md           ← 线上故障，先止血再复盘
│   ├── 07-代码重构-refactor.md         ← 行为不变，只清理结构
│   ├── 08-新项目脚手架-new-project.md  ← 从 0 到 1
│   ├── 09-任务交接-handoff.md          ← 做到一半转交他人
│   ├── 10-PR合并收尾-pr-merge-closeout.md
│   ├── 11-复盘流程改进-retro.md
│   ├── 12-外部依赖环境排障-env-troubleshooting.md
│   └── rules-team.md                    ← 团队共享规范
│
├── rules/                      ← 工程规范：模型的、Git 的、编码的、协作的
│   ├── 能力入口索引-skill-entry-index.md  ← 三端 skill 统一管理
│   ├── 模型分工-model-roles.md            ← 推理模型 vs 执行模型
│   ├── Git规范-git-convention.md          ← commit 格式、分支管理
│   ├── 编码规范-python-style.md           ← 代码风格约定
│   ├── 失败处理规则-failure-handling.md   ← 异常处理策略
│   └── 文档同步矩阵-doc-sync-matrix.md    ← 代码变更 → 文档同步
│
└── specs/                      ← 质量标准 + 设计模板
    ├── quality/
    │   ├── 完成定义-definition-of-done.md       ← 代码写完 ≠ 完成
    │   ├── 测试策略-test-strategy.md            ← 测什么、怎么测
    │   ├── 审查清单-code-review-checklist.md    ← 审查逐项检查
    │   └── 性能基线-performance-baseline.md     ← 性能基准
    ├── design/
    │   ├── adr/模板-架构决策记录-ADR-template.md
    │   └── rfc/模板-RFC设计文档-template.md
    └── contracts/
        └── 模板-接口契约-API-contract-template.md

skills/                         ← 16 个通用工程 skill，开箱即用
README.md
```

<br>

---

<a id="skills"></a>

## 🔧 配套 Skill

VibeToShip 自带 **16 个通用工程 Skill**，与 Prompt 深度配套。

> ⚠️ **注意：** 标 📝 的 skill 为通用模板，需结合你的项目做微调（如替换业务术语、端口号、文件路径等）。标 ✅ 的开箱即用。

| Skill | 用途 | 对应 prompt | 状态 |
|-------|------|------------|------|
| [`writing-plans`](skills/writing-plans/SKILL.md) | 拆解实现计划，生成独立 commit 清单 | 02-计划拆解 | ✅ |
| [`diagnose`](skills/diagnose/SKILL.md) | 系统化复现 → 定位 → 修复 Bug | 05-最终验收 | ✅ |
| [`review`](skills/review/SKILL.md) | 代码审查清单：正确性、安全、性能、覆盖 | 04-代码审查 | ✅ |
| [`refactor`](skills/refactor/SKILL.md) | 安全重构方案生成，行为不变 | 07-代码重构 | ✅ |
| [`find-skill`](skills/find-skill/SKILL.md) | 根据关键词发现合适的 skill / rule | 00-任务分流 | ✅ |
| [`design-an-interface`](skills/design-an-interface/SKILL.md) | 并行生成多种接口设计方案 | 02-计划拆解 | ✅ |
| [`request-refactor-plan`](skills/request-refactor-plan/SKILL.md) | 重构 RFC，拆成小 commit | 07-代码重构 | ✅ |
| [`caveman`](skills/caveman/SKILL.md) | 极简通信模式，token 消耗降低 ~75% | 全局 | ✅ |
| [`qa`](skills/qa/SKILL.md) | 交互式 Bug 上报与 GitHub Issue 归档 | 11-复盘改进 | ✅ |
| [`zoom-out`](skills/zoom-out/SKILL.md) | 不熟悉代码时获取高层视角 | 全局 | ✅ |
| [`git-guardrails-claude-code`](skills/git-guardrails-claude-code/SKILL.md) | 拦截危险 git 命令（push --force 等） | 全局 | ✅ |
| [`setup-pre-commit`](skills/setup-pre-commit/SKILL.md) | Husky pre-commit hooks 一键配置 | 项目初始化 | ✅ |
| [`ubiquitous-language`](skills/ubiquitous-language/SKILL.md) | DDD 通用语言提取，统一团队术语 | 01-需求夯实 | 📝 |
| [`api-docs`](skills/api-docs/SKILL.md) | API 文档标准与格式约定 | 10-PR收尾 | 📝 |
| [`docs`](skills/docs/SKILL.md) | 代码 → 文档同步工作流 | 10-PR收尾 | 📝 |
| [`readme`](skills/readme/SKILL.md) | README 维护标准 | 项目维护 | 📝 |

所有 skill 位于 `skills/` 目录，可直接复制到你的项目中使用。📝 类 skill 建议先替换其中的占位路径和业务关键词再投入使用。

> 💡 此外推荐安装 Matt Pocock 的公共 skill（grill-me、tdd、handoff、to-issues、to-prd、triage 等）：
> ```bash
> npx skills@latest add mattpocock/skills
> ```

<br>

---

<a id="quality"></a>

## 🛡️ 配套体系

### 质量门禁（Definition of Done）

代码写完 ≠ 完成。每个任务必须通过：

- ✅ 新增/更新测试，目标测试 + 回归全绿
- ✅ 审查清单逐项通过：正确性、安全、性能、测试覆盖
- ✅ API 变化同步文档
- ✅ 无残留 `print` / 调试日志 / 无编号 TODO
- ✅ PR 写清楚测试结果和回滚方式

### 多工具链

VibeToShip 兼容主流 AI 编程工具，Skill 入口索引统一管理多端同步：

| 工具 | 适合部署 | 典型用途 |
|------|---------|---------|
| **Claude Code** | 高效率模型 | 编码执行、TDD 实现、紧急修复 |
| **Codex** | 强推理模型 | 架构思考、需求分析、代码审查、验收 |
| **Cursor** | 均可 | 同上，按需配置 |

> 📌 Skill 入口索引（`playbook/rules/能力入口索引-skill-entry-index.md`）统一管理三端的 Skill 安装与同步。模型可自由替换，方法论不变。

### 团队规范

| 规范 | 约定 |
|------|------|
| Commit | `<type>: <subject> (<scope>)` — `feat` / `fix` / `refactor` / `docs` / `test` / `chore` |
| PR Body | 改动摘要 + 测试结果 + 文档同步 + 回滚方式 |
| 分支 | `main` 稳定分支，功能走 `feature/xxx`，修 bug 走 `fix/xxx` |
| 提交前 | 跑测试 → 跑 TDD 门禁脚本 → `git diff --stat` |

<br>

---

<a id="author"></a>

## 👤 创作者

<p>
  <b>庄赖宏</b>  ·  zhuanglaihong  ·  iHeadWater
</p>

**硕士研究生 · iHeadWater 团队成员**

目前正处于从研究训练走向真实工程实践的阶段，持续探索如何借助 Claude Code、Codex 等 AI 编程助手，把想法更快地做成可运行的软件，同时不丢掉需求澄清、测试验证、代码审查和交付复盘这些工程基本功。

VibeToShip 就来自这些实践中的体感：AI 能很快写出第一版代码，但要把 Vibe 真正推到 Ship，仍需要一条清晰、可复用的工程路径。这个项目去掉具体业务场景，沉淀下适合个人开发者和小团队直接使用的工作流。

> GitHub：[zhuanglaihong](https://github.com/zhuanglaihong) · 所属团队：[iHeadWater](https://github.com/iHeadWater)

<br>

---

## 📄 License

[MIT](LICENSE) © 庄赖宏 · 大连理工大学水资源与防洪研究所产学研联合团队
