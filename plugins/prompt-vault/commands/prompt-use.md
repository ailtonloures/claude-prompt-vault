---
description: Reutiliza um prompt salvo, opcionalmente com contexto extra
argument-hint: <nome> [contexto adicional]
allowed-tools: Read, Bash(ls:*)
---

Contexto do usuário: $ARGUMENTS

Passos:
1. O primeiro token em `$ARGUMENTS` é o NOME do prompt salvo (mesmo slug usado em `/prompt-save`). O restante do texto (se houver) é CONTEXTO ADICIONAL fornecido pelo usuário só para esta execução.
2. Tente ler `~/.claude/prompt-vault/prompts/<nome>.md`.
   - Se o arquivo não existir: liste os arquivos `.md` em `~/.claude/prompt-vault/prompts/` (sem a extensão) e informe ao usuário que "<nome>" não foi encontrado, sugerindo os nomes disponíveis mais próximos. Pare aqui.
3. Extraia o conteúdo do prompt (tudo que vem depois do segundo `---` do frontmatter).
4. Trate esse conteúdo como se fosse a instrução do usuário para este turno — execute-o diretamente. Se houver contexto adicional (passo 1), incorpore-o como informação relevante para adaptar a execução do prompt salvo a esta situação específica.
