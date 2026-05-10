---
title: Post-Ingest Weekly Report
type: routine
schedule: "Friday 7 PM (2h after ingest)"
last_improved: 2026-05-09
version: 1
tags: [routine, report, analysis, post-ingest]
---

> **Note:** User input and output can be in PT-BR. Prompts are English for portability.

# Post-Ingest Weekly Report

Runs 2h after weekly ingest. Analyzes the week's sources, generates analytical report.

---

## Execution

Equivalent to running `/article-report 7d` automatically.

### 1. Scan sources ingested in the last 7 days

```bash
cd $VAULT_DIR
grep -rl "^ingested:" wiki/sources/ | while read f; do
  date=$(grep "^ingested:" "$f" | head -1 | sed 's/ingested: *//');
  if [[ "$date" > "$(date -d '7 days ago' -I 2>/dev/null || date -v-7d -I)" ]]; then
    echo "$date|$f"
  fi
done | sort -r
```

0 sources → report "nothing ingested this week" and stop.

### 2. Read and group

For each source (parallel if >5):
- Read complete file
- Extract: central thesis, key insights, wikilinks

Group by thematic clusters (tags + semantic content).

### 3. Per-cluster analysis

For each cluster:
- **Summary** (2-3 sentences)
- **Convergences** — 2+ sources agree
- **Contradictions** — sources diverge (with citation)
- **Actionable insights** — applicable to vault/workflow now
- **Gaps** — what remains to explore

### 4. Cross-connections

Non-obvious connections between clusters:
```
[Source A] ↔ [Source B] — connection: reason
```

### 5. Vault impact

Compare insights with CLAUDE.md + wiki/hot.md:

| Improvement | Priority | Effort | Source |
|-------------|----------|--------|--------|

### 6. Generate report

Output: `Generated/relatorios/article-report-YYYY-MM-DD.md`

Frontmatter: title, type:report, period, sources_analyzed, clusters, generated_by, created.

Max 300 lines. Valid wikilinks. Terse mode.

### 7. Update hot cache

Append to wiki/hot.md with clusters, top insight, top action.

---

## Constraints

- Read-only on wiki/sources/
- If >30 sources: top 20 by relevance + appendix
- Verify wikilinks exist before using
- If 0 sources in the week: skip (do not generate empty report)

---

## Changelog

- v1 (2026-05-09): created. Based on /article-report tested manually.
