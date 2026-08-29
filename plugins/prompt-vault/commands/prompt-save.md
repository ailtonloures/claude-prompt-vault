---
description: Save a reusable prompt in your personal vault
argument-hint: <name> <prompt text>
allowed-tools: Bash(mkdir:*), Bash(ls:*), Write, Read
---

User input: $ARGUMENTS

You must save a reusable prompt in the user's personal "vault".

Steps:
1. The first token in `$ARGUMENTS` is the NAME of the prompt. Normalize it to kebab-case (lowercase, spaces become hyphens, no accents/special characters). Everything after the name is the prompt CONTENT to save — keep it exactly as the user wrote it.
2. Make sure the directory `~/.claude/prompt-vault/prompts/` exists (`mkdir -p ~/.claude/prompt-vault/prompts`).
3. If `~/.claude/prompt-vault/prompts/<name>.md` already exists, show its current content and ask the user whether to overwrite it before continuing. Only proceed with explicit confirmation.
4. Write the file `~/.claude/prompt-vault/prompts/<name>.md` with this exact format:

```
---
name: <name>
created: <current ISO 8601 date/time>
---

<prompt content>
```

5. Confirm to the user: "✅ Prompt '<name>' saved to ~/.claude/prompt-vault/prompts/<name>.md. Use `/prompt-use <name>` to reuse it."
