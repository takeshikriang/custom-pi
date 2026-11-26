---
name: test-composition
description: Write, review, and run software tests. Use when choosing test coverage or adding regression tests.
---

# Details

1. **Use the existing test setup.** Check nearby tests and documented commands for the runner, file placement, naming, and fixture conventions.
2. **Choose the smallest test that exercises the behavior.** Use unit tests for isolated logic, integration tests for behavior that depends on components working together, and end-to-end tests for critical user flows.
3. **Assert observable outcomes.** Prefer public behavior over private state or internal call order. Cover the expected result and relevant edge cases and failures. For a bug fix, verify the regression test fails before the fix when practical.
4. **Keep tests readable and independent.** Name each test after the behavior it verifies. Parameterize cases that share assertions, and avoid relying on shared mutable state or test order.
5. **Run targeted tests first.** Then run the relevant broader suite. Report failures and anything you couldn't run.

See [testing examples](references/examples.md) for file layout and parameterized tests.
