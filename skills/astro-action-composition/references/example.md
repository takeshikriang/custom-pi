# Example: Create a course

This is an example, not drop-in code. Adapt the file paths, authorization, and data access to your project.
`requireCreator` throws an `ActionError` if access is denied.

Return only what the caller needs, not the full record.
Here, the Drizzle query returns `{ id }` or `undefined` if no row is returned.

```ts
// src/features/course-management/actions/create.ts
import { ActionError, defineAction } from 'astro:actions'
import { z } from 'astro/zod'

import { courseTable } from '../schema'
import { db } from '../db'
import { logger } from '../logger'
import { requireCreator } from '../require-creator'

export const createCourseSchema = z.object({
  title: z.string().trim().min(1).max(100),
  description: z.string().max(250).optional(),
  published: z.boolean().optional(),
})

export const create = defineAction({
  input: createCourseSchema,
  handler: async (input, context) => {
    try {
      await requireCreator(context)

      const course = await db
        .insert(courseTable)
        .values({
          title: input.title,
          description: input.description,
          published: input.published,
        })
        .returning({
          id: courseTable.id
        })
        .get()

      if (!course) {
        throw new ActionError({
          code: 'INTERNAL_SERVER_ERROR',
          message: 'Failed to create course',
        })
      }

      return course
    } catch (error) {
      logger.error('courseManagement.create failed', error)
      throw error
    }
  },
})
```

Register it so callers can use `actions.courseManagement.create`.

```ts
// src/actions/index.ts
import { create } from '../features/course-management/actions/create'

export const server = {
  courseManagement: { create },
} as const
```
