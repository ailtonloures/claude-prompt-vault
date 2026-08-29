---
description: Lista todos os prompts salvos na vault
allowed-tools: Bash(ls:*), Read
---

Passos:
1. Liste os arquivos `.md` em `~/.claude/prompt-vault/prompts/`.
2. Se o diretório não existir ou estiver vazio, informe: "Nenhum prompt salvo ainda. Use `/prompt-save <nome> <texto>` para criar o primeiro." e pare.
3. Para cada prompt encontrado, leia o arquivo e extraia:
   - `name` e `created` do frontmatter
   - uma prévia com os primeiros ~80 caracteres do conteúdo (após o frontmatter)
4. Apresente como uma lista organizada, um item por prompt, por exemplo:

```
📋 Prompts salvos:

• deploy-checklist (criado em 2026-08-20)
  "Antes de fazer deploy, verifique: 1) testes passando 2) migra..."

• code-review-strict (criado em 2026-08-15)
  "Faça uma revisão rigorosa focando em segurança e performance..."
```
