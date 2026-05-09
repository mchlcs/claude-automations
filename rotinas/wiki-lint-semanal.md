---
title: Wiki Health Check Semanal
type: rotina
schedule: "domingo 4h AM"
last_improved: 2026-05-09
version: 2
tags: [rotina, lint, wiki, health-check]
---

# Wiki Health Check Semanal

Caveman full. Autônomo.

**Referências vault:**
- [[wiki/wiki-index]] — home do vault
- [[wiki/concepts/karpathy-four-principles]] — surgical changes, verify before done
- [[wiki/concepts/knowledge-compounding]] — lint preserva integridade do compound

---

## 1. Lint wiki vault

Use `claude-obsidian:wiki-lint` skill. Scan:
- Orphan pages (no inbound links)
- Dead wikilinks (target missing)
- Stale claims (entities/concepts not updated >30d, referenced as "current")
- Frontmatter gaps (missing title/type/created/updated/tags)
- Empty sections (## headings with no content)
- Duplicate concepts (semantic overlap, e.g. "agent-skills" + "claude-skills")

Report counts + top 10 examples each category.

---

## 2. Manifest dedup sweep

Read `.raw/.manifest.json`. Detect:
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

Output: `Generated/wiki-lint-YYYY-MM-DD.md`

```
- Lint: orphans=N, dead=N, stale=N, frontmatter-gaps=N, dup-concepts=N
- Manifest dups: N (suggested merges)
- Lessons: N → N (clusters: M)
- Hot cache: N lines → N lines (archived M to log)
- Queue archive: N cleaned
- Action items: N (priority high)
```

Sem fixes automáticos. Só relatório + sugestões.

---

## Changelog

- v2 (2026-05-09): migrado pra Queue/rotinas/. Adicionado Queue cleanup step. Wikilinks. Output pra Generated/.
- v1: original em scheduled-tasks SKILL.md
