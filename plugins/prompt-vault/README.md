# prompt-vault

Plugin do [Claude Code](https://code.claude.com) para salvar prompts repetitivos que **não merecem virar uma skill** — pedidos pontuais, formulações que você usa toda hora, checklists de review, templates de commit, etc. — e reutilizá-los depois com um slash command.

## Comandos

| Comando | O que faz |
|---|---|
| `/prompt-save <nome> <texto do prompt>` | Salva um novo prompt na vault (`~/.claude/prompt-vault/prompts/<nome>.md`) |
| `/prompt-use <nome> [contexto extra]` | Executa um prompt salvo, opcionalmente com contexto adicional para esta execução |
| `/prompt-list` | Lista todos os prompts salvos com prévia |
| `/prompt-delete <nome>` | Remove um prompt salvo (pede confirmação) |

## Exemplo

```
/prompt-save commit-msg Gere uma mensagem de commit seguindo Conventional Commits, em português, baseada no `git diff --staged`

/prompt-use commit-msg
```

## Onde os prompts ficam salvos

Em `~/.claude/prompt-vault/prompts/*.md` — nível de usuário, então funcionam em qualquer projeto, não só no repo atual. Cada arquivo é markdown simples com frontmatter (`name`, `created`) + o conteúdo do prompt, então você pode editar manualmente se quiser.
