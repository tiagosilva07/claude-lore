---
name: lore-capture
description: Capture durable knowledge from the current conversation to the lore inbox. Use when decisions are made, bugs are root-caused, patterns are identified, or lessons are learned. Do NOT capture ephemeral task details.
---

# Lore Capture

Save durable knowledge from this conversation to the lore inbox for later compilation.

## What to capture

Identify the most important durable knowledge:
- Decisions made and why (including rejected alternatives)
- Bugs root-caused (the root cause, not just the fix)
- Architectural choices and their tradeoffs
- Patterns identified in the codebase
- Lessons learned
- User preferences or workflow rules stated explicitly

**Skip:** what files were edited, what commands were run, in-progress task state.

## Step 1: Determine the inbox path

Derive the project slug from the current working directory:
- Replace path separators (`/` or `\`) with `-`
- Strip leading drive letter on Windows (e.g. `C:` → drop it)
- Example: `/home/user/my-project` → `home-user-my-project`

Inbox path:
- **Windows:** `$env:USERPROFILE\.claude\projects\<slug>\lore\inbox\`
- **Unix/macOS:** `~/.claude/projects/<slug>/lore/inbox/`

If the inbox directory does not exist, tell the user to run `/lore-init` first and stop.

## Step 2: Write the capture file

Filename: `YYYY-MM-DDTHH-MM-SS-<content-slug>.md`
- Use current UTC time
- `<content-slug>` is 2-4 kebab-case words describing the content
- Example: `2026-05-25T14-32-00-async-queue-decision.md`

File content:
```
---
source: conversation
captured: <ISO 8601 UTC timestamp>
topic: <one-line description of what was captured>
suggested_category: <project|architecture|decisions|feedback|user>
---

<The durable knowledge, written clearly so it makes sense weeks later
with no context from this conversation.

Include:
- What was decided or discovered
- Why (motivation, constraints)
- What alternatives were considered and rejected
- Any caveats or conditions that might change this>
```

## Step 3: Confirm

Report: "Captured to lore/inbox/<filename>. Run /lore-ingest to compile."

Do NOT ingest immediately — just write to inbox.
