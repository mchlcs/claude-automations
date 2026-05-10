Process all pending tasks in Queue/.

> **Note:** User input and output can be in PT-BR. Prompts are English for portability.

## Execution

### 1. Scan Queue/

```bash
find $VAULT_DIR/Queue -maxdepth 1 -name "*.md" -not -name "_template.md" | sort
```

If 0 tasks → report "Queue empty" and stop.

### 2. Prioritize

Read frontmatter of each task. Sort by priority (high → medium → low).

### 3. Process each task

For each task (sequential by priority, parallel within same priority if independent):

1. Read complete instructions from file
2. Resolve referenced [[wikilinks]] (read sources/concepts if needed)
3. Execute task as described
4. Save output to specified location (default: Generated/)
5. Update frontmatter: `status: completed`, `completed: YYYY-MM-DD`
6. Move task to Queue/.archive/

### 4. Report

```
✓ Queue: N tasks processed
✓ High: N | Medium: N | Low: N
✓ Outputs: [list of generated files]
✓ Errors: N (details inline)
```

Append summary to wiki/hot.md.

## Constraints

- Read and respect each task's constraints
- Do NOT process files in Queue/routines/ (those are recurring routine templates, not one-shot tasks)
- If task references a non-existent source → skip with warning
- Max 10 tasks per execution (backpressure)
- Terse mode in output
