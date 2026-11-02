# Subagent

Delegate tasks to specialized subagents with isolated context windows.

## Installation

From the Pi repository root (requires a checkout of the pi repo),
symlink the files:

```bash
# Symlink the extension (must be in a subdirectory with index.ts)
mkdir -p ~/.pi/agent/extensions/subagent
ln -sf "$PWD/packages/coding-agent/examples/extensions/subagent/index.ts" ~/.pi/agent/extensions/subagent/index.ts
ln -sf "$PWD/packages/coding-agent/examples/extensions/subagent/agents.ts" ~/.pi/agent/extensions/subagent/agents.ts

# Verify the symlinks are not broken
file ~/.pi/agent/extensions/subagent/*.ts
```

From this repository root, symlink the files:

```bash
# Symlink guideline
ln -sf "$PWD/guideline/AGENTS.md" ~/.pi/agent/AGENTS.md

# Symlink agents
mkdir -p ~/.pi/agent/agents
for f in agents/*.md; do
  ln -sf "$PWD/$f" ~/.pi/agent/agents/"$(basename "$f")"
done

# Symlink workflow prompts
mkdir -p ~/.pi/agent/prompts
for f in prompts/*.md; do
  ln -sf "$PWD/$f" ~/.pi/agent/prompts/"$(basename "$f")"
done

# Verify the symlinks are not broken
file ~/.pi/agent/AGENTS.md
file ~/.pi/agent/agents/*.md
file ~/.pi/agent/prompts/*.md
```

## Reference

See the original Pi's subagent example [here](https://github.com/earendil-works/pi/tree/main/packages/coding-agent/examples/extensions/subagent)
