# Subagent

Delegate tasks to specialized subagents with isolated context windows.

## Index

- [Subagent installation](#installation)
- [Install skills for a specific directory](docs/skill-installation.md)
- [Reference](#reference)

## Installation

1. From the Pi repository root (requires a checkout of the pi repo).
Symlink the subagent: see the original [README](pi/packages/coding-agent/examples/extensions/subagent/README.md)


2. From this repository root, symlink the files:

```bash
# Symlink and verify guideline
ln -sf "$PWD/guideline/AGENTS.md" ~/.pi/agent/AGENTS.md
file ~/.pi/agent/AGENTS.md

# Symlink and verify agents
mkdir -p ~/.pi/agent/agents
for f in agents/*.md; do
  ln -sf "$PWD/$f" ~/.pi/agent/agents/"$(basename "$f")"
done
file ~/.pi/agent/agents/*.md

# Symlink and verify workflow prompts
mkdir -p ~/.pi/agent/prompts
for f in prompts/*.md; do
  ln -sf "$PWD/$f" ~/.pi/agent/prompts/"$(basename "$f")"
done
file ~/.pi/agent/prompts/*.md
```

## Reference

See the original Pi's subagent example [here](https://github.com/earendil-works/pi/tree/main/packages/coding-agent/examples/extensions/subagent)
