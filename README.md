# claude-automations

> Slash commands and scheduled routines for Claude Code + Obsidian.  
> Prompts live in Obsidian — edit a note, next run picks up changes automatically.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-compatible-8A2BE2)](https://claude.ai/code)
[![Language: EN](https://img.shields.io/badge/prompts-EN-green)](README.md)

> **Note:** All prompts are in English for portability. User input/output can be in PT-BR.

---

## What this is

7 slash commands for Claude Code. 7 scheduled routines that run autonomously on a cron schedule.  
The key pattern: **thin harness + live prompts**. Routines are plain markdown files in your Obsidian vault. Claude reads them at runtime — no redeploy needed.

---

## Architecture

```
claude-automations/
├── commands/           Slash commands → copy to .claude/commands/
└── routines/           Routine prompts → copy to $VAULT_DIR/Queue/routines/

$VAULT_DIR/
├── Queue/
│   ├── routines/       Source of truth for routine prompts (editable in Obsidian)
│   └── .archive/       Completed one-shot tasks
└── Generated/          Autonomous outputs, organized by routine
    ├── ingest-reports/
    ├── connections/
    ├── daily-briefs/
    ├── wiki-lint/
    └── meta-coaching/

~/.claude/scheduled-tasks/
└── <routine>/
    └── SKILL.md        Thin wrapper — reads $VAULT_DIR/Queue/routines/<routine>.md
```

**Flow:**

```
Obsidian note (routine prompt)
        ↓  reads at runtime
~/.claude/scheduled-tasks (thin wrapper, cron trigger)
        ↓  executes
Claude Code agent
        ↓  writes output
$VAULT_DIR/Generated/<routine>/
```

---

## Commands

| Command | What it does |
|---------|-------------|
| `/article-report` | Analytical report of ingested articles (by period) |
| `/process-queue` | Process one-shot tasks from `Queue/` |
| `/commit-standard` | Generate a Conventional Commits message |
| `/create-crud` | Complete Java CRUD scaffold (DAO pattern) |
| `/explain` | Explain code or concept adapted for learners |
| `/session-summary` | Structured summary of current session |
| `/code-review` | Code review of current file |

---

## Routines

| Routine | Schedule | Description |
|---------|----------|-------------|
| `weekly-ingest` | Fri 5 PM | Ingest articles + clippings — token-economic batch |
| `post-ingest-report` | Fri 7 PM | Cross-analysis report after ingest |
| `weekly-meta-coaching` | Sat 1 AM | Self-audit of Claude Code usage patterns |
| `weekly-wiki-lint` | Sun 4 AM | Health check: orphans, dead links, duplicates |
| `connection-finder` | Sun 6 AM | Surface non-obvious links between sources |
| `daily-brief` | Mon–Fri 11 PM | Nightly context digest: changes, connections, open question |
| `process-queue` | 8AM / 12PM / 4PM / 8PM | Drain async task queue |

### Weekly schedule

```
Mon  Tue  Wed  Thu  Fri  Sat  Sun
 ▪    ▪    ▪    ▪    ▪    ▫    ▫   daily-brief (11 PM)
                      ▪              weekly-ingest (5 PM)
                      ▪              post-ingest-report (7 PM)
                           ▪         weekly-meta-coaching (1 AM)
                                ▪    weekly-wiki-lint (4 AM)
                                ▪    connection-finder (6 AM)
 ▪    ▪    ▪    ▪    ▪    ▪    ▪   process-queue (×4/day)
```

---

## Quick start

### 1. Copy commands

```bash
cp commands/*.md /path/to/project/.claude/commands/
```

Use in Claude Code: `/article-report`, `/commit-standard`, etc.

### 2. Copy routines

```bash
cp routines/*.md $VAULT_DIR/Queue/routines/
```

### 3. Register scheduled tasks

For each routine, create a thin wrapper in `~/.claude/scheduled-tasks/<name>/SKILL.md`:

```markdown
---
name: weekly-ingest
schedule: "0 17 * * 5"
---

Read $VAULT_DIR/Queue/routines/weekly-ingest.md and execute it.
```

That's it. Edit the routine in Obsidian — the next scheduled run picks up changes.

---

## How it works

**Thin harness pattern:** The scheduled task wrapper contains no prompt logic. It just reads the routine file from the vault and executes it. This means:

- Prompts are version-controlled inside Obsidian (your vault, your git)
- Wikilinks connect routines to vault concepts, sources, entities
- Editing a prompt is just editing a note — no settings.json touch needed
- Claude Code handles scheduling, tool execution, output writing

**Token economy:** Routines are designed to minimize token use — bash over AI where possible, batch processing over per-item loops, append-only output files.

---

## Examples

See [`examples/x-sources/`](examples/x-sources/) for sample ingested X (Twitter) threads in vault source format. Useful as templates for adding your own sources.

---

## Contributing

Issues and PRs welcome. Keep prompts in English. Wikilinks must resolve in a standard Obsidian vault layout.

## License

MIT
