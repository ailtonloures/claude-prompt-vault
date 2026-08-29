---
description: Export saved prompts (optionally filtered by tag) into a single markdown file
argument-hint: [--tag=x]
allowed-tools: Bash(ls:*), Bash(date:*), Read, Write
---

User input: $ARGUMENTS

Steps:
1. If `$ARGUMENTS` contains a `--tag=x` token, extract TAG_FILTER (single tag, trimmed).
2. List the `.md` files in `~/.claude/prompt-vault/prompts/`. If the directory doesn't exist or is empty, say: "Nenhum prompt salvo ainda." and stop.
3. For each prompt, read and parse it (v2 frontmatter or v1 legacy defaults, same detection as `/prompt-list`).
4. If TAG_FILTER was given, keep only prompts whose `tags` list contains it (case-insensitive). If none match, say so and stop.
5. Determine the export filename: `prompts-export-<YYYYMMDD>.md` using today's date, written to the current working directory.
6. Write ONE file containing every selected prompt as its own section, in this format:

```
# Prompt Vault Export — <YYYY-MM-DD>

## <name>
- Tags: <tags ou "nenhuma">
- Vars: <vars ou "nenhuma">
- Use count: <use_count>
- Created: <created ou "desconhecido">
- Last used: <last_used ou "nunca">

\`\`\`
<prompt content>
\`\`\`

---

## <next name>
...
```

7. Confirm to the user: "📦 Exportado <N> prompt(s) para ./prompts-export-<YYYYMMDD>.md" and mention the tag filter if one was applied.
