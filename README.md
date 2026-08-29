# claude-prompt-vault

Marketplace pessoal de plugins do [Claude Code](https://code.claude.com) do Ailton Loures.

## Plugins disponíveis

- **[prompt-vault](./plugins/prompt-vault)** — salve prompts repetitivos que não fazem sentido virar uma skill (pedidos pontuais, checklists, templates) e reutilize via slash command.

## Como instalar

Dentro de uma sessão do Claude Code:

```
/plugin marketplace add ailtonloures/claude-prompt-vault
/plugin install prompt-vault@ailton-plugins
```

Depois disso os comandos `/prompt-save`, `/prompt-use`, `/prompt-list` e `/prompt-delete` ficam disponíveis em qualquer projeto.

## Estrutura do repo

```
claude-prompt-vault/
├── .claude-plugin/
│   └── marketplace.json       # manifesto do marketplace
├── plugins/
│   └── prompt-vault/
│       ├── .claude-plugin/
│       │   └── plugin.json    # manifesto do plugin
│       ├── commands/          # slash commands
│       └── README.md
└── README.md
```
