---
name: astro-action-composition
description: Build, call, review, and test Astro Actions.
---

# Details

- **Define and register.** Keep handlers with their feature. Use `defineAction` with a Zod input schema, then register the action in the `server` export of `src/actions/index.ts`.
- **Authorize and handle errors.** For protected actions, check the caller’s identity and permissions inside the handler. Throw `ActionError` for expected failures. If you catch an error only to log it, rethrow it unchanged.
- **Call and check the result.** Use `Astro.callAction(action, input)` in Astro frontmatter or `actions.<group>.<name>(input)` on the client. Check `error` before reading `data`. Show a safe message on failure.
- **Test both paths.** Cover success and relevant failures. Direct handler tests skip Astro’s input parsing, so test validation separately.

See the [project example](references/example.md) and Astro’s [guide to calling actions from forms](https://docs.astro.build/en/guides/actions/#call-actions-from-an-html-form).
