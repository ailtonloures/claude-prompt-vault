---
description: Replay a saved prompt, filling in {{variables}} if present, with fuzzy-match suggestions on miss
argument-hint: <name> [extra context]
allowed-tools: Read, Write, Bash(ls:*), Bash(date:*)
---

User input: $ARGUMENTS

Steps:
1. The first token in `$ARGUMENTS` is the NAME of the saved prompt (same slug used in `/prompt-save`). Any remaining text is EXTRA CONTEXT provided by the user for this run only.
2. Try to read `~/.claude/prompt-vault/prompts/<name>.md`.
   - If the file doesn't exist: list the `.md` files in `~/.claude/prompt-vault/prompts/` (names without extension). Using a simple fuzzy heuristic (shared substring / common prefix length / small edit distance — no external library needed, just reasoned comparison), pick the 3 closest names to `<name>`. Reply: "Prompt '<name>' não encontrado. Você quis dizer: <sugestao1>, <sugestao2>, <sugestao3>?" (fewer than 3 if fewer prompts exist, or "Nenhum prompt salvo ainda" if the vault is empty). Stop here.
3. Determine the format:
   - **v2 (frontmatter)**: file starts with a `---` line. Parse the YAML frontmatter block (name, tags, created, last_used, use_count, vars) and treat everything after the closing `---` as CONTENT.
   - **v1 (legacy, no frontmatter)**: the whole file is CONTENT. Treat it as tags=[], vars=[], use_count=0, created=unknown, last_used=none.
4. If `vars` is non-empty:
   - For EACH variable, ask the user for its value, one at a time, in plain chat text (e.g. "Valor para {{linguagem}}:"). Wait for each answer before asking the next.
   - Substitute every `{{<var>}}` occurrence in CONTENT with the value the user provided.
5. Update the file on disk:
   - Increment `use_count` by 1.
   - Set `last_used` to the current ISO 8601 date.
   - If the file was v1 legacy (no frontmatter), this is also the moment to silently upgrade it to v2 format: add a full frontmatter block (`name: <name>`, `tags: []`, `created: <today, since the real creation date is unknown>`, `vars: []`, `use_count: 1`, `last_used: <today>`) above the original content, which becomes the body. This keeps v1 prompts working forever while migrating them opportunistically the first time they're reused.
6. Treat the final (variable-substituted) CONTENT as if it were the user's instruction for this turn — execute it directly. If extra context was provided in step 1, factor it in as relevant information to adapt the saved prompt's execution to this specific situation.
