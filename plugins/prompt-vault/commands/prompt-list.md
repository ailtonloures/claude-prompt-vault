---
description: List saved prompts sorted by most recently used, optionally filtered by tag
argument-hint: [--tag=x]
allowed-tools: Bash(ls:*), Read
---

User input: $ARGUMENTS

Steps:
1. If `$ARGUMENTS` contains a `--tag=x` token, extract TAG_FILTER (single tag, trimmed).
2. List the `.md` files in `~/.claude/prompt-vault/prompts/`. If the directory doesn't exist or is empty, say: "Nenhum prompt salvo ainda. Use `/prompt-save <nome> <texto>` para criar o primeiro." and stop.
3. For each prompt found, read the file and determine:
   - **v2 (frontmatter)**: extract `name`, `tags`, `created`, `last_used`, `use_count`, `vars` from the YAML block; CONTENT = everything after the closing `---`.
   - **v1 (legacy, no frontmatter)**: `tags=[]`, `vars=[]`, `use_count=0`, `last_used=none`, `created=unknown`; CONTENT = whole file.
   - A preview: the first ~80 characters of CONTENT.
4. If TAG_FILTER was given, keep only prompts whose `tags` list contains it (case-insensitive match). If none match, say "Nenhum prompt encontrado com a tag '<tag>'." and stop.
5. Sort the remaining prompts by `last_used` descending (most recently used first). Prompts never used (`last_used` empty/none) go to the bottom, sub-sorted by `created` descending when known, otherwise by name.
6. Present the list, one item per prompt, showing name, tags, number of vars, and use_count, for example:

```
📋 Prompts salvos (mais usados recentemente no topo):

• pr-review — tags: git, review — 2 vars — usado 3x (último uso: 2026-08-29)
  "Revise este PR em {{linguagem}}, focando em {{escopo}}. Aponte prob..."

• deploy-checklist — sem tags — 0 vars — nunca usado
  "Before deploying, check: 1) tests passing 2) migrat..."
```
