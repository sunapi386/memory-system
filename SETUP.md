# Setup Guide

## Quick Start

1. **Copy this folder** to wherever you want it:
   ```bash
   cp -r memory-system ~/my-memory/
   ```

2. **Edit `surface/state.md`** — fill in who you are, what's happening, what you're tracking.

3. **Edit `detail/people/self.md`** — write your initial self-profile.

4. **Tell Claude about it.** Pick one:

### Option A: Project-level auto-load (best for Claude Code)
Just work from this directory. Claude Code reads `CLAUDE.md` automatically, which instructs it to read the surface layer first.

### Option B: Add a memory pointer
Ask Claude to save a memory:
> "Remember: I have a memory system at ~/my-memory/. On first load, read everything in surface/ then descend as needed."

### Option C: Manual each time
Start conversations with:
> "Read ~/my-memory/surface/state.md and surface/session-log.md, then let's pick up where we left off."

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
> "Check my Claude memory files and migrate anything useful into my memory system at ~/my-memory/. Place summaries in detail/, raw material in archive/."

Claude will read the scattered memories, place them at the appropriate density tier, and suggest removing the originals.

## Structure

```
surface/              ← Always loaded. Lightweight. Current state + pointers.
  state.md            ← Who you are, what's happening now
  threads.md          ← Active threads spanning multiple sessions
  index.md            ← Signal-annotated map of all files
  session-log.md      ← One-line-per-session chronological list

detail/               ← Loaded on demand. Profiles, analysis, active work.
  people/
    _index.md         ← One-line summary of each person
    self.md           ← Your self-profile
  patterns/
    _index.md         ← One-line summary of each pattern
  skills/
    _index.md         ← Distilled operational knowledge
  drafts/
    _index.md         ← Active work-in-progress

archive/              ← Loaded rarely. Dense source material, history.
  sessions/
    _index.md         ← Full session notes index
  context/
    _index.md         ← Source material summaries
  raw/                ← Unprocessed transcripts, emails, documents
```

## Tips

- **Start messy.** You don't need every folder populated. Let it grow organically.
- **Update `surface/state.md` often.** It's the entry point — if it's stale, every new conversation starts confused.
- **One session per conversation.** Even short ones. The numbering creates a timeline.
- **Let Claude manage the tiers.** The CLAUDE.md instructions tell it where to place new content by density. You just talk.
- **Review periodically.** Every few weeks, ask Claude: "Review my memory system — what's stale, what should be promoted or archived?"
- **Watch for skills.** When Claude suggests distilling a skill, that's the system compounding — validated approaches that carry forward across every future conversation.
- **Don't worry about cleanup.** Stale content degrades through tiers automatically. Nothing gets deleted without your say — it just gets compressed and moved deeper.
