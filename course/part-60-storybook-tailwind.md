# Part 60: Storybook + Tailwind Component Documentation

## เป้าหมาย
- Storybook 7+ setup กับ Next.js/React + Tailwind
- CSF3 (Component Story Format)
- Args, ArgTypes, Controls
- Documentation pages
- Steps 591–600

---

## Step 591: Storybook Setup

```bash
# ใน Next.js project ที่มี Tailwind อยู่แล้ว
npx storybook@latest init

# Storybook จะติดตั้ง:
# @storybook/react-webpack5 หรือ @storybook/nextjs
# @storybook/addon-essentials (controls, actions, docs, viewport)
# @storybook/addon-interactions
```

`.storybook/main.ts`:
```ts
import type { StorybookConfig } from '@storybook/nextjs'

const config: StorybookConfig = {
  stories: ['../src/**/*.mdx', '../src/**/*.stories.@(js|jsx|mjs|ts|tsx)'],
  addons: [
    '@storybook/addon-links',
    '@storybook/addon-essentials',
    '@storybook/addon-interactions',
  ],
  framework: {
    name: '@storybook/nextjs',
    options: {},
  },
  docs: { autodocs: 'tag' },
}

export default config
```

`.storybook/preview.ts`:
```ts
import type { Preview } from '@storybook/react'
import '../src/app/globals.css'  // import Tailwind

const preview: Preview = {
  parameters: {
    actions: { argTypesRegex: '^on[A-Z].*' },
    controls: {
      matchers: {
        color: /(background|color)$/i,
        date: /Date$/,
      },
    },
    backgrounds: {
      default: 'light',
      values: [
        { name: 'light', value: '#f9fafb' },
        { name: 'white', value: '#ffffff' },
        { name: 'dark', value: '#111827' },
      ],
    },
  },
}

export default preview
```

---

## Step 592: Button Stories (CSF3)

`src/components/Button/Button.stories.tsx`:
```tsx
import type { Meta, StoryObj } from '@storybook/react'
import { Button } from './Button'

const meta: Meta<typeof Button> = {
  title: 'Components/Button',
  component: Button,
  tags: ['autodocs'],
  argTypes: {
    variant: {
      control: 'select',
      options: ['primary', 'outline', 'ghost', 'danger', 'subtle'],
      description: 'Visual style ของ button',
    },
    size: {
      control: 'select',
      options: ['xs', 'sm', 'md', 'lg', 'xl', 'icon'],
      description: 'ขนาดของ button',
    },
    loading: {
      control: 'boolean',
      description: 'แสดง loading spinner',
    },
    disabled: {
      control: 'boolean',
    },
    children: {
      control: 'text',
    },
  },
  args: {
    children: 'Click me',
    variant: 'primary',
    size: 'md',
    loading: false,
    disabled: false,
  },
}

export default meta
type Story = StoryObj<typeof Button>

// Basic stories
export const Primary: Story = {}

export const Outline: Story = {
  args: { variant: 'outline' },
}

export const Ghost: Story = {
  args: { variant: 'ghost' },
}

export const Danger: Story = {
  args: { variant: 'danger', children: 'ลบรายการ' },
}

// Size stories
export const Small: Story = {
  args: { size: 'sm', children: 'Small Button' },
}

export const Large: Story = {
  args: { size: 'lg', children: 'Large Button' },
}

// State stories
export const Loading: Story = {
  args: { loading: true, children: 'กำลังบันทึก' },
}

export const Disabled: Story = {
  args: { disabled: true, children: 'Disabled' },
}

// Showcase: all variants
export const AllVariants: Story = {
  render: () => (
    <div className="flex flex-wrap gap-3 p-4 bg-gray-50 rounded-2xl">
      <Button variant="primary">Primary</Button>
      <Button variant="outline">Outline</Button>
      <Button variant="ghost">Ghost</Button>
      <Button variant="danger">Danger</Button>
      <Button variant="subtle">Subtle</Button>
    </div>
  ),
}

// Showcase: all sizes
export const AllSizes: Story = {
  render: () => (
    <div className="flex flex-wrap items-center gap-3 p-4">
      <Button size="xs">XSmall</Button>
      <Button size="sm">Small</Button>
      <Button size="md">Medium</Button>
      <Button size="lg">Large</Button>
      <Button size="xl">XLarge</Button>
    </div>
  ),
}
```

---

## Step 593: Badge Stories

`src/components/Badge/Badge.stories.tsx`:
```tsx
import type { Meta, StoryObj } from '@storybook/react'
import { Badge } from './Badge'

const meta: Meta<typeof Badge> = {
  title: 'Components/Badge',
  component: Badge,
  tags: ['autodocs'],
  argTypes: {
    variant: {
      control: 'select',
      options: ['default', 'success', 'warning', 'danger', 'outline'],
    },
    size: {
      control: 'select',
      options: ['sm', 'md', 'lg'],
    },
    children: { control: 'text' },
  },
  args: {
    children: 'Badge',
    variant: 'default',
    size: 'md',
  },
}

export default meta
type Story = StoryObj<typeof Badge>

export const Default: Story = {}
export const Success: Story = { args: { variant: 'success', children: 'Active' } }
export const Warning: Story = { args: { variant: 'warning', children: 'Pending' } }
export const Danger: Story = { args: { variant: 'danger', children: 'Error' } }

export const Showcase: Story = {
  render: () => (
    <div className="flex flex-wrap gap-2 p-4">
      <Badge variant="default">Default</Badge>
      <Badge variant="success">Success</Badge>
      <Badge variant="warning">Warning</Badge>
      <Badge variant="danger">Danger</Badge>
      <Badge variant="outline">Outline</Badge>
    </div>
  ),
}
```

---

## Step 594: Input Stories

`src/components/Input/Input.stories.tsx`:
```tsx
import type { Meta, StoryObj } from '@storybook/react'
import { Input } from './Input'

const meta: Meta<typeof Input> = {
  title: 'Components/Input',
  component: Input,
  tags: ['autodocs'],
  argTypes: {
    label: { control: 'text' },
    placeholder: { control: 'text' },
    error: { control: 'text' },
    hint: { control: 'text' },
    disabled: { control: 'boolean' },
    type: {
      control: 'select',
      options: ['text', 'email', 'password', 'number', 'tel'],
    },
  },
  args: {
    label: 'อีเมล',
    placeholder: 'กรอกอีเมล...',
    type: 'text',
  },
}

export default meta
type Story = StoryObj<typeof Input>

export const Default: Story = {}

export const WithError: Story = {
  args: {
    error: 'รูปแบบอีเมลไม่ถูกต้อง',
    defaultValue: 'invalid@',
  },
}

export const WithHint: Story = {
  args: { hint: 'ใช้อีเมลที่คุณใช้สมัครสมาชิก' },
}

export const Disabled: Story = {
  args: { disabled: true, defaultValue: 'disabled@email.com' },
}

export const Password: Story = {
  args: { type: 'password', label: 'รหัสผ่าน', placeholder: 'กรอกรหัสผ่าน' },
}
```

---

## Step 595: Card Stories

`src/components/Card/Card.stories.tsx`:
```tsx
import type { Meta, StoryObj } from '@storybook/react'
import { Card, CardHeader, CardTitle, CardDescription, CardContent, CardFooter } from './Card'
import { Button } from '../Button/Button'
import { Badge } from '../Badge/Badge'

const meta: Meta<typeof Card> = {
  title: 'Components/Card',
  component: Card,
  tags: ['autodocs'],
}

export default meta
type Story = StoryObj<typeof Card>

export const Simple: Story = {
  render: () => (
    <Card className="w-72">
      <CardContent>
        <p className="text-sm text-gray-600">เนื้อหาใน Card</p>
      </CardContent>
    </Card>
  ),
}

export const WithHeader: Story = {
  render: () => (
    <Card className="w-72">
      <CardHeader>
        <CardTitle>ชื่อ Card</CardTitle>
        <CardDescription>คำอธิบาย Card</CardDescription>
      </CardHeader>
      <CardContent>
        <p className="text-sm text-gray-600">เนื้อหาหลักของ Card</p>
      </CardContent>
    </Card>
  ),
}

export const WithFooter: Story = {
  render: () => (
    <Card className="w-72">
      <CardHeader>
        <div className="flex items-center justify-between">
          <CardTitle>ตั้งค่าโปรไฟล์</CardTitle>
          <Badge variant="success">Active</Badge>
        </div>
        <CardDescription>อัปเดตข้อมูลส่วนตัวของคุณ</CardDescription>
      </CardHeader>
      <CardContent>
        <p className="text-sm text-gray-600">เนื้อหาฟอร์ม...</p>
      </CardContent>
      <CardFooter className="flex gap-2">
        <Button variant="outline" size="sm">ยกเลิก</Button>
        <Button size="sm">บันทึก</Button>
      </CardFooter>
    </Card>
  ),
}
```

---

## Step 596: MDX Documentation Page

`src/components/Button/Button.mdx`:
```mdx
import { Meta, Story, Canvas, Controls, ArgTypes } from '@storybook/blocks'
import * as ButtonStories from './Button.stories'

<Meta of={ButtonStories} />

# Button

Component สำหรับปุ่มกดที่ใช้ทั่วทั้งระบบ สร้างด้วย `cva` (class-variance-authority) เพื่อจัดการ variant อย่างมีระบบ

## Installation

```bash
npm install class-variance-authority clsx tailwind-merge
```

## Usage

```tsx
import { Button } from '@/components/Button'

<Button variant="primary" size="md">Click me</Button>
<Button variant="outline" loading>Loading...</Button>
```

## Examples

### All Variants

<Canvas of={ButtonStories.AllVariants} />

### All Sizes

<Canvas of={ButtonStories.AllSizes} />

### Loading State

<Canvas of={ButtonStories.Loading} />

## Controls

<Controls />

## Props

<ArgTypes of={ButtonStories} />

## Accessibility

- ใช้ `<button>` element จริง ไม่ใช่ `<div>`
- Support `disabled` attribute
- `focus-visible:ring-2` สำหรับ keyboard navigation
- `aria-disabled` เมื่ออยู่ใน loading state
```

---

## Step 597: Interaction Testing

`src/components/Button/Button.stories.tsx` — เพิ่ม interaction test:
```tsx
import { expect, userEvent, within } from '@storybook/test'

export const ClickInteraction: Story = {
  args: { children: 'Click me' },
  play: async ({ canvasElement, args }) => {
    const canvas = within(canvasElement)
    const button = canvas.getByRole('button')

    await userEvent.click(button)

    // ตรวจสอบว่า onClick ถูกเรียก
    await expect(args.onClick).toHaveBeenCalledOnce()
  },
}

export const LoadingState: Story = {
  play: async ({ canvasElement }) => {
    const canvas = within(canvasElement)
    const button = canvas.getByRole('button')

    // ตรวจสอบว่า button disabled เมื่อ loading
    await expect(button).toBeDisabled()
  },
  args: { loading: true },
}
```

---

## Steps 598–600: Decorator + Theme + Workshop

`.storybook/preview.ts` — Theme decorator:
```ts
import type { Preview, Decorator } from '@storybook/react'

const withTheme: Decorator = (Story, context) => {
  const theme = context.globals.theme || 'light'
  return (
    <div data-theme={theme} className={theme === 'dark' ? 'dark bg-gray-900 min-h-screen' : 'bg-gray-50 min-h-screen'}>
      <div className="p-8">
        <Story />
      </div>
    </div>
  )
}

const preview: Preview = {
  decorators: [withTheme],
  globalTypes: {
    theme: {
      description: 'Theme',
      defaultValue: 'light',
      toolbar: {
        title: 'Theme',
        icon: 'circlehollow',
        items: [
          { value: 'light', title: 'Light', icon: 'sun' },
          { value: 'dark', title: 'Dark', icon: 'moon' },
        ],
        dynamicTitle: true,
      },
    },
  },
  parameters: {
    actions: { argTypesRegex: '^on[A-Z].*' },
    controls: { expanded: true },
    layout: 'centered',
  },
}

export default preview
```

Storybook structure สมบูรณ์:
```
src/
  components/
    Button/
      Button.tsx
      Button.stories.tsx
      Button.mdx
    Badge/
      Badge.tsx
      Badge.stories.tsx
    Card/
      Card.tsx
      Card.stories.tsx
    Input/
      Input.tsx
      Input.stories.tsx
    Modal/
      Modal.tsx
      Modal.stories.tsx
    Table/
      Table.tsx
      Table.stories.tsx
    Avatar/
      Avatar.tsx
      Avatar.stories.tsx

.storybook/
  main.ts
  preview.ts
```

คำสั่งใช้งาน:
```bash
# รัน Storybook development server
npm run storybook

# Build static Storybook
npm run build-storybook

# ดูที่ http://localhost:6006
```

---

## สรุป Part 60

| Step | เนื้อหา |
|------|---------|
| 591 | Storybook 7 setup กับ Next.js + Tailwind |
| 592 | Button stories — Primary/Outline/Ghost/Danger, AllVariants, AllSizes |
| 593 | Badge stories — all variants showcase |
| 594 | Input stories — Default/Error/Hint/Disabled |
| 595 | Card stories — Simple/WithHeader/WithFooter |
| 596 | MDX documentation page |
| 597 | Interaction testing ด้วย @storybook/test |
| 598 | Theme decorator — dark/light toggle ใน toolbar |
| 599 | globalTypes + toolbar + layout: centered |
| 600 | Workshop: Complete Storybook design system documentation |

**Level 5 เสร็จสมบูรณ์ (Steps 501–600) — Framework Integrations**

**Part ถัดไป:** Part 61 — Design Tokens System (Steps 601–610)
