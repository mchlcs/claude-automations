Analise as mudanças atuais (via `git diff --staged` ou descrição fornecida) e gere uma mensagem de commit no padrão Conventional Commits.

## Formato

```
<tipo>(<escopo opcional>): <descrição curta em imperativo>

<corpo opcional — por quê, não o quê>

<rodapé opcional — breaking changes, closes #issue>
```

## Tipos

| Tipo | Quando usar |
|------|-------------|
| `feat` | Nova funcionalidade |
| `fix` | Correção de bug |
| `refactor` | Refatoração sem mudar comportamento |
| `docs` | Documentação |
| `test` | Testes |
| `chore` | Config, build, deps |
| `style` | Formatação, sem lógica |

## Regras
- Descrição em **português**, imperativo ("adiciona", "corrige", "remove")
- Máximo 72 caracteres na primeira linha
- Se houver breaking change: adicione `BREAKING CHANGE:` no rodapé

## Output
Gere 2-3 opções de commit message ordenadas da mais simples à mais detalhada.
Depois sugira qual usar e por quê.
