---
title: 日志格式规范
created: 2026-04-21
updated: 2026-04-21
type: tool
status: active
tags:
  - 知识管理
  - 日志规范
  - 操作记录
summary: WikiNote 日志文件的格式规范与模板。新条目追加到文件顶部（日期降序）。
---

# 日志格式规范 / Log Format

> [!abstract]
> 定义 WikiNote 日志文件的格式。日志用于记录所有对 Wiki 的实质性操作。

---

## 一、排列规则

**日期降序：新条目追加到文件顶部。**

```text
## [2026-04-21] lint | 最新条目    ← 最上面
---
## [2026-04-20] fix | 较早条目
---
## [2026-04-06] schema | 更早条目
```

> [!warning]
> 不要追加到文件底部。最新的操作应该在最上面，方便快速查看。

---

## 二、条目格式

```markdown
## [YYYY-MM-DD] 操作类型 | 简述
- **操作**: 做了什么
- **影响**: 涉及文件（使用 [[wikilink]]）
- **原因**: 为什么做（可选）
```

---

## 三、操作类型词表

| 类型 | 含义 |
|------|------|
| `create` | 新建文件/页面 |
| `update` | 修改文件/页面 |
| `ingest` | 导入新知识 |
| `archive` | 归档 |
| `restructure` | 结构调整 |
| `schema` | 规范变更 |
| `lint` | 健康检查 |
| `fix` | 修复问题 |
| `capture` | 实时捕获（洞察、框架、教训） |

---

## 四、记录时机

记录：
- ✅ 会话中产出了实质性操作（ingest/query/lint/restructure）
- ✅ 会话中修改了 SCHEMA 或重要工具
- ✅ 会话时间超过 10 分钟且有产出

不记录：
- ❌ 纯格式修复（typo、缩进）
- ❌ 只读操作
- ❌ 未落库的对话分析
- ❌ 纯聊天/确认类会话

---

## 五、空模板

```markdown
## [YYYY-MM-DD] 操作类型 | 简述
- **操作**:
- **影响**:
- **原因**:
```

---

## Related / 相关

- [WikiNote SKILL.md](../SKILL.md) — Main schema
- [tool-wiki-workflow.md](tool-wiki-workflow.md) — Maintenance workflow
