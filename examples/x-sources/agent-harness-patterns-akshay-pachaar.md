---
title: "12 components every production agent harness needs — @akshay_pachaar"
type: source
source_url: https://x.com/akshay_pachaar/status/1908734520000000001
author: "@akshay_pachaar"
published: 2026-04-06
ingested: 2026-05-09
tags: [agent-harness, orchestration, memory, context-management, production]
---

# 12 components every production agent harness needs

> "If you're not the model, you're the harness."

Two products with identical models can have radically different performance based solely on harness design. LangChain changed only the harness and jumped from top-30 to position 5 on TerminalBench 2.0.

## Key insight

Analogy from Beren Millidge: raw LLM = CPU with no RAM, no disk, no I/O.

- Context window = RAM
- External DBs = disk
- Tools = device drivers
- **Harness = Operating System**

"We have reinvented the Von Neumann architecture."

## The 12 production components

| # | Component | Function |
|---|-----------|----------|
| 1 | Orchestration Loop | TAO/ReAct cycle — intelligence lives in the model, not the loop |
| 2 | Tools | Schemas + registration + sandboxed execution + result formatting |
| 3 | Memory | Short-term (session) + long-term (CLAUDE.md, MEMORY.md, SQLite) |
| 4 | Context Management | Avoid context rot: compaction, observation masking, JIT retrieval |
| 5 | Planning | Task decomposition before execution |
| 6 | Verification | Output validation, self-correction loops |
| 7 | Error Recovery | Retry logic, fallback strategies |
| 8 | Safety | Output filtering, scope limiting |
| 9 | State Persistence | Resume after interruption |
| 10 | Observability | Token tracking, cost monitoring, traces |
| 11 | Lifecycle Management | Spawn/kill subagents, parallelize |
| 12 | Human-in-the-Loop | Approval gates for irreversible actions |

## Takeaways

- Most "bad AI" is bad harness engineering, not bad model
- Prompt engineering < context engineering < harness engineering
- The harness determines what the model sees and when — that's the real leverage point
- A minimal harness (CLAUDE.md + MEMORY.md + hooks) beats a complex one badly configured

## Related concepts

- [[03-RESOURCES/concepts/harness-engineering]]
- [[03-RESOURCES/concepts/context-management]]
- [[04-SYSTEM/agents/nexus]]
