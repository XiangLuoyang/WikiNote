# WikiNote

**LLM-native knowledge management skill** based on the Karpathy LLM Wiki pattern.

## What

WikiNote is a skill that teaches an AI agent to maintain a personal wiki using a **four-directory MECE architecture**:

```
Sources/  →  Wiki/  →  Projects/  +  Daily/
(Immutable)  (Knowledge)  (Work)      (Records)
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

## Four Directories

| Directory | Purpose | LLM Action |
|-----------|---------|------------|
| **Sources/** | All immutable content (raw materials + archive) | Read only |
| **Wiki/** | Curated knowledge (entities, concepts, synthesis, tools) | Read + Write + Maintain |
| **Projects/** | Time-bounded work with clear start/end | Read + Write |
| **Daily/** | Daily records and journals | Read + Write |

## Quick Start

### For Claude Code / OpenCode Users

```bash
# Clone to skills directory
git clone https://github.com/XiangLuoyang/WikiNote.git ~/.claude/skills/WikiNote
```

## Key Features

### Real-time Capture Protocol (v3.1)

The AI continuously monitors conversations for valuable insights and automatically saves them to the wiki — no manual intervention needed.

**How it works:**
1. AI scans every reply for **9 structured signal patterns** (new frameworks, comparisons, debugging lessons, etc.)
2. Matches go into a **capture queue** (no interruption)
3. Queue is presented at **thresholds or checkpoints** for user confirmation
4. Approved items are written to wiki with frontmatter, cross-references, index entry, and log

**Commands:**

| Command | Effect |
|---------|--------|
| `/capture` | Show current capture queue |
| `/capture [description]` | Manually add an item |
| `/capture auto` | Silent capture mode (no confirmation needed) |
| `/capture propose` | Default mode (batch confirmation) |
| `/capture suppress` | Disable capture for this session |

**Signal types:**

| Signal | Target |
|--------|--------|
| New framework or mental model | `Wiki/concepts/` |
| Structured comparison (>2 dimensions) | `Wiki/comparisons/` |
| Multi-source synthesis with conclusion | `Wiki/synthesis/` |
| Debugging discovery with reusable lesson | `Wiki/concepts/` or `Wiki/tools/` |
| Architecture/design decision with rationale | `Wiki/tools/` or `Wiki/concepts/` |
| Counter-intuitive finding | `Wiki/concepts/` |

### Other Workflows

**Ingest:** New information → extract → update wiki → log

**Query:** Search wiki → read → synthesize → answer → capture if valuable

**Lint** (`/linting`): Auto-fix frontmatter, naming, lifecycle status, and dead document protection across all files.

## File Structure

```
WikiNote/
├── SKILL.md                        # Core schema and conventions
└── tools/
    ├── tool-capture-protocol.md    # Complete capture reference
    ├── tool-page-guide.md          # Page writing guide
    ├── tool-wiki-workflow.md       # Maintenance workflows
    └── tool-log-format.md          # Log format specification
```

## Related

- [Karpathy LLM Wiki Gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
- [spec-first](https://github.com/XiangLuoyang/spec-first) — Development workflow that integrates with WikiNote's capture protocol

---

**License:** MIT
**Author:** George Hsiang
**Tag:** #wiki #knowledge-management #llm-wiki
