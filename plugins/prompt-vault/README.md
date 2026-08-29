<p align="center">
  <img src="../../assets/icon.svg" width="120" height="120" alt="prompt-vault icon">
</p>

<h1 align="center">prompt-vault</h1>

A [Claude Code](https://code.claude.com) plugin for saving repetitive prompts that **don't deserve to become a skill** — one-off requests, phrasing you use all the time, review checklists, commit templates, etc. — and replaying them later with a slash command.

## Commands

| Command | What it does |
|---|---|
| `/prompt-save <name> <prompt text>` | Saves a new prompt to the vault (`~/.claude/prompt-vault/prompts/<name>.md`) |
| `/prompt-use <name> [extra context]` | Runs a saved prompt, optionally with extra context for this run |
| `/prompt-list` | Lists all saved prompts with a preview |
| `/prompt-delete <name>` | Removes a saved prompt (asks for confirmation) |

## Example

```
/prompt-save commit-msg Generate a commit message following Conventional Commits, based on `git diff --staged`

/prompt-use commit-msg
```

## Where prompts are stored

In `~/.claude/prompt-vault/prompts/*.md` — user-level, so they work in any project, not just the current repo. Each file is plain markdown with frontmatter (`name`, `created`) plus the prompt content, so you can edit it by hand if you want.
