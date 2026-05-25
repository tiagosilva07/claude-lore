# claude-lore

A persistent, git-versioned knowledge base for Claude Code projects — plus coding discipline guidelines.

Knowledge from your conversations doesn't disappear when the context window closes. `claude-lore` captures decisions, bugs, patterns, and lessons into a structured wiki that grows with your project.

---

## What it does

### Knowledge wiki

Three slash commands manage your project knowledge:

| Command | What it does |
|---------|-------------|
| `/lore-init` | Scaffold the wiki and memory system for the current project (run once) |
| `/lore-capture` | Save durable knowledge from the current conversation to the inbox |
| `/lore-ingest` | Compile inbox captures into cross-linked wiki pages, commit to git |

The wiki lives at `~/.claude/projects/<project-slug>/lore/` — outside your repo, so private notes stay private. It's git-versioned so the full history is preserved.

### Auto memory

A structured memory system at `~/.claude/projects/<project-slug>/memory/` that persists across sessions. Claude reads `MEMORY.md` at session start and writes typed memory files (user, feedback, project, reference) as it learns about your project and preferences.

### Coding guidelines

Four principles that reduce common LLM coding mistakes:

1. **Think Before Coding** — surface assumptions, ask when uncertain, push back on overcomplication
2. **Simplicity First** — minimum code that solves the problem, nothing speculative
3. **Surgical Changes** — touch only what you must, clean up only your own mess
4. **Goal-Driven Execution** — define verifiable success criteria, loop until met

---

## Install

**Option A: Claude Code Plugin (recommended)**

```
/plugin marketplace add <your-github-username>/claude-lore
/plugin install claude-lore@lore
```

**Option B: Manual**

Copy `CLAUDE.md` into your project or append to your existing `CLAUDE.md`:

```bash
curl https://raw.githubusercontent.com/<your-github>/claude-lore/main/CLAUDE.md >> CLAUDE.md
```

Copy the skill files to your Claude commands directory:
- `skills/lore-capture/SKILL.md` → `~/.claude/commands/lore-capture.md`
- `skills/lore-ingest/SKILL.md` → `~/.claude/commands/lore-ingest.md`
- `skills/lore-init/SKILL.md` → `~/.claude/commands/lore-init.md`

---

## Usage

**First time on a project:**
```
/lore-init
```

**During a conversation (when something worth keeping comes up):**
```
/lore-capture
```

**To compile captured knowledge into the wiki:**
```
/lore-ingest
```

---

## Wiki structure

```
~/.claude/projects/<project-slug>/
  lore/
    index.md              ← read this first every session
    INGESTER.md           ← rules for merging captures into pages
    inbox/                ← captures waiting to be compiled
    .pending/             ← in-flight during ingest (auto-managed)
    project/              ← platform status, product vision, roadmap
    architecture/         ← modules, data flows, technical design
    decisions/            ← ADRs: technology choices and tradeoffs
    feedback/             ← coding preferences, workflow rules
    user/                 ← user profile and working style
  memory/
    MEMORY.md             ← index, loaded every session
    *.md                  ← individual memory files
```

---

## What to capture vs. what to skip

**Capture:**
- A decision was made — what, why, what was rejected
- A bug was root-caused — the actual cause, not just the fix
- A pattern was identified in the codebase
- The user stated a preference or workflow rule
- An architectural choice was made with a tradeoff

**Skip:**
- What file was edited, what command was run
- In-progress task state
- Anything derivable from reading the code or `git log`

---

## Philosophy

Claude Code's context window is ephemeral. Every new session starts cold — Claude re-derives everything from the codebase and conversation history. `claude-lore` gives Claude a persistent layer of *why*: why the architecture is shaped this way, why this library was chosen, why this approach was rejected.

The goal is that a senior engineer opening this project for the first time — or Claude starting a new session — can read the lore wiki and understand not just *what* the code does, but *why* it was built this way.

---

## License

MIT
