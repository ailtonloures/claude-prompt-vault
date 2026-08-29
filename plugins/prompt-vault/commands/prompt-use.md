---
description: Replay a saved prompt, optionally with extra context
argument-hint: <name> [extra context]
allowed-tools: Read, Bash(ls:*)
---

User input: $ARGUMENTS

Steps:
1. The first token in `$ARGUMENTS` is the NAME of the saved prompt (same slug used in `/prompt-save`). Any remaining text is EXTRA CONTEXT provided by the user for this run only.
2. Try to read `~/.claude/prompt-vault/prompts/<name>.md`.
   - If the file doesn't exist: list the `.md` files in `~/.claude/prompt-vault/prompts/` (without the extension) and tell the user "<name>" wasn't found, suggesting the closest available names. Stop here.
3. Extract the prompt content (everything after the frontmatter's closing `---`).
4. Treat that content as if it were the user's instruction for this turn — execute it directly. If extra context was provided (step 1), factor it in as relevant information to adapt the saved prompt's execution to this specific situation.
