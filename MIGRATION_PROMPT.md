# Memory Migration Prompt

Use this prompt to get Claude to consolidate scattered memories into your system. Copy-paste into a new conversation:

---

```
I have a centralized memory system at ~/memory-system/. Read the CLAUDE.md and surface/ files first.

Then check for any scattered context I have in:
1. ~/.claude/projects/*/memory/ (Claude's built-in memory files)
2. Any other personal notes, journals, or context files you can find in my home directory

For each piece of useful context you find:
- Determine its density tier:
  - Summaries, current state, pointers → surface/
  - Profiles, analysis, active drafts → detail/
  - Raw transcripts, source documents, old notes → archive/
- Create or update the appropriate file
- Update the relevant _index.md files
- Tell me what you migrated, from where, and which tier you placed it in
- Suggest whether the original can be removed

Don't delete anything without my approval. Just show me the migration plan first.
```

---

## Why migrate?

Claude's built-in memory system (`~/.claude/projects/*/memory/`) works, but:
- Memories are scattered across project directories
- You can't easily browse or edit them
- They're tied to Claude's internal format
- They don't transfer between AI tools
- There's no density stratification — everything is treated as equally important

This system is markdown files in a folder, organized by informational density. You own it. You can read it, edit it, version it, back it up, or use it with any AI.
