---
title: Daily Brief — End of Day Context
type: routine
schedule: "Mon-Fri 11 PM"
last_improved: 2026-05-09
version: 1
tags: [routine, brief, daily, context]
---

> **Note:** User input and output can be in PT-BR. Prompts are English for portability.

# Daily Brief

Nightly vault summary. What changed, what connects, what to think about tomorrow.

---

## Execution

### 1. Collect today's changes

```bash
cd $VAULT_DIR
# Files modified today
find wiki/ Queue/ Generated/ -name "*.md" -newer /tmp/today_start -type f 2>/dev/null
# Git changes (if tracked)
git diff --name-only --diff-filter=ACMR HEAD~1 2>/dev/null | grep -E "^(wiki|Queue|Generated)/"
```

### 2. Read hot cache (last 30 lines)

```bash
tail -30 wiki/hot.md
```

### 3. Scan pending Queue/

```bash
find Queue/ -maxdepth 1 -name "*.md" -not -name "_template.md" | wc -l
```

### 4. Generate brief

Output: `Generated/daily-briefs/daily-brief-YYYY-MM-DD.md`

```markdown
---
title: "Daily Brief — YYYY-MM-DD"
type: brief
generated_by: claude-code
created: YYYY-MM-DD
---

# Brief — YYYY-MM-DD (day of week)

## What changed today
- [list of files created/modified with 1 line of context]

## Queue status
- Pending: N tasks
- Next routine: [name] in [when]

## 3 suggested connections
Based on today's changes vs existing vault:
1. [recent source/concept] ↔ [older source/concept] — why
2. ...
3. ...

## 1 question to think about tomorrow
[Provocative question based on emerging pattern in vault]

## Active context
- **Study:** [current topic, next deadline]
- **AI Agents:** [experiment in progress]
```

### 5. Hot cache (DO NOT update)

Daily brief does NOT write to hot cache — it's ephemeral, for next-day consumption only. Accumulating briefs in hot would pollute it.

Keep only in `Generated/daily-briefs/daily-brief-YYYY-MM-DD.md`.

---

## Constraints

- Max 30 lines total in brief
- If nothing changed today → 5-line brief: "Nothing changed. Queue: N pending. Next routine: X."
- Connections: non-obvious only. If can't find 3, report however many found
- Question: genuine, not generic. Based on real vault data
- Mon-Fri only — weekends have maintenance routines, no brief needed

---

## Cleanup

Briefs >7 days old can be deleted automatically by wiki-lint (step 5 Queue cleanup).

---

## Changelog

- v1 (2026-05-09): created. Inspired by CyrilXBT Daily Context Generator + DamiDefi daily ritual.
