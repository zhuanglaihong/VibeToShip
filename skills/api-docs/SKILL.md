---
name: api-docs
description: API 文档标准 — 端点列表、请求/响应示例、格式约定。触发词：API 文档、接口文档、REST API、WebSocket、endpoint、Swagger、FastAPI docs
---

# API Docs — API 文档标准

## 文档格式约定

每个 API 端点应包含：

- **方法 & 路径**：`GET /api/resource`
- **说明**：一句话描述用途
- **请求参数**：Query / Body / Path 参数及类型
- **响应示例**：成功 + 典型错误
- **错误码**：常见状态码及含义

## 请求/响应示例

### 标准成功响应

```json
{
  "success": true,
  "data": { ... },
  "message": "操作成功"
}
```

### 标准错误响应

```json
{
  "success": false,
  "error": {
    "code": "RESOURCE_NOT_FOUND",
    "message": "请求的资源不存在"
  }
}
```

### WebSocket 消息格式

```json
{
  "type": "event_type",
  "data": { ... },
  "timestamp": 1717425600.123
}
```

## 维护规则

- 新增端点 → 更新 API 文档
- 修改 schema → 同步更新请求/响应示例
- 状态码变更 → 更新错误码说明
- 保持文档与实际响应一致（建议用自动化测试验证）
