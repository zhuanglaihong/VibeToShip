---
tags: [rules, python, 编码]
---

# Python 编码规范

## 这个文件干什么

写 Python 代码的统一风格。不同协作者写出来的代码也要像同一个团队维护的。**不是建议，是规则**。

## 规则

### 必须的

- 所有函数加 type hints：`def foo(x: str) -> int:`
- 异步优先：IO 操作用 async/await
- 日志用 `logging`，别用 `print`
- import 放文件顶部
- 环境变量用 `os.getenv("KEY", "default")`

### FastAPI 专用

- 路由层薄——只做参数解析和调用 service，不写业务逻辑
- 查询参数用 `Query()` 声明约束

### 禁止的

- 函数内 import（除循环引用）
- 裸 except 吞异常
- 硬编码密钥/密码/Token
