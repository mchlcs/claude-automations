---
title: Daily Brief — Contexto do Dia
type: rotina
schedule: "seg-sex 23h"
last_improved: 2026-05-09
version: 1
tags: [rotina, brief, diário, contexto]
---

# Daily Brief

Resumo noturno do vault. O que mudou, o que conecta, o que pensar amanhã.

**Referências vault:**
- [[wiki/concepts/self-writing-vault]] — vault que produz contexto autonomamente
- [[wiki/sources/obsidian-vault-writes-back-cyrilxbt]] — Daily Context Generator (6h)
- [[wiki/sources/jarvis-obsidian-second-brain-thinks-full-setup]] — daily ritual 20 min
- [[wiki/concepts/knowledge-compounding]] — brief diário acelera compound

---

## Execução

### 1. Coletar mudanças do dia

```bash
cd ~/Obsidian/vault-michel
# Arquivos modificados hoje
find wiki/ Queue/ Generated/ -name "*.md" -newer /tmp/today_start -type f 2>/dev/null
# Git changes (se tracked)
git diff --name-only --diff-filter=ACMR HEAD~1 2>/dev/null | grep -E "^(wiki|Queue|Generated)/"
```

### 2. Ler hot cache (últimas 30 linhas)

```bash
tail -30 wiki/hot.md
```

### 3. Scan Queue/ pendente

```bash
find Queue/ -maxdepth 1 -name "*.md" -not -name "_template.md" | wc -l
```

### 4. Gerar brief

Output: `Generated/daily-brief-YYYY-MM-DD.md`

```markdown
---
title: "Daily Brief — YYYY-MM-DD"
type: brief
generated_by: claude-code
created: YYYY-MM-DD
---

# Brief — YYYY-MM-DD (dia da semana)

## O que mudou hoje
- [lista de arquivos criados/modificados com 1 linha de contexto]

## Queue status
- Pendentes: N tarefas
- Próxima rotina: [nome] em [quando]

## 3 conexões sugeridas
Baseado no que mudou hoje vs vault existente:
1. [source/concept recente] ↔ [source/concept antigo] — porquê
2. ...
3. ...

## 1 pergunta pra pensar amanhã
[Pergunta provocativa baseada em padrão emergente no vault]

## Contexto ativo
- **FIAP:** [fase atual, próximo prazo]
- **Concurso:** [tópico em foco]
- **AI Agents:** [experimento em andamento]
```

### 5. Hot cache (NÃO atualizar)

Daily brief NÃO escreve em hot cache — é efêmero, pra consumo do dia seguinte. Acumular briefs no hot poluiria.

Manter apenas em `Generated/daily-brief-YYYY-MM-DD.md`.

---

## Constraints

- Max 30 linhas total no brief
- Se nada mudou hoje → brief de 5 linhas: "Nada mudou. Queue: N pendentes. Próxima rotina: X."
- Conexões: só não-óbvias. Se não encontrar 3, reportar quantas encontrou
- Pergunta: genuína, não genérica. Baseada em dados reais do vault
- Seg-sex apenas — weekend tem rotinas de manutenção, não precisa brief

---

## Cleanup

Briefs >7 dias podem ser deletados automaticamente pelo wiki-lint (step 5 Queue cleanup).

---

## Changelog

- v1 (2026-05-09): criado. Inspirado por CyrilXBT Daily Context Generator + DamiDefi daily ritual.
