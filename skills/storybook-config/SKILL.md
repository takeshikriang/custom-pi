---
name: storybook-config
description: Set up Storybook once for a project, including the framework, story discovery, shared preview, and test integration. Use when initializing Storybook or changing shared configuration, not when writing individual stories.
---

# Storybook configuration

1. **Check for existing setup.** Inspect the workspace's package scripts and `.storybook/` before adding anything. Extend the existing configuration rather than reinitializing it.
2. **Configure the framework and story discovery in `.storybook/main.*`.** Choose the Storybook framework that matches the project’s UI framework and builder. Set story globs to match the project and enable only the addons it needs.
3. **Set up the shared preview in `.storybook/preview.*`.** Load the app's global styles and configure shared decorators and layout defaults. Keep per-story fixtures and layout overrides in story files.
4. **Configure Storybook-only aliases and tag filters as needed.** Keep mock aliases scoped to Storybook so production imports stay unchanged.
5. **Wire up story tests if needed.** For Vitest, add `@storybook/addon-vitest` and add the `storybookTest` plugin to a Vitest project and point the plugin’s `configDir` at the Storybook configuration directory.
6. **Verify setup.** Run the project’s Storybook build and, if configured, its story test suite using the project’s package scripts.

## References

- [Layout configuration](references/layout.md) — shared container decorator and preview defaults.
- [Tags and aliases](references/tags-and-aliases.md) — sidebar filters and Storybook-only mock resolution.
