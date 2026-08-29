---
description: Search saved prompts by content, not just by name
argument-hint: <termo>
allowed-tools: Bash(ls:*), Read
---

User input (search term): $ARGUMENTS

Steps:
1. TERM = the full `$ARGUMENTS` text, trimmed. If empty, ask the user what to search for and stop.
2. List the `.md` files in `~/.claude/prompt-vault/prompts/`. If the directory doesn't exist or is empty, say: "Nenhum prompt salvo ainda." and stop.
3. For each prompt, read its body (CONTENT — everything after the frontmatter's closing `---` for v2 files, or the whole file for v1 legacy files) and search case-insensitively for TERM.
4. For every match found, capture a short excerpt: the matching line, trimmed to a reasonable length, with 1 line of context above/below if that helps readability.
5. If no prompt matches, say: "Nenhum prompt encontrado contendo '<termo>'." and stop.
6. Present results grouped by prompt name, one excerpt per match (multiple excerpts per prompt if it matches in more than one place), for example:

```
🔍 Resultados para "docker":

• deploy-checklist
  "...revisar o Dockerfile antes do build final..."

• pr-review
  "...atenção especial a mudanças em docker-compose.yml..."
```

Use `/prompt-use <nome>` para reutilizar qualquer um destes.
