---
description: Edit an existing saved prompt's content inline
argument-hint: <name>
allowed-tools: Read, Edit, Write, Bash(ls:*), Bash(date:*)
---

User input: $ARGUMENTS

Steps:
1. NAME = the first (and normally only) token in `$ARGUMENTS`, normalized to kebab-case using the same rule as `/prompt-save`.
2. Check whether `~/.claude/prompt-vault/prompts/<name>.md` exists.
   - If it doesn't: list the available prompts in the vault and tell the user, suggesting the closest names if helpful. Stop here.
3. Read the file and determine its format (v2 frontmatter vs v1 legacy, same detection as `/prompt-use`). Show the user the current CONTENT (body only, not the frontmatter) and ask: "O que você quer alterar no prompt '<name>'? Pode mandar o texto novo completo, ou descrever o ajuste (ex: 'troca X por Y')."
4. Wait for the user's reply. Produce the NEW CONTENT:
   - If the reply looks like a full replacement prompt, use it verbatim as the new body.
   - If the reply describes an edit/adjustment, apply that change to the existing CONTENT yourself and produce the resulting full new body.
5. Re-detect `{{variable}}` patterns in the NEW CONTENT and recompute `vars` (same rule as `/prompt-save`).
6. Rewrite the file:
   - If it was v2: PRESERVE `name`, `tags`, `created`, `use_count`, `last_used` exactly as they were. Only replace the body and the `vars` field with the recomputed list.
   - If it was v1 legacy: this edit also upgrades it to v2 — write a fresh frontmatter block (`name: <name>`, `tags: []`, `created: <today, real date unknown>`, `use_count: 0`, `last_used:` blank, `vars:` recomputed) plus the new body.
7. Confirm: "✏️ Prompt '<name>' atualizado." and mention the new `vars` if they changed from before.
