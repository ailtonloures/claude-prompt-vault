<p align="center">
  <img src="assets/icon.svg" width="140" height="140" alt="claude-prompt-vault icon">
</p>

<h1 align="center">claude-prompt-vault</h1>

Ailton Loures' personal [Claude Code](https://code.claude.com) plugin marketplace.

## Available plugins

- **[prompt-vault](./plugins/prompt-vault)** — save repetitive prompts that don't make sense as a skill (one-off requests, checklists, templates) and reuse them via slash command.

## Install

Inside a Claude Code session:

```
/plugin marketplace add ailtonloures/claude-prompt-vault
/plugin install prompt-vault@ailton-plugins
```

After that, `/prompt-save`, `/prompt-use`, `/prompt-list`, and `/prompt-delete` are available in any project.

## Repo structure

```
claude-prompt-vault/
├── .claude-plugin/
│   └── marketplace.json       # marketplace manifest
├── assets/
│   └── icon.svg                # marketplace/plugin icon
├── plugins/
│   └── prompt-vault/
│       ├── .claude-plugin/
│       │   └── plugin.json    # plugin manifest
│       ├── commands/          # slash commands
│       └── README.md
└── README.md
