# Memory System — Instructions for Claude

This is a structured memory system designed around **density-stratified knowledge layers**, inspired by the Intelligence Manifold architecture. Knowledge is organized into three tiers by informational density. Claude should read the minimum needed and descend only when required.

## Reading Protocol

Follow this protocol at the start of every conversation:

### 1. Surface scan (always)
Read everything in `surface/`. This is lightweight — current state, active threads, pointers to deeper material. This alone should orient you for most conversations.

- `surface/state.md` — Who this person is, what's happening now, open questions
- `surface/threads.md` — Active threads spanning multiple sessions, with status and pointers
- `surface/index.md` — Signal-annotated map of everything in the system
- `surface/session-log.md` — Chronological list of all sessions with one-line summaries

### 2. Directed descent (on demand)
Read files in `detail/` only when the surface layer points to them as relevant, or when the conversation requires deeper context. Each subfolder has an `_index.md` with summaries so you can decide whether to descend further.

- `detail/people/` — Profiles of key people
- `detail/patterns/` — Recurring dynamics and behaviors
- `detail/skills/` — Distilled operational knowledge validated across sessions
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
- **Use structured extraction.** Subagents should use Grep, Glob, and targeted Reads — not load entire files. For factual queries (dates, names, events), start with keyword search. For thematic queries (patterns, dynamics), read indexes and scan semantically. Try both before reporting "not found."
- **Parallel where possible.** If the conversation touches multiple independent topics, spawn subagents in parallel for each.
- **File-level granularity.** Each subagent should operate on a bounded set of files. This mirrors the manifold's fauxna: local agents with bounded perception, not omniscient readers.
- **Return summaries, not raw content.** The subagent reads the dense material and returns a concise answer to the main context. The main context stays light.

## During Conversation

### Writing rules

- **Provenance**: Every insight, pattern, or claim must link back to its source — session number, document, direct quote. Use the format `[source: sessions/003, detail/people/sophie.md]`.
- **Staleness**: Include a `last_updated: YYYY-MM-DD` line in the frontmatter of every file. When reading a file, flag if it's more than 30 days old.
- **Density placement**: New content goes to the tier matching its density:
  - Summaries, pointers, current state → `surface/`
  - Profiles, analysis, active work, distilled skills → `detail/`
  - Raw material, transcripts, historical sessions → `archive/`
- **Per-folder indexes**: When creating or updating a file in `detail/` or `archive/`, update the corresponding `_index.md` in that subfolder.
- **Date everything**: Use absolute dates. "April 5, 2025" not "last Thursday."
- **Write for zero context**: Someone reading a file cold should understand it without needing other files.

### Session protocol

At the end of each substantive conversation:

1. Create session file in `archive/sessions/` (e.g., `004-2025-04-05-topic.md`)
2. Add one-line entry to `surface/session-log.md`
3. Update `surface/state.md` if the current state changed
4. Update `surface/threads.md` — add new threads, update existing ones, resolve completed ones
5. Update any `detail/` files that gained new information
6. Update `surface/index.md` — add new files, update signal annotations (see below)
7. Check if any repeated approach should be promoted to a skill (see Skills below)

### Signal tracking

The index (`surface/index.md`) tracks signal strength for every file. At the end of each session, update signal annotations for files that were accessed or referenced:

- **Increment access count** for every file read during the session
- **Assess signal level** heuristically based on: how often it's accessed, how recently, and whether the user has marked it as important
  - `high` — accessed frequently, referenced in active threads
  - `medium` — accessed occasionally, still relevant
  - `low` — rarely accessed, not tied to active threads
  - `fading` — not accessed in 60+ days, candidate for cascade
  - `pinned` — user explicitly marked as always-relevant, never degrades

### Skills: distilling operational knowledge

Skills are validated approaches distilled from multiple sessions. They live in `detail/skills/`.

**When to create a skill:**
- The same approach has worked 3+ times across different sessions
- The user explicitly validates an approach ("yes, keep doing that", "that worked")
- The user explicitly corrects an approach ("don't do X") — capture as a negative skill

**Skill file format:**
```markdown
---
last_updated: YYYY-MM-DD
derived_from: [sessions/001, sessions/005]
confidence: validated | emerging | negative
---
# Skill: [name]

[What to do / what not to do, with evidence]
```

**Always read `detail/skills/_index.md` before starting work.** Skills represent validated preferences and should be followed unless the user overrides them.

### Degradation cascade

Content degrades through tiers over time — it is compressed and moved down, not deleted.

**Cascade direction:** `surface → detail → archive → git history`

**When to cascade:**
- Signal drops to `fading` in the index → suggest cascading to the next tier
- Detail-tier content not accessed in 60 days → suggest cascade to archive
- Archive content not accessed in 90 days → suggest removal (git history preserves it)

**How to cascade:**
- Compress the content (200-line detail file → 20-line archive summary)
- Move the file to the lower tier
- Update indexes at both the source and destination tier
- Update any threads or skill files that reference the moved file

**Reverse cascade (promotion):** If an archived file becomes relevant again, promote it back to detail and update indexes at both tiers.

**Never auto-cascade.** Always suggest and get confirmation first. Provenance-linked content (sources for active skills or threads) is protected from cascade.

## Always Ask Before Committing

Never commit without asking. Keep commit messages bland (e.g., "update notes", "add session 004"). No emotional or descriptive content in git history.

## Tone

Follow the user's lead. Be direct, cite evidence, offer perspective when asked. Don't therapist-speak. Don't moralize.

## Memory Migration

When you notice relevant context in Claude's built-in memory (`~/.claude/projects/*/memory/`), suggest migrating it here. The goal is one source of truth. Maintain a minimal pointer in Claude's memory system that says: "This user has a memory system at [path]. Read surface/ on first load."
