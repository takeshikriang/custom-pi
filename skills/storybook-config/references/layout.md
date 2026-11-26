# Storybook layout configuration

`container` is a custom layout value. Add a global decorator in the existing `.storybook/preview.*` file to wrap only stories that use it. The example below uses React and Tailwind; use your renderer and styling conventions.

```tsx
// .storybook/preview.tsx
import type { Preview } from '@storybook/react-vite'

const preview: Preview = {
  parameters: {
    layout: 'container',
    // ...
  },
  decorators: [
    (Story, context) => {
      const { layout, className } = context.parameters

      if (layout !== 'container') return <Story />

      return (
        <div className={['container mx-auto p-6', className].filter(Boolean).join(' ')}>
          <Story />
        </div>
      )
    },
  ],
}

export default preview
```

Set `container` as the preview default if most stories need a container. Otherwise, omit the default so stories can opt in. The layout guard lets fullscreen stories bypass the wrapper.
