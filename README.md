# memory-system

A structured folder system for maintaining context and continuity across AI conversations.

Instead of relying on scattered built-in AI memory, this gives you a single set of markdown files that any AI can read, you can browse and edit, and that persists across tools and platforms.

## How it works

1. **`CLAUDE.md`** — Instructions that tell Claude how to use the system (auto-loaded by Claude Code)
2. **`START_HERE.md`** — Entry point for every conversation. Fill this in with who you are and what you're tracking.
3. **`sessions/`** — Each conversation gets a numbered session file, creating a timeline.
4. **`people/`**, **`patterns/`**, **`context/`**, **`drafts/`** — Organized storage for everything that emerges.

Claude reads `START_HERE.md` at the start of each conversation, picks up where you left off, and updates the relevant files as you go.

## Quick start

```bash
git clone https://github.com/sunapi386/memory-system.git ~/memory
cd ~/memory
```

Edit `START_HERE.md` and `people/self.md`, then start a Claude Code session in that directory.

See [SETUP.md](SETUP.md) for integration options and [MIGRATION_PROMPT.md](MIGRATION_PROMPT.md) to consolidate existing AI memories.

## Structure

```
├── CLAUDE.md              # Instructions for Claude (auto-loaded)
├── START_HERE.md          # Entry point — fill this in first
├── SETUP.md               # Setup and integration guide
├── MIGRATION_PROMPT.md    # Prompt to consolidate scattered memories
├── context/               # Summaries of source material reviewed
├── people/
│   └── self.md            # Self-profile template
├── patterns/              # Recurring dynamics and behaviors
├── sessions/              # Numbered conversation notes
└── drafts/                # Documents being iterated on
```

## Why not just use built-in AI memory?

- You own the files — plain markdown, no vendor lock-in
- Browsable and editable outside of AI conversations
- Works with any AI tool, not just Claude
- Version controlled with git
- Stays local and private unless you choose otherwise
