# memory-system

A density-stratified memory architecture for maintaining context and continuity across AI conversations.

Inspired by the [Intelligence Manifold](https://doi.org/10.48550/arXiv.XXXX.XXXXX) — a geometric memory architecture that stratifies knowledge by informational density across layered surfaces, with autonomous agents processing each tier independently. Incorporates signal-based memory management concepts from the [Memory Genesis Competition](https://lu.ma/memorychallenge) (2025).

## Design Approach

### The problem

AI conversations lose context. Built-in memory systems scatter fragments across opaque internal stores. Long context windows degrade output quality ([context rot](https://research.trychroma.com/context-rot)). The user ends up as the bottleneck — manually curating what the AI sees.

### The solution: density-stratified layers

Instead of flat folders where everything lives at the same level, knowledge is organized into three tiers by **informational density** — how much cognitive load a piece of knowledge carries:

```
surface/     Light. Always loaded.     Current state, threads, signal-annotated index.
detail/      Medium. Loaded on demand. People profiles, patterns, skills, active drafts.
archive/     Dense. Loaded rarely.     Raw transcripts, source documents, old sessions.
```

The AI reads **surface first** (cheap — a few hundred tokens), then **descends only when needed**. This mirrors the Intelligence Manifold's core principle: a task agent should start at the outermost, lightest stratum and descend through provenance links only when a specific question requires denser material.

### Key design principles

**Minimum viable context.** Every conversation starts by reading only `surface/`. The surface layer contains enough to orient — current state, active threads, pointers to deeper material. The AI decides whether to descend based on what the conversation requires, not by preloading everything.

**Signal-annotated index.** The surface index tracks access frequency and signal strength for every file in the system. Files that are accessed often and tied to active threads have high signal; files that haven't been touched in months have fading signal. This lets the AI make informed decisions about what to load — not all files are equally important, and the index reflects that. Inspired by EverMind's signal strength model (frequency, recency, importance).

**Active threads.** Topics that span multiple sessions are tracked explicitly in `surface/threads.md` with status, session pointers, and one-line summaries. This solves the "what's still open?" problem — the AI can see continuity across conversations without reading every session file.

**Skills: distilled operational knowledge.** When the same approach works repeatedly across sessions, or the user explicitly validates/rejects an approach, it gets distilled into a skill file in `detail/skills/`. Skills compound over time — each new conversation starts with validated knowledge instead of rediscovering what works. Inspired by EverMind's cases-to-skills pattern, where completed tasks become indexed cases and successful patterns become reusable skills.

**Degradation cascade, not deletion.** Content doesn't get deleted — it degrades through tiers over time. A surface-tier item that goes stale gets compressed and moved to detail. Detail content that goes cold gets compressed and moved to archive. Archive content that's truly dead exits to git history. "A system that never forgets is a hoarder's apartment" — but a system that hard-deletes loses provenance. Compression on cascade preserves the essence while freeing the tier.

**Per-folder indexes as stratum surfaces.** Each subfolder in `detail/` and `archive/` has an `_index.md` — a one-line-per-entry summary. The AI reads the index to decide whether descending into that folder is worth the context cost.

**Provenance links.** Every derived insight links back to its source (session number, document, quote). These are the manifold's "strands" — elastic connections between knowledge at different density tiers that let you trace any claim back to raw evidence.

**Subagent delegation for deep retrieval.** When the AI needs to search or extract from the archive tier, it spawns subagents rather than loading dense material into the main conversation context. Each subagent operates with bounded context on a specific file or topic — mirroring the manifold's "fauxna" that inhabit specific strata and process knowledge locally. Keyword search (grep) for factual queries, semantic reading for thematic queries, parallel execution when topics are independent.

### How it maps to the Intelligence Manifold

| Manifold concept | Implementation here |
|-----------------|-------------------|
| Outer strata (lightweight) | `surface/` — state, threads, signal-annotated index |
| Middle strata (semantic) | `detail/` — people, patterns, skills, drafts |
| Inner strata (dense) | `archive/` — sessions, context, raw material |
| Strands (provenance) | `[source: ...]` links in every derived file |
| Fauxna (local agents) | Subagents spawned per-file for archive retrieval |
| Surface tension (density gating) | `_index.md` files + signal annotations in index |
| Signal propagation | Updates cascade: archive → detail index → surface state |
| Selective forgetting | Signal tracking + degradation cascade through tiers |
| Self-evolving agents | Skills distilled from repeated successful patterns |

## Quick start

```bash
git clone https://github.com/sunapi386/memory-system.git ~/memory
cd ~/memory
```

Edit `surface/state.md` and `detail/people/self.md`, then start a Claude Code session in that directory.

See [SETUP.md](SETUP.md) for integration options and [MIGRATION_PROMPT.md](MIGRATION_PROMPT.md) to consolidate existing AI memories.

## Structure

```
├── CLAUDE.md                    # Instructions for Claude (auto-loaded)
├── surface/                     # Always loaded — lightweight pointers
│   ├── state.md                 # Current state, identity, active focus
│   ├── threads.md               # Active threads spanning sessions
│   ├── index.md                 # Signal-annotated map of all files
│   └── session-log.md           # One-line-per-session chronological list
├── detail/                      # Loaded on demand — profiles, analysis
│   ├── people/
│   │   ├── _index.md            # Summary of all people profiles
│   │   └── self.md              # Self-profile template
│   ├── patterns/
│   │   └── _index.md            # Summary of identified patterns
│   ├── skills/
│   │   └── _index.md            # Distilled operational knowledge
│   └── drafts/
│       └── _index.md            # Active work-in-progress
├── archive/                     # Loaded rarely — dense source material
│   ├── sessions/
│   │   └── _index.md            # Full session notes index
│   ├── context/
│   │   └── _index.md            # Source material summaries
│   └── raw/                     # Unprocessed transcripts, emails, docs
├── SETUP.md                     # Setup and integration guide
└── MIGRATION_PROMPT.md          # Prompt to consolidate scattered memories
```

## Why not just use built-in AI memory?

- **You own the files** — plain markdown, no vendor lock-in
- **Density-aware** — not everything is treated as equally important
- **Signal-tracked** — access frequency and relevance tracked per file
- **Self-compounding** — validated approaches distill into reusable skills
- **Self-cleaning** — stale content degrades through tiers, not deleted but compressed
- **Browsable and editable** outside of AI conversations
- **Works with any AI tool**, not just Claude
- **Version controlled** with git
- **Private by default** — stays local unless you choose otherwise
- **Subagent-friendly** — structured for parallel retrieval without blowing up context
