---
tags: [spec, quality, done, 验收]
relates:
  - "[[../../prompts/rules-team]]"
  - "[[测试策略-test-strategy]]"
  - "[[../../rules/文档同步矩阵-doc-sync-matrix]]"
---

# 完成定义

## 这个文件干什么

定义一个任务什么时候才算“真的完成”。不是代码写完就完成，必须测试、文档、审查和回滚路径都闭环。

## 最小完成标准

- [ ] 需求边界清楚，知道做什么和不做什么。
- [ ] 代码改动集中，没有顺手重构无关模块。
- [ ] 行为变更有对应测试。
- [ ] 目标测试通过。
- [ ] 相关最小回归通过。
- [ ] Issue/行为变更通过 `bash scripts/guardrails/check_tdd_template.sh`。
- [ ] API、状态码、响应字段变化已同步文档。
- [ ] Issue 状态、TDD 记录、设计文档按需同步。
- [ ] 没有残留 `print`、调试日志、无编号 TODO。
- [ ] PR 或交付说明写清楚测试结果和回滚方式。

## 推荐交付说明

```markdown
## 改动摘要
- xxx

## 测试结果
/Users/gaoyu/miniconda3/envs/met-py311/bin/python -m pytest tests/unit/test_xxx.py -v  # N passed
bash scripts/guardrails/check_tdd_template.sh  # passed

## 文档同步
- 已同步：docs/reference/API.md
- 无需同步：未改变 API/数据流/issue 状态

## 回滚方式
git revert <commit>
```

## 不能算完成的情况

- 只跑了 happy path，错误路径没覆盖。
- 只说“应该可以”，没有命令结果。
- API 变了但文档没变。
- 测试红了但继续交付。
- review 有阻塞问题但没有回到 TDD 修复。
