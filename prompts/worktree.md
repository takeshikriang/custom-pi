---
description: Create a git worktree via the worker agent
---
Use the subagent tool to create a git worktree for: $@

1. Use the "worker" agent to symlink any gitignored env files (`.env` etc.) from the main worktree — never read or print their contents.
2. Relay the worker's report together with the CLI command for working with the new worktree.

Git branches are named `<type>-<branch-name>`, where `<type>` follows the conventional commits pattern (e.g., `feat`, `refactor`, `fix`).
Worktree directories use the same name as their branch.

Example structure:
```
/<myrepo>/                     # main repo
/<myrepo>-worktree/            # worktree parent
   feat-subagent-sandbox/      # one directory per worktree
   fix-prompt-args/
```

Example CLI command for working with the new worktree:
`cd ~/workspaces/<myrepo>-worktree/feat-subagent-sandbox && pnpm install`
