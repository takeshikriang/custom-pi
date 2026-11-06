# AGENTS.md
Version: 2026-11-02

## Cloudflare
When interacting with Cloudflare, use the cf CLI unless the project has a Wrangler configuration file.

## Git safety rules
- Never commit
- Never stage
- Never push

Exception: these are allowed only when the user explicitly instructs it.

## Environment variable rules
- Never create / update / read / delete environment variables
- Never print content of environment variables

## Code guidelines
- Keep It Simple Stupid (KISS)
- Prefer async/await over promise chains, but avoid nested async/await blocks
- Never add junk comments (e.g. restating the code, narrating changes, redundant docstrings)
- Explicitly declare brackets for switch/case blocks

## TypeScript guidelines
- Prefer interface over type for object shape
Example:
```ts
// Do
interface MyInterface {
  firstName: string
  lastName: string
}
// Don't
type MyType = {
  firstName: string
  lastName: string
}
```

- Prefer undefined over null
Example:
```ts
// Do
interface MyInterface {
  firstName: string
  lastName: string | undefined
}

// Don't
interface MyInterface {
  firstName: string
  lastName: string | null
}
```

- Explicitly declare `undefined` for optional parameters instead of using `?` syntax
Example:
```ts
// Do
interface MyInterface {
  firstName: string
  lastName: string | undefined
}

// Don't
interface MyInterface {
  firstName: string
  lastName?: string
}
```

## Import sorting guidelines
- First external module imports, Second internal module imports, Third relative module imports
- Separate groups by a blank line
- Sort alphabetically (A-Z) within each group

Example import sorting:
```ts
// External module imports from remote registries
import { betterAuth } from 'better-auth'
import { env } from 'cloudflare:workers'

// Internal module imports within the monorepo
import { accountTable } from '@duckhub/db'
import { t } from '@duckhub/i18n'

// Relative module imports within the current workspace
import { resolveDbContext } from '#composition'
import { MainMenu } from '../components/main-menu'
import { useCreateOwner } from './hooks/use-create-owner'
```
