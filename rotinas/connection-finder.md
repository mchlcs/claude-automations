---
title: Connection Finder — Conexões Não-Óbvias
type: rotina
schedule: "domingo 6h (após wiki-lint das 4h)"
last_improved: 2026-05-09
version: 1
tags: [rotina, conexões, cross-link, compounding]
---

# Connection Finder

Scan sources dos últimos 7 dias vs vault inteiro. Surfar conexões que humano não veria.

**Referências vault:**
- [[wiki/concepts/knowledge-compounding]] — conexões são o mecanismo de compound
- [[wiki/concepts/second-brain]] — brain pensa quando conecta, não quando arquiva
- [[wiki/sources/obsidian-vault-writes-back-cyrilxbt]] — Connection Finder original (CyrilXBT)
- [[wiki/sources/jarvis-obsidian-second-brain-thinks-full-setup]] — conexões tipadas (DamiDefi)

---

## Execução

### 1. Coletar sources recentes

```bash
cd ~/Obsidian/vault-michel
grep -rl "^ingested:" wiki/sources/ | while read f; do
  date=$(grep "^ingested:" "$f" | head -1 | sed 's/ingested: *//');
  if [[ "$date" > "$(date -d '7 days ago' -I 2>/dev/null || date -v-7d -I)" ]]; then
    echo "$f"
  fi
done
```

0 sources → parar.

### 2. Coletar vault antigo

```bash
find wiki/sources/ -name "*.md" -not -newer /tmp/recent_cutoff | shuf | head -50
find wiki/concepts/ -name "*.md" | head -30
```

Amostra aleatória de sources antigas + todos concepts = pool de conexão.

### 3. Extrair temas de cada source recente

Ler frontmatter (tags) + tese central de cada source recente.

### 4. Match contra vault antigo

Para cada source recente, buscar 2-3 sources antigas com:
- Tags overlapping
- Tese complementar ou contraditória
- Mesmo autor/entidade referenciado

### 5. Classificar conexões

Tipos (baseado em DamiDefi JARVIS):

| Tipo | Critério |
|------|----------|
| **Cross-domain** | Áreas diferentes, insight compartilhado |
| **Contradição** | Mesma claim, conclusões opostas |
| **Padrão 3+** | 3+ sources convergem no mesmo ponto |
| **Pergunta-resposta** | Source antiga faz pergunta, recente responde |
| **Evolução** | Mesma ideia, versão atualizada |

Só conexões **não-óbvias** qualificam. Se tags iguais e tese similar → óbvia → skip.

### 6. Gerar output

Output: `Generated/connections-YYYY-MM-DD.md`

```markdown
---
title: "Conexões Semanais — YYYY-MM-DD"
type: report
connections_found: N
sources_scanned: N
generated_by: claude-code
created: YYYY-MM-DD
---

# Conexões Não-Óbvias da Semana

## Cross-domain
[[source-A]] ↔ [[source-B]]
**Conexão:** razão
**Ação:** wikilink a criar / concept a consolidar

## Contradições
...

## Padrões 3+
...

## Sugestões
- Criar [[wiki/concepts/X]] — 3+ sources convergem
- Consolidar [[source-A]] + [[source-B]] — tese complementar
- Investigar contradição X vs Y
```

### 7. Atualizar hot cache

Append em wiki/hot.md:
```
## Connections YYYY-MM-DD | N conexões encontradas
**Top:** [conexão mais valiosa]
→ [[Generated/connections-YYYY-MM-DD]]
```

### 8. Aplicar wikilinks (opcional)

Se conexão tem confiança alta (padrão 3+ ou pergunta-resposta):
- Adicionar wikilink bidirecional nas sources envolvidas
- Section "## Relações" de cada source

Se confiança média: apenas sugerir no relatório. Não editar sources.

---

## Constraints

- Max 10 conexões por relatório (qualidade > quantidade)
- Não inventar conexões forçadas — se <3 conexões reais, reportar <3
- Não modificar sources sem confiança alta
- Scan aleatório do vault antigo evita bias recency

---

## Changelog

- v1 (2026-05-09): criado. Inspirado por CyrilXBT Connection Finder + DamiDefi JARVIS.
