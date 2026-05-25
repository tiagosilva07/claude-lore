# Claude Lore — Project Configuration

## Auto Memory

You have a persistent, file-based memory system. Derive its location from the current working directory:

1. Take the absolute path of the working directory
2. Replace path separators (`/` or `\`) with `-`, strip Windows drive letter
3. Memory base:
   - **Windows:** `$env:USERPROFILE\.claude\projects\<slug>\memory\`
   - **Unix/macOS:** `~/.claude/projects/<slug>/memory/`

This directory already exists after running `/lore-init` — write to it directly.

### Memory types

| Type | When to save | File naming |
|------|-------------|-------------|
| `user` | Role, expertise, preferences | `user_<topic>.md` |
| `feedback` | Corrections or confirmed approaches | `feedback_<topic>.md` |
| `project` | Ongoing work, goals, decisions | `project_<topic>.md` |
| `reference` | Where to find things in external systems | `reference_<topic>.md` |

### Memory file format

```markdown
---
name: <short-kebab-slug>
description: <one-line summary used to decide relevance in future sessions>
metadata:
  type: <user|feedback|project|reference>
---

<Memory content. For feedback/project: lead with the rule or fact, then **Why:** and **How to apply:** lines.>
```

### MEMORY.md index

Keep `memory/MEMORY.md` as a one-line-per-entry index:
```
- [Title](file.md) — one-line hook describing what this memory is for
```

Lines past 200 are truncated — keep entries concise.

### When to save

- User states a preference, role, or expertise → save immediately
- User corrects your approach → save with why
- User confirms a non-obvious approach worked → save (confirmations matter as much as corrections)
- User asks you to remember something → save immediately

### What NOT to save

- Code patterns, file paths, architecture — derivable from the codebase
- Git history — `git log` is authoritative
- Ephemeral task state — use tasks for in-conversation tracking
- Anything already in CLAUDE.md files

### Before acting on a memory

A memory that names a file, function, or flag is a claim about the past. Verify before recommending:
- File path named → check it exists
- Function named → grep for it
- Memory conflicts with current state → trust what you observe now, update the memory

---

## Coding Guidelines

### Think before coding

Before implementing anything:
- State assumptions explicitly. If uncertain, ask — don't pick an interpretation silently.
- If multiple interpretations exist, surface them and let the user choose.
- If a simpler approach exists, say so and push back.
- If something is genuinely unclear, stop. Name what's confusing. Ask.

### Goal-driven execution

Transform tasks into verifiable goals before starting:
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan with explicit verification:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
```

Strong success criteria allow independent looping to completion.

### Simplicity first

- No features beyond what was asked
- No abstractions for single-use code
- No error handling for impossible scenarios
- Three similar lines is better than a premature abstraction

### Surgical changes

- Don't touch adjacent code, comments, or formatting
- Don't refactor things that aren't broken
- Remove only imports/variables/functions that YOUR changes made unused
- If you notice unrelated dead code, mention it — don't delete it
