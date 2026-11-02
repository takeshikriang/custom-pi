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

## Security Model

This tool executes a separate `pi` subprocess with a delegated system prompt and tool/model configuration.

**Project-local agents** (`.pi/agents/*.md`) are repo-controlled prompts that can instruct the model to read files, run bash commands, etc.

**Default behavior:** Only loads **user-level agents** from `~/.pi/agent/agents`.

To enable project-local agents, pass `agentScope: "both"` (or `"project"`). Only do this for repositories you trust.

When running interactively, the tool prompts for confirmation before running project-local agents in untrusted projects. Trusted projects skip the additional prompt. Set `confirmProjectAgents: false` to disable confirmation.

## Usage

### Single agent
```
Use scout to find all authentication code
```

### Parallel execution
```
Run 2 scouts in parallel: one to find models, one to find providers
```

### Chained workflow
```
Use a chain: first have scout find the read tool, then have planner suggest improvements
```

### Workflow prompts
```
/implement add Redis caching to the session store
/scout-and-plan refactor auth to support OAuth
/implement-and-review add input validation to API endpoints
```

## Tool Modes

| Mode | Parameter | Description |
|------|-----------|-------------|
| Single | `{ agent, task }` | One agent, one task |
| Parallel | `{ tasks: [...] }` | Multiple agents run concurrently (max 8, 4 concurrent) |
| Chain | `{ chain: [...] }` | Sequential with `{previous}` placeholder |


## Agent Definitions

Agents are markdown files with YAML frontmatter:

```markdown
---
name: my-agent
description: What this agent does
tools: read, grep, find, ls
model: claude-haiku-4-5
---

System prompt for the agent goes here.
```

When `model` is omitted, the subagent inherits the dispatching session's active model and thinking level.

**Locations:**
- `~/.pi/agent/agents/*.md` - User-level (always loaded)
- `.pi/agents/*.md` - Project-level (only with `agentScope: "project"` or `"both"`)

Project agents override user agents with the same name when `agentScope: "both"`.
