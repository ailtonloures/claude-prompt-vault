---
description: Save a reusable prompt in your personal vault (auto-detects {{variables}}, supports tags)
argument-hint: <name> [--tag=a,b] <prompt text>
allowed-tools: Bash(mkdir:*), Bash(ls:*), Bash(date:*), Write, Read
---

User input: $ARGUMENTS

You must save a reusable prompt in the user's personal "vault", using the v2 frontmatter format.

Steps:
1. Parse `$ARGUMENTS`:
   - The first token is the NAME of the prompt. Normalize it to kebab-case (lowercase, spaces become hyphens, no accents/special characters).
   - If a token matches `--tag=a,b,c` anywhere in the input, extract it as TAGS: a trimmed, deduplicated list of comma-separated tags. Remove this token from the input.
   - Everything else remaining (after removing the name token and the `--tag=` token) is the prompt CONTENT — keep it exactly as the user wrote it, just trimmed of leading/trailing whitespace.
2. Detect variables: scan CONTENT for `{{variable}}` patterns (double curly braces, no spaces inside). Collect the unique variable names, in order of first appearance, into VARS. If none found, VARS is an empty list.
3. Ensure the directory `~/.claude/prompt-vault/prompts/` exists (`mkdir -p ~/.claude/prompt-vault/prompts`).
4. If `~/.claude/prompt-vault/prompts/<name>.md` already exists:
   - Read and show its current content to the user.
   - Ask explicitly: "Já existe um prompt chamado '<name>'. Deseja sobrescrever?" Only proceed with explicit confirmation ("sim"/"yes" or equivalent). If the user declines, stop without writing anything.
   - On confirmed overwrite, treat it as a brand new save: `created` is reset to now, `use_count` resets to 0, `last_used` is cleared, `tags`/`vars` are recomputed from the new input.
5. Write the file `~/.claude/prompt-vault/prompts/<name>.md` with this exact frontmatter format:

```
---
name: <name>
tags: [tag1, tag2]
created: <current ISO 8601 date>
last_used:
use_count: 0
vars: [var1, var2]
---

<prompt content>
```

   - Use `tags: []` if no tags were given.
   - Use `vars: []` if no `{{variables}}` were detected (omit the `vars:` line entirely is also acceptable if none were found, per spec — but keeping `vars: []` explicit is fine and simpler to parse later).
   - Leave `last_used:` blank (never used yet).
6. Confirm to the user, including what was detected, e.g.:
   "✅ Prompt '<name>' salvo em ~/.claude/prompt-vault/prompts/<name>.md.
   Tags: <tags ou 'nenhuma'> · Vars detectadas: <vars ou 'nenhuma'>.
   Use `/prompt-use <name>` para reutilizar."
