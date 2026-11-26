# Examples

Apply these patterns using the host project's test runner and conventions. The code sample uses Vitest, but the same approach works with other runners.

## Follow the existing test layout

Some projects colocate tests with source files; others use a separate test directory. In the second case, mirroring the source path makes related tests easier to find. For example, under `src/features/cart/`:

```text
utils/parse-quantity.ts
tests/utils/parse-quantity.test.ts
```

## Split a suite when it gets hard to scan

Start with one file. If a suite grows, split it by behavior, such as `parse-quantity.valid.test.ts` and `parse-quantity.invalid.test.ts`. If both files need the same fixtures, move them into a shared helper such as `parse-quantity.test-setup.ts`. Create fresh mutable fixtures for each test.

## Parameterize repeated assertions

Use your runner's parameterized test API when cases share the same assertion. Give each case a name that explains what failed. In Vitest, use `it.each`:

```ts
import { describe, expect, it } from 'vitest'

import { parseQuantity } from '../../utils/parse-quantity'

describe('parseQuantity', () => {
  it('returns a number for a positive whole quantity', () => {
    expect(parseQuantity('3')).toBe(3)
  })

  it.each([
    { input: '', reason: 'a blank quantity' },
    { input: '0', reason: 'zero' },
    { input: '1.5', reason: 'a fractional quantity' },
  ])('rejects $reason', ({ input }) => {
    expect(() => parseQuantity(input)).toThrow(RangeError)
  })
})
```
