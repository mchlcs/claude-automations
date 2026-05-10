---
title: Weekly Ingest — Articles + Clippings
type: routine
schedule: "Friday 5 PM"
last_improved: 2026-05-09
version: 3
tags: [routine, ingest, token-economy]
---

> **Note:** User input and output can be in PT-BR. Prompts are English for portability.

# Weekly Ingest — Token-Economic

Scheduled run. User absent. Decide autonomously. Terse mode active.

**Principle**: Bash > AI. Batch > loop. Append > rewrite. Link existing > create stub.

---

## 0. Bash scan (no AI)

```bash
cd $VAULT_DIR
ls ingest/articles/*.md 2>/dev/null > /tmp/art.txt
ls ingest/clippings/*.md 2>/dev/null > /tmp/clip.txt
ART_TOTAL=$(wc -l < /tmp/art.txt); CLIP_TOTAL=$(wc -l < /tmp/clip.txt)

# Dedup by basename (NOT md5 — manifest tracks by path)
ART_NEW=$(while read f; do bn=$(basename "$f"); grep -q "\"$bn\"" ingest/.manifest.json || echo "$f"; done < /tmp/art.txt)
CLIP_NEW=$(while read f; do bn=$(basename "$f"); grep -q "\"$bn\"" ingest/.manifest.json || echo "$f"; done < /tmp/clip.txt)
```

Empty new lists → skip to step 7. **Cost: 0 tokens.**

---

## 1. Batch metadata extract (bash, no AI)

Per new file:

```bash
for f in $ART_NEW $CLIP_NEW; do
  bn=$(basename "$f" .md)
  slug=$(echo "$bn" | tr '[:upper:] ' '[:lower:]-' | tr -cd 'a-z0-9-')
  # Extract text, strip non-structured data
  head -c 5000 "$f" | \
    sed -E 's/!\[([^]]*)\]\([^)]+\)//g' | \
    sed -E 's/data:[a-z]+\/[a-z]+;base64,[A-Za-z0-9+/=]+//g' | \
    sed -E 's/<svg[^>]*>.*<\/svg>//g' | \
    sed -E 's/<img[^>]*>//g' | \
    sed -E 's/<style[^>]*>.*<\/style>//g' | \
    head -c 3000 > /tmp/snippet_$slug
done
```

**Filters applied** (dead tokens removed):
- `![alt](image.png)` — markdown images
- `data:image/...;base64,...` — inline base64
- `<svg>...</svg>` — inline SVG
- `<img>` tags
- `<style>...</style>` — CSS blocks
- Truncates at 3000 chars AFTER filtering (more useful content)

Heuristic classification via filename pattern:
- `claude|cowork|skill|mcp` → ai-agents
- `agent|llm|rag|prompt` → ai-agents
- else → articles

**Cost: 0 tokens.**

---

## 2. Batch concept lookup (bash-first)

```bash
ls wiki/concepts/ wiki/entities/ > /tmp/existing.txt
for kw in $KEYWORDS; do
  grep -qi "^${kw}.md$\|^${kw}-" /tmp/existing.txt && echo "$kw=exists" || echo "$kw=new"
done
```

**Prefer bash. AI call only if ambiguous.** Cost: ~0–100 tokens.

---

## 3. Create source pages (template-driven)

Per new file, single Write:

```markdown
---
title: <Title from H1 or filename>
type: source
source: ingest/articles/<filename>
created: YYYY-MM-DD
ingested: <today>
tags: [<class>]
---

## Central thesis
<1 sentence: main argument of the article>

## Key insights
- <insight 1>
- <insight 2>
- <insight 3>

## Links
- [[wiki/concepts/<existing-kw>]]
- [[wiki/entities/<existing-entity>]]
```

**Rules**: Do NOT create stubs. Read first 3000 chars (enough for thesis + insights). Slug = lowercase-kebab basename.

---

## 4. Clippings: same flow + delete

After Write success: `rm ingest/clippings/<filename>`. Manifest entry: `"source_type": "social-media"`.

---

## 5. Manifest append (bash atomic)

```bash
for f in $PROCESSED; do
  bn=$(basename "$f")
  jq ".sources[\"$f\"] = {hash:\"$(md5 -q $f)\", ingested_at:\"$(date -I)\", pages_created:[\"wiki/sources/$slug.md\"]}" ingest/.manifest.json > /tmp/manifest.tmp && mv /tmp/manifest.tmp ingest/.manifest.json
done
```

---

## 6. Hot cache append (NOT rewrite)

```bash
{
  echo ""
  echo "## Weekly Ingest $(date -I)"
  echo "**Articles:** $ART_NEW_COUNT new / $ART_TOTAL total"
  echo "**Clippings:** $CLIP_NEW_COUNT processed, deleted"
  echo "**Pages:** sources=$N (no stubs — only existing links)"
  for slug in $CREATED; do echo "- [[wiki/sources/$slug]]"; done
} >> wiki/hot.md
```

---

## 7. Report (ultra-terse)

```
✓ Scan: $ART_TOTAL articles, $CLIP_TOTAL clippings
✓ New: $ART_NEW_COUNT / $CLIP_NEW_COUNT (skipped: $DUP)
✓ Created: sources=$N, concepts=0 (linked existing only)
✓ Manifest: +$N entries
✓ Hot cache: appended
✓ Errors: $ERR_COUNT
```

---

## 8. Ingest Report

Generate report at `Generated/ingest-reports/ingest-report-YYYY-MM-DD.md`.

Input: hot cache append + snippets. Do NOT re-read files.

Sections: Executive Summary, Sources of the Day, Clusters, Contradictions, Token Economy, Suggested Actions.

Max ~300 tokens per section. Total report ~2K tokens max.

---

## Cost budget

| Step | Tokens | Notes |
|------|--------|-------|
| 0-1 Scan+Classify | 0 | bash |
| 2 Concept lookup | 0–100 | bash first |
| 3-4 Source pages | ~150/file | thesis + 3 insights + links |
| 5-6 Manifest+Hot | 0 | bash append |
| 7 Report terse | ~30 | — |
| 8 Report analysis | ~500-1000 | 1 AI call |
| **Total** | **~150N + 1130** | N = new files |

---

## Guardrails

- Single Read per file MAX (3000 chars)
- Zero stubs — link existing concepts/entities only
- Cross-link pass: skip (deferred to weekly lint)
- Errors: log + continue batch
- If >20 files: dispatch wiki-ingest agents in parallel

---

## Changelog

- v3 (2026-05-09): deep reading (3000 chars), source pages with thesis+insights, Friday 5 PM schedule.
- v2 (2026-05-09): migrated to Queue/routines/. Added wikilinks, references, changelog.
- v1: original in scheduled-tasks SKILL.md
