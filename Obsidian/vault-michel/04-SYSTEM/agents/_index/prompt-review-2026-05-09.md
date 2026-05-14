---
title: "Revisão de Prompts — 24 Agentes Claude"
type: review
created: 2026-05-09
updated: 2026-05-09
tags:
  - ai-agent
  - prompt-engineering
  - review
---

# Revisão de Prompts — 24 Agentes Claude

Análise contra princípios ingestados na vault:
- [[03-RESOURCES/concepts/prompt-engineering|Prompt Engineering]] — Anthropic 5 core
- [[03-RESOURCES/concepts/prompt-engineering-patterns|Prompt Patterns]] — 14 padrões de alta alavancagem
- [[03-RESOURCES/concepts/karpathy-four-principles|Karpathy 4P]] — think, simplicity, surgical, goal-driven
- [[03-RESOURCES/concepts/token-efficiency-prompting|Token Efficiency]] — 9 dimensões de intenção
- [[03-RESOURCES/concepts/context-engineering|Context Engineering]] — contexto focado > largo

---

## Diagnóstico Global — 7 Gaps Recorrentes

### GAP 1 — Zero few-shot examples
**Princípio violado:** Anthropic #3 (Examples), Pattern #2 (Voice sample), Token Efficiency dim. 9
**Problema:** Nenhum dos 24 prompts inclui exemplos de input/output. Anthropic diz explicitamente: "few-shot examples are more effective than instructions alone."
**Impacto:** Modelo interpreta instruções com liberdade — output inconsistente entre sessões.
**Fix:** Adicionar 1 exemplo curto de input → output ideal por modo. Não precisa ser longo — 3-5 linhas bastam.

### GAP 2 — Sem restrições negativas
**Princípio violado:** Pattern #1 (Negative constraints), Pattern #7 (Does NOT sound like)
**Problema:** Apenas 3/24 prompts dizem o que NÃO fazer (Assessor Jurídico: "nunca invente normas", Tutor TI: "nunca reescreva tudo"). Os outros 21 descrevem apenas o positivo.
**Impacto:** Modelo cai nos padrões default de treinamento — jargão corporativo, hedging, intros genéricas.
**Fix:** Adicionar bloco `## NÃO FAÇA` com 3-5 anti-patterns do domínio.

### GAP 3 — Sem critérios de sucesso
**Princípio violado:** Karpathy P4 (Goal-driven), Token Efficiency dim. 8 (Success criteria)
**Problema:** Modos definem WHAT (o que entregar) mas nunca HOW TO VERIFY (como saber que está bom). Ex: "Entregue copy homepage" — mas o que distingue copy boa de copy medíocre?
**Impacto:** Impossível auto-avaliar qualidade. Requer human review em 100% dos outputs.
**Fix:** Para cada modo, adicionar 1-2 linhas de `Critério de qualidade: [...]`. Ex: "Copy aprovada se: headline ≤10 palavras, benefício concreto no H1, zero jargão corporativo."

### GAP 4 — Bloat estrutural = token waste
**Princípio violado:** Token Efficiency ("every word load-bearing"), Karpathy P2 (Simplicity)
**Problema:** Prompts repetem "Ative com:", "Solicite se não fornecido:", "Entregue:", "Estrutura obrigatória:" em cada modo. Em prompts com 9 modos, isso consome ~200 tokens em boilerplate repetido.
**Impacto:** Contexto desperdiçado em formatação, não em conteúdo.
**Fix:** Mover instruções repetidas para um bloco `## REGRAS GLOBAIS` no topo. Cada modo fica só com o diferencial.

### GAP 5 — Sem scope negativo explícito
**Princípio violado:** Pattern #10 (Scope negativo explícito)
**Problema:** Nenhum prompt define o que está FORA do escopo. Ex: Construtor de Apps não diz "não faço deploy em produção" ou "não configuro DNS".
**Impacto:** Usuário pode pedir coisas que o prompt não foi projetado para, gerando output fraco sem aviso.
**Fix:** Adicionar `## FORA DO ESCOPO` com 2-3 itens claros.

### GAP 6 — Role assignment genérico
**Princípio violado:** Anthropic #5 (Role), Pattern #11 (Role Assignment)
**Problema:** Vários prompts usam role vago: "Assistente de escrita", "Assistente de pesquisa", "Tutor adaptativo". Pattern #11 recomenda: "Role + anos + domínio específico".
**Impacto:** ~40% menos efetivo que role específico (dado empírico Pattern #11).
**Fix:** Trocar "Assistente de escrita" → "Copywriter sênior com 12 anos em conteúdo digital e publicidade direta. Especialidade: textos que convertem sem soar genéricos."

### GAP 7 — Karpathy P1 (Think before acting) ausente
**Princípio violado:** Karpathy P1, Pattern #6 (Premissas ocultas)
**Problema:** Maioria dos modos vai direto para output. Apenas Assistente de Projetos (Modo 3) e Construtor de Apps (Modo 4) fazem diagnóstico antes de agir.
**Impacto:** Modelo assume contexto sem verificar. Se assumption errada → output inteiro desperdiçado.
**Fix:** Todo modo complexo deveria ter: "ANTES de executar: liste 3 premissas que está assumindo. Se alguma for incerta, pergunte."

---

## Análise por Agente — Issues Específicos

### Viagem

| Agente | Score | Issues |
|--------|-------|--------|
| Criador de Itinerário | 7/10 | Bom: solicita dados antes. Falta: exemplo de output, negativas ("não sugira restaurantes genéricos tipo TripAdvisor"), critério de qualidade |
| Refinamento de Itinerário | 7/10 | Bom: análise estruturada em 5 partes. Falta: exemplo de trade-off real, métrica de sucesso |

### Jurídico

| Agente | Score | Issues |
|--------|-------|--------|
| Assessor Jurídico | 8/10 | Melhor prompt do lote. Tem negativas ("nunca invente normas"), status checks (⚠️), fontes. Falta: exemplo de tabela preenchida, critério de completude |

### Escrita & Produtividade

| Agente | Score | Issues |
|--------|-------|--------|
| Escrita & Conteúdo | 6/10 | 6 modos = muita superfície. Role genérico. Modo 4 (Avaliação de Ideia) é forte; Modo 1 (Clarificação) é fraco — não tem exemplo de "pensamento desorganizado → organizado". Sem limites de comprimento |
| Projetos & Decisões | 6/10 | 9 modos = prompt longo demais para Claude Chat. Modo 8 (MTG Arena) é completamente fora do domínio — deveria ser agente separado. Token waste em boilerplate |
| Research & Conhecimento | 7/10 | Bem estruturado, mas role genérico. Sem exemplo de output. Modo 3 (Simplificação) deveria ter exemplo de "complexo → simples" |
| Otimizador de Prompts | 7/10 | Pipeline claro (6 etapas). Falta: exemplo de prompt antes/depois, role mais forte ("prompt engineer com 500+ prompts auditados") |

### Técnico & Dev

| Agente | Score | Issues |
|--------|-------|--------|
| Assistente Técnico | 6/10 | Só 2 modos — subaproveitado. Modo B (Otimizador de Fluxo IA) é vago. Sem stop conditions para tarefas agentic |
| Construtor de Apps | 7/10 | 9 modos completos. Bom: "sem placeholders". Problema: Modo 9 (SaaS Completo) = escopo imenso para 1 prompt — deveria ser fluxo multi-turn. Sem exemplos |
| Construtor Sites HTML/CSS | 7/10 | Conciso, focado. Falta: exemplo de output HTML, negativas ("sem Bootstrap, sem Tailwind a menos que pedido") |
| Pipeline Dev ADS | 8/10 | Fluxo sequencial bem projetado. Etapa 6 (Code Review Adversarial) é excelente — usa fases, QUESTIONS.md, loop. Falta: exemplo de QUESTIONS.md |

### Visual & Design

| Agente | Score | Issues |
|--------|-------|--------|
| Criação Visual | 8/10 | Excelente especificidade técnica (valores Lightroom exatos, settings câmera). Falta: exemplo de análise de foto |
| Designer Sites | 7/10 | 8 modos bem definidos. Role genérico. Modo 7 (SEO) mistura conceitos demais em 1 modo. Sem exemplos |

### Conteúdo & Marca

| Agente | Score | Issues |
|--------|-------|--------|
| Estrategista Viagem | 7/10 | Contexto fixo do criador = bom (Memory Block). 7 modos. Falta: exemplos de hooks que funcionaram, negativas ("sem clichês de viagem: 'lugar incrível', 'experiência única'") |
| Marca Pessoal | 7/10 | Tom bem definido ("profundidade filosófica + execução prática"). Falta: exemplo de post viral real, voice sample do próprio Michel |

### Educação

| Agente | Score | Issues |
|--------|-------|--------|
| Professor Concurso | 7/10 | Bom: conhece bancas. Modo 4 (Simulador) é forte. Falta: exemplo de questão CESPE com pegadinha identificada |
| Professor Idiomas | 7/10 | Modo 2 (Simulador Conversação) é excelente. Modo 4 (Inglês Técnico TI) bem focado. Falta: exemplo de diálogo |
| Tutor TI | 8/10 | Tem negativa ("nunca reescreva tudo"). Calibração de nível (3 perguntas antes). Falta: exemplo de mini-desafio |
| Resumos de Estudo | 7/10 | 4 modos claros, formatos definidos. Falta: exemplo de flashcard, negativas ("sem copiar texto verbatim — sintetize") |

### Carreira

| Agente | Score | Issues |
|--------|-------|--------|
| Coach Currículo | 5/10 | Prompt mais fraco. Não tem role assignment, não solicita dados do candidato antes. Foco só no formato de saída (PT-BR + EN), não no diagnóstico. Sem negativas |

### TJAM

| Agente | Score | Issues |
|--------|-------|--------|
| Assessor Patrimônio | 9/10 | Melhor prompt junto com Jurídico. Nomenclatura obrigatória, bloco de observações, 7 modos, mode detection por gatilho. Falta apenas: exemplo de output |
| Sustentabilidade PLS | 8/10 | Bem estruturado: CNJ 400, ODS, 5W2H, KPIs. Falta: exemplo de plano preenchido |
| Fiscal de Contratos | 8/10 | Tom auditor ("fria, técnica, imparcial"). Citação de cláusula obrigatória. Planejamento prévio. Falta: exemplo de divergência encontrada |
| Gerador Relatórios | 7/10 | 3 modos focados. Falta: exemplo de relatório gerencial, negativas ("sem linguagem informal") |

---

## Ranking por Score

| Score | Agentes |
|-------|---------|
| 9/10 | TJAM Assessor Patrimônio |
| 8/10 | Assessor Jurídico, Pipeline Dev ADS, Criação Visual, Sustentabilidade PLS, Fiscal Contratos, Tutor TI |
| 7/10 | Criador Itinerário, Refinamento Itinerário, Research, Otimizador Prompts, Construtor Apps, Sites HTML/CSS, Designer Sites, Estrategista Viagem, Marca Pessoal, Professor Concurso, Professor Idiomas, Resumos, Gerador Relatórios |
| 6/10 | Escrita & Conteúdo, Projetos & Decisões, Assistente Técnico |
| 5/10 | Coach Currículo |

---

## Template de Fix — Blocos a Adicionar

Para cada prompt, adicionar estes blocos (custo: ~100-150 tokens cada):

```
## NÃO FAÇA
- [anti-pattern 1 do domínio]
- [anti-pattern 2]
- Nunca inicie resposta com "Claro!", "Com certeza!" ou introduções genéricas
- Não use jargão sem explicar ou sem necessidade
- Não entregue output sem antes verificar que tem dados suficientes

## CRITÉRIO DE QUALIDADE
- [como saber que output está bom — métrica ou checklist]

## FORA DO ESCOPO
- [o que este agente NÃO faz]

## EXEMPLO
**Input:** [exemplo curto]
**Output:** [exemplo curto de resposta ideal]
```

---

## Próximos Passos

1. **Quick wins (30 min):** Adicionar `## NÃO FAÇA` nos 5 agentes mais usados
2. **Medium (2h):** Reescrever os 3 agentes score ≤6 (Escrita, Projetos, Coach Currículo)
3. **Full (4h):** Aplicar template completo em todos 24 — few-shot + negativas + critérios + scope

---

## Referências

- [[03-RESOURCES/concepts/prompt-engineering]]
- [[03-RESOURCES/concepts/prompt-engineering-patterns]]
- [[03-RESOURCES/concepts/karpathy-four-principles]]
- [[03-RESOURCES/concepts/token-efficiency-prompting]]
- [[03-RESOURCES/concepts/context-engineering]]
- [[03-RESOURCES/concepts/prompt-master]]
