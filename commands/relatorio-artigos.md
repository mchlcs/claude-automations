Gere um relatório analítico dos artigos ingestados no vault.

## Parâmetros

- **Período**: $ARGUMENTS (default: "7d" = últimos 7 dias). Aceita: "7d", "30d", "all", ou data ISO "2026-05-01".
- **Formato saída**: `Generated/relatorio-artigos-YYYY-MM-DD.md`

---

## Execução

### 1. Scan sources

```bash
# Listar fontes ingestadas no período
grep -rl "^ingested:" wiki/sources/ | head -200
```

Para cada arquivo: ler frontmatter (`ingested`, `tags`, `title`, `author`).
Filtrar por período solicitado. Se nenhum resultado → reportar "0 artigos no período" e parar.

### 2. Ler conteúdo

Para cada source no período (paralelo se >5):
- Ler arquivo completo
- Extrair: tese central, key insights, relações (wikilinks)

### 3. Agrupar por tema

Clusters temáticos baseados em tags + conteúdo semântico. Exemplo:
- AI Agents / Patterns
- Claude / Skills / Prompts
- Memory / PKM / Second Brain
- Research Papers
- Outros

### 4. Análise por cluster

Para cada cluster:
- **Resumo** (2-3 frases): o que esse grupo de artigos diz coletivamente
- **Convergências**: onde 2+ artigos concordam ou reforçam
- **Contradições**: onde artigos divergem (com citação)
- **Insights acionáveis**: o que pode ser aplicado ao vault/workflow AGORA
- **Gaps**: o que falta ser explorado nesse tema

### 5. Cross-connections

Identificar conexões não-óbvias entre clusters diferentes. Formato:
```
[Fonte A] ↔ [Fonte B] — conexão: razão
```

### 6. Vault impact analysis

Comparar insights com setup atual do vault (ler CLAUDE.md + wiki/hot.md):
- **Melhorias sugeridas**: mudanças concretas no workflow, estrutura, ou CLAUDE.md
- **Prioridade**: alta/média/baixa
- **Esforço**: quick-win / sessão dedicada / projeto

### 7. Gerar relatório

Salvar em `Generated/relatorio-artigos-YYYY-MM-DD.md` com frontmatter:

```yaml
---
title: "Relatório de Artigos — YYYY-MM-DD"
type: report
period: "YYYY-MM-DD a YYYY-MM-DD"
sources_analyzed: N
clusters: N
generated_by: claude-code
created: YYYY-MM-DD
---
```

Estrutura do body:

```markdown
# Relatório de Artigos Ingestados

## Resumo executivo
(3-5 bullets: volume, temas dominantes, insight mais valioso, ação principal)

## Clusters temáticos
### [Cluster 1]
**Sources:** [[link1]], [[link2]]
**Resumo:** ...
**Convergências:** ...
**Contradições:** ...
**Insights acionáveis:** ...

### [Cluster N]
...

## Cross-connections
...

## Vault impact
| Melhoria | Prioridade | Esforço | Fonte |
|----------|-----------|---------|-------|
| ... | alta | quick-win | [[source]] |

## Métricas
- Sources analisados: N
- Clusters: N
- Conexões cross-cluster: N
- Melhorias sugeridas: N
```

### 8. Atualizar hot cache

Append em `wiki/hot.md`:

```markdown
## Report YYYY-MM-DD | Relatório de Artigos (N sources)

**Clusters:** [lista]
**Top insight:** [mais valioso]
**Top action:** [melhoria mais impactante]
→ [[Generated/relatorio-artigos-YYYY-MM-DD]]
```

---

## Constraints

- Read-only em wiki/sources/ — não modificar fontes
- Relatório conciso — max 300 linhas
- Wikilinks válidos (verificar que targets existem)
- Caveman mode no relatório (sem filler, direto ao ponto)
- Se >30 sources: amostrar top 20 por relevância + listar resto em apêndice
