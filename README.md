<p align="center">
  <br>
  <h1 align="center">🧠 软件工程 · AI 辅助工作流套件</h1>
  <p align="center">
    <i>面向 Claude Code / Codex 的结构化软件工程流程体系<br>一个 Obsidian Vault，管住从需求到复盘的全部环节</i>
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
  <img src="https://img.shields.io/badge/Obsidian-%23483699?style=for-the-badge&logo=obsidian&logoColor=white" alt="Obsidian">
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
- [👤 作者 & 团队](#author)

---

<a id="why"></a>

## 💡 为什么你需要这个

<p align="center">
  <br>
  <b>你的 AI 编程助手很强，但你的工程流程还是散装的。</b>
  <br><br>
</p>

| 😩 痛点 | ✅ 这个 Vault 的做法 |
|----------|---------------------|
| prompt 散落在聊天记录里，每次重新粘贴 | 12 个 prompt 文件在 Obsidian 中相互链接，点一下就到下一步 |
| AI 写完没人审，质量靠运气 | 代码审查清单逐项检查，最终验收全链路验证 |
| 推理模型写代码、执行模型做架构，角色错配 | **推理模型想 → 执行模型做 → 推理模型审**，各司其职 |
| 流程断在"代码写完" | 从需求到复盘 12 阶段闭环，每个阶段都有质量门禁 |
| 团队协作没有统一规范 | DoD、Commit 格式、PR 模板、分支规则全部内嵌 |
| Claude Code 和 Codex 各管各的 | skill 入口索引统一管理多工具链 |

> **一句话：** 克隆下来，Obsidian 打开，AI 不再是聊天工具，而是你的工程系统。

<br>

---

<a id="quickstart"></a>

## ⚡ 快速开始

```bash
git clone https://gitcode.com/skyveo/obsidian-vault.git
```

**三步跑通：**

**1. 用 Obsidian 打开** — 将这个文件夹作为 Vault 打开

**2. 打开 `00-任务分流`** — 告诉 AI 你要做什么，系统自动判断该进哪个阶段

**3. 安装配套 skill** — 部分 prompt 依赖外部 skill。本 Vault 已自带 16 个通用工程 skill（位于 `skills/`），另外推荐安装：

```bash
# 公共 skill（grill-me、tdd、handoff、to-issues 等）
npx skills@latest add mattpocock/skills
```

> 详见 `playbook/rules/能力入口索引-skill-entry-index.md`

---

### 使用示例

在 Claude Code 或 Codex 中输入：

```
请帮我分流这个任务：我要做一个气象预报查询接口。
前端在地图上选区域，查未来24小时的降水和温度。
数据在 MinIO 的 Parquet 文件里，每天新增几百万行。
```

AI 会判断 → 进入 `01-需求夯实` → 追问边界 → 明确后进入 `02-计划拆解` → …… → 一路到 `10-PR收尾`。

每个阶段 prompt 标注了**该用哪个模型**、**为什么**、**下一步去哪**。不必记流程，跟着链接走就行。

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
  <i>三步两模型，缺一不可。不是建议——是刻在 Vault 规则里的。</i>
</p>

<br>

| 阶段 | 模型类型 | 定位 |
|------|---------|------|
| 🧭 需求分析 · 拆计划 · 审查 · 验收 · 复盘 | **强推理模型** | 擅长推理和架构，负责"想"和"审" |
| ⚡ 代码实现 · TDD · 紧急修复 | **高效率模型** | 执行效率高、输出简洁，负责"做" |

> 📌 Vault 默认以 GPT 5.5 + DeepSeek 为例做了预配置，但方法论本身模型无关。你可以替换为你用惯的任何模型组合——只要保持"强推理 → 高效率 → 强推理"的分工逻辑即可。每个 prompt 的 `model` 字段可自由修改。

<br>

---

<a id="pipeline"></a>

## 🔄 全流程总览

**12 个阶段，覆盖软件工程完整生命周期。**

| # | 阶段 | 一句话 | 推荐模型 |
|---|------|--------|---------|
| 00 | 任务分流 | 不知道该干啥？先来这里 | 推理 |
| 01 | 需求夯实 | AI 反问你，把模糊需求问清楚 | 推理 |
| 02 | 计划拆解 | 拆成独立 commit，逐个击破 | 推理 |
| 03 | TDD 实现 | Red → Green → Refactor | 执行 |
| 04 | 代码审查 | 正确性 · 安全 · 性能 · 覆盖 | 推理 |
| 05 | 最终验收 | 全链路验证，确认能上线 | 推理 |
| 06 | 紧急修复 | 线上炸了？先止血再复盘 | 执行 |
| 07 | 代码重构 | 行为不变，只清理结构 | 推理 |
| 08 | 项目脚手架 | 新项目从 0 到 1 | 推理 |
| 09 | 任务交接 | 做到一半给别人接 | 推理 |
| 10 | PR 合并收尾 | PR 模板、测试结果、回滚方式 | 推理 |
| 11 | 复盘改进 | 事故/Issue 结束后总结 | 推理 |
| 12 | 环境排障 | 依赖、服务、外部异常 | 推理 |

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

Vault 自带 **16 个通用工程 skill**，与 prompt 深度配套。在真实项目中反复打磨，开箱即用。

| Skill | 用途 | 对应 prompt |
|-------|------|------------|
| [`writing-plans`](skills/writing-plans/SKILL.md) | 拆解实现计划，生成独立 commit 清单 | 02-计划拆解 |
| [`diagnose`](skills/diagnose/SKILL.md) | 系统化复现 → 定位 → 修复 Bug | 05-最终验收 |
| [`review`](skills/review/SKILL.md) | 代码审查清单：正确性、安全、性能、覆盖 | 04-代码审查 |
| [`refactor`](skills/refactor/SKILL.md) | 安全重构方案生成，行为不变 | 07-代码重构 |
| [`find-skill`](skills/find-skill/SKILL.md) | 根据关键词发现合适的 skill / rule | 00-任务分流 |
| [`design-an-interface`](skills/design-an-interface/SKILL.md) | 并行生成多种接口设计方案 | 02-计划拆解 |
| [`request-refactor-plan`](skills/request-refactor-plan/SKILL.md) | 重构 RFC，拆成小 commit | 07-代码重构 |
| [`caveman`](skills/caveman/SKILL.md) | 极简通信模式，token 消耗降低 ~75% | 全局 |
| [`qa`](skills/qa/SKILL.md) | 交互式 Bug 上报与 GitHub Issue 归档 | 11-复盘改进 |
| [`zoom-out`](skills/zoom-out/SKILL.md) | 不熟悉代码时获取高层视角 | 全局 |
| [`ubiquitous-language`](skills/ubiquitous-language/SKILL.md) | DDD 通用语言提取，统一团队术语 | 01-需求夯实 |
| [`api-docs`](skills/api-docs/SKILL.md) | API 文档标准与格式约定 | 10-PR收尾 |
| [`docs`](skills/docs/SKILL.md) | 代码 → 文档同步工作流 | 10-PR收尾 |
| [`readme`](skills/readme/SKILL.md) | README 维护标准 | 项目维护 |
| [`git-guardrails-claude-code`](skills/git-guardrails-claude-code/SKILL.md) | 拦截危险 git 命令（push --force 等） | 全局 |
| [`setup-pre-commit`](skills/setup-pre-commit/SKILL.md) | Husky pre-commit hooks 一键配置 | 项目初始化 |

所有 skill 位于 `skills/` 目录，可直接复制到你的项目中使用。

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

Vault 兼容主流 AI 编程工具，skill 入口索引统一管理多端同步：

| 工具 | 适合部署 | 典型用途 |
|------|---------|---------|
| **Claude Code** | 高效率模型 | 编码执行、TDD 实现、紧急修复 |
| **Codex** | 强推理模型 | 架构思考、需求分析、代码审查、验收 |
| **Cursor** | 均可 | 同上，按需配置 |

> 📌 skill 入口索引（`playbook/rules/能力入口索引-skill-entry-index.md`）统一管理三端的 skill 安装与同步。模型可自由替换，方法论不变。

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

## 👤 作者 & 团队

<p>
  <b>高宇</b>  ·  gaoyussdut  ·  iHeadWater
</p>

**大连理工大学 · 水资源与防洪研究所产学研联合团队**

这套流程并非纸上谈兵——它在真实工程中反复打磨成型。从无人机飞控系统到百万级气象数据管线，每一个 prompt、每一个 skill 背后都是踩过的坑。本 Vault 中沉淀的是提炼后的通用工程方法论，配套的 skill 体系也同时在多个生产项目中持续演进。

> 🔗 团队主页：[gitcode.com/dlut-water](https://gitcode.com/dlut-water)  ·  [github.com/iHeadWater](https://github.com/iHeadWater)

<br>

---

## 📄 License

[MIT](LICENSE) © 高宇 · 大连理工大学水资源与防洪研究所产学研联合团队
