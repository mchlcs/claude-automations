Processe todas as tarefas pendentes em Queue/.

## Execução

### 1. Scan Queue/

```bash
find ~/Obsidian/vault-michel/Queue -maxdepth 1 -name "*.md" -not -name "_template.md" | sort
```

Se 0 tarefas → reportar "Queue vazia" e parar.

### 2. Priorizar

Ler frontmatter de cada task. Ordenar por priority (alta → média → baixa).

### 3. Processar cada task

Para cada task (sequencial por priority, paralelo dentro de mesma priority se independentes):

1. Ler instruções completas do arquivo
2. Resolver [[wikilinks]] referenciados (ler sources/concepts se necessário)
3. Executar tarefa conforme descrito
4. Salvar output em local especificado (default: Generated/)
5. Atualizar frontmatter: `status: concluído`, `completed: YYYY-MM-DD`
6. Mover task pra Queue/.archive/

### 4. Report

```
✓ Queue: N tarefas processadas
✓ Alta: N | Média: N | Baixa: N
✓ Outputs: [lista de arquivos gerados]
✓ Erros: N (detalhes inline)
```

Append resumo em wiki/hot.md.

## Constraints

- Ler e respeitar constraints de cada task
- Não processar arquivos em Queue/rotinas/ (são templates de rotinas, não tasks one-shot)
- Se task referencia source que não existe → skip com warning
- Max 10 tasks por execução (backpressure)
- Caveman mode no output
