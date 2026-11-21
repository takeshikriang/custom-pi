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

- Avoid nested if conditions; use guard clauses (early returns) instead

Example:
```ts
// Correct
function processOrder(order: Order) {
  if (!order.isValid) {
    return
  }
  if (order.items.length === 0) {
    return
  }
  process(order)
}

// Incorrect
function processOrder(order: Order) {
  if (order.isValid) {
    if (order.items.length > 0) {
      process(order)
    }
  }
}
```

- Prefer switch case for complex nested conditions over if-else chains

Example:
```ts
// Correct
function getShippingCost(order: Order) {
  switch (order.tier) {
    case 'premium': {
      return 0
    }
    case 'standard': {
      return order.weight > 10 ? 15 : 5
    }
    case 'economy': {
      return 5
    }
    default: {
      return calculateDefaultCost(order)
    }
  }
}

// Incorrect
function getShippingCost(order: Order) {
  if (order.tier === 'premium') {
    return 0
  } else if (order.tier === 'standard') {
    if (order.weight > 10) {
      return 15
    } else {
      return 5
    }
  } else if (order.tier === 'economy') {
    return 5
  } else {
    return calculateDefaultCost(order)
  }
}
```

- Pass only the values a function needs, not the whole containing object

Example:
```ts
// Correct
createEnqueueEmailOtp(env.EMAIL_OTP_QUEUE)

// Incorrect
createEnqueueEmailOtp(env)
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
- Test observable behavior, not implementation details
