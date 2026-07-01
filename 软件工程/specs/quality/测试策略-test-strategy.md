---
tags: [spec, test, 测试, 质量]
relates: "[[../../prompts/03-TDD实现-tdd-hub]]"
---

# 测试策略

## 这个文件干什么

定测试的规则——哪些要测、怎么测、跑什么命令。

## 测试层级

| 层级 | 测什么 | 框架 | 命令 |
|------|--------|------|------|
| 单元测试 | 函数/类行为 | pytest + mock | `/Users/gaoyu/miniconda3/envs/met-py311/bin/python -m pytest tests/unit/ -v` |
| 集成测试（未来） | 接口全链路 | pytest + TestClient | `/Users/gaoyu/miniconda3/envs/met-py311/bin/python -m pytest tests/integration/ -v` |

## 每个 Commit 的测试要求

- Red 阶段：至少新增 1 个测试文件
- Green 阶段：目标单测文件全绿 + 相关回归全绿
- Refactor 阶段：行为不变，同一批测试全绿
- Issue/行为变更收尾：必须跑 `bash scripts/guardrails/check_tdd_template.sh`

## 命令约定

```bash
# 目标单测
/Users/gaoyu/miniconda3/envs/met-py311/bin/python -m pytest tests/unit/test_issue{编号}_{模块名}.py -v

# 受影响单测回归
/Users/gaoyu/miniconda3/envs/met-py311/bin/python -m pytest tests/unit/ -q

# TDD 文档门禁
bash scripts/guardrails/check_tdd_template.sh
```

## Mock 规则

- 单元测试不连真实外部服务（PostGIS、MinIO、DuckDB 可 mock）
- Mock 路径：`services.<模块>.<依赖>`，不是原始库路径

## 命名约定

```
tests/unit/test_issue{编号}_{模块名}.py
例：tests/unit/test_issue05_postgis_client.py
```
