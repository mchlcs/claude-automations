---
title: Weekly Wiki Health Check
type: routine
schedule: "Sunday 4 AM"
last_improved: 2026-05-09
version: 2
tags: [routine, lint, wiki, health-check]
---

> **Note:** User input and output can be in PT-BR. Prompts are English for portability.

# Weekly Wiki Health Check

Terse mode. Autonomous.

---

## 1. Lint wiki vault

Scan:
- Orphan pages (no inbound links)
- Dead wikilinks (target missing)
- Stale claims (entities/concepts not updated >30d, referenced as "current")
- Frontmatter gaps (missing title/type/created/updated/tags)
- Empty sections (## headings with no content)
- Duplicate concepts (semantic overlap, e.g. "agent-skills" + "claude-skills")

Report counts + top 10 examples each category.

---

## 2. Manifest dedup sweep

Read `ingest/.manifest.json`. Detect:
- Same hash across multiple entries → flag as dup
- Same source different filename → flag as dup
- Entries marked "stub-missing-raw-file" → flag as orphan

Report dups + suggest merges.

---

## 3. Lessons consolidation

Read `tasks/lessons.md`. If >30 entries:
- Group similar (same domain/pattern)
- Merge redundant
- Keep most recent N per cluster
- Cap at 30 total

Report: entries before/after.

---

## 4. Hot cache trim

Read `wiki/hot.md`. If >300 lines:
- Archive entries >14d to `wiki/log.md`
- Keep last 14d in hot

Report: lines before/after.

---

## 5. Queue cleanup

Scan `Queue/.archive/`. If >20 entries >30d old:
- Delete processed tasks older than 30 days
- Report count deleted

---

## 6. Report

Output: `Generated/wiki-lint/wiki-lint-YYYY-MM-DD.md`

```
- Lint: orphans=N, dead=N, stale=N, frontmatter-gaps=N, dup-concepts=N
- Manifest dups: N (suggested merges)
- Lessons: N → N (clusters: M)
- Hot cache: N lines → N lines (archived M to log)
- Queue archive: N cleaned
- Action items: N (priority high)
```

No automatic fixes. Report + suggestions only.

---

## Changelog

- v2 (2026-05-09): migrated to Queue/routines/. Added Queue cleanup step. Output to Generated/.
- v1: original in scheduled-tasks SKILL.md
