# Setup Guide

## Quick Start

1. **Copy this folder** to wherever you want it:
   ```bash
   cp -r memory-system ~/my-memory/
   ```

2. **Edit `START_HERE.md`** — fill in who you are and what you're tracking.

3. **Edit `people/self.md`** — write your initial self-profile.

4. **Tell Claude about it.** Pick one:

### Option A: Project-level auto-load (best for Claude Code)
Just work from this directory. Claude Code reads `CLAUDE.md` automatically.

### Option B: Add a memory pointer
Ask Claude to save a memory:
> "Remember: I have a memory system at ~/my-memory/. When I start a personal conversation, read START_HERE.md and the latest session file first."

### Option C: Manual each time
Start conversations with:
> "Read ~/my-memory/START_HERE.md and pick up where we left off."

## Initializing Git (optional but recommended)

```bash
cd ~/my-memory
git init
echo "*.tmp" > .gitignore
git add -A
git commit -m "init"
```

Keep it local. Don't push to GitHub unless you've thought carefully about privacy.

## Memory Migration

If you already have Claude memories scattered around (`~/.claude/projects/*/memory/`), ask Claude:
> "Check my Claude memory files and migrate anything useful into my memory system at ~/my-memory/."

Claude will read the scattered memories, create proper files in your system, and suggest removing the originals so there's one source of truth.

## Folder Purposes

| Folder | What goes here | Example |
|--------|---------------|---------|
| `context/` | Summaries of source material you've reviewed | Email threads, documents, transcripts |
| `people/` | Profiles of important people | Partners, mentors, colleagues, `self.md` |
| `patterns/` | Recurring dynamics you've identified | Avoidance patterns, relationship cycles |
| `sessions/` | Notes from each AI conversation | `001-2025-04-05-initial.md` |
| `drafts/` | Documents being iterated on | Emails, letters, plans |

## Tips

- **Start messy.** You don't need every folder populated. Let it grow organically.
- **Update `START_HERE.md` often.** It's the entry point — if it's stale, every new conversation starts confused.
- **One session per conversation.** Even short ones. The numbering creates a timeline.
- **Let Claude update files.** Don't feel like you need to manually maintain this — that's what the CLAUDE.md instructions are for. Just tell Claude when something's changed and it'll update the right files.
- **Review periodically.** Every few weeks, ask Claude: "Review my memory system — what's stale, what's missing, what should be updated?"
