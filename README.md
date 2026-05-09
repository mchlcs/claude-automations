# Claude Automations

Skills e rotinas para Claude Code + Obsidian vault.

## Estrutura

```
commands/       Slash commands (disponíveis via /comando no Claude Code)
rotinas/        Scheduled routines (prompts editáveis no Obsidian)
```

## Commands

| Comando | Descrição |
|---------|-----------|
| `/relatorio-artigos` | Relatório analítico de artigos ingestados (por período) |
| `/process-queue` | Processa tarefas one-shot em Queue/ |
| `/commit-padrao` | Commit com mensagem padronizada |
| `/criar-crud` | CRUD completo em Java |
| `/explicar` | Explica código ou conceito |
| `/resumo-sessao` | Resumo estruturado da sessão |
| `/revisar-codigo` | Code review do arquivo atual |

## Rotinas

| Rotina | Schedule | Descrição |
|--------|----------|-----------|
| `ingest-diario` | Sexta 17h | Ingest semanal .raw/articles + clippings |
| `relatorio-pos-ingest` | Sexta 19h | Análise cruzada pós-ingest |
| `meta-coaching-semanal` | Sábado 1h | Self-audit de uso do Claude Code |
| `wiki-lint-semanal` | Domingo 4h | Health check: orphans, dead links, dedup |
| `connection-finder` | Domingo 6h | Conexões não-óbvias entre sources |
| `daily-brief` | Seg-Sex 23h | Contexto noturno: mudanças, conexões, pergunta |
| `process-queue` | 8h/12h/16h/20h | Processa tarefas one-shot em Queue/ |

## Como usar

### Commands
Copiar para `.claude/commands/` do projeto:
```bash
cp commands/*.md /path/to/project/.claude/commands/
```

### Rotinas
Copiar para `Queue/rotinas/` do vault Obsidian:
```bash
cp rotinas/*.md ~/Obsidian/vault-michel/Queue/rotinas/
```

Cada rotina é referenciada por um scheduled task (thin wrapper) em `~/.claude/scheduled-tasks/`.

## Princípios

- Prompts vivem no Obsidian, não em settings — editáveis como qualquer nota
- Wikilinks integram rotinas com vault (sources, concepts, entities)
- Melhoria contínua: editar prompt no Obsidian = próximo run já pega
- Token economy: bash > AI, batch > loop, append > rewrite
