---
description: Salva um prompt reutilizável na sua vault pessoal
argument-hint: <nome> <texto do prompt>
allowed-tools: Bash(mkdir:*), Bash(ls:*), Write, Read
---

Contexto do usuário: $ARGUMENTS

Você deve salvar um prompt reutilizável na "vault" pessoal do usuário.

Passos:
1. O primeiro token em `$ARGUMENTS` é o NOME do prompt. Normalize para kebab-case (minúsculas, espaços viram hífen, sem acentos/caracteres especiais). O restante do texto após o nome é o CONTEÚDO do prompt a ser salvo — preserve exatamente como o usuário escreveu.
2. Garanta que o diretório `~/.claude/prompt-vault/prompts/` existe (`mkdir -p ~/.claude/prompt-vault/prompts`).
3. Se já existir um arquivo `~/.claude/prompt-vault/prompts/<nome>.md`, mostre o conteúdo atual e pergunte ao usuário se deseja sobrescrever antes de continuar. Só prossiga com confirmação explícita.
4. Escreva o arquivo `~/.claude/prompt-vault/prompts/<nome>.md` com este formato exato:

```
---
name: <nome>
created: <data e hora ISO 8601 atual>
---

<conteúdo do prompt>
```

5. Confirme ao usuário: "✅ Prompt '<nome>' salvo em ~/.claude/prompt-vault/prompts/<nome>.md. Use `/prompt-use <nome>` para reutilizá-lo."
