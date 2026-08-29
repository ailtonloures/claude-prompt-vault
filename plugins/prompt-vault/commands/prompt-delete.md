---
description: Remove um prompt salvo da vault
argument-hint: <nome>
allowed-tools: Bash(rm:*), Bash(ls:*), Read
---

Contexto do usuário: $ARGUMENTS

Passos:
1. O argumento é o NOME (slug) do prompt a remover.
2. Verifique se `~/.claude/prompt-vault/prompts/<nome>.md` existe.
   - Se não existir: liste os prompts disponíveis em `~/.claude/prompt-vault/prompts/` e avise o usuário. Pare aqui.
3. Leia o arquivo e mostre ao usuário uma prévia curta do conteúdo, pedindo confirmação explícita antes de apagar.
4. Somente após confirmação, delete o arquivo: `rm ~/.claude/prompt-vault/prompts/<nome>.md`.
5. Confirme: "🗑️ Prompt '<nome>' removido."
