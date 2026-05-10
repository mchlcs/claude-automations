# Claude Automations

Skills and routines for Claude Code + Obsidian vault.

> **Language:** All prompts are in English for portability. User input and output can be in **PT-BR**.

## Structure

```
commands/       Slash commands (available via /command in Claude Code)
routines/       Scheduled routines (editable prompts living in Obsidian)
```

## Commands

| Command | Description |
|---------|-------------|
| `/article-report` | Analytical report of ingested articles (by period) |
| `/process-queue` | Process one-shot tasks from Queue/ |
| `/commit-standard` | Conventional Commits message generator |
| `/create-crud` | Complete Java CRUD (DAO pattern) |
| `/explain` | Explain code or concept for learners |
| `/session-summary` | Structured session summary |
| `/code-review` | Code review of current file |

## Routines

| Routine | Schedule | Description |
|---------|----------|-------------|
| `weekly-ingest` | Friday 5 PM | Weekly ingest of articles + clippings (token-economic) |
| `post-ingest-report` | Friday 7 PM | Cross-analysis report after ingest |
| `weekly-meta-coaching` | Saturday 1 AM | Self-audit of Claude Code usage |
| `weekly-wiki-lint` | Sunday 4 AM | Health check: orphans, dead links, dedup |
| `connection-finder` | Sunday 6 AM | Non-obvious connections between sources |
| `daily-brief` | Mon-Fri 11 PM | Nightly context: changes, connections, question |
| `process-queue` | 8AM/12PM/4PM/8PM | Process one-shot tasks from Queue/ |

## How to use

### Commands
Copy to your project's `.claude/commands/`:
```bash
cp commands/*.md /path/to/project/.claude/commands/
```

### Routines
Copy to your Obsidian vault's `Queue/routines/`:
```bash
cp routines/*.md $VAULT_DIR/Queue/routines/
```

Each routine is referenced by a scheduled task (thin wrapper) in `~/.claude/scheduled-tasks/`. The wrapper just reads the routine file and executes — edit the routine in Obsidian, next run picks up changes automatically.

## Architecture

```
$VAULT_DIR/
├── ingest/              Raw sources (articles + clippings)
│   ├── articles/
│   └── clippings/
├── Queue/               Async human→agent interface
│   ├── _template.md     Task template
│   ├── .archive/        Completed tasks
│   └── routines/        Recurring routine prompts (source of truth)
├── Generated/           Autonomous outputs (organized by routine)
│   ├── relatorios/
│   ├── connections/
│   ├── daily-briefs/
│   ├── wiki-lint/
│   ├── meta-coaching/
│   └── ingest-reports/
└── wiki/                Knowledge base
```

## Principles

- Prompts live in Obsidian, not in settings — editable like any note
- Wikilinks integrate routines with vault (sources, concepts, entities)
- Continuous improvement: edit prompt in Obsidian = next run picks up changes
- Token economy: bash > AI, batch > loop, append > rewrite
