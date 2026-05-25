---
name: lore-ingest
description: Compile all pending lore captures into structured wiki pages. Merges inbox files into cross-linked pages, updates indexes, and commits to git.
---

# Lore Ingest

Compile captures from the inbox into the lore wiki.

## Step 1: Determine paths

Derive the project slug and lore base path using the same logic as lore-capture:
- Replace path separators with `-`, strip Windows drive letter
- **Windows:** `$env:USERPROFILE\.claude\projects\<slug>\lore\`
- **Unix/macOS:** `~/.claude/projects/<slug>/lore/`

If lore base does not exist, tell the user to run `/lore-init` first and stop.

## Step 2: Check inbox

List files in `<lore-base>/inbox/`.

If empty: report "Lore inbox is empty — nothing to ingest." and stop.

## Step 3: Move inbox → .pending

Move all files from `inbox/` to `.pending/` before processing (so a crash mid-ingest doesn't lose captures or re-process them).

Use the appropriate shell:
- **Windows (PowerShell):** `Move-Item "$lore\inbox\*" "$lore\.pending\"`
- **Unix/macOS (Bash):** `mv "$lore/inbox/"* "$lore/.pending/"`

## Step 4: Read current wiki state

Read:
1. `<lore-base>/index.md` — overall structure
2. Each file in `.pending/`
3. Any existing wiki pages the pending files should update (based on `suggested_category`)
4. `<lore-base>/INGESTER.md` — merge/create rules (if present); otherwise use the rules below

## Step 5: Process each pending file

For each capture file in `.pending/`:

**Determine target page:**
- Use `suggested_category` to pick the category folder (`project/`, `architecture/`, `decisions/`, `feedback/`, `user/`)
- Find the most relevant existing page in that folder, or create a new one
- Page filename: 2-4 kebab-case words describing the subject

**Merge or create:**
- Existing page on this topic → merge new content in, update rather than duplicate
- No existing page → create with a clear title and the capture content

**Page format:**
```markdown
# <Title>

<Content written to be understood weeks later with no conversation context.
Include: what was decided, why, alternatives rejected, caveats.>

## Related
- [[other-page-slug]]
```

**Update indexes after each page:**
1. `<category>/_index.md` — one-line entry per page in that category
2. `<lore-base>/index.md` — add new pages under their category

## Step 6: Commit

```bash
# Unix/macOS
git -C "<lore-base>" add -A
git -C "<lore-base>" commit -m "lore: ingest N capture(s)"

# Windows (PowerShell)
git -C "$lore" add -A
git -C "$lore" commit -m "lore: ingest N capture(s)"
```

If git is not initialized, tell the user to run `/lore-init` first.

## Step 7: Clean .pending

Delete all files from `.pending/` after a successful commit.

## Step 8: Confirm

Report: "Lore ingest complete — N page(s) updated, M page(s) created. Run /lore-capture to add more knowledge."
