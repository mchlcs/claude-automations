---
title: Meta-Coaching Claude Code — Weekly Self-Audit
type: routine
schedule: "Saturday 1 AM"
last_improved: 2026-05-09
version: 2
tags: [routine, meta, coaching, token-economy]
---

> **Note:** User input and output can be in PT-BR. Prompts are English for portability.

# Meta-Coaching Claude Code

Audit Claude Code usage from the past week. Surface high-leverage behavior changes.

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

Patterns I likely don't notice. Do NOT fabricate to hit a quota.

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

Save to `Generated/meta-coaching/meta-coaching-YYYY-MM-DD.md`

Markdown. Tables for clusters. Bulleted patterns. Numbered fixes.

Also check if any pattern matches existing vault concepts or lessons — cross-reference if so.

---

## Changelog

- v2 (2026-05-09): migrated to Queue/routines/. Added output to Generated/, cross-ref with vault.
- v1: original in scheduled-tasks SKILL.md
