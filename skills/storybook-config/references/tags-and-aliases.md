# Storybook tags and aliases

Configure tags and Storybook-only import aliases in `.storybook/main.*`.

```ts
import { resolve } from 'node:path'
import { type InlineConfig, mergeConfig } from 'vite'

// Add these fields to the existing Storybook config.
const config = {
  tags: {
    'test-only': { defaultFilterSelection: 'exclude' },
  },
  viteFinal: (viteConfig: InlineConfig) =>
    mergeConfig(viteConfig, {
      resolve: {
        alias: {
          'astro:actions': resolve(import.meta.dirname, '../src/mocks/astro-actions.storybook.ts'),
        },
      },
    }),
}
```

- **Tag filters:** Exclude `test-only` by default to keep interaction-test-only stories out of the sidebar without disabling their tests. Leave `ai-generated` visible by default.
- **Aliases:** With Vite, use `viteFinal` to resolve imports that need a mock in Storybook, such as Astro runtime modules or app clients. Match the production import specifier, point it to an absolute mock path, and preserve existing aliases. Keep these overrides in Storybook so production imports stay unchanged.
