---
title: Ingest Diário — .raw/articles + Clippings
type: rotina
schedule: "sexta 17h"
last_improved: 2026-05-09
version: 2
tags: [rotina, ingest, token-economy]
---

# Ingest Diário — Token-Economic

Scheduled run. User ausente. Decidir autônomo. Caveman full. RTK active.

**Princípio**: Bash > AI. Batch > loop. Append > rewrite. Link existing > create stub.

**Referências vault:**
- [[wiki/concepts/karpathy-four-principles]] — think before acting, simplicity first
- [[wiki/concepts/llm-wiki-pattern]] — padrão wiki mantida por LLM
- [[wiki/concepts/knowledge-compounding]] — cada ingest compound com vault existente

---

## 0. Bash scan (no AI)

```bash
cd ~/Obsidian/vault-michel
ls .raw/articles/*.md 2>/dev/null > /tmp/art.txt
ls clippings/*.md 2>/dev/null > /tmp/clip.txt
ART_TOTAL=$(wc -l < /tmp/art.txt); CLIP_TOTAL=$(wc -l < /tmp/clip.txt)

# Dedup por basename (NÃO md5 — manifest registra por path)
ART_NEW=$(while read f; do bn=$(basename "$f"); grep -q "\"$bn\"" .raw/.manifest.json || echo "$f"; done < /tmp/art.txt)
CLIP_NEW=$(while read f; do bn=$(basename "$f"); grep -q "\"$bn\"" .raw/.manifest.json || echo "$f"; done < /tmp/clip.txt)
```

Empty new lists → skip to step 7. **Cost: 0 tokens.**

---

## 1. Batch metadata extract (bash, no AI)

Per arquivo novo:

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

**Filtros aplicados** (tokens mortos removidos):
- `![alt](image.png)` → removido (markdown images)
- `data:image/...;base64,...` → removido (inline base64)
- `<svg>...</svg>` → removido (SVG inline)
- `<img>` tags → removido
- `<style>...</style>` → removido (CSS blocks)
- Trunca em 3000 chars APÓS filtragem (mais conteúdo útil)

Classify heurístico via filename pattern:
- `claude|cowork|skill|mcp` → ai-agents
- `agent|llm|rag|prompt` → ai-agents
- `concurso|portuguê|lógica|redação` → concurso
- `fiap|fase-|java|fintech` → fiap
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

**Prefer bash. AI call only se ambíguo.** Cost: ~0–100 tokens.

---

## 3. Create source pages (template-driven)

Per file novo, single Write:

```markdown
---
title: <Title from H1 or filename>
type: source
source: .raw/articles/<filename>
created: YYYY-MM-DD
ingested: <today>
tags: [<class>]
---

## Tese central
<1 frase: argumento principal do artigo>

## Key insights
- <insight 1>
- <insight 2>
- <insight 3>

## Links
- [[wiki/concepts/<existing-kw>]]
- [[wiki/entities/<existing-entity>]]
```

**Regras**: NÃO criar stubs. Ler primeiros 3000 chars (suficiente pra tese + insights). Slug = lowercase-kebab basename.

---

## 4. Clippings: same flow + delete

Após Write success: `rm clippings/<filename>`. Manifest entry: `"source_type": "social-media"`.

---

## 5. Manifest append (bash atomic)

```bash
for f in $PROCESSED; do
  bn=$(basename "$f")
  jq ".sources[\"$f\"] = {hash:\"$(md5 -q $f)\", ingested_at:\"$(date -I)\", pages_created:[\"wiki/sources/$slug.md\"]}" .raw/.manifest.json > /tmp/manifest.tmp && mv /tmp/manifest.tmp .raw/.manifest.json
done
```

---

## 6. Hot cache append (NÃO rewrite)

```bash
{
  echo ""
  echo "## Daily Ingest $(date -I)"
  echo "**Articles:** $ART_NEW_COUNT novos / $ART_TOTAL total"
  echo "**Clippings:** $CLIP_NEW_COUNT processados, deletados"
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

## 8. Daily Report

Gerar relatório em `Generated/ingest-report-YYYY-MM-DD.md`.

Input: hot cache append + snippets. NÃO re-ler arquivos.

Seções: Resumo Executivo, Fontes do Dia, Clusters, Contradições, Token Economy, Ações Sugeridas.

Max ~300 tokens por seção. Total report ~2K tokens max.

---

## Cost budget

| Step | Tokens | Notes |
|------|--------|-------|
| 0-1 Scan+Classify | 0 | bash |
| 2 Concept lookup | 0–100 | bash first |
| 3-4 Source pages | ~150/file | tese + 3 insights + links |
| 5-6 Manifest+Hot | 0 | bash append |
| 7 Report terse | ~30 | — |
| 8 Report analysis | ~500-1000 | 1 AI call |
| **Total** | **~150N + 1130** | N = files novos |

---

## Guardrails

- Single Read per file MAX (3000 chars)
- Zero stubs — linkar concepts/entities existentes
- Cross-link pass: skip (deferred to weekly lint)
- Errors: log + continue batch
- Se >20 arquivos: dispatch wiki-ingest agents paralelo

---

## Changelog

- v3 (2026-05-09): leitura profunda (3000 chars), source pages com tese+insights, schedule sexta 17h.
- v2 (2026-05-09): migrado pra Queue/rotinas/. Adicionado wikilinks, references, changelog.
- v1: original em scheduled-tasks SKILL.md
