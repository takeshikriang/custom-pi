---
name: storybook-composition
description: Write and review individual Storybook stories for components and full pages. Use when mirroring production pages, showing component states, or adding story interactions.
---

# Writing stories

1. **Follow nearby stories.** Reuse the project's naming, layouts, decorators, and mocks.
2. **Match the production page.** For a full-page story, follow the project’s story hierarchy and match the production page’s layout and visible content. Reuse production components and styles. If Storybook can’t render the route file directly (for example, an Astro route), use a thin wrapper and keep it in sync with the route.
3. **Choose the story’s layout.** Use `parameters: { layout: 'fullscreen' }` for full pages so the page controls its own spacing. If the project defines a `container` layout, use it when a component needs the shared wrapper. If the decorator supports it, set `parameters.className` to adjust that wrapper.
4. **Mock dependencies at the boundary.** Use fixtures for request data and reuse Storybook-only mocks for server-only dependencies or side effects. Keep mocks out of production code.
5. **Show meaningful states.** Start with the default view, then add variants for relevant states such as empty, loading, or error. Use a `play` function for important interactions, and compare page stories with the production page at relevant viewport sizes.

## Story tags

- **`test-only`:** Set `tags: ['test-only']` on an individual story used only as an interaction test. Projects that configure `test-only` as a default sidebar exclusion hide these stories without disabling their tests. Don’t use this tag just because a story has a `play` function.
- **`ai-generated`:** Set `tags: ['ai-generated']` on AI-generated stories. Put it on the default export if every story in the file is AI-generated; otherwise, tag the individual stories. This tag does not hide stories from the sidebar.
