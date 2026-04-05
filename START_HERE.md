# Memory System

**Created:** [DATE]
**Purpose:** A structured system for tracking context, patterns, and continuity across AI conversations. Designed to be the single source of truth so every new conversation can pick up where the last one left off.

## Who I Am

[Write a brief paragraph about yourself — role, situation, what you're working on or working through. This is the first thing a new conversation reads.]

## Current State

[What's happening right now. Update this whenever things shift meaningfully.]

- **Primary focus:** [what's top of mind]
- **Open questions:** [what you're trying to figure out]
- **Last session:** [pointer to latest session file]

## How This System Works

- `CLAUDE.md` — Instructions for Claude on how to use this system (read automatically)
- `START_HERE.md` — This file. Entry point for every conversation.
- `context/` — Background material, source summaries, timelines
- `people/` — Profiles of key people (including `self.md`)
- `patterns/` — Recurring dynamics, behaviors, tendencies identified over time
- `sessions/` — Numbered notes from each conversation
- `drafts/` — Work-in-progress documents being iterated on

## Quick Start for New Conversations

Tell Claude:
```
Read ~/memory-system/START_HERE.md and the latest session file, then let's pick up where we left off.
```

Or set up the pointer so Claude does this automatically (see Setup Guide below).

## Setup Guide

### Option 1: CLAUDE.md auto-load (recommended)
The `CLAUDE.md` file in this directory is automatically read by Claude Code when you're working in this folder. Just `cd` here or open it as your project.

### Option 2: Claude memory pointer
Save a memory in Claude's system that points here:
```
"I have a memory system at ~/memory-system/. On first load, read START_HERE.md and the latest session file."
```

### Option 3: Manual prompt
Start each conversation with:
```
Read ~/memory-system/START_HERE.md first.
```

## Principles

1. **One source of truth.** Everything goes here, not scattered across Claude's memory, random files, or your head.
2. **Write for your future self.** Every file should be understandable by someone with zero context.
3. **Evidence over narrative.** Direct quotes, specific dates, actual events — not interpretations.
4. **Update, don't hoard.** Old information gets corrected, not left to rot alongside new information.
5. **Privacy first.** This is local. Don't push to remote unless you explicitly choose to.
