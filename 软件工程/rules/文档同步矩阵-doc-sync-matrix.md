---
tags: [rules, docs, 同步, matrix]
---

# 文档同步矩阵

## 这个文件干什么

代码变了以后，告诉 AI 和人要同步哪些文档。避免功能已经改完，但 API、Issue、TDD 或设计文档还是旧的。

## 同步矩阵

| 改动类型 | 必须同步 | 验证方式 |
|----------|----------|----------|
| 新增/修改 API 路由 | `docs/reference/API.md`、接口契约文档 | 对照路由参数、状态码、响应字段 |
| 改请求参数、响应字段、状态码 | `docs/reference/API.md`、前端实现指南 | 前后端字段名一致，错误码一致 |
| 完成 issue 修复 | `docs/issue/README.md`、`docs/tdd/issue-NN/README.md` | TDD 文档有 Red/Green/回归验证 |
| 改数据管线 | 数据管线设计文档、相关 issue/TDD | search -> download -> storage -> inventory 链路一致 |
| 改测试策略或门禁 | `测试策略-test-strategy.md`、团队规范 | 命令可直接执行 |
| 改架构选型 | ADR 或 RFC | 写清背景、备选、决策、后果 |
| 改团队流程/prompt | `能力入口索引-skill-entry-index.md`、相关 prompt | skill/trigger 入口真实存在或有 fallback |

## 收尾检查

每次交付前至少回答三句话：

1. 这次是否改变 API、状态码或响应字段？
2. 这次是否改变 issue 状态或 TDD 证据？
3. 这次是否改变数据流、存储或部署方式？

如果答案是“是”，必须同步矩阵里的文档。如果答案是“否”，交付说明里写一句无需同步的理由。

## met-terra 常用门禁

```bash
bash scripts/guardrails/check_tdd_template.sh
bash scripts/guardrails/check_dual_rules_sync.sh
```
