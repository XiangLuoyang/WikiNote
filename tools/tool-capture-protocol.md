---
title: 实时捕获协议
created: 2026-04-21
updated: 2026-04-21
type: tool
status: active
tags:
  - 知识管理
  - 实时捕获
  - 洞察提取
  - 会话沉淀
summary: WikiNote 实时捕获协议的完整参考。信号检测、队列积累、3 种模式、写入模板和边缘情况。
---

# 实时捕获协议

> [!abstract]
> 本文件是 WikiNote 实时捕获协议的完整参考。紧凑版本见 [SKILL.md](../SKILL.md) 的「实时捕获协议」章节。

---

## 一、信号检测完整参考

### 强信号（始终排队，除非 suppress 模式）

#### 1. 新框架或心智模型

**检测：** 回复定义了一个可命名的、>2 个组件的结构，用于组织思维。

**示例模式：**
- "四步分析法：锚点、偏差、催化剂、赔率"
- "三层架构：…"
- "这个模型的核心是…"
- 任何有明确"步骤""层次""支柱""维度"计数的结构

**映射：**
- 跨领域通用 → `Wiki/concepts/concept-{Name}.md`
- 单一领域专用 → `Wiki/tools/tool-{Name}.md`

#### 2. 结构化比较

**检测：** 回复包含涉及 >2 个维度的显式对比。

**示例模式：**
- 对比表格或对比矩阵
- "A 这样做，但 B 那样做"
- "方案 A vs B vs C" 逐一列出优缺点

**映射：** → `Wiki/comparisons/compare-{A}-vs-{B}.md`

#### 3. 多源综合

**检测：** 回复综合了 >2 个不同来源或视角的信息，并得出结论。

**示例模式：**
- "综合 A、B 和 C 的信息…"
- "从多个角度看…"
- 引用 3+ 外部来源的综合分析

**映射：** → `Wiki/synthesis/synthesis-{Topic}.md`

#### 4. 排错发现

**检测：** 在调试过程中，找到了根因并得出可复用的教训。

**示例模式：**
- "根因是 X，这在 Y 条件下会发生"
- "解决方案是 Z，因为…"
- "这里的教训是…"

**映射：**
- 通用教训 → `Wiki/concepts/concept-{Lesson}.md`
- 特定工具/技术 → `Wiki/tools/tool-{Tool}.md`（或更新已有页面）

#### 5. 架构或设计决策

**检测：** 在规划或设计过程中，做出了有记录理由的决策。

**示例模式：**
- "我们选方案 B，因为…"
- "权衡是 X vs Y，我们优先 X"
- "决定：使用 Z 而不是 W"

**映射：**
- 模式级决策 → `Wiki/concepts/concept-{Pattern}.md`
- 配置/流程决策 → `Wiki/tools/tool-{Process}.md`

#### 6. 反直觉发现

**检测：** 回复得出了挑战现有理解的结论。

**示例模式：**
- "出乎意料的是…"
- "直觉上应该 X，但实际是 Y"
- "这与普遍认知相反…"

**映射：** → `Wiki/concepts/concept-{Finding}.md`

### 中等信号（propose 模式下排队，auto 模式下静默积累）

#### 7. 实体概述

**检测：** 回复汇编了关于某个实体（人物、公司、产品）的 >4 个独立事实。

**映射：** → `Wiki/entities/entity-{Name}.md`

#### 8. 可复用流程

**检测：** 回复包含逐步流程，可被重复用于类似任务。

**映射：** → `Wiki/tools/tool-{ProcessName}.md`

#### 9. 重要开放问题

**检测：** 对话中识别出具有长期价值但当前无结论的问题。

**映射：** → 添加到已有的相关页面

### 不捕获

- 对已有文档的直接读取/复述
- 用户可从聊天记录恢复的琐碎事实
- 无结论的利弊思考
- 单一来源信息、无综合
- 闲聊、确认消息
- 已被近期 capture 覆盖的重复信号

---

## 二、队列积累与呈现

### 队列机制

AI 在内存中维护一个有序列表。每个候选项包含：

```
{signal_type, content_summary, suggested_target, suggested_action: create|update}
```

### 呈现触发器

| 触发器 | 条件 | 动作 |
|--------|------|------|
| **阈值** | 队列达 3 项 | 立即呈现 |
| **主题切换** | 用户换了话题（新问题、不相关任务） | 呈现队列（如有项） |
| **阶段转换** | spec-first 从一个阶段进入下一个 | 队列 ≥2 时呈现 |
| **会话结束** | 用户说"bye""done"或会话明显结束 | 呈现剩余队列 |
| **手动** | 用户输入 `/capture` | 立即呈现队列 |

### 呈现格式

```markdown
📋 **Capture Queue** (3 items)

1. **[concept]** "Harness Engineering 三代进化" → 创建 `Wiki/concepts/concept-Harness-Engineering`
   > 核心洞察：在当前节点，优化模型外面的壳，回报率可能比等下一代模型更高。

2. **[update]** "本地 LLM 批量超时问题" → 更新 `Wiki/concepts/concept-AI-API汇总`
   > 添加：Ollama qwen2.5:3b batch size >10 导致超时，解决方案：num_predict 限制 + 正则解析。

3. **[tool]** "邮件分类分阶段方案" → 创建 `Wiki/tools/tool-email-classification-pipeline`
   > 三阶段：规则匹配 → 未匹配 → LLM 分类。

回复：`all` 全部接受，`1 3` 接受指定项，`edit 2` 修改，`drop` 清空队列，或给出具体反馈。
```

---

## 三、3 种捕获模式

| 模式 | 行为 | 切换方式 |
|------|------|----------|
| **`propose`**（默认） | 队列达阈值或命中检查点时呈现给用户 | 默认；`/capture propose` |
| **`auto`** | 静默捕获并立即写入，每项写入后显示通知行 | `/capture auto` |
| **`suppress`** | 不检测、不排队 | `/capture suppress`；或说"不写入" |

模式不跨会话持久化。每个新会话从 `propose` 开始。

---

## 四、写入操作（7 步）

当项目被批准（或在 `auto` 模式下命中）时，AI 执行：

1. **识别目标** — 检查 Wiki 中是否有匹配页面（读索引，按别名和标签搜索）。匹配则更新，否则创建。
2. **写入内容** — 遵循 WikiNote 页面类型模板（见 `tool-page-guide.md`）。更新：追加带时间戳的段落，不覆盖。创建：使用对应模板。
3. **更新 Frontmatter** — 已有页面：更新 `updated` 为今日。新页面：填所有必填字段。
4. **更新交叉引用** — 添加 `[[wikilinks]]` 到 ≥2 个相关页面。新页面至少链接到 1 个已有页面。
5. **更新索引** — 如果创建了新页面，向对应索引 section 添加条目。
6. **写入日志** — 向 log.md 添加条目（类型：`capture`），使用标准格式。
7. **通知用户** — 打印：`✓ Captured to [[target]] (+log +index)`

---

## 五、按页面类型的写入模板

### 更新已有页面

```markdown
## {New Section Title} (YYYY-MM-DD 添加)

{Content}

---

> [!note]- 更新记录
> - YYYY-MM-DD：创建
> - YYYY-MM-DD：添加 {简要描述}
```

Frontmatter：`updated` 改为今日。

### 创建新 concept 页面

必填字段：`title`, `created`, `updated`, `type: concept`, `status: active`, `summary`（一句话），至少 2 个 `tags`，底部至少链接 1 个已有 Wiki 页面。

### 创建新 comparison 页面

必须有明确的对比维度表。

### 创建新 entity 页面

事实密度高，时间线明确。

---

## 六、边缘情况

### 队列项重叠

如果两个队列项指向相同或非常相似的洞察：
- 合并为一项
- 呈现时显示合并后的项

### 目标页面已存在且内容相似

- 新内容与已有内容重复 → 跳过（从队列移除，不通知）
- 新内容有增量 → 更新已有页面，追加新段落
- 有歧义 → 呈现时标记给用户判断

### 跨会话连续性

捕获队列不跨会话持久化。如果某个洞察值得在未来会话中捕获，它应该已被写入 Wiki，而非留在队列中。

### 与 spec-first 交互

当 spec-first 处于活动状态时：
- Stage 1-6 中的捕获遵循同样规则（队列、阈值、呈现）
- Stage 7（Compound）委托给本协议——本质上是强制队列 flush
- 阶段转换时，如果队列有 ≥2 项，呈现

---

## 七、/capture 斜杠命令

| 命令 | 行为 |
|------|------|
| `/capture` | 立即呈现当前队列 |
| `/capture [描述]` | 手动添加一个捕获项到队列 |
| `/capture auto` | 切换到 auto 模式 |
| `/capture propose` | 切换到 propose 模式 |
| `/capture suppress` | 切换到 suppress 模式 |

---

## 八、capture 日志格式

```markdown
## [YYYY-MM-DD] capture | {简要描述}
- **操作**: {创建新页面 | 更新已有页面} — [[target-page]]
- **信号**: {信号类型}
- **影响**: [[target-page]], [[related-page-1]], [[index-page]]
- **原因**: {为什么值得捕获}
```

---

## Related / 相关

- [WikiNote SKILL.md](../SKILL.md) — 包含紧凑的捕获协议章节
- [tool-page-guide.md](tool-page-guide.md) — 页面模板和写作规范
- [tool-wiki-workflow.md](tool-wiki-workflow.md) — 维护流程
- [tool-log-format.md](tool-log-format.md) — 日志格式规范
