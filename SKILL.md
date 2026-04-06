---
name: wikinote
description: |
  WikiNote — LLM-native knowledge management skill based on the Karpathy LLM Wiki pattern.
  
  This skill teaches an agent to maintain a personal wiki using a three-layer architecture:
  1. **Raw-Sources/** — immutable original materials (articles, PDFs, web clips)
  2. **Wiki/** — LLM-curated notes organized by type (entities, concepts, comparisons, synthesis)
  3. **Schema** — self-reinforcing conventions that guide all maintenance actions
  
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

# WikiNote — LLM-native Knowledge Management

Based on the [Karpathy LLM Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

## Core Philosophy

> Knowledge is a ** compounding asset**, not a retrieval system.
> The LLM's job is to read, integrate, cross-link, and maintain — not just search and retrieve.

**Three laws:**
1. Every valuable answer gets saved to the wiki
2. The wiki is always ready for a new reader (not just the current conversation)
3. Cross-references are maintained automatically

## Three-Layer Architecture

```
Vault/
├── Raw-Sources/          ← Layer 1: Original materials (read-only)
│   ├── 投资理财/
│   ├── 全球供应链/
│   └── 奇门遁甲/
│
├── Wiki/                 ← Layer 2: LLM-curated knowledge
│   ├── entities/         ← Specific things: people, brands, events
│   ├── concepts/         ← Abstract ideas: methods, frameworks, theories
│   ├── comparisons/       ← Comparisons: A vs B analyses
│   └── synthesis/         ← Deep dives: research, annual reviews
│
└── SCHEMA.md            ← Layer 3: Conventions (this file)
```

## File Naming

| Type | Format | Example |
|------|--------|---------|
| Entity | `entity-{name}.md` | `entity-Polymarket.md` |
| Concept | `concept-{name}.md` | `concept-Prediction-Market.md` |
| Comparison | `compare-{A}-vs-{B}.md` | `compare-TikTok-vs-Instagram.md` |
| Synthesis | `synthesis-{topic}.md` | `synthesis-US-Iran-War.md` |
| Daily | `YYYY-MM-DD.md` | `2026-04-06.md` |

## Frontmatter Standard

```yaml
---
title: 标题
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: entity | concept | comparison | synthesis | note
tags: [tag1, tag2]
sources: [url1, url2]  # Only for Raw-Sources layer
summary: 一句话描述
---
```

## Workflows

### Ingest (New Information)

```
1. Identify the source
   ├── Raw material (article, PDF, web) → Raw-Sources/
   └── Discussion → Go to step 2

2. Extract key information
   ├── What is this about?
   ├── What are the key points?
   ├── What does it relate to existing knowledge?

3. Update or create Wiki/ entry
   ├── Already exists → Update with new info
   └── New topic → Create appropriate Wiki/ file

4. Update cross-references
   ├── Link from related entities/concepts
   └── Check for contradictions

5. Log the ingestion
   └── Append to log.md
```

### Query (Answer Questions)

```
1. Check index for relevant pages
2. Read relevant Wiki/ pages
3. Synthesize answer
4. If answer is valuable → Save as new Wiki/ page
5. Log the query and answer
```

### Lint (Regular Maintenance)

Trigger: Start of each session, or when contradictions are suspected.

```
Check:
├── [ ] Contradictions between pages?
├── [ ] Outdated information superseded by new data?
├── [ ] Orphan pages with no inbound links?
├── [ ] Missing cross-references?
└── [ ] Broken links?
```

## Cross-Reference Convention

In wiki pages, use Obsidian-style links:

```markdown
See also: [[Wiki/concepts/prediction-market-analysis]]
Related: [[Wiki/entities/Polymarket]]
```

## Index Structure

The index should be a graph of connections, not a tree.

```markdown
# Wiki Index

## Entities
- [[Wiki/entities/Polymarket]] — 预测市场平台
- [[Wiki/entities/George]] — User, channel partner

## Concepts  
- [[Wiki/concepts/prediction-market-analysis]] — 四步分析框架

## Synthesis
- [[Wiki/synthesis/us-iran-war]] — 美伊战争综合分析
```

## Agent Behavior Rules

1. **Proactive saving**: When providing a valuable answer, save it to wiki without being asked
2. **Cross-linking**: Always check existing pages before creating new ones
3. **No orphan pages**: Every new page should link to at least one existing page
4. **Summarize before linking**: Don't just dump links; include context
5. **Update not overwrite**: When updating pages, preserve valuable existing content

## Source

Inspired by [Karpathy's LLM Wiki gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

---

**Tag:** #wiki #knowledge-management #llm-wiki #wikinote
