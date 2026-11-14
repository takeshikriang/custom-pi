# AGENTS.md
Version: 2026-11-14

## Precedence

When instructions conflict, follow this order:

1. Explicit user instructions in the conversation
2. Project-level AGENTS.md and the repository's own conventions
3. This file

Exception: the secrets rule has no override.

## Standard Rules

- Ask for clarification only when a decision is irreversible, security-sensitive, or changes architecture or scope. For everything else, state the assumption and proceed. Never guess on destructive operations.
- Keep It Simple Stupid (KISS), do not repeat yourself (DRY)
- Prefer async/await over promise chains, but avoid nested async/await blocks
- Never add junk comments (e.g. restating the code, narrating changes, redundant docstrings)

## Secrets

- Never print, log, copy into output, or otherwise expose secret values (API keys, tokens, credentials). This rule has no exceptions.
- If it is unclear whether a value is secret, treat it as secret.

## Environment variables

- Never create, update, or delete environment variables or env files (`.env`, `.dev.vars`) unless the user explicitly asks.
- Reading non-secret variables (e.g. session or tooling config such as `PI_*`) is allowed when the task requires it.

## Cloudflare

When interacting with Cloudflare, use the cf CLI unless the project has a Wrangler configuration file.

## Git safety rules

- Never commit
- Never stage
- Never push

Exception: these are allowed only when the user explicitly instructs it.


## TypeScript rules

- Explicitly declare brackets for switch/case blocks

Example:
```ts
// Correct
switch (action) {
  case 'start': {
    start()
    break
  }
  case 'stop': {
    stop()
    break
  }
  default: {
    handleUnknown()
  }
}

// Incorrect
switch (action) {
  case 'start':
    start()
    break
  case 'stop':
    stop()
    break
  default:
    handleUnknown()
}
```

- Prefer interface over type for object shapes; use type for unions, intersections, and mapped/utility types

Example:
```ts
// Correct
interface MyInterface {
  firstName: string
  lastName: string
}

// Incorrect
type MyType = {
  firstName: string
  lastName: string
}
```

- Prefer undefined over null

Example:
```ts
// Correct
interface MyInterface {
  firstName: string
  lastName: string | undefined
}

// Incorrect
interface MyInterface {
  firstName: string
  lastName: string | null
}
```

- Never use `null` with the `?` modifier (required for `exactOptionalPropertyTypes` compatibility)

Example:
```ts
// Correct
interface MyInterface {
  firstName: string
  lastName?: string | undefined
}

// Incorrect
interface MyInterface {
  firstName: string
  lastName?: string | undefined | null
}
```

## Import sorting guidelines

- Group order: external module imports, then internal monorepo imports, then relative imports
- Separate groups by a blank line
- Sort alphabetically (A-Z) within each group

Example import sorting:

```ts
// External module imports
import { betterAuth } from 'better-auth'
import { env } from 'cloudflare:workers'

// Internal monorepo imports
import { accountTable } from '@duckhub/db'
import { t } from '@duckhub/i18n'

// Relative imports within the current workspace
import { resolveDbContext } from '#composition'
import { MainMenu } from '../components/main-menu'
import { useCreateOwner } from './hooks/use-create-owner'
```

## Test guidelines

- Use parameterized tests for repetitive assertions
