---
title: Queue Processor — Tarefas One-Shot
type: rotina
schedule: "a cada 4h (08h, 12h, 16h, 20h)"
last_improved: 2026-05-09
version: 1
tags: [rotina, queue, automação]
---

# Queue Processor

Scan Queue/ para tarefas one-shot pendentes. Processar e arquivar.

**Referências vault:**
- [[wiki/concepts/self-writing-vault]] — vault que escreve de volta
- [[wiki/concepts/karpathy-four-principles]] — think before acting, verify before done

---

## 1. Scan

```bash
cd ~/Obsidian/vault-michel
find Queue/ -maxdepth 1 -name "*.md" -not -name "_template.md" | sort
```

0 tarefas → reportar "Queue vazia" e parar.

---

## 2. Priorizar

Ler frontmatter de cada task. Ordenar: alta → média → baixa.

---

## 3. Processar

Para cada task (sequencial por priority):

1. Ler instruções completas
2. Resolver [[wikilinks]] referenciados — ler sources/concepts se necessário pra contexto
3. Executar tarefa conforme descrito
4. Salvar output em local especificado (default: `Generated/`)
5. Verificar wikilinks válidos no output
6. Atualizar frontmatter: `status: concluído`, `completed: YYYY-MM-DD`
7. Mover task pra `Queue/.archive/`

---

## 4. Report

```
✓ Queue: N tarefas processadas
✓ Alta: N | Média: N | Baixa: N
✓ Outputs: [lista de arquivos gerados]
✓ Erros: N
```

Append resumo em `wiki/hot.md`.

---

## Guardrails

- NÃO processar arquivos em `Queue/rotinas/` (são templates recorrentes)
- NÃO processar `_template.md`
- Se task referencia source inexistente → skip com warning
- Max 10 tasks por execução (backpressure)
- Se task ambígua ou perigosa → skip com `status: revisão-manual`
- Caveman mode em outputs

---

## Changelog

- v1 (2026-05-09): criado. Inspirado por [[wiki/sources/obsidian-vault-writes-back-cyrilxbt]] Queue/ concept.
