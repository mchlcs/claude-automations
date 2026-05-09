---
title: Relatório Pós-Ingest Semanal
type: rotina
schedule: "sexta 19h (2h após ingest)"
last_improved: 2026-05-09
version: 1
tags: [rotina, relatório, análise, pós-ingest]
---

# Relatório Pós-Ingest Semanal

Roda 2h após ingest semanal. Analisa sources da semana, gera relatório analítico.

**Referências vault:**
- [[wiki/concepts/knowledge-compounding]] — cada relatório compound com vault
- [[wiki/concepts/karpathy-four-principles]] — verify before done
- [[Generated/relatorio-artigos-2026-05-09]] — exemplo de output

---

## Execução

Equivale a rodar `/relatorio-artigos 7d` automaticamente.

### 1. Scan sources ingestadas nos últimos 7 dias

```bash
cd ~/Obsidian/vault-michel
grep -rl "^ingested:" wiki/sources/ | while read f; do
  date=$(grep "^ingested:" "$f" | head -1 | sed 's/ingested: *//');
  if [[ "$date" > "$(date -d '7 days ago' -I 2>/dev/null || date -v-7d -I)" ]]; then
    echo "$date|$f"
  fi
done | sort -r
```

0 sources → reportar "nada ingestado essa semana" e parar.

### 2. Ler e agrupar

Para cada source (paralelo se >5):
- Ler arquivo completo
- Extrair: tese central, key insights, wikilinks

Agrupar por clusters temáticos (tags + conteúdo semântico).

### 3. Análise por cluster

Para cada cluster:
- **Resumo** (2-3 frases)
- **Convergências** — 2+ sources concordam
- **Contradições** — sources divergem (com citação)
- **Insights acionáveis** — aplicável ao vault/workflow agora
- **Gaps** — o que falta explorar

### 4. Cross-connections

Conexões não-óbvias entre clusters:
```
[Fonte A] ↔ [Fonte B] — conexão: razão
```

### 5. Vault impact

Comparar insights com CLAUDE.md + wiki/hot.md:

| Melhoria | Prioridade | Esforço | Fonte |
|----------|-----------|---------|-------|

### 6. Gerar relatório

Output: `Generated/relatorio-artigos-YYYY-MM-DD.md`

Frontmatter: title, type:report, period, sources_analyzed, clusters, generated_by, created.

Max 300 linhas. Wikilinks válidos. Caveman mode.

### 7. Atualizar hot cache

Append em wiki/hot.md com clusters, top insight, top action.

---

## Constraints

- Read-only em wiki/sources/
- Se >30 sources: top 20 por relevância + apêndice
- Verificar wikilinks existem antes de usar
- Se 0 sources na semana: skip (não gerar relatório vazio)

---

## Changelog

- v1 (2026-05-09): criado. Baseado em /relatorio-artigos testado manualmente.
