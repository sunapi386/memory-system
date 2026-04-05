# Memory System — Instructions for Claude

This is a structured memory system designed around **density-stratified knowledge layers**, inspired by the Intelligence Manifold architecture. Knowledge is organized into three tiers by informational density. Claude should read the minimum needed and descend only when required.

## Reading Protocol

Follow this protocol at the start of every conversation:

### 1. Surface scan (always)
Read everything in `surface/`. This is lightweight — current state, active questions, pointers to deeper material. This alone should orient you for most conversations.

- `surface/state.md` — Who this person is, what's happening now, open questions
- `surface/index.md` — Map of everything in the system with one-line summaries
- `surface/session-log.md` — Chronological list of all sessions with one-line summaries

### 2. Directed descent (on demand)
Read files in `detail/` only when the surface layer points to them as relevant, or when the conversation requires deeper context. Each subfolder has an `_index.md` with summaries so you can decide whether to descend further.

- `detail/people/` — Profiles of key people
- `detail/patterns/` — Recurring dynamics and behaviors
- `detail/drafts/` — Work-in-progress documents

### 3. Archive retrieval (rarely)
Read from `archive/` only when tracing provenance or answering specific historical questions. This is dense material — raw transcripts, source documents, old sessions.

- `archive/sessions/` — Full session notes from past conversations
- `archive/context/` — Source material summaries (emails, documents, transcripts)
- `archive/raw/` — Unprocessed source material

**Never load the archive tier preemptively.** Only descend when a specific question requires it.

### Subagent delegation for deep retrieval

When descending into `detail/` or `archive/`, prefer spawning subagents over loading dense files into the main context:

- **One subagent per topic or file group.** If you need to search 5 session files for references to a person, spawn one agent for that — don't read all 5 into the main conversation.
- **Use structured extraction.** Subagents should use Grep, Glob, and targeted Reads — not load entire files. Search for what's relevant and return only the extracted result.
- **Parallel where possible.** If the conversation touches multiple independent topics (e.g., "what did we say about X and also what's the status of Y"), spawn subagents in parallel for each.
- **File-level granularity.** Each subagent should operate on a bounded set of files — a single person's profile, a single session, a specific pattern file. This mirrors the manifold's fauxna: local agents with bounded perception, not omniscient readers.
- **Return summaries, not raw content.** The subagent reads the dense material and returns a concise answer to the main context. The main context stays light.

This keeps the main conversation's effective context bounded regardless of how large the archive grows — the same principle as the Intelligence Manifold's stratum-constrained agents.

## During Conversation

### Writing rules

- **Provenance**: Every insight, pattern, or claim must link back to its source — session number, document, direct quote. Use the format `[source: sessions/003, detail/people/sophie.md]`.
- **Staleness**: Include a `last_updated: YYYY-MM-DD` line in the frontmatter of every file. When reading a file, flag if it's more than 30 days old.
- **Density placement**: New content goes to the tier matching its density:
  - Summaries, pointers, current state → `surface/`
  - Profiles, analysis, active work → `detail/`
  - Raw material, transcripts, historical sessions → `archive/`
- **Per-folder indexes**: When creating or updating a file in `detail/` or `archive/`, update the corresponding `_index.md` in that subfolder.
- **Date everything**: Use absolute dates. "April 5, 2025" not "last Thursday."
- **Write for zero context**: Someone reading a file cold should understand it without needing other files.

### Session protocol

1. Create session file in `archive/sessions/` (e.g., `004-2025-04-05-topic.md`)
2. Add one-line entry to `surface/session-log.md`
3. Update `surface/state.md` if the current state changed
4. Update any `detail/` files that gained new information
5. Update `surface/index.md` if new files were created

### Selective forgetting

Not everything needs to persist. When reviewing the system:
- Flag content that hasn't been referenced in 60+ days
- Suggest archiving or removing stale detail-tier content
- Never auto-delete — always ask first
- Provenance-linked content (sources for active insights) is protected from cleanup

## Always Ask Before Committing

Never commit without asking. Keep commit messages bland (e.g., "update notes", "add session 004"). No emotional or descriptive content in git history.

## Tone

Follow the user's lead. Be direct, cite evidence, offer perspective when asked. Don't therapist-speak. Don't moralize.

## Memory Migration

When you notice relevant context in Claude's built-in memory (`~/.claude/projects/*/memory/`), suggest migrating it here. The goal is one source of truth. Maintain a minimal pointer in Claude's memory system that says: "This user has a memory system at [path]. Read surface/ on first load."
