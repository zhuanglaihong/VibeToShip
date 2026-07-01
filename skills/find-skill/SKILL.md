---
name: find-skill
description: 技能/规则/Agent 发现助手 — 根据关键词找到正确的 skill、rule 或 agent。触发词：用什么 skill、找 skill、哪个规则、skill 列表、怎么处理、该用谁、我能做什么、不知道找谁
---

# Find Skill — 技能/规则/Agent 发现助手

> **本 skill 是元技能**：回答"我能做什么"、"该用谁"、"哪个 rule 必读"等问题。
>
> 找到对应 skill/rule/agent 后，调用方负责实际执行。

## 调度优先级（重要）

```
Rule (硬约束)  →  Skill (工作流)  →  Agent (执行体)
   必读            怎么做            谁来做
```

**Rule 优先于 Skill** —— 修改文件前先确认有没有对应硬约束 rule。
**Skill 优先于 Agent** —— skill 提供完整工作流，agent 只是具体执行者。

---

## 1. 按口语化场景速查

### 1.1 紧急情况

| 你说 | 优先级 1（rule） | 优先级 2（skill） | 优先级 3（agent） |
|---|---|---|---|
| "飞控失联了"、"心跳没了" | `connection-safety` | `connection-setup` | `connection-debugger` |
| "紧急停止"、"emergency"、"abort" | `connection-safety` | `emergency-response` | `connection-debugger` |
| "坠机"、"炸机"、"失控" | `connection-safety` + `mavlink-protocol` | `emergency-response` | `connection-debugger` |
| "失控保护没触发" | `connection-safety` | `flight-safety` | `connection-debugger` |

### 1.2 资源与性能（机载重点）

| 你说 | Rule | Skill | Agent |
|---|---|---|---|
| "内存爆了"、"OOM"、"内存泄漏" | `onboard-constraints` | `onboard-resource-watch` | `deploy-ops` |
| "CPU 满了"、"CPU 占用高" | `onboard-constraints` | `onboard-resource-watch` | `deploy-ops` |
| "启动太慢"、"开机慢" | `onboard-constraints` | `onboard-resource-watch` | `deploy-ops` |
| "板卡跑不起来" | `onboard-constraints` | `field-deploy` | `deploy-ops` |

### 1.3 断网与容错

| 你说 | Rule | Skill | Agent |
|---|---|---|---|
| "断网了"、"4G 断了" | `onboard-constraints` | `onboard-network-resilience` | `deploy-ops` |
| "数传断了"、"数传模块不通" | `connection-safety` | `onboard-network-resilience` + `network-setup` | `connection-debugger` |
| "mavlink router 转发失败" | `mavlink-protocol` | `network-setup` | `connection-debugger` |
| "重连不上" | `connection-safety` | `connection-setup` | `connection-debugger` |

### 1.4 兼容性与错误

| 你说 | Rule | Skill | Agent |
|---|---|---|---|
| "Python 3.8 报错"、"语法错误" | `onboard-constraints` + `python-style` | `py38-compat-check` | `test-fixer` |
| "import 出错"、"模块找不到" | `onboard-constraints` | `py38-compat-check` | `test-fixer` |
| "在 RK3588 跑不起来" | `onboard-constraints` | `field-deploy` | `deploy-ops` |

### 1.5 现场部署与飞行

| 你说 | Rule | Skill | Agent |
|---|---|---|---|
| "现场装机"、"烧镜像" | `px4-sitl` | `field-deploy` | `sitl-qa-runner` |
| "起飞前检查"、"桨叶" | `connection-safety` + `mavlink-protocol` | `flight-safety` | `sitl-qa-runner` |
| "GPS 没锁定" | `connection-safety` | `connection-setup` | `connection-debugger` |
| "磁罗盘飘" | `mavlink-protocol` | `flight-safety` | `sitl-qa-runner` |
| "一键返航"、"RTL" | `mission-protocol` | `mission-planning` | `mission-specialist` |
| "云台"、"gimbal"、"品灵"、"Viewpro"、"Q30T"、"Q36T"、"gimbal_bridge" | `gimbal-integration` + `mqtt-topic-contract` | `gimbal-hitl` | `onboard-qa-runner` |
| "相机 MQTT"、"camera_action"、"RTSP"、"RTMP" | `gimbal-integration` + `mqtt-topic-contract` | `gimbal-hitl` | `onboard-qa-runner` |
| "HITL"、"人在回路"、"测试流程" | `connection-safety` + `gimbal-integration`（如含云台） | `flight-safety` / `gimbal-hitl` → 参考 `docs/hitl/` | `sitl-qa-runner` |
| "安全验证"、"飞行前验证" | `connection-safety` | `docs/hitl/runbook-rk3588-gimbal.md` | `sitl-qa-runner` |

### 1.6 飞行控制

| 你说 | Rule | Skill | Agent |
|---|---|---|---|
| "解锁失败"、"arm 不上" | `connection-safety` | `flight-safety` | `test-fixer` |
| "起飞失败"、"takeoff 报错" | `connection-safety` | `flight-safety` | `test-fixer` |
| "航点上传失败" | `mission-protocol` | `mission-planning` | `mission-specialist` |
| "任务执行到一半停了" | `mission-protocol` | `mission-planning` | `mission-specialist` |
| "多机编队"、"机群调度" | `fleet-management` | `fleet-orchestration` | `fleet-orchestrator` |
| "改 geofence"、"围栏" | | `geofence-management` | `geofence-specialist` |

### 1.7 数据与接口

| 你说 | Rule | Skill | Agent |
|---|---|---|---|
| "遥测数据不更新" | `mavlink-protocol` | `telemetry-dashboard` | `telemetry-specialist` |
| "WebSocket 断了" | `backend-conventions` | `telemetry-dashboard` | `telemetry-specialist` |
| "API 504 超时" | | `api-docs` | `api-contract-guardian` |
| "API 兼容性" | | `api-docs` | `api-contract-guardian` |

### 1.8 测试

| 你说 | Rule | Skill | Agent |
|---|---|---|---|
| "测试挂了"、"pytest 失败" | `tdd-workflow` + `tdd-test-scope` | `testing` | `test-fixer` |
| "覆盖率不够" | `tdd-workflow` | `testing` | `test-fixer` |
| "机载代码 QA" | `onboard-constraints` | `onboard-resource-watch` | `onboard-qa-runner` |

### 1.9 业务实现

| 你说 | Rule | Skill | Agent |
|---|---|---|---|
| "加个接口"、"新加 API" | `backend-conventions` | `api-docs` | `backend-implementer` |
| "改前端"、"加面板" | `frontend-conventions` | — | `frontend-implementer` |
| "代码审查" | `python-style` + `tdd-workflow` | `review` | `api-contract-guardian` |
| "重构" | `python-style` | `refactor` | — |

### 1.10 文档与变更

| 你说 | Rule | Skill | Agent |
|---|---|---|---|
| "改文档"、"更新 README" | `docs-sync` | `docs` / `readme` | — |
| "发版前检查" | `docs-sync` | `testing` | `release-smoke-runner` |

---

## 2. 按文件类型速查（修改前必看）

| 修改的文件 | 必读 rule（优先级 1） | 推荐 skill（优先级 2） | 委派 agent（优先级 3） |
|---|---|---|---|
| `src/connection.py` | `connection-safety` + `mavlink-protocol` + `pixhawk-integration` + `onboard-constraints` | `connection-setup` → 改后必读 `docs/hitl.md` | `connection-debugger` |
| `src/control.py` | `mavlink-protocol` + `pixhawk-integration` + `onboard-constraints` | `flight-safety` → 改后必读 `docs/hitl.md` | `connection-debugger` |
| `src/mission.py` | `mission-protocol` + `mavlink-protocol` + `onboard-constraints` | `mission-planning` → 改后必读 `docs/hitl.md` | `mission-specialist` |
| `src/fleet.py` | `fleet-management` + `onboard-constraints` | `fleet-orchestration` → 改后必读 `docs/hitl.md` | `fleet-orchestrator` |
| `src/telemetry.py` | `mavlink-protocol` + `onboard-constraints` | `telemetry-dashboard` | `telemetry-specialist` |
| `src/config.py` | `pixhawk-integration` + `onboard-constraints` | `connection-setup` | `connection-debugger` |
| `src/logger.py` | `python-style` + `onboard-constraints` | `py38-compat-check` | `test-fixer` |
| 任何 `src/*.py` | **`onboard-constraints`（机载硬约束）** | `py38-compat-check` | `onboard-qa-runner` |
| `backend/app/routers/*.py` | `backend-conventions` | `api-docs` | `api-contract-guardian` |
| `backend/app/services/*.py` | `backend-conventions` | `review` | `api-contract-guardian` |
| `backend/app/ws/*.py` | `backend-conventions` | `telemetry-dashboard` | `telemetry-specialist` |
| `frontend/src/**/*.vue` | `frontend-conventions` | — | `frontend-implementer` |
| `frontend/src/**/*.ts` | `frontend-conventions` | — | `frontend-implementer` |
| `docs/hitl.md` / `docs/hitl/**/*.md` | `docs-sync` + `connection-safety` + `gimbal-integration` | `flight-safety` / `gimbal-hitl` | `sitl-qa-runner` / `onboard-qa-runner` |
| `docs/连接方式指南.md` | `connection-safety` | `connection-setup` | `connection-debugger` |
| `docs/PX4-SITL-Docker使用指南.md` | `px4-sitl` | `sitl-setup` | `sitl-qa-runner` |
| `docs/reference/mqtt-vs-mavlink-evaluation.md` | `mavlink-protocol` | `connection-setup` | `mavlink-specialist` |
| `.claude/rules/*.md` | `python-style` | `find-skill` | — |
| `.codex/skills/*.md` | `python-style` | `find-skill` | — |
| `.codex/agents/*.md` | `python-style` | `find-skill` | — |
| **任何代码文件** | `tdd-workflow` + `tdd-test-scope` + `reply-language` | `testing` | — |

---

## 3. 完整 Skill 列表

### 3.1 TDD 与质量
- `testing` — 测试执行与失败诊断
- `review` — 三级代码审查（🔴🟡🟢）
- `refactor` — 安全重构建议

### 3.2 飞控与连接
- `connection-setup` — 飞控连接建立
- `mavlink-debug` — MAVLink 协议调试
- `flight-safety` — 飞行安全检查
- `sitl-setup` — PX4 SITL 模拟环境
- `px4-integration` — PX4 集成知识
- `network-setup` — 网络配置

### 3.3 任务与编队
- `mission-planning` — 航点任务规划
- `fleet-orchestration` — 多机编队协同
- `geofence-management` — 地理围栏

### 3.4 遥测与数据
- `telemetry-dashboard` — 遥测面板与数据流
- `api-docs` — REST API 文档

### 3.5 文档与交付
- `docs` — 文档更新工作流
- `readme` — README 维护

### 3.6 机载专项
- `onboard-resource-watch` — 机载资源监测与优化
- `onboard-network-resilience` — 断网/弱网容错
- `py38-compat-check` — Python 3.8 兼容性检查
- `emergency-response` — 紧急情况标准操作
- `field-deploy` — 现场部署与飞行前检查

### 3.7 元技能
- `find-skill` — 本 skill（调度中心）

---

## 4. 完整 Agent 列表

详见 `.claude/agents/agent-index.md`。速查：

### 4.1 调试类（haiku）
- `connection-debugger` — 飞控连接故障排查
- `test-fixer` — pytest 失败修复
- `deploy-ops` — 部署/资源问题
- `sitl-qa-runner` — SITL 模拟器质量验证
- `onboard-qa-runner` — 机载程序专项 QA
- `release-smoke-runner` — 发版前烟测

### 4.2 实现类（sonnet）
- `backend-implementer` — FastAPI 后端实现
- `frontend-implementer` — Vue 3 GCS 前端实现
- `mavlink-specialist` — MAVLink 协议深度专家
- `mission-specialist` — 任务/航点协议专家
- `fleet-orchestrator` — 多机编队调度
- `telemetry-specialist` — 遥测数据管线专家
- `api-contract-guardian` — API 契约稳定性守护
- `geofence-specialist` — 地理围栏专项

### 4.3 索引
- `agent-index` — agent 调度索引

---

## 5. 完整 Rule 列表

详见 `.claude/rules/README.md`。

### 5.1 alwaysApply: true（每次必读）
- `project-conventions` — 项目总体约定
- `connection-safety` — 连接安全
- `onboard-constraints` — 机载硬约束（RK3588 / Python 3.8）
- `tdd-workflow` — TDD 红-绿-重构
- `tdd-test-scope` — 测试范围与执行
- `docs-sync` — 文档同步规则
- `reply-language` — 简体中文回复

### 5.2 按文件类型触发
- `pixhawk-integration` (src/**/*.py) — 核心架构
- `mavlink-protocol` (src/**/*.py) — MAVLink 协议
- `mission-protocol` (src/mission.py) — 任务协议
- `fleet-management` (src/fleet.py) — 机群管理
- `python-style` (**/*.py) — Python 风格
- `code-style` (**/*.py) — 代码风格别名
- `backend-conventions` (backend/**/*.py) — FastAPI 约定
- `frontend-conventions` (frontend/**/*.{ts,vue}) — Vue 3 约定
- `svg-generation` (svg) — SVG 图表标准
- `px4-sitl` — PX4 SITL 仿真
- — API 变更日志

---

## 6. 文档入口

`docs/` 精简到 4 个文件：

- `docs/hitl.md` — HITL 文档入口
- `docs/hitl/architecture-rk3588-gimbal.md` — 当前 RK3588 + 品灵云台架构
- `docs/hitl/runbook-rk3588-gimbal.md` — 现场 HITL runbook
- `docs/hitl/mqtt-topics.md` — MQTT topic 契约
- `docs/连接方式指南.md` — 飞控连接方式速查
- `docs/PX4-SITL-Docker使用指南.md` — SITL 环境搭建
- `docs/reference/mqtt-vs-mavlink-evaluation.md` — MQTT 飞控选型评估

历史文档保留在 `legacy/docs-archive` 分支。

---

## 7. 匹配策略

```
用户口语描述
    ↓
find-skill 匹配（rule + skill + agent）
    ↓
读 rule（硬约束）
    ↓
按 skill 走工作流
    ↓
委派 agent 执行
    ↓
docs skill 同步文档
```

不确定时，直接调用本 skill。改代码前看文件类型速查。改 `src/` 任何文件都先看 `onboard-constraints`。

---

**维护者**: 项目 owner
**最后更新**: 2026-06-08
**触发词**: 用什么 skill、找 skill、哪个规则、skill 列表、怎么处理、该用谁、我能做什么、不知道找谁
