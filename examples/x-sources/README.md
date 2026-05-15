# Example X (Twitter) Sources

Sample ingested X threads in vault source format. Use as templates when adding your own sources.

## Format

Each file follows the standard source frontmatter:

```markdown
---
title: "Thread Title — @author"
type: source
source_url: https://x.com/@author/status/...
author: "@author"
published: YYYY-MM-DD
ingested: YYYY-MM-DD
tags: [tag1, tag2]
---
```

Body: summary, key insights organized by section, wikilinks to related vault concepts.

## Files

| File | Topic | Author |
|------|-------|--------|
| `agent-harness-patterns-akshay-pachaar.md` | 12-component production harness anatomy | @akshay_pachaar |
| `token-economy-claude-code-tips.md` | 7 token waste patterns + fixes | @simonw |
| `obsidian-vault-automation-patterns.md` | Vault automation: what works vs what doesn't | @kepano |
| `memory-systems-for-ai-agents.md` | 4 types of agent memory, practical stack | @goodside |

## Ingest into your vault

```bash
cp examples/x-sources/*.md $VAULT_DIR/03-RESOURCES/sources/
```

Then run `/article-report` or `weekly-ingest` routine to synthesize.
