---
title: "Memory systems for AI agents: what actually persists — @goodside"
type: source
source_url: https://x.com/goodside/status/1915000000000000001
author: "@goodside"
published: 2026-05-01
ingested: 2026-05-09
tags: [memory, ai-agents, persistence, claude-code, context-engineering, CLAUDE.md]
---

# Memory systems for AI agents: what actually persists

The word "memory" gets used for 4 completely different things in AI agent systems. Conflating them causes most agent memory failures.

## The 4 types

### 1. In-context memory (session RAM)

What the model sees right now. Disappears when session ends.  
- Limit: context window (200k for Claude, but economics push you to use less)
- Use for: active task state, recent tool outputs, current reasoning
- Failure mode: context rot — old irrelevant content crowding out new relevant content

### 2. External memory (retrieval)

Vector DBs, SQL, file search. Agent queries at runtime.  
- Limit: retrieval quality — you get what you can find, not what you know
- Use for: large corpora (thousands of documents), fuzzy search
- Failure mode: retrieval misses the relevant chunk; agent acts on partial info

### 3. Parametric memory (weights)

What the base model learned during training. Cannot change at runtime.  
- Limit: knowledge cutoff, no project-specific info
- Use for: language understanding, general reasoning, world knowledge
- Failure mode: model "remembers" something that's now wrong or project-specific

### 4. Persistent file memory (what actually works for agents)

Plain text files that load at session start. CLAUDE.md, MEMORY.md, project notes.  
- Limit: context budget (keep tight, link instead of paste)
- Use for: user profile, project conventions, past decisions, behavioral rules
- Failure mode: file grows stale, never gets pruned

## The practical stack

```
CLAUDE.md          → behavioral rules, project conventions (always loaded)
MEMORY.md          → index of memory files (always loaded)
memory/*.md        → individual memory facts (loaded on relevance)
03-RESOURCES/      → vault knowledge base (read on demand)
```

## Key rules

1. **Don't paste, link.** `[[03-RESOURCES/concepts/auth-flow]]` beats copy-pasting the auth docs into context.
2. **Prune memory files.** Stale memory is worse than no memory — agent acts on wrong facts confidently.
3. **Separate types.** Don't put retrieval logic where file memory belongs or vice versa.
4. **Memory ≠ documentation.** Memory is for non-obvious facts. Code comments, README, docstrings handle the obvious.

## Related concepts

- [[03-RESOURCES/concepts/context-engineering]]
- [[03-RESOURCES/concepts/harness-engineering]]
- [[04-SYSTEM/wiki/agent-registry]]
