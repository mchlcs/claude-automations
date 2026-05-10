Generate an analytical report of ingested articles in the vault.

> **Note:** User input and output can be in PT-BR. Prompts are English for portability.

## Parameters

- **Period**: $ARGUMENTS (default: "7d" = last 7 days). Accepts: "7d", "30d", "all", or ISO date "2026-05-01".
- **Output format**: `Generated/relatorios/article-report-YYYY-MM-DD.md`

---

## Execution

### 1. Scan sources

```bash
grep -rl "^ingested:" wiki/sources/ | head -200
```

For each file: read frontmatter (`ingested`, `tags`, `title`, `author`).
Filter by requested period. If no results → report "0 articles in period" and stop.

### 2. Read content

For each source in period (parallel if >5):
- Read complete file
- Extract: central thesis, key insights, relationships (wikilinks)

### 3. Group by theme

Thematic clusters based on tags + semantic content. Example:
- AI Agents / Patterns
- Claude / Skills / Prompts
- Memory / PKM / Second Brain
- Research Papers
- Other

### 4. Per-cluster analysis

For each cluster:
- **Summary** (2-3 sentences): what this group of articles says collectively
- **Convergences**: where 2+ articles agree or reinforce
- **Contradictions**: where articles diverge (with citation)
- **Actionable insights**: what can be applied to vault/workflow NOW
- **Gaps**: what remains unexplored in this theme

### 5. Cross-connections

Identify non-obvious connections between different clusters. Format:
```
[Source A] ↔ [Source B] — connection: reason
```

### 6. Vault impact analysis

Compare insights with current vault setup (read CLAUDE.md + wiki/hot.md):
- **Suggested improvements**: concrete changes to workflow, structure, or CLAUDE.md
- **Priority**: high/medium/low
- **Effort**: quick-win / dedicated session / project

### 7. Generate report

Save to `Generated/relatorios/article-report-YYYY-MM-DD.md` with frontmatter:

```yaml
---
title: "Article Report — YYYY-MM-DD"
type: report
period: "YYYY-MM-DD to YYYY-MM-DD"
sources_analyzed: N
clusters: N
generated_by: claude-code
created: YYYY-MM-DD
---
```

### 8. Update hot cache

Append to `wiki/hot.md` with clusters, top insight, top action.

---

## Constraints

- Read-only on wiki/sources/ — do not modify sources
- Concise report — max 300 lines
- Valid wikilinks (verify targets exist)
- Terse mode in report (no filler, straight to the point)
- If >30 sources: sample top 20 by relevance + list rest in appendix
