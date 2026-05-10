---
title: Queue Processor — One-Shot Tasks
type: routine
schedule: "every 4h (8 AM, 12 PM, 4 PM, 8 PM)"
last_improved: 2026-05-09
version: 1
tags: [routine, queue, automation]
---

> **Note:** User input and output can be in PT-BR. Prompts are English for portability.

# Queue Processor

Scan Queue/ for pending one-shot tasks. Process and archive.

---

## 1. Scan

```bash
cd $VAULT_DIR
find Queue/ -maxdepth 1 -name "*.md" -not -name "_template.md" | sort
```

0 tasks → report "Queue empty" and stop.

---

## 2. Prioritize

Read frontmatter of each task. Sort: high → medium → low.

---

## 3. Process

For each task (sequential by priority):

1. Read complete instructions
2. Resolve referenced [[wikilinks]] — read sources/concepts if needed for context
3. Execute task as described
4. Save output to specified location (default: `Generated/`)
5. Verify valid wikilinks in output
6. Update frontmatter: `status: completed`, `completed: YYYY-MM-DD`
7. Move task to `Queue/.archive/`

---

## 4. Report

```
✓ Queue: N tasks processed
✓ High: N | Medium: N | Low: N
✓ Outputs: [list of generated files]
✓ Errors: N
```

Append summary to `wiki/hot.md`.

---

## Guardrails

- Do NOT process files in `Queue/routines/` (those are recurring templates)
- Do NOT process `_template.md`
- If task references non-existent source → skip with warning
- Max 10 tasks per execution (backpressure)
- If task is ambiguous or dangerous → skip with `status: manual-review`
- Terse mode in outputs

---

## Changelog

- v1 (2026-05-09): created.
