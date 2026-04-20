---
title: Wiki 页面写作指南
created: 2026-04-18
updated: 2026-04-20
type: tool
status: active
tags:
  - 知识管理
  - 页面质量
  - 写作规范
  - 模板规范
summary: Wiki 页面的排版规范与模板指南。统一全库风格，确保结构清晰、可维护。
---

# 📘 Wiki 页面写作指南

> [!abstract] 定位
> 本文件是 Nebula Wiki 的排版规范。所有 Wiki 页面应遵循本规范，确保全库风格统一。

---

## 一、 文档结构模板

所有 Wiki 页面应遵循以下结构：

```markdown
# [emoji] 页面标题

> [!abstract] 摘要
> 一句话说明这是什么、为什么重要。

## 一、 [章节标题]

内容...

---

## 二、 [章节标题]

内容...

---

## 三、 [章节标题]

内容...

---

> [!note]- 更新记录
> - YYYY-MM-DD：创建
> - YYYY-MM-DD：[更新内容]
```

---

## 二、 Frontmatter 规范

```yaml
---
title: 标题
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: entity | concept | comparison | synthesis | tool | note
status: active | dormant | archived
tags: [tag1, tag2, tag3]
aliases: [别名]
summary: 一句话描述
---
```

---

## 三、 标题层级

| 层级 | 格式 | 示例 |
|:---:|:---|:---|
| # | emoji + 标题 | `# 🇮🇩 印尼渠道结构描述` |
| ## | 中文编号 + 标题 | `## 一、 代理商分工现状` |
| ### | 数字编号 + 标题 | `### 1. CE 渠道覆盖率` |

> [!warning]
> 不要用 **粗体文本** 当伪标题。这在 Obsidian 大纲视图中不可见。

---

## 四、 Callout 强制规范

页面中**必须使用 callout** 承载关键信息：

| Callout | 语义 | 使用场景 |
|:---|:---|:---|
| `> [!abstract]` | 摘要/结论 | 页首、章节开头 |
| `> [!tip]` | 建议/机会 | 战略建议、最佳实践 |
| `> [!warning]` | 风险/问题 | 风险警示、竞品威胁 |
| `> [!caution]` | 待确认 | 待验证数字、边界条件 |
| `> [!important]` | 关键决策 | 重大决策推导逻辑 |
| `> [!info]` | 背景参考 | 政策说明、历史背景 |
| `> [!note]` | 补充说明 | 案例、额外注释 |
| `> [!question]` | 开放问题 | 待决事项 |

---

## 五、 表格规范

以下场景**必须用表格**：

- 代理商分工 / 覆盖范围
- 渠道对比 / 模式对比
- 能力模型 / 评估维度
- 进度追踪 / 看板
- 数据对比 / 历史变化

**表格格式：**
```markdown
| 列1 | 列2 | 列3 |
|:---|:---|:---|
| 左对齐 | 左对齐 | 默认 |
```

---

## 六、 分隔线

大章节之间**必须用 `---` 分隔**，使页面结构清晰。

---

## 七、 列表规范

- 有序列表：步骤/流程（有先后顺序）
- 无序列表：枚举（无先后顺序）
- **最多 2 层嵌套**，禁止 3 层以上

---

## 八、 链接格式

| 场景 | 格式 | 示例 |
|:---|:---|:---|
| 内部引用 | [[wikilink]] | [[Wiki/concepts/concept-XXX]] |
| 图片嵌入 | ![[image.png]] | ![[diagram.drawio]] |
| 外部链接 | [文字](URL) | [参考](https://...) |

---

## 九、 更新标记

时效性内容在章节标题末尾标注：

```markdown
## 四、 认证进展 (2026.03.20 更新)
```

---

## 十、 页面模板

### Entity 模板

```markdown
# 🏢 [实体名称]

> [!abstract] 简介
> 一句话描述实体。

## 一、 基本信息

| 项目 | 内容 |
|:---|:---|
| 成立时间 | YYYY |
| 总部 | 地点 |
| 规模 | 人数/门店数 |

---

## 二、 关键事实

---

## 三、 时间线

| 时间 | 事件 |
|:---|:---|
| YYYY-MM | 事件描述 |

---

## 四、 相关链接

- [[Wiki/entities/entity-XXX]]
- [[Wiki/concepts/concept-XXX]]

---

> [!note]- 更新记录
> - YYYY-MM-DD：创建
```

### Concept 模板

```markdown
# 💡 [概念名称]

> [!abstract] 定义
> 一句话定义这个概念。

## 一、 核心命题

> [!important] 核心观点
> 最重要的判断。

---

## 二、 展开说明

---

## 三、 应用场景

---

## 四、 相关概念

- [[Wiki/concepts/concept-XXX]]

---

> [!note]- 更新记录
> - YYYY-MM-DD：创建
```

### Synthesis 模板

```markdown
# 📊 [分析主题]

> [!abstract] 摘要
> 分析结论一句话总结。

## 一、 背景

---

## 二、 关键变量

---

## 三、 分析框架

| 维度 | 发现 |
|:---|:---|
| 变量1 | 分析 |

---

## 四、 核心判断

> [!tip] 建议
> 行动建议。

---

## 五、 风险与不确定性

---

> [!note]- 更新记录
> - YYYY-MM-DD：创建
```

---

## Related / 相关

- [WikiNote SKILL.md](../SKILL.md) — Main schema
- [tool-wiki-workflow.md](tool-wiki-workflow.md) — Maintenance workflow
- Index page in Wiki/index/ — Vault navigation
