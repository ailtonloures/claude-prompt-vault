---
description: List all prompts saved in the vault
allowed-tools: Bash(ls:*), Read
---

Steps:
1. List the `.md` files in `~/.claude/prompt-vault/prompts/`.
2. If the directory doesn't exist or is empty, say: "No prompts saved yet. Use `/prompt-save <name> <text>` to create the first one." and stop.
3. For each prompt found, read the file and extract:
   - `name` and `created` from the frontmatter
   - a preview of the first ~80 characters of the content (after the frontmatter)
4. Present it as an organized list, one item per prompt, for example:

```
📋 Saved prompts:

• deploy-checklist (created 2026-08-20)
  "Before deploying, check: 1) tests passing 2) migrat..."

• code-review-strict (created 2026-08-15)
  "Do a strict review focused on security and performa..."
```
