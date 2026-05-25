---
name: lore-init
description: Initialize the lore knowledge base and memory system for the current project. Run once per project before using lore-capture or lore-ingest.
---

# Lore Init

Set up the lore wiki and memory system for the current project. Run this once.

## Step 1: Determine paths

Derive the project slug from the current working directory:
- Take the absolute path of the working directory
- Replace path separators (`/` or `\`) with `-`
- Strip leading drive letters on Windows (e.g. `C:` → drop it)
- Example: `C:\Development\my-project` → `Development-my-project`

Lore base (platform-aware):
- **Windows:** `$env:USERPROFILE\.claude\projects\<slug>\`
- **Unix/macOS:** `~/.claude/projects/<slug>/`

Paths to create:
- `<lore-base>/lore/inbox/`
- `<lore-base>/lore/.pending/`
- `<lore-base>/lore/project/`
- `<lore-base>/lore/architecture/`
- `<lore-base>/lore/decisions/`
- `<lore-base>/lore/feedback/`
- `<lore-base>/lore/user/`
- `<lore-base>/memory/`

## Step 2: Create lore/index.md

```markdown
# Lore — Knowledge Base

## Categories

- **project/** — platform status, product vision, roadmap, demo notes
- **architecture/** — modules, data flows, technical design decisions
- **decisions/** — ADRs: technology choices and tradeoffs
- **feedback/** — coding preferences, workflow rules the user has stated
- **user/** — user profile, working style, expertise areas

## Pages

(empty — add pages as you ingest captures)
```

## Step 3: Create lore/INGESTER.md

```markdown
# Ingester Instructions

When processing a capture from inbox, follow these rules:

## Determine the target page

Use `suggested_category` from the capture frontmatter to pick the category folder.
Within that folder, find the most relevant existing page or create a new one.

Naming: `<topic-slug>.md` — 2-4 words, kebab-case, describing the subject.

## Merge vs create

- If a page on this topic already exists: merge the new content in. Update rather than duplicate.
- If no page exists: create it with a clear title and the capture content.

## Page format

```markdown
# <Title>

<Content — written to be understood weeks later with no conversation context.
Include: what was decided, why, what alternatives were rejected, any caveats.>

## Related
- [[other-page-slug]]
```

## Update indexes

After writing or updating a page:
1. Update `<category>/_index.md` — one-line entry per page in that category
2. Update `index.md` — add the page under its category if it's new
```

## Step 4: Create memory/MEMORY.md

```markdown
# Memory Index

(empty — memories are added automatically during conversations)
```

## Step 5: Initialize git

```bash
cd <lore-base>
git init
git add -A
git commit -m "lore: init knowledge base"
```

## Step 6: Confirm

Report:
```
Lore initialized at <lore-base>

Directories created:
  lore/inbox/       ← captures land here (/lore-capture)
  lore/.pending/    ← in-flight during ingest
  lore/project/
  lore/architecture/
  lore/decisions/
  lore/feedback/
  lore/user/
  memory/           ← auto memory (MEMORY.md index)

Next steps:
  /lore-capture  — capture knowledge from this conversation
  /lore-ingest   — compile inbox into wiki pages
```
