# Memory System — Instructions for Claude

This is a structured memory and context system. When working in this directory, follow these instructions exactly.

## On First Load

1. Read `START_HERE.md` to understand who this person is and what this system tracks.
2. Read the latest file in `sessions/` to pick up where the last conversation left off.
3. Skim `people/self.md` for the most current state and open questions.

## During Conversation

### When to write

- **Sessions**: Create a new session file (`sessions/NNN-YYYY-MM-DD-topic.md`) for every substantive conversation. Number sequentially. Include: key revelations, decisions made, questions raised, state at end.
- **People**: Update or create profiles in `people/` when new information emerges about someone important. Include: who they are, their role, key quotes, patterns observed.
- **Patterns**: Update `patterns/` when recurring dynamics are identified — behavioral patterns, relationship dynamics, avoidance mechanisms, inherited tendencies.
- **Context**: Add to `context/` when source material is reviewed (emails, documents, transcripts) — store summaries, not raw data.
- **Drafts**: Use `drafts/` for any written output being iterated on (emails, messages, letters, plans).

### How to write

- Be specific and evidence-based. Use direct quotes where possible.
- Date everything. Use absolute dates, not relative ("April 5, 2025" not "last Thursday").
- Write for a future conversation that has zero context. Someone reading this cold should understand.
- Don't sanitize. Record what was actually said and felt, not a cleaned-up version.
- Keep files focused — one person per file, one pattern per file, one session per conversation.

### When to update vs create new

- **Update** existing files when new information adds to or corrects them.
- **Create new** files when a genuinely new topic, person, or pattern emerges.
- Always update `START_HERE.md` when the current state changes meaningfully.

## Always Ask Before Committing

If this is a git repo, never commit without asking first. Keep commit messages bland and non-descriptive (e.g., "update notes", "add session 004"). No emotional content in commit messages.

## Tone

Follow the user's lead. This system exists to serve their self-understanding, not to impose a framework. Don't therapist-speak. Be direct, cite evidence, offer perspective when asked. Don't moralize.

## Memory Migration

When you notice the user has relevant context stored in Claude's built-in memory system (`~/.claude/projects/*/memory/`) that would be better centralized here, suggest migrating it. The goal is one source of truth, not scattered memories across systems.

Conversely, maintain a minimal pointer in Claude's memory system that says: "This user has a memory system at [path]. Read START_HERE.md on first load."
