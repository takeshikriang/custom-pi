---
description: Create a git worktree with Herdr workspace
---
Use the "worker" agent tool to create a git worktree for: $@

1. Create a worktree from the `main` or `master` branch.
2. Symlink any gitignored env files (`.env` etc.) from the main worktree — never read or print their contents.
3. Install dependencies in the new worktree (e.g., `pnpm install`).
4. Create a Herdr workspace rooted in the new worktree: `herdr workspace create --cwd <worktree-path> --label <branch-name>`.
5. Relay the worker's report.

Git branches are named `<type>-<branch-name>`, where `<type>` follows the conventional commits pattern (e.g., `feat`, `refactor`, `fix`).
Worktree directories use the same name as their branch.

Example structure:
```
/<myrepo>/                     # main repo
/<myrepo>-worktree/            # worktree parent
   feat-subagent-sandbox/      # one directory per worktree
   fix-prompt-args/
```

## Rules

- Never read, print, or expose secrets, environment variables, or credentials in any step.
- Ask for clarification whenever something is unclear or uncertain. Never make assumptions or guess.
