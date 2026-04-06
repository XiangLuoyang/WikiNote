# WikiNote

**LLM-native knowledge management skill** based on the Karpathy LLM Wiki pattern.

## What

WikiNote is a skill that teaches an AI agent to maintain a personal wiki using a **three-layer architecture**:

```
Raw-Sources/  →  Wiki/  →  Schema
(Immutable)       (Curated)   (Conventions)
```

## Why

Traditional note-taking is **write-only**: you capture information but never revisit it. LLMs change this — they can read, integrate, cross-link, and maintain your knowledge base automatically.

The insight from Karpathy:
> "Good answers are also worth saving. When you answer a question, save it back to the wiki."

## Core Principles

1. **Every valuable answer → saved to wiki**
2. **Wiki is always ready** for a new reader
3. **Cross-references are maintained** automatically
4. **LLM does all maintenance** — not the human

## Three Layers

| Layer | Purpose | LLM Action |
|-------|---------|------------|
| **Raw-Sources/** | Original materials (articles, PDFs, web clips) | Read only |
| **Wiki/** | Curated knowledge (entities, concepts, synthesis) | Read + Write + Maintain |
| **Schema** | Conventions that guide all actions | Follow + Evolve |

## Quick Start

### For OpenClaw Users

```bash
npx skills add XiangLuoyang/wikinote -g -y
```

### For Claude Code / OpenCode Users

```bash
# Clone to skills directory
git clone https://github.com/XiangLuoyang/WikiNote.git ~/wikinote
```

## Workflow

### Ingest
1. Receive new information
2. Extract key points
3. Update existing wiki page OR create new one
4. Add cross-references
5. Log the ingestion

### Query
1. Search wiki for relevant pages
2. Read and synthesize
3. Provide answer
4. Save valuable answer back to wiki

### Lint
1. Check for contradictions
2. Find outdated information
3. Identify orphan pages
4. Fix broken links

## File Structure

```
Wiki/
├── entities/         # Things: people, brands, events
├── concepts/         # Ideas: methods, frameworks, theories
├── comparisons/      # A vs B analyses
└── synthesis/       # Deep dives: research, reviews

Raw-Sources/         # Original materials (read-only)
SCHEMA.md            # This skill's conventions
log.md              # Ingestion and maintenance log
```

## Related

- [Karpathy LLM Wiki Gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
- [Obsidian-Nebula Vault](https://github.com/XiangLuoyang/Obsidian-Nebula)

---

**License:** MIT  
**Author:** George Hsiang  
**Tag:** #wiki #knowledge-management #llm-wiki
