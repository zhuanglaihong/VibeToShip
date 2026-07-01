---
tags: [trigger, setup, 新项目, 脚手架]
model: GPT5.5 (Codex)
skill: writing-plans
---

# 08 — 新项目脚手架

## 这个文件干什么

开新项目时，自动生成项目骨架：目录结构、CLAUDE.md、skills 清单、CI 配置。**不写业务代码，只搭房子框架**。

## 什么时候用

- 新建一个工程
- 从 met-terra / drone_inspect 复制基础设施经验

## 用什么模型

**GPT 5.5（Codex）**——需要理解参考工程的结构。

## 怎么用

在 Codex 或 Claude Code 里输入：

```
/writing-plans：新项目 [名称]。
技术栈：[Python/FastAPI/DuckDB/...]。
参考工程：[met-terra/drone_inspect]。
输出：项目结构、CLAUDE.md 模板、skills 清单、CI 配置。
```

## 举个例子

```
/writing-plans：新项目 crop-monitor。
技术栈：Python 3.11 + FastAPI + DuckDB + MinIO + pytest。
参考工程：met-terra。
输出：目录结构、CLAUDE.md 草稿、需要移植的 skills、.gitignore + .env.example。
```

## 会得到什么

- 目录结构图
- CLAUDE.md 模板（项目定位/常用命令/技术栈）
- 从参考工程移植的 skills 清单
- .gitignore / .env.example / requirements.txt 模板
