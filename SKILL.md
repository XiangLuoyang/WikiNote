---
name: wikinote
description: |
  WikiNote — LLM-native knowledge management skill based on the Karpathy LLM Wiki pattern.
  This skill teaches an agent to maintain a personal wiki using a three-layer architecture:
  1. **Raw-Sources/** — immutable original materials (articles, PDFs, web clips)
  2. **Wiki/** — LLM-curated notes organized by type (entities, concepts, comparisons, synthesis)
  3. **SCHEMA** — self-reinforcing conventions that guide all maintenance actions
  When George asks about: managing notes, organizing knowledge, curating information,
  searching wiki content, following up on previous discussions, or any knowledge workflow,
  this skill should be loaded.
  Core principle: Every valuable answer should be saved back to the wiki, not lost in chat.
homepage: https://github.com/XiangLuoyang/WikiNote
metadata:
  openclaw:
    emoji: '📚'
    requires: {}
---

# Nebula Wiki Schema

**版本：** 2.1
**创建时间：** 2026-04-06
**更新时间：** 2026-04-20
**基于：** [Karpathy LLM Wiki Pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)

> [!abstract]
> SCHEMA 是 Nebula Wiki 的核心规范文件，统一知识库的 **架构、命名、链接与质量标准**。
> 详细参考：
> - 页面写作指南（审美、骨架、反模式）→ tools/tool-page-guide.md
> - 维护流程（索引、操作、节奏）→ tools/tool-wiki-workflow.md

---

## 核心理念

> 知识是**持续累积的复合资产**，不是每次从零开始检索。
> 人类的职责是 curation 和提问，LLM 的职责是维护、整理、链接与结构化表达。

Nebula Wiki 的目标，不是把资料堆进去，而是把资料转化成：

- 可引用的概念
- 可回溯的事实
- 可连接的实体
- 可复用的洞察
- 可长期演化的知识资产

**核心原则：** 本规范是给 AI 看的，AI 应主动执行沉淀、维护和检查。

**三条原则：**
1. Every valuable answer gets saved to the wiki
2. The wiki is always ready for a new reader (not just the current conversation)
3. Cross-references are maintained automatically

---

## 三层架构

```text
Obsidian-Nebula/
│
├── Raw-Sources/          ← 【第一层】原始资料，只读不修改
│   ├── 投资理财/
│   ├── 全球供应链/
│   └── 奇门遁甲/
│
├── Wiki/                 ← 【第二层】LLM 维护的整理层
│   ├── entities/         ← 实体页（人物、公司、产品、事件）
│   ├── concepts/         ← 概念页（方法论、框架、理论）
│   ├── comparisons/      ← 对比页（竞品对比、方案对比）
│   ├── synthesis/        ← 综合分析页（专题研究、年度总结）
│   ├── tools/            ← 工具页（提示词模板、工具配置）
│   ├── index/            ← Wiki 内部索引
│   └── archive/          ← 归档区（沉寂/归档的页面）
│
├── 00-索引/              ← Vault 级目录入口
├── Projects/             ← 项目资料与过程文档
├── Daily/                ← 日记与临时记录
└── Archive/              ← 长期归档区
```

- **第一层（Raw-Sources）**：只读，保留原貌，不做重写，不混入长期判断。
- **第二层（Wiki）**：可复用、可链接、可更新、可演化、可被持续引用。
- **第三层（入口与外围）**：区分过程与结论、临时与长期、原始输入与整理输出。
- **Wiki/archive/**：沉寂期和归档期的页面。

> [!warning]- 死文档保护
> Raw-Sources 层的文件受 **死文档保护机制** 约束，详见下方 [死文档保护机制](#死文档保护机制)。

---

## 页面类型

| 类型 | 用途 | 特点 |
|------|------|------|
| **Entity** | 描述稳定实体（人物、公司、产品、事件） | 事实密度高，时间线明确 |
| **Concept** | 描述稳定概念（方法论、框架、理论） | 主命题明确，适合长期复用 |
| **Comparison** | 横向比较对象（产品、方案、路线） | 维度清晰，结论明确 |
| **Synthesis** | 综合分析主题（专题、年度总结、研判） | 跨多来源整合，输出可引用结论 |
| **Tool** | 工具与配置（Prompt、插件、流程） | 操作性强，便于更新维护 |

页面骨架建议与审美指南见 tools/tool-page-guide.md。

---

## 文件命名规范

### 通用规则

- 使用 `-` 分隔词，不用空格
- 英文优先，兼顾 Git 兼容性
- 避免特殊字符
- 名称尽量短、稳定、可长期使用
- 文件名描述"对象"，不要描述"这次我写了什么"

### 类型命名规范

| 类型 | 格式 | 示例 |
|------|------|------|
| Entity | `entity-{name}.md` | `entity-Polymarket.md` |
| Concept | `concept-{name}.md` | `concept-Prediction-Market.md` |
| Comparison | `compare-{A}-vs-{B}.md` | `compare-TikTok-vs-Instagram.md` |
| Synthesis | `synthesis-{topic}.md` | `synthesis-US-Iran-War.md` |
| Tool | `tool-{name}.md` | `tool-wiki-workflow.md` |
| Daily | `YYYY-MM-DD.md` | `2026-04-06.md` |

### 命名建议

优先命名为：

- 稳定概念名
- 稳定实体名
- 稳定主题名

避免命名为：

- 临时会议标题
- 带强过程感的标题
- 带日期的概念标题
- 含"最终版""整理版""新版"之类后缀的标题

---

## Frontmatter 规范

所有 Wiki 页面应包含基础 Frontmatter。

```yaml
---
title: 标题
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: entity | concept | comparison | synthesis | tool | project | note
status: active | dormant | archived  # 生命周期状态（Wiki页面强制）
immutable: true | false  # 死文档标记（Raw-Sources默认true）
tags: [tag1, tag2]
aliases: [别名1, 别名2]
sources: [url1, url2]  # 仅在需要时填写
summary: 一句话描述
---
```

**字段说明：**

| 字段 | 必填 | 说明 |
|------|------|------|
| title | ✅ | 页面显示标题 |
| created | ✅ | 首次创建日期 |
| updated | ✅ | 最近重要修改日期 |
| type | ✅ | 页面类型 |
| status | ✅ | 生命周期状态 |
| immutable | ⚠️ | 死文档标记（Raw-Sources强制true，禁止修改） |
| tags | 推荐 | 主题标签 |
| aliases | 可选 | 别名，便于搜索 |
| sources | 可选 | 信息来源 |
| summary | 推荐 | 索引引用的一句话概述 |

---

## 知识生命周期

Nebula 采用四阶段生命周期：

```text
┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐
│  创作期  │ → │  活跃期  │ → │  沉寂期  │ → │  归档期  │
└─────────┘    └─────────┘    └─────────┘    └─────────┘
     ↓              ↓              ↓              ↓
  新建笔记      被频繁引用      30天无更新      移至archive
```

### 状态判断

| 状态 | 判断标准 |
|------|----------|
| **active** | 30天内有更新 或 有 inbound links |
| **dormant** | 30天以上无更新 且 inbound links < 3 |
| **archived** | 明确不再维护，移至 Wiki/archive/ |

### 状态转换

- 新建页面 → `active`
- 30天无更新 → 标记 `dormant`
- 确认不维护 → 移至 `Wiki/archive/`，标记 `archived`

---

## 死文档保护机制

> **Raw-Sources 层的文件受死文档保护，严禁修改内容。** 修改死文档等于篡改历史。

### 死文档定义

**死文档 = 历史快照，一旦创建不可修改**

符合以下任一条件即为死文档：

| 判断条件 | 示例 | 说明 |
|----------|------|------|
| **带时间戳的记录** | `2026-Q1-渠道复盘.md` | 季度/月度数据快照 |
| **会议纪要/出差报告** | `2026-04-15-代理商会议.md` | 特定日期的活动记录 |
| **公司下发的政策文件** | `26-S1-考核政策.pdf` | 官方文件，了解管理要求 |
| **一次性调研报告** | `东南亚市场调研-202603.md` | 完成后不再更新的研究 |
| **数据快照** | `团单数据-2026-Q1.xlsx` | 时间锁定的数据分析 |
| **合同/协议** | `代理商合同-XX公司.pdf` | 法律文件，不可篡改 |

### 判断标准

**自动判断规则（优先级从高到低）**：

1. **路径判断**：位于 `Raw-Sources/` 目录 → 默认为死文档
2. **文件名判断**：包含以下模式 → 死文档
   - 时间戳：`YYYY-MM-DD`、`YYYY-QX`、`26.S1`
   - 关键词：`会议`、`纪要`、`出差`、`调研`、`团单`、`政策`、`合同`
3. **Frontmatter 判断**：`immutable: true` → 死文档
4. **人工判断**：以上都不满足时，询问用户

### 保护机制

#### AI 执行规则

```yaml
禁止操作：
  - ❌ 修改死文档内容
  - ❌ 重写死文档
  - ❌ 删除死文档
  - ❌ 移动死文档到 Wiki/

允许操作：
  - ✅ 读取死文档
  - ✅ 引用死文档内容到 Wiki/
  - ✅ 提取洞察到 Wiki/（引用式）
  - ✅ 为死文档添加索引/元数据
```

#### 引用式提取

当死文档中包含有价值的分析或方法论时：

```
✅ 正确做法：
  在 Wiki/ 中创建新页面，引用死文档：
  "根据 [[Raw-Sources/XX会议纪要]] 的讨论..."

❌ 错误做法：
  直接修改死文档，添加"新发现"或"更正"
```

#### Frontmatter 标记

死文档应包含以下 Frontmatter：

```yaml
---
immutable: true  # 强制标记，AI 读取后禁止修改
original_date: 2026-04-15  # 原始日期
document_type: meeting | report | policy | snapshot | contract
sources: [url1, url2]  # 原始来源
---
```

### 与 Wiki 的关系

```
Raw-Sources/（死文档）          Wiki/（活文档）
┌─────────────────┐            ┌─────────────────┐
│ 2026-Q1-渠道复盘 │            │ concept-渠道管理 │
│ - 数据快照      │  ──────→   │ - 提炼方法论    │
│ - 会议记录      │  引用式提取 │ - 可持续更新    │
│ - 不可修改      │            │ - 可链接复用    │
└─────────────────┘            └─────────────────┘
```

**原则**：
- 死文档提供事实依据
- Wiki/ 提供可演化的知识
- 两者通过 `[[wikilink]]` 连接
- 提取时不修改原文

---

## 会话沉淀流程

**核心原则：AI 自动执行，用户可通过指令干预。**

### Log 记录机制

> **每次会话结束后，AI 应自动记录到日志。**

**记录时机**：
- ✅ 会话中产出了实质性操作（ingest/query/lint/restructure）
- ✅ 会话中修改了 SCHEMA 或重要工具
- ✅ 会话时间超过 10 分钟且有产出
- ❌ 纯聊天/确认类会话不记录

**Log 格式**：

```markdown
## [YYYY-MM-DD] 操作类型 | 简述
- **操作**: 做了什么
- **影响**: 涉及文件（使用 [[wikilink]]）
- **原因**: 为什么做（可选）
```

**操作类型词表**：
- `create` 新建文件/页面
- `update` 修改文件/页面
- `ingest` 导入新知识
- `archive` 归档
- `restructure` 结构调整
- `schema` 规范变更
- `lint` 健康检查

**不记录**：
- ❌ 纯格式修复（typo、缩进）
- ❌ 只读操作
- ❌ 未落库的对话分析

---

### 会话沉淀流程

**核心原则：AI 自动执行，用户可通过指令干预。**

### 触发条件

满足任一即自动沉淀：

- 产出了包含结论/建议/策略的综合分析
- 产出了可复用的框架、模型、对比表
- 涉及多个数据点的交叉分析

### 操作流程

```
1. 识别 → 这段分析属于哪个现有笔记？
2. 写入 → 直接更新目标笔记，加 (YYYY-MM-DD 更新) 时间戳
3. 通知 → 告知用户「已更新到 [[XXX]]」
4. 检查 → 更新相关索引
```

### 用户干预

| 用户指令 | AI 行为 |
|----------|---------|
| "不写入" | 停止沉淀 |
| "只提议" | 切换为提议模式 |
| "记录" | 直接执行沉淀 |

### 不触发场景

- 简单事实查询（"XX的GDP是多少"）
- 文件操作指令（"帮我改个格式"）
- 闲聊、确认类对话

---

## Cross-Reference 规范

Wiki 页面之间默认使用双向链接。

推荐写法：

```markdown
参见：[[Wiki/concepts/concept-prediction-market]]
相关：[[Wiki/entities/entity-Polymarket]]
延伸：[[Wiki/synthesis/synthesis-US-Iran-War]]
```

链接原则：

- 优先使用 [[WikiLink]]
- 链接应帮助跳转，不应成为装饰
- 只链接真正相关的页面
- 避免一段话里密集堆砌链接
- Related 区域保持简洁

---

## Schema 的两层规则

### 硬规范

必须统一：

- 文件命名
- Frontmatter 基础字段（含 status）
- 页面类型
- Cross-reference 方式
- 索引维护
- Lint 检查流程

### 软规范

按内容类型调整：

- 页面风格
- 组件使用
- 是否需要导航
- 是否需要摘要 callout
- 是否适合表格
- 页面长度与节奏安排

---

## Workflows

### Restructure (Migrate Existing Knowledge Base)

When George asks to restructure or migrate an existing knowledge base：

```
1. ANALYZE
   ├── List all existing files and folders
   ├── Identify content types (entities, concepts, synthesis, etc.)
   ├── Note frontmatter patterns (if any)
   └── Document current structure

2. PLAN
   ├── Map old structure → new WikiNote structure
   ├── Identify files to migrate to Raw-Sources/
   ├── Identify files to migrate to Wiki/
   ├── Plan cross-reference updates
   └── Identify files to archive/delete

3. EXECUTE
   ├── Create new directory structure
   ├── Migrate files with renaming if needed
   ├── Add frontmatter to migrated files
   ├── Create cross-references
   └── Update SCHEMA.md if customized

4. REPORT
   ├── Output BEFORE structure (tree view)
   ├── Output AFTER structure (tree view)
   ├── List all changes made
   ├── Highlight key decisions
   └── Generate Use Case Guide
```

**Report Template：**

```markdown
# 📊 Restructure Report: {vault-name}

## Before → After

**Before:**
```
{your-old-structure}
```

**After:**
```
{your-new-structure}
```

## Changes Made

| Action | File/Folder | Details |
|--------|-------------|---------|
| Move | `old/path` → `new/path` | Reason |
| Rename | `old.md` → `entity-new.md` | Type: entity |
| Create | `Wiki/` | New layer |
| Delete | `old-folder/` | Obsolete |

## Use Case Guide

### For Daily Use

**Adding new information:**
1. [Step-by-step guide]
2. [Step-by-step guide]

**Finding information:**
1. [Step-by-step guide]
2. [Step-by-step guide]

**Maintaining the wiki:**
1. [Step-by-step guide]

### Quick Reference

| Intent | Action |
|--------|--------|
| New article to read | → Raw-Sources/ |
| New concept/framework | → Wiki/concepts/ |
| New brand/company/person | → Wiki/entities/ |
| Comparison analysis | → Wiki/comparisons/ |
| Deep research/analysis | → Wiki/synthesis/ |
```

### Ingest (New Information)

```
1. 判断来源类型
   ├── Raw-Source（网页、PDF、文章、访谈）→ 放入 Raw-Sources/
   └── 直接讨论或内部判断 → 进入 Wiki/ 或 Projects/

2. 如果是 Raw-Source
   ├── 保存到对应分类
   ├── 提取关键信息
   ├── 更新相关 Wiki 页面（自动沉淀）
   └── 记录到 log.md

3. 如果是直接讨论
   ├── 提炼核心观点
   ├── 判断是更新旧页还是新建页面
   ├── 如有长期价值则沉淀到 Wiki/（自动沉淀）
   └── 记录到 log.md
```

### Query (Answer Questions)

```
1. 先读 index.md 找相关页面
2. 读相关 Wiki 页面
3. 必要时回看 Raw-Sources
4. 综合信息形成回答
5. 如果回答本身有长期价值，则沉淀为新页面或更新现有页面（自动沉淀）
```

### Lint (Regular Maintenance)

Trigger: Start of each session, or when contradictions are suspected.

#### 基础检查项

- **断链检查** — 扫描所有 `[[wikilink]]`，找出指向不存在文件的链接
- **孤立页面** — 找出没有被任何其他文件链接的笔记（不含 Wiki/archive/）
- **索引完整性** — 检查各索引是否覆盖了对应文件夹下的所有活文档

#### 增强检查项

- **时效性检查** — `updated` 超过 90 天的 `active` 状态页面
- **沉寂期标记** — 30天以上无更新的活跃笔记，建议标记 `dormant`
- **归档预判** — 长期沉寂的页面，建议移至 Wiki/archive/
- **死文档完整性** — Raw-Sources 文件是否有 `immutable: true`
- **生命周期一致性** — 页面 status 是否符合实际状态
- **Frontmatter 完整性** — 所有 Wiki/ 页面是否包含必填字段
- **未归类文件** — vault 根目录非标准位置的 .md 文件

### Lint 输出格式

```markdown
## 🔍 Vault Lint 报告 (YYYY-MM-DD)

### 🛡️ 死文档检查 (N 处)
- `Raw-Sources/XX.md` 缺少 `immutable: true` 标记
- `Wiki/XX.md` 看起来是死文档，不应放在 Wiki/

### ❌ 断链 (N 处)
- [[文件A]] → `[[不存在的链接]]`

### 🏝️ 孤立页面 (N 个)
- `某文件.md` — 建议链接到 [[XX索引]]

### 📋 索引遗漏 (N 个)
- `Wiki/concepts/XX.md` 未被对应索引收录

### ⏰ 时效性 (N 处)
- [[文件B]] 最后更新为 2025-11，已超 90 天，建议评估 status

### 🔄 生命周期不一致 (N 个)
- [[文件C]] 标记为 active 但 60 天无更新且无引用

### 📝 Frontmatter 缺失 (N 个)
- `Wiki/entities/XX.md` 缺少必填字段：status

### 📂 未归类文件 (N 个)
- `根目录/某文件.md` — 建议移动到 Raw-Sources/ 或 Wiki/

### ✅ 健康项
- 死文档保护：所有 Raw-Sources 文件均正确标记
- 生命周期：98% 的页面状态符合实际
- 索引覆盖：主要索引完整
```

---

## Index Structure

The index should be a graph of connections, not a tree.

```markdown
# Nebula Index

## Entities
- [[Wiki/entities/科技/entity-ByteDance]] — 全球增长最快的互联网巨头

## Concepts
- [[Wiki/concepts/concept-奇门断局心法]] — 以权谋剧本推演视角重构奇门遁甲断局体系

## Synthesis
- [[Wiki/synthesis/synthesis-全球供应链关键节点]] — 全球供应链核心节点的地缘政治分析
```

原则：

- 索引不是复制目录
- 索引要帮助判断"我该先读什么"
- 每个条目最好有一句摘要
- 定期清理无效索引项

---

## Agent Behavior Rules

1. **Proactive saving**: When providing a valuable answer, save it to wiki without being asked
2. **Cross-linking**: Always check existing pages before creating new ones
3. **No orphan pages**: Every new page should link to at least one existing page
4. **Summarize before linking**: Don't just dump links; include context
5. **Update not overwrite**: When updating pages, preserve valuable existing content

---

## 最终标准

一个高质量的 Wiki 页面，不一定"组件齐全"，但通常会满足以下条件：

- 主命题清晰
- 信息组织自然
- 结构层次稳定
- 便于引用和链接
- 能长期阅读
- 能长期维护
- 能在未来继续生长，而不必推倒重来

Nebula Wiki 追求的不是"每页都标准化"，而是：

- 知识结构标准化
- 页面表达成熟化
- 长期维护低摩擦化

---

## Source

Inspired by [Karpathy's LLM Wiki gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

---

**Tag:** #wiki #knowledge-management #llm-wiki #wikinote
