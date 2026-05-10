Analyze current changes (via `git diff --staged` or provided description) and generate a commit message following Conventional Commits.

> **Note:** User input and output can be in PT-BR. Prompts are English for portability.

## Format

```
<type>(<optional scope>): <short imperative description>

<optional body — why, not what>

<optional footer — breaking changes, closes #issue>
```

## Types

| Type | When to use |
|------|-------------|
| `feat` | New feature |
| `fix` | Bug fix |
| `refactor` | Refactoring without behavior change |
| `docs` | Documentation |
| `test` | Tests |
| `chore` | Config, build, deps |
| `style` | Formatting, no logic change |

## Rules
- Description in **imperative mood** ("add", "fix", "remove")
- Max 72 characters on first line
- If breaking change: add `BREAKING CHANGE:` in footer

## Output
Generate 2-3 commit message options ordered from simplest to most detailed.
Then suggest which to use and why.
