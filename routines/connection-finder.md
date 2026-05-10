---
title: Connection Finder — Non-Obvious Links
type: routine
schedule: "Sunday 6 AM (after wiki-lint at 4 AM)"
last_improved: 2026-05-09
version: 1
tags: [routine, connections, cross-link, compounding]
---

> **Note:** User input and output can be in PT-BR. Prompts are English for portability.

# Connection Finder

Scan sources from last 7 days vs entire vault. Surface connections humans wouldn't see.

---

## Execution

### 1. Collect recent sources

```bash
cd $VAULT_DIR
grep -rl "^ingested:" wiki/sources/ | while read f; do
  date=$(grep "^ingested:" "$f" | head -1 | sed 's/ingested: *//');
  if [[ "$date" > "$(date -d '7 days ago' -I 2>/dev/null || date -v-7d -I)" ]]; then
    echo "$f"
  fi
done
```

0 sources → stop.

### 2. Collect older vault content

```bash
find wiki/sources/ -name "*.md" -not -newer /tmp/recent_cutoff | shuf | head -50
find wiki/concepts/ -name "*.md" | head -30
```

Random sample of older sources + all concepts = connection pool.

### 3. Extract themes from each recent source

Read frontmatter (tags) + central thesis of each recent source.

### 4. Match against older vault

For each recent source, find 2-3 older sources with:
- Overlapping tags
- Complementary or contradictory thesis
- Same author/entity referenced

### 5. Classify connections

Types:

| Type | Criteria |
|------|----------|
| **Cross-domain** | Different areas, shared insight |
| **Contradiction** | Same claim, opposite conclusions |
| **Pattern 3+** | 3+ sources converge on same point |
| **Q&A** | Older source asks question, recent one answers |
| **Evolution** | Same idea, updated version |

Only **non-obvious** connections qualify. Same tags + similar thesis → obvious → skip.

### 6. Generate output

Output: `Generated/connections/connections-YYYY-MM-DD.md`

```markdown
---
title: "Weekly Connections — YYYY-MM-DD"
type: report
connections_found: N
sources_scanned: N
generated_by: claude-code
created: YYYY-MM-DD
---

# Non-Obvious Connections of the Week

## Cross-domain
[[source-A]] ↔ [[source-B]]
**Connection:** reason
**Action:** wikilink to create / concept to consolidate

## Contradictions
...

## Patterns 3+
...

## Suggestions
- Create [[wiki/concepts/X]] — 3+ sources converge
- Consolidate [[source-A]] + [[source-B]] — complementary thesis
- Investigate contradiction X vs Y
```

### 7. Update hot cache

Append to wiki/hot.md:
```
## Connections YYYY-MM-DD | N connections found
**Top:** [most valuable connection]
→ [[Generated/connections/connections-YYYY-MM-DD]]
```

### 8. Apply wikilinks (optional)

If connection has high confidence (pattern 3+ or Q&A):
- Add bidirectional wikilink in involved sources
- Section "## Relations" of each source

If medium confidence: only suggest in report. Do not edit sources.

---

## Constraints

- Max 10 connections per report (quality > quantity)
- Do not invent forced connections — if <3 real connections, report <3
- Do not modify sources without high confidence
- Random sampling of older vault avoids recency bias

---

## Changelog

- v1 (2026-05-09): created. Inspired by CyrilXBT Connection Finder + DamiDefi JARVIS.
