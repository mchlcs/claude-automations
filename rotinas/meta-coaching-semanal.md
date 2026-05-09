---
title: Meta-Coaching Claude Code — Self-Audit Semanal
type: rotina
schedule: "sábado 1h AM"
last_improved: 2026-05-09
version: 2
tags: [rotina, meta, coaching, token-economy]
---

# Meta-Coaching Claude Code

Auditar uso de Claude Code da semana. Surfar mudanças de comportamento de alto impacto.

**Referências vault:**
- [[wiki/concepts/karpathy-four-principles]] — think before acting
- [[wiki/sources/how-to-cut-claude-code-costs-by-3x-using-karpathy-s-context-engineering]] — context engineering = cost engineering
- [[wiki/sources/best-practices-for-using-claude-opus-4-7-with-claude-code]] — front-load context, less turns

---

## Input

Aggregated session logs from past 7 days.
Treat single-session anomalies as noise. Look for repeated patterns.

---

## Task

### 1. Cluster tool calls by intent

Clusters: search / read / edit / bash / agent / mcp.
For each: count, est. tokens consumed, % of total session time.

### 2. Identify 3-5 waste patterns

Ranked by (frequency × estimated waste). For each:
- Pattern name (one line)
- Evidence: 2-3 concrete examples with file/command
- Root cause hypothesis
- Estimated cost (tokens or wall-clock)

### 3. Surface surprises

Patterns I likely don't notice. Do NOT fabricate to hit quota.

### 4. Top 2 fixes

For top 2 patterns only, propose ONE concrete fix each:
- Specific behavior change (not vague — name tool, situation, threshold)
- How to verify next week (metric)
- Cost if I do nothing

---

## Constraints

- Outcome-weighted, not count-weighted
- No vague advice — every fix names specific tool, situation, threshold
- If logs insufficient for a claim, say so
- Compare against prior baseline if previous report exists in Generated/

---

## Output

Save to `Generated/meta-coaching-YYYY-MM-DD.md`

Markdown. Tables for clusters. Bulleted patterns. Numbered fixes.

Also check if any pattern matches existing [[wiki/concepts/]] or lessons in `tasks/lessons.md` — cross-reference if so.

---

## Changelog

- v2 (2026-05-09): migrado pra Queue/rotinas/. Adicionado wikilinks, output pra Generated/, cross-ref com vault.
- v1: original em scheduled-tasks SKILL.md
