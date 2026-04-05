# memory-system

A density-stratified memory architecture for maintaining context and continuity across AI conversations.

Inspired by the [Intelligence Manifold](https://doi.org/10.48550/arXiv.XXXX.XXXXX) — a geometric memory architecture that stratifies knowledge by informational density across layered surfaces, with autonomous agents processing each tier independently.

## Design Approach

### The problem

AI conversations lose context. Built-in memory systems scatter fragments across opaque internal stores. Long context windows degrade output quality ([context rot](https://research.trychroma.com/context-rot)). The user ends up as the bottleneck — manually curating what the AI sees.

### The solution: density-stratified layers

Instead of flat folders where everything lives at the same level, knowledge is organized into three tiers by **informational density** — how much cognitive load a piece of knowledge carries:

```
surface/     Light. Always loaded.     Current state, pointers, session log.
detail/      Medium. Loaded on demand. People profiles, pattern analysis, active drafts.
archive/     Dense. Loaded rarely.     Raw transcripts, source documents, old sessions.
```

The AI reads **surface first** (cheap — a few hundred tokens), then **descends only when needed**. This mirrors the Intelligence Manifold's core principle: a task agent should start at the outermost, lightest stratum and descend through provenance links only when a specific question requires denser material.

### Key design principles

**Minimum viable context.** Every conversation starts by reading only `surface/`. The surface layer contains enough to orient — current state, active threads, pointers to deeper material. The AI decides whether to descend based on what the conversation requires, not by preloading everything.

**Per-folder indexes as stratum surfaces.** Each subfolder in `detail/` and `archive/` has an `_index.md` — a one-line-per-entry summary. The AI reads the index to decide whether descending into that folder is worth the context cost. This is the manifold's "surface scan" implemented as markdown.

**Provenance links.** Every derived insight links back to its source (session number, document, quote). These are the manifold's "strands" — elastic connections between knowledge at different density tiers that let you trace any claim back to raw evidence.

**Staleness and selective forgetting.** Every file carries a `last_updated` date. The AI flags content older than 30 days and suggests archiving or removing detail-tier content that hasn't been referenced in 60+ days. Knowledge that isn't reinforced decays — just like in the manifold's surface tension model.

**Subagent delegation for deep retrieval.** When the AI needs to search or extract from the archive tier, it should spawn subagents rather than loading dense material into the main conversation context. Each subagent operates with bounded context on a specific file or topic — mirroring the manifold's "fauxna" that inhabit specific strata and process knowledge locally. Topics and groupings can go file-level, using structured extraction (grep, glob, targeted reads) to surface only what's relevant.

### How it maps to the Intelligence Manifold

| Manifold concept | Implementation here |
|-----------------|-------------------|
| Outer strata (lightweight) | `surface/` — state, index, session log |
| Middle strata (semantic) | `detail/` — people, patterns, drafts |
| Inner strata (dense) | `archive/` — sessions, context, raw material |
| Strands (provenance) | `[source: ...]` links in every derived file |
| Fauxna (local agents) | Subagents spawned per-file for archive retrieval |
| Surface tension (density gating) | `_index.md` files — AI reads summary before descending |
| Signal propagation (communication) | Updates cascade: archive → detail index → surface state |
| Selective forgetting | `last_updated` dates + staleness review protocol |

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
│   ├── state.md                 # Current state, identity, active threads
│   ├── index.md                 # Map of everything in the system
│   └── session-log.md           # One-line-per-session chronological list
├── detail/                      # Loaded on demand — profiles, analysis
│   ├── people/
│   │   ├── _index.md            # Summary of all people profiles
│   │   └── self.md              # Self-profile template
│   ├── patterns/
│   │   └── _index.md            # Summary of identified patterns
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
- **Browsable and editable** outside of AI conversations
- **Works with any AI tool**, not just Claude
- **Version controlled** with git
- **Private by default** — stays local unless you choose otherwise
- **Subagent-friendly** — structured for parallel retrieval without blowing up context
