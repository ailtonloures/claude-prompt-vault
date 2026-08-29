<p align="center">
  <img src="../../assets/icon.svg" width="120" height="120" alt="prompt-vault icon">
</p>

<h1 align="center">prompt-vault</h1>

A [Claude Code](https://code.claude.com) plugin for saving repetitive prompts that **don't deserve to become a skill** — one-off requests, phrasing you use all the time, review checklists, commit templates, etc. — and replaying them later with a slash command.

## Commands

| Command | What it does |
|---|---|
| `/prompt-save <name> [--tag=a,b] <prompt text>` | Saves a new prompt to the vault. Auto-detects `{{variables}}`. Asks before overwriting an existing name. |
| `/prompt-use <name> [extra context]` | Runs a saved prompt. Asks for each `{{variable}}` value if the prompt has any. Suggests the 3 closest names if `<name>` isn't found. |
| `/prompt-list [--tag=x]` | Lists all saved prompts, most recently used first, with tags/vars/use count. Filter with `--tag=x`. |
| `/prompt-search <term>` | Full-text search across all saved prompts' content, not just the name. |
| `/prompt-edit <name>` | Edits a saved prompt's content inline, preserving `created` and `use_count`. |
| `/prompt-export [--tag=x]` | Exports all (or tag-filtered) prompts into one `prompts-export-YYYYMMDD.md` file. |
| `/prompt-delete <name>` | Removes a saved prompt (asks for confirmation). |

## Examples

```
/prompt-save pr-review --tag=git,review Revise este PR em {{linguagem}}, focando em {{escopo}}.

/prompt-use pr-review
> Valor para {{linguagem}}: português
> Valor para {{escopo}}: legibilidade

/prompt-list --tag=git
/prompt-search dockerfile
/prompt-edit pr-review
/prompt-export --tag=git
```

## Where prompts are stored

In `~/.claude/prompt-vault/prompts/*.md` — user-level, so they work in any project, not just the current repo.

Since v2, each file is markdown with YAML frontmatter:

```
---
name: pr-review
tags: [git, review]
created: 2026-08-29
last_used: 2026-08-29
use_count: 3
vars: [linguagem, escopo]
---

Revise este PR em {{linguagem}}, focando em {{escopo}}.
```

`vars` only appears if the prompt has `{{placeholders}}`. Prompts saved with v1 (plain text, no frontmatter) keep working unmodified — they're treated as `tags: []`, `vars: []`, `use_count: 0`, and get silently upgraded to the frontmatter format the first time they're used via `/prompt-use` or `/prompt-edit`. No manual migration needed.
