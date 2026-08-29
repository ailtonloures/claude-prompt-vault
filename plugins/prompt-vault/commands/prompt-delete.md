---
description: Remove a saved prompt from the vault
argument-hint: <name>
allowed-tools: Bash(rm:*), Bash(ls:*), Read
---

User input: $ARGUMENTS

Steps:
1. The argument is the NAME (slug) of the prompt to remove.
2. Check whether `~/.claude/prompt-vault/prompts/<name>.md` exists.
   - If it doesn't: list the available prompts in `~/.claude/prompt-vault/prompts/` and let the user know. Stop here.
3. Read the file and show the user a short preview of its content (body only, whether v1 legacy or v2 frontmatter), asking for explicit confirmation before deleting.
4. Only after confirmation, delete the file: `rm ~/.claude/prompt-vault/prompts/<name>.md`.
5. Confirm: "🗑️ Prompt '<name>' removido."
