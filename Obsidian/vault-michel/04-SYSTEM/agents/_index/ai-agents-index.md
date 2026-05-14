---
title: Agentes de IA
type: index
space: ai-agents
created: 2026-04-14
updated: 2026-05-14
---

# Agentes de IA — Prompts & Workflows

Biblioteca de 37 agentes Claude organizados em 8 categorias. Prompts otimizados com Claude Sonnet 4.6 / Opus.

## Core Ops (Qualidade, Segurança, Infra)

**Propósito:** Gates contínuos de qualidade, validação de segurança, saúde do repositório

- [[04-SYSTEM/agents/00-core-ops/guard|Guard]] — Security auditor (OWASP LLM Top 10) · `@guard` · Opus
- [[04-SYSTEM/agents/00-core-ops/verify|Verify]] — Post-impl QA gate · `@verify` · Opus
- [[04-SYSTEM/agents/00-core-ops/review|Review]] — Drift review docs↔code↔config · `@review` · Sonnet
- [[04-SYSTEM/agents/00-core-ops/ingest-report|Ingest Report]] — Relatório semanal Clippings/ · Manual/cron · Haiku

## Feature Dev (Spec→Impl→Verify Pipeline)

**Propósito:** Desenvolvimento orientado por especificação com avaliação iterativa

- [[04-SYSTEM/agents/01-feature-dev/spec|Spec]] — Spec-Driven Development · `@spec` · Haiku
- [[04-SYSTEM/agents/01-feature-dev/extend|Extend]] — Extensão cirúrgica de agentes · `@extend` · Sonnet
- [[04-SYSTEM/agents/01-feature-dev/hill|Hill]] — Hill-climbing: eval→diagnose→fix · `@hill` · Haiku

## Domain Experts (Funcional por Domínio)

### Escrita & Conteúdo

- [[04-SYSTEM/agents/02-domain-experts/escrita-conteudo/assistente-de-escrita-conteudo|Assistente de Escrita & Conteúdo]] — 6 modos: pensamento, escrita, problemas, negócio, reflexão, conteúdo
- [[04-SYSTEM/agents/02-domain-experts/escrita-conteudo/assistente-de-projetos-decisoes|Assistente de Projetos & Decisões]] — 9 modos: contexto, tarefas, problemas, vieses, estratégia
- [[04-SYSTEM/agents/02-domain-experts/escrita-conteudo/assistente-de-research-conhecimento|Assistente de Research & Conhecimento]] — 3 modos: investigação, aprendizagem, simplificação
- [[04-SYSTEM/agents/02-domain-experts/escrita-conteudo/otimizador-de-prompts|Otimizador de Prompts]] — diagnóstico, reescrita, comparativo

### Técnico & Dev

- [[04-SYSTEM/agents/02-domain-experts/tech-dev/assistente-tecnico-sistemas-produtividade-ia|Assistente Técnico — Sistemas & IA]] — integração, otimização de fluxo
- [[04-SYSTEM/agents/02-domain-experts/tech-dev/construtor-de-apps-dev-no-code|Construtor de Apps — Dev & No-Code]] — 9 modos: app completo, screenshot→código, debug, MVP, SaaS
- [[04-SYSTEM/agents/02-domain-experts/tech-dev/construtor-de-sites-htmlcss|Construtor de Sites — HTML/CSS]] — landing pages, revisão de código
- [[04-SYSTEM/agents/02-domain-experts/tech-dev/conversor-markdown-html|Conversor Markdown → HTML]] — 3 modos: análise de formato, conversão econômica, multi-view
- [[04-SYSTEM/agents/02-domain-experts/tech-dev/pipeline-de-desenvolvimento-ads|Pipeline de Desenvolvimento — ADS]] — 6 etapas: arquitetura→scaffolding→review→refatoração→docs

### Visual & Design

- [[04-SYSTEM/agents/02-domain-experts/design-visual/criacao-visual-fotos-videos|Criação Visual — Fotos & Vídeos]] — editor Lightroom/Picsart, diretor DJI/iPhone
- [[04-SYSTEM/agents/02-domain-experts/design-visual/designer-estrategista-de-sites|Designer & Estrategista de Sites]] — 8 modos: planejamento, copy, identidade visual, SEO, UX

### Conteúdo & Marca Pessoal

- [[04-SYSTEM/agents/02-domain-experts/content-personal-brand/estrategista-de-conteudo-viagem-fotografia|Estrategista de Conteúdo — Viagem & Fotografia]] — 7 modos: estratégia, pilares, calendário, performance
- [[04-SYSTEM/agents/02-domain-experts/content-personal-brand/marca-pessoal-monetizacao-solo|Marca Pessoal & Monetização Solo]] — 7 modos: auditoria, posicionamento, monetização

### Educação

- [[04-SYSTEM/agents/02-domain-experts/educacao/professor-de-concurso-publico|Professor de Concurso Público]] — aulas, questões, plano de estudos por banca
- [[04-SYSTEM/agents/02-domain-experts/educacao/professor-de-idiomas-para-viajantes|Professor de Idiomas para Viajantes]] — 4 modos: aula situacional, conversação, inglês técnico
- [[04-SYSTEM/agents/02-domain-experts/educacao/tutor-de-ti-ads-carreira-tech|Tutor de TI — ADS & Carreira Tech]] — 5 modos: tutor adaptativo, projeto guiado, entrevista
- [[04-SYSTEM/agents/02-domain-experts/educacao/gerador-de-resumos-de-estudo|Gerador de Resumos de Estudo]] — 4 modos: resumo Obsidian, flashcards Anki, questões, mapa mental
- [[04-SYSTEM/agents/02-domain-experts/educacao/coach-de-curriculo-linkedin-estagios-ti|Coach de Currículo & LinkedIn]] — CV PT-BR + EN, otimização ATS

### Viagem

- [[04-SYSTEM/agents/02-domain-experts/travel/agente-de-viagem-criador-de-itinerario|Criador de Itinerário]] — roteiros diários, voos/hotel fechados
- [[04-SYSTEM/agents/02-domain-experts/travel/agente-de-viagem-refinamento-de-itinerario|Refinamento de Itinerário]] — otimiza roteiros já fechados

### Legal & Governança

- [[04-SYSTEM/agents/02-domain-experts/legal-gov/assessor-juridico-direito-publico-administrativo|Assessor Jurídico — Direito Público]] — citações normativas, marco legal

## Institutional (TJAM — Compliance & Governança)

**Propósito:** Agentes especializados para fluxos de conformidade e relatórios institucionais

- [[04-SYSTEM/agents/03-institutional/patrimonio-almoxarifado/tjam-assessor-patrimonio-e-almoxarifado|Assessor Patrimônio & Almoxarifado]] — 7 modos: redação, revisão, normativo, PLS, fiscalização, automação
- [[04-SYSTEM/agents/03-institutional/sustentabilidade/tjam-assessor-de-sustentabilidade-pls|Assessor de Sustentabilidade — PLS]] — planos 5W2H, CNJ 400/2021, ODS
- [[04-SYSTEM/agents/03-institutional/contratos-dados/tjam-fiscal-de-contratos-analista-de-dados|Fiscal de Contratos & Analista de Dados]] — auditoria contratual, Google Sheets ETL
- [[04-SYSTEM/agents/03-institutional/relatorios/gerador-de-relatorios-sheets-documento|Gerador de Relatórios — Sheets → Documento]] — 3 modos: relatório gerencial, sumário executivo, ata

## Infrastructure (Proxies, Integrations)

**Propósito:** System-level integrations e extensões de acesso

- [[04-SYSTEM/agents/04-infrastructure/claude-hermes-proxy|Claude-Hermes Proxy]] — HTTP proxy OpenAI-compat para Claude Code · 127.0.0.1:8080

## Referências

- [[03-RESOURCES/concepts/prompt-engineering|Prompt Engineering]] — princípios e técnicas
- [[03-RESOURCES/concepts/prompt-engineering-patterns|Prompt Engineering Patterns]] — 10 padrões
- [[03-RESOURCES/concepts/agentic-agents|Agentic Agents]] — agentes IA
- [[03-RESOURCES/entities/anthropic|Anthropic]] — empresa por trás do Claude
