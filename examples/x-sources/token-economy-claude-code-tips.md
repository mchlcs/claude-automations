---
title: "7 Claude Code mistakes that waste tokens — @simonw"
type: source
source_url: https://x.com/simonw/status/1910000000000000001
author: "@simonw"
published: 2026-04-14
ingested: 2026-05-09
tags: [token-economy, claude-code, cost-optimization, bash, prompting]
---

# 7 Claude Code mistakes that waste tokens

Running Claude Code for a week taught me that most token waste comes from 6 patterns. Here's what changed after I fixed them.

## The 7 mistakes

### 1. AI where bash works

Asking Claude to list files, count lines, grep for strings. Every question = API call + context. `find`, `grep`, `wc` cost 0 tokens.

Rule: if a bash one-liner solves it, write the bash one-liner.

### 2. Asking Claude to read files it doesn't need

"Review my project" → Claude reads 40 files. "Review src/auth.ts" → 1 file.  
Context = RAM. Don't fill it with irrelevant data.

### 3. Loop over AI instead of batch

Bad: ask Claude to process 10 articles one at a time (10 API calls + growing context).  
Good: batch all 10 into one prompt (1 call, clean context).

### 4. Rewriting instead of appending

"Update the report" causes Claude to read the old report and rewrite it.  
"Append new findings to the report" = append-only = no read = fewer tokens.

### 5. No context compaction

Default: context grows until it hits limit, then truncates badly.  
Fix: `/compact` proactively at 70% context. Or configure auto-compact in hooks.

### 6. Long CLAUDE.md with examples

Every session loads CLAUDE.md into context. Examples bloat it.  
Rule: rules + references only. No sample outputs. No repeated explanations.

### 7. Asking AI for what memory already has

"What was the structure we decided on?" = unnecessary API call if it's in MEMORY.md.  
Let memory answer pattern questions. Use AI for novel reasoning only.

## Numbers (rough)

With these fixes: ~60% token reduction on typical dev sessions. Cost dropped from $4/day to $1.50.

## Related concepts

- [[03-RESOURCES/concepts/token-economy]]
- [[03-RESOURCES/concepts/context-management]]
- [[04-SYSTEM/wiki/principles]]
