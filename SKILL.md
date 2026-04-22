---
name: wikinote
description: |
  LLM-native knowledge management skill based on the Karpathy LLM Wiki pattern.
  Triggers when the user asks about: managing notes, organizing knowledge, curating information,
  searching wiki content, following up on previous discussions, or any knowledge workflow.
homepage: https://github.com/XiangLuoyang/WikiNote
metadata:
  openclaw:
    emoji: '📚'
    requires: {}
---

# Wiki Schema

**版本：** 3.1
**创建时间：** 2026-04-06
**更新时间：** 2026-04-21
**基于：** [Karpathy LLM Wiki Pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)

> [!abstract]
> SCHEMA 是 Wiki 的核心规范文件，统一知识库的 **架构、命名、链接与质量标准**。
> 详细参考：
> - 页面写作指南（审美、骨架、反模式）→ tools/tool-page-guide.md
> - 维护流程（索引、操作、节奏）→ tools/tool-wiki-workflow.md

---

## 核心理念

> 知识是**持续累积的复合资产**，不是每次从零开始检索。
> 人类的职责是 curation 和提问，LLM 的职责是维护、整理、链接与结构化表达。

Wiki 的目标，不是把资料堆进去，而是把资料转化成：

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
Vault/
│
├── Sources/              ← 【第一层】所有不可修改的内容
│   ├── {topic}/          ← 外部原始材料（PDF、网页、文章）
│   ├── {topic}/          ← 外部原始材料
│   └── archive/          ← 从 Wiki/Projects 退役的内容
│
├── Wiki/                 ← 【第二层】LLM 维护的整理层
│   ├── entities/         ← 实体页（人物、公司、产品、事件）
│   ├── concepts/         ← 概念页（方法论、框架、理论）
│   ├── comparisons/      ← 对比页（竞品对比、方案对比）
│   ├── synthesis/        ← 综合分析页（专题研究、年度总结）
│   ├── tools/            ← 工具页（提示词模板、工具配置）
│   └── index/            ← Wiki 内部索引
│
├── Projects/             ← 【第三层】有始有终的项目
└── Daily/                ← 【第三层】每日记录
```

- **第一层（Sources）**：所有不可修改的内容。
- **第二层（Wiki）**：可复用知识。
- **第三层（Projects + Daily）**：有限期工作和每日记录。

> [!warning]- 死文档保护
> Sources 层的文件受 **死文档保护机制** 约束，详见下方 [死文档保护机制](#死文档保护机制)。

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
immutable: true | false  # 死文档标记（Sources默认true）
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
| immutable | ⚠️ | 死文档标记（Sources强制true，禁止修改） |
| tags | 推荐 | 主题标签 |
| aliases | 可选 | 别名，便于搜索 |
| sources | 可选 | 信息来源 |
| summary | 推荐 | 索引引用的一句话概述 |

---

## 知识生命周期

Wiki 采用四阶段生命周期：

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
| **archived** | 明确不再维护，移至 Sources/archive/ |

### 状态转换

- 新建页面 → `active`
- 30天无更新 → 标记 `dormant`
- 确认不维护 → 移至 `Sources/archive/`，标记 `archived`

---

## 死文档保护机制

> **Sources 层的文件受死文档保护，严禁修改内容。** 修改死文档等于篡改历史。

### 死文档定义

**死文档 = 历史快照，一旦创建不可修改**

符合以下任一条件即为死文档：

| 判断条件 | 示例 | 说明 |
|----------|------|------|
| **带时间戳的记录** | `2026-Q1-review.md` | 季度/月度数据快照 |
| **会议纪要/出差报告** | `2026-04-15-team-meeting.md` | 特定日期的活动记录 |
| **公司下发的政策文件** | `2026-S1-policy.pdf` | 官方文件，了解管理要求 |
| **一次性调研报告** | `market-research-202603.md` | 完成后不再更新的研究 |
| **数据快照** | `metrics-2026-Q1.xlsx` | 时间锁定的数据分析 |
| **合同/协议** | `contract-vendor.pdf` | 法律文件，不可篡改 |

### 判断标准

**自动判断规则（优先级从高到低）**：

1. **路径判断**：位于 `Sources/` 目录 → 默认为死文档
2. **文件名判断**：包含以下模式 → 死文档
   - 时间戳：`YYYY-MM-DD`、`YYYY-QX`
   - 关键词：`meeting`、`report`、`survey`、`policy`、`contract`（及其本地化表达）
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
  "根据 [[Sources/XX会议纪要]] 的讨论..."

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
Sources/（死文档）          Wiki/（活文档）
┌─────────────────┐            ┌─────────────────┐
│ 2026-Q1-review  │            │ concept-review   │
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

## ⚠️ 强制执行规范

> **以下规范必须被执行，无例外。违反这些规范的操作是错误操作。**
>
> 这些规范确保关键操作不会因 AI "忘记"而被跳过。

### 一、死文档保护执行（最高优先级）

#### Sources 层文件绝对不可修改

```
Sources/ 下的所有文件都是"死文档" = 历史快照，一旦创建不可修改
```

#### 绝对禁止的操作 ❌

- 修改 Sources/ 下任何文件的内容
- 重写 Sources/ 下任何文件
- 删除 Sources/ 下任何文件
- 移动 Sources/ 文件到 Wiki/

#### 允许的操作 ✅

- 读取 Sources/ 文件
- 引用 Sources/ 内容到 Wiki/ 页面
- 从 Sources/ 提取洞察到 Wiki/（引用式，不修改原文）
- 为 Sources/ 文件添加索引或元数据

---

### 二、操作完成自检（强制检查点）

#### 触发条件

任何涉及以下操作的任务完成后，**必须**执行自检：

- 创建/修改 Wiki/ 文件
- 创建/修改 Sources/ 文件
- 删除任何文件
- 移动任何文件
- 归档任何文件

#### 自检清单（不可跳过）

```
□ 操作类型是什么？（create/update/delete/ingest/restructure）
□ 影响了哪些文件？（列出所有文件名）
□ 是否已记录到 [[📂日志记录]]？
  ├─ 如果否 → 操作未完成，必须补录
  └─ 如果是 → 操作完成
```

#### 完成标准

```
只有完成日志记录，操作才算真正完成。

缺少日志记录 = 操作未完成 = 必须补录
```

#### 日志记录格式

```markdown
## [YYYY-MM-DD] 操作类型 | 简述
- **操作**: 做了什么
- **影响**:
  - 涉及的文件（使用 [[wikilink]]）
- **原因**: 为什么做（可选）
```

---

### 三、Frontmatter 强制字段

#### Wiki/ 页面必填字段

```yaml
---
title: 标题
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: entity | concept | comparison | synthesis | tool
status: active | dormant | archived
---
```

#### Sources/ 页面必填字段

```yaml
---
immutable: true  ← 强制，死文档标记
created: YYYY-MM-DD
document_type: meeting | report | policy | snapshot | contract
---
```

**缺少必填字段 = 文件不完整 = 必须补全**

---

### 四、违规处理

#### 如果发现违反上述规范

```
1. 立即停止操作
2. 向用户说明违规情况
3. 询问如何修复
4. 执行修复
5. 记录到日志（包括修复过程）
```

---

## 会话沉淀流程

### Log 记录机制

> **每次会话结束后，AI 应自动记录到日志。**

**核心规则：**
- 日期降序排列（新条目追加到文件顶部）
- 只有实质性操作才记录

完整格式规范和空模板见 [tools/tool-log-format.md](tools/tool-log-format.md)。

---

## 实时捕获协议

> [!important] 核心机制（v3.1）
> 取代旧版"会话沉淀"。旧版问题：触发条件模糊、无积累机制、实际几乎不触发。
> 新版关键：**结构化信号检测 + 队列积累 + 3 种模式 + 强制检查点。**
> 完整参考见 [tools/tool-capture-protocol.md](tools/tool-capture-protocol.md)。

### 设计原则

1. **检测信号，而非"判断价值"** — AI 按具体结构模式评估，不做模糊的价值判断
2. **积累，而非立即行动** — 洞察收集到队列，批量呈现，不打断对话
3. **模式驱动的用户控制** — 用户通过命令切换模式，不靠会话参数
4. **强制执行检查点** — 3 个时刻 AI 必须评估队列，即使为空

### 信号检测规则

AI 在每次回复后扫描以下模式。匹配则加入捕获队列。

**强信号（始终排队，除非 suppress 模式）：**

| 信号 | 示例 | 目标类型 |
|------|------|----------|
| 定义了可命名概念的新框架或心智模型 | "四步分析法：锚点、偏差、催化剂、赔率" | `concept` |
| 涉及 >2 维度的结构化比较 | 工具 X vs Y 对比表 | `comparison` |
| 多源综合得出结论 | "综合 A、B 和 C，方法应该是…" | `synthesis` |
| 排错发现产生可复用教训 | "根因是 Ollama batch size >10 导致超时" | `concept` 或 `tool` |
| 架构或设计决策及其理由 | "选方案 B：分阶段方案，因为…" | `tool` 或 `concept` |
| 挑战现有理解的发现 | "直觉上应该 X，但实际是 Y" | `concept` |

**中等信号（propose 模式下排队，auto 模式下静默积累）：**

| 信号 | 示例 | 目标类型 |
|------|------|----------|
| 实体概述（>4 个独立事实） | "Polymarket：2020年成立，已处理 $1B，基于 Polygon" | `entity` |
| 可复用流程或配方 | "从 URL 提取数据的步骤…" | `tool` |
| 重要的开放问题 | "尚不清楚 X 是否导致 Y" | 添加到已有页面 |

**不捕获：**
- 对已有文档的直接读取/复述
- 用户可从聊天记录恢复的琐碎事实
- 无结论的利弊思考、单源信息、闲聊

### 3 种捕获模式

| 模式 | 行为 | 切换方式 |
|------|------|----------|
| **`propose`**（默认） | 队列达阈值或命中检查点时呈现给用户 | 默认；`/capture propose` |
| **`auto`** | 静默捕获并立即写入，每项写入后显示通知行 | `/capture auto` |
| **`suppress`** | 不检测、不排队 | `/capture suppress`；或说"不写入" |

模式不跨会话持久化。每个新会话从 `propose` 开始。

### 队列积累与呈现

**队列机制：** AI 在内存中维护有序的捕获候选列表。每个候选包含 `{signal_type, content_summary, suggested_target, suggested_action}`。

**呈现触发器：**

| 触发器 | 条件 | 动作 |
|--------|------|------|
| 阈值 | 队列达 3 项 | 立即呈现 |
| 主题切换 | 用户换了话题 | 呈现队列（如有项） |
| 阶段转换 | spec-first 阶段切换 | 队列 ≥2 时呈现 |
| 会话结束 | 用户说"bye""done"或会话明显结束 | 呈现剩余队列 |
| 手动 | 用户输入 `/capture` | 立即呈现队列 |

**呈现格式：**

```markdown
📋 **Capture Queue** (N items)

1. **[concept]** "洞察标题" → 创建 `Wiki/concepts/concept-Name`
   > 一句话摘要。

2. **[update]** "更新标题" → 更新 `Wiki/concepts/concept-Existing`
   > 要添加的内容摘要。

回复：`all` 全部接受，`1 3` 接受指定项，`edit 2` 修改，`drop` 清空队列。
```

### 写入操作

当项目被批准（或在 `auto` 模式下命中）时，AI 执行 7 步：

1. **识别目标** — 检查 Wiki 中是否有匹配页面。匹配则更新，否则创建。
2. **写入内容** — 更新：追加带时间戳段落。创建：使用对应模板。
3. **更新 Frontmatter** — 更新 `updated` 为今日。新页面填所有必填字段。
4. **更新交叉引用** — 添加 `[[wikilinks]]` 到 ≥2 个相关页面。
5. **更新索引** — 新页面向对应索引 section 添加条目。
6. **写入日志** — 向 log.md 添加 `capture` 类型条目。
7. **通知用户** — 打印：`✓ Captured to [[target]] (+log +index)`

### 强制执行检查点

AI 必须在以下时刻明确评估捕获队列：

1. 提供实质性回复后（回复 >200 字或含代码/分析）
2. 读取文件后（ingest 场景）
3. 执行代码或完成任务后

每次评估时内部检查：自上次检查以来是否有信号模式匹配？有则加入队列。队列达阈值则呈现。

### /capture 命令

| 命令 | 行为 |
|------|------|
| `/capture` | 立即呈现当前队列 |
| `/capture [描述]` | 手动添加一个捕获项 |
| `/capture auto` | 切换到 auto 模式 |
| `/capture propose` | 切换到 propose 模式 |
| `/capture suppress` | 切换到 suppress 模式 |

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

When the user asks to restructure or migrate an existing knowledge base:

```
1. ANALYZE
   ├── List all existing files and folders
   ├── Identify content types (entities, concepts, synthesis, etc.)
   ├── Note frontmatter patterns (if any)
   └── Document current structure

2. PLAN
   ├── Map old structure → new WikiNote structure
   ├── Identify files to migrate to Sources/
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
| New article to read | → Sources/ |
| New concept/framework | → Wiki/concepts/ |
| New brand/company/person | → Wiki/entities/ |
| Comparison analysis | → Wiki/comparisons/ |
| Deep research/analysis | → Wiki/synthesis/ |
```

### Ingest (New Information)

```
1. 判断来源类型
   ├── Source（网页、PDF、文章、访谈）→ 放入 Sources/
   └── 直接讨论或内部判断 → 进入 Wiki/ 或 Projects/

2. 如果是 Source
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
3. 必要时回看 Sources
4. 综合信息形成回答
5. 如果回答本身有长期价值，则沉淀为新页面或更新现有页面（自动沉淀）
6. **Capture evaluation** — 评估回答是否命中捕获信号。如果命中，加入队列。
```

### Lint (Regular Maintenance)

Trigger: Start of each session, or when contradictions are suspected.

#### 基础检查项

- **断链检查** — 扫描所有 `[[wikilink]]`，找出指向不存在文件的链接
- **孤立页面** — 找出没有被任何其他文件链接的笔记（不含 Sources/archive/）
- **索引完整性** — 检查各索引是否覆盖了对应文件夹下的所有活文档

#### 增强检查项

- **时效性检查** — `updated` 超过 90 天的 `active` 状态页面
- **沉寂期标记** — 30天以上无更新的活跃笔记，建议标记 `dormant`
- **归档预判** — 长期沉寂的页面，建议移至 Sources/archive/
- **死文档完整性** — Sources 文件是否有 `immutable: true`
- **生命周期一致性** — 页面 status 是否符合实际状态
- **Frontmatter 完整性** — 所有 Wiki/ 页面是否包含必填字段
- **未归类文件** — vault 根目录非标准位置的 .md 文件

### Lint 输出格式

```markdown
## 🔍 Vault Lint 报告 (YYYY-MM-DD)

### 🛡️ 死文档检查 (N 处)
- `Sources/XX.md` 缺少 `immutable: true` 标记
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
- `根目录/某文件.md` — 建议移动到 Sources/ 或 Wiki/

### ✅ 健康项
- 死文档保护：所有 Sources 文件均正确标记
- 生命周期：98% 的页面状态符合实际
- 索引覆盖：主要索引完整
```

---

## Index Structure

The index should be a graph of connections, not a tree.

```markdown
# Wiki Index

## Entities
- [[Wiki/entities/entity-ExampleCompany]] — A short description of this entity

## Concepts
- [[Wiki/concepts/concept-ExampleConcept]] — A short description of this concept

## Synthesis
- [[Wiki/synthesis/synthesis-ExampleTopic]] — A short description of this synthesis
```

原则：

- 索引不是复制目录
- 索引要帮助判断"我该先读什么"
- 每个条目最好有一句摘要
- 定期清理无效索引项

---

## Agent Behavior Rules

1. **Signal-driven capture**: After every substantive reply, evaluate against capture signal rules. Queue matches. Present at threshold or checkpoint.
2. **Cross-linking**: Always check existing pages before creating new ones
3. **No orphan pages**: Every new page should link to at least one existing page
4. **Summarize before linking**: Don't just dump links; include context
5. **Update not overwrite**: When updating pages, preserve valuable existing content
6. **Log every capture**: Every approved capture must produce a log entry. No exceptions.

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

Wiki 追求的不是"每页都标准化"，而是：

- 知识结构标准化
- 页面表达成熟化
- 长期维护低摩擦化

---

## Source

Inspired by [Karpathy's LLM Wiki gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

---

**Tag:** #wiki #knowledge-management #llm-wiki #wikinote
