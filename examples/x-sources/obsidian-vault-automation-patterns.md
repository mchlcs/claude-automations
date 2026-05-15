---
title: "Obsidian + Claude Code: vault automation patterns that actually work — @kepano"
type: source
source_url: https://x.com/kepano/status/1912500000000000001
author: "@kepano"
published: 2026-04-22
ingested: 2026-05-09
tags: [obsidian, vault-automation, claude-code, second-brain, wikilinks, scheduled-tasks]
---

# Obsidian + Claude Code: vault automation patterns that actually work

Spent 3 months connecting Claude Code to my Obsidian vault. What works vs what seems like it should work but doesn't.

## What works

### 1. Prompts as vault notes

Store your Claude prompts inside Obsidian as regular `.md` files. Scheduled tasks read the file at runtime.

Benefits:
- Edit prompt in Obsidian → next run picks up changes automatically
- No settings files to touch
- Prompt is a searchable, linkable vault note
- Wikilinks let you connect prompts to sources and concepts

### 2. Wikilinks as agent memory

Your agent knows about `[[concepts/distributed-systems]]` because that page exists.  
Wikilinks in prompts = structured context injection without API overhead.

### 3. Append-only outputs

Agent writes to `Generated/daily-briefs/2026-05-09.md`. Never overwrites.  
Next day: new file. No read-modify-write cycle. Cheap, auditable, versionable.

### 4. Queue as async human-agent interface

Drop a `.md` task file in `Queue/`. Agent processes it on next scheduled run, moves to `Queue/.archive/`.  
No real-time back-and-forth required. Works while you're offline.

### 5. Weekly synthesis over daily capture

Don't ask Claude to process every article as it comes in. Batch weekly.  
Fewer API calls. Better cross-article synthesis. Agent has full week's context in one pass.

## What doesn't work (as well as expected)

- Real-time Obsidian plugin integration — latency unpredictable
- Asking Claude to "organize my vault" without a precise scope — too broad, too expensive
- Per-note automation triggers — hook-based approaches are more reliable than file watchers

## Numbers

My vault: 800+ notes, 120+ ingested sources, 7 routines.  
Weekly token cost for all automation: ~$2.50. Manual equivalent: 4-6 hours.

## Related concepts

- [[03-RESOURCES/concepts/second-brain]]
- [[04-SYSTEM/wiki/principles]]
- [[03-RESOURCES/concepts/wikilinks-as-context]]
