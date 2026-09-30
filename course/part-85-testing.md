# Part 85: Testing & Quality Assurance

## เป้าหมาย
- Unit testing components ด้วย Vitest + Testing Library
- Visual regression testing
- Storybook component docs
- Playwright E2E tests
- Steps 841–850

---

## Step 841: Unit Testing Setup

```bash
# Vitest + React Testing Library
npm install -D vitest @testing-library/react @testing-library/user-event jsdom @vitejs/plugin-react

# vite.config.ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  test: {
    environment: 'jsdom',
    globals: true,
    setupFiles: './src/test/setup.ts',
  },
})
```

```ts
// src/test/setup.ts
import '@testing-library/jest-dom'
```

---

## Step 842: Testing Button Component

```tsx
// src/components/Button.tsx
interface ButtonProps {
  variant?: 'primary' | 'secondary' | 'destructive'
  disabled?: boolean
  loading?: boolean
  onClick?: () => void
  children: React.ReactNode
}

export function Button({ variant = 'primary', disabled, loading, onClick, children }: ButtonProps) {
  const base = 'inline-flex items-center justify-center px-4 py-2 text-sm font-semibold rounded-xl transition-all focus-visible:ring-2 focus-visible:ring-offset-2'
  const variants = {
    primary:     'bg-indigo-600 text-white hover:bg-indigo-700 focus-visible:ring-indigo-500',
    secondary:   'bg-gray-100 text-gray-800 hover:bg-gray-200 focus-visible:ring-gray-400',
    destructive: 'bg-red-600 text-white hover:bg-red-700 focus-visible:ring-red-500',
  }
  return (
    <button
      className={`${base} ${variants[variant]} ${(disabled || loading) ? 'opacity-50 cursor-not-allowed' : ''}`}
      disabled={disabled || loading}
      onClick={onClick}
    >
      {loading && <span className="w-4 h-4 border-2 border-current border-t-transparent rounded-full animate-spin mr-2" />}
      {children}
    </button>
  )
}
```

```tsx
// src/components/Button.test.tsx
import { describe, it, expect, vi } from 'vitest'
import { render, screen } from '@testing-library/react'
import userEvent from '@testing-library/user-event'
import { Button } from './Button'

describe('Button', () => {
  it('renders children', () => {
    render(<Button>Click me</Button>)
    expect(screen.getByRole('button', { name: 'Click me' })).toBeInTheDocument()
  })

  it('calls onClick when clicked', async () => {
    const handler = vi.fn()
    render(<Button onClick={handler}>Click</Button>)
    await userEvent.click(screen.getByRole('button'))
    expect(handler).toHaveBeenCalledOnce()
  })

  it('does not call onClick when disabled', async () => {
    const handler = vi.fn()
    render(<Button disabled onClick={handler}>Click</Button>)
    await userEvent.click(screen.getByRole('button'))
    expect(handler).not.toHaveBeenCalled()
  })

  it('shows spinner when loading', () => {
    render(<Button loading>Click</Button>)
    expect(screen.getByRole('button')).toBeDisabled()
    // spinner element present
    expect(document.querySelector('.animate-spin')).toBeTruthy()
  })

  it('applies variant classes', () => {
    const { rerender } = render(<Button variant="primary">P</Button>)
    expect(screen.getByRole('button')).toHaveClass('bg-indigo-600')
    rerender(<Button variant="destructive">D</Button>)
    expect(screen.getByRole('button')).toHaveClass('bg-red-600')
  })
})
```

---

## Step 843: Testing Form Components

```tsx
// src/components/Input.tsx
interface InputProps {
  label: string
  id: string
  error?: string
  required?: boolean
}
export function Input({ label, id, error, required, ...props }: InputProps & React.InputHTMLAttributes<HTMLInputElement>) {
  return (
    <div>
      <label htmlFor={id} className="block text-sm font-medium text-gray-700 mb-1.5">
        {label}{required && <span className="text-red-500 ml-1">*</span>}
      </label>
      <input
        id={id}
        aria-invalid={!!error}
        aria-describedby={error ? `${id}-error` : undefined}
        className={`w-full px-3 py-2 border rounded-xl text-sm focus:outline-none focus:ring-2 ${
          error ? 'border-red-400 focus:ring-red-400' : 'border-gray-300 focus:ring-indigo-500'
        }`}
        {...props}
      />
      {error && <p id={`${id}-error`} role="alert" className="mt-1 text-sm text-red-600">{error}</p>}
    </div>
  )
}
```

```tsx
// src/components/Input.test.tsx
describe('Input', () => {
  it('renders label + input', () => {
    render(<Input id="name" label="Full Name" />)
    expect(screen.getByLabelText('Full Name')).toBeInTheDocument()
  })

  it('shows required indicator', () => {
    render(<Input id="email" label="Email" required />)
    expect(screen.getByText('*')).toBeInTheDocument()
  })

  it('shows error message with aria-invalid', () => {
    render(<Input id="email" label="Email" error="Invalid email" />)
    const input = screen.getByLabelText('Email')
    expect(input).toHaveAttribute('aria-invalid', 'true')
    expect(screen.getByRole('alert')).toHaveTextContent('Invalid email')
  })

  it('has no error state by default', () => {
    render(<Input id="name" label="Name" />)
    expect(screen.getByLabelText('Name')).not.toHaveAttribute('aria-invalid', 'true')
    expect(screen.queryByRole('alert')).not.toBeInTheDocument()
  })
})
```

---

## Step 844: Testing Hooks

```tsx
// src/hooks/useDebounce.ts
import { useState, useEffect } from 'react'

export function useDebounce<T>(value: T, delay: number): T {
  const [debouncedValue, setDebouncedValue] = useState(value)
  useEffect(() => {
    const timer = setTimeout(() => setDebouncedValue(value), delay)
    return () => clearTimeout(timer)
  }, [value, delay])
  return debouncedValue
}
```

```tsx
// src/hooks/useDebounce.test.ts
import { renderHook, act } from '@testing-library/react'
import { vi, describe, it, expect, beforeEach, afterEach } from 'vitest'
import { useDebounce } from './useDebounce'

describe('useDebounce', () => {
  beforeEach(() => vi.useFakeTimers())
  afterEach(() => vi.useRealTimers())

  it('returns initial value immediately', () => {
    const { result } = renderHook(() => useDebounce('hello', 500))
    expect(result.current).toBe('hello')
  })

  it('delays value update', () => {
    const { result, rerender } = renderHook(({ v }) => useDebounce(v, 500), { initialProps: { v: 'hello' } })
    rerender({ v: 'world' })
    expect(result.current).toBe('hello') // still old value
    act(() => vi.advanceTimersByTime(500))
    expect(result.current).toBe('world')
  })

  it('cancels pending update on quick re-renders', () => {
    const { result, rerender } = renderHook(({ v }) => useDebounce(v, 300), { initialProps: { v: 'a' } })
    rerender({ v: 'b' })
    act(() => vi.advanceTimersByTime(100))
    rerender({ v: 'c' })
    act(() => vi.advanceTimersByTime(300))
    expect(result.current).toBe('c')
  })
})
```

---

## Step 845: Storybook Setup

```bash
npx storybook@latest init
# เลือก React + Vite
```

```tsx
// src/stories/Button.stories.tsx
import type { Meta, StoryObj } from '@storybook/react'
import { Button } from '../components/Button'

const meta: Meta<typeof Button> = {
  title: 'Components/Button',
  component: Button,
  parameters: { layout: 'centered' },
  tags: ['autodocs'],
  argTypes: {
    variant: { control: 'select', options: ['primary', 'secondary', 'destructive'] },
    disabled: { control: 'boolean' },
    loading:  { control: 'boolean' },
    onClick:  { action: 'clicked' },
  },
}
export default meta
type Story = StoryObj<typeof meta>

export const Primary: Story = { args: { children: 'Primary Button', variant: 'primary' } }
export const Secondary: Story = { args: { children: 'Secondary Button', variant: 'secondary' } }
export const Destructive: Story = { args: { children: 'Delete', variant: 'destructive' } }
export const Loading: Story = { args: { children: 'Saving...', loading: true } }
export const Disabled: Story = { args: { children: 'Disabled', disabled: true } }
```

---

## Step 846: Visual Regression Testing

```bash
npm install -D @storybook/test-runner chromatic
```

```bash
# Chromatic: visual regression บน Storybook
npx chromatic --project-token=<your-token>

# หรือใช้ Percy
npm install -D @percy/cli @percy/storybook
npx percy storybook ./storybook-static
```

```js
// .storybook/test-runner.ts
import type { TestRunnerConfig } from '@storybook/test-runner'
import { toMatchImageSnapshot } from 'jest-image-snapshot'

const config: TestRunnerConfig = {
  async postVisit(page, context) {
    // Screenshot ทุก story
    const image = await page.screenshot()
    expect(image).toMatchImageSnapshot({
      failureThreshold: 0.01,
      failureThresholdType: 'percent',
    })
  },
}
export default config
```

---

## Step 847: Playwright E2E Tests

```bash
npm install -D @playwright/test
npx playwright install
```

```ts
// tests/auth.spec.ts
import { test, expect } from '@playwright/test'

test.describe('Authentication', () => {
  test('login page renders correctly', async ({ page }) => {
    await page.goto('/login')
    await expect(page.getByRole('heading', { name: 'Sign in' })).toBeVisible()
    await expect(page.getByLabel('Email')).toBeVisible()
    await expect(page.getByLabel('Password')).toBeVisible()
    await expect(page.getByRole('button', { name: 'Sign in' })).toBeVisible()
  })

  test('shows error on invalid credentials', async ({ page }) => {
    await page.goto('/login')
    await page.getByLabel('Email').fill('wrong@example.com')
    await page.getByLabel('Password').fill('wrongpassword')
    await page.getByRole('button', { name: 'Sign in' }).click()
    await expect(page.getByRole('alert')).toContainText('Invalid credentials')
  })

  test('redirects to dashboard after login', async ({ page }) => {
    await page.goto('/login')
    await page.getByLabel('Email').fill('user@example.com')
    await page.getByLabel('Password').fill('password123')
    await page.getByRole('button', { name: 'Sign in' }).click()
    await expect(page).toHaveURL('/dashboard')
  })
})
```

---

## Step 848: Playwright Component Tests

```ts
// tests/components.spec.ts
import { test, expect } from '@playwright/test'

test.describe('Modal', () => {
  test('opens and closes correctly', async ({ page }) => {
    await page.goto('/components')
    const openBtn = page.getByRole('button', { name: 'Open Modal' })
    await openBtn.click()
    const modal = page.getByRole('dialog')
    await expect(modal).toBeVisible()
    // Close via Escape
    await page.keyboard.press('Escape')
    await expect(modal).not.toBeVisible()
  })

  test('focus trapped inside modal', async ({ page }) => {
    await page.goto('/components')
    await page.getByRole('button', { name: 'Open Modal' }).click()
    const firstFocusable = page.getByRole('dialog').getByRole('button').first()
    await expect(firstFocusable).toBeFocused()
    // Tab through all focusable elements — should stay inside
    const focusableCount = await page.getByRole('dialog').getByRole('button').count()
    for (let i = 0; i < focusableCount + 1; i++) await page.keyboard.press('Tab')
    // Focus should be on first focusable inside modal (wrapped)
    await expect(firstFocusable).toBeFocused()
  })
})

test.describe('Form validation', () => {
  test('shows errors on empty submit', async ({ page }) => {
    await page.goto('/register')
    await page.getByRole('button', { name: 'Create account' }).click()
    const errors = page.getByRole('alert')
    await expect(errors.first()).toBeVisible()
  })
})
```

---

## Step 849: CI/CD Integration

```yaml
# .github/workflows/test.yml
name: Test

on: [push, pull_request]

jobs:
  unit-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: '20', cache: 'npm' }
      - run: npm ci
      - run: npm test -- --coverage
      - uses: codecov/codecov-action@v4

  e2e-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: '20', cache: 'npm' }
      - run: npm ci
      - run: npx playwright install --with-deps
      - run: npm run build
      - run: npx playwright test
      - uses: actions/upload-artifact@v4
        if: failure()
        with:
          name: playwright-report
          path: playwright-report/

  visual-regression:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: '20', cache: 'npm' }
      - run: npm ci
      - run: npm run build-storybook
      - run: npx chromatic --project-token=${{ secrets.CHROMATIC_PROJECT_TOKEN }}
```

---

## Step 850: Workshop — Test Coverage Report

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Test Coverage</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 min-h-screen p-6">
<div class="max-w-3xl mx-auto">
  <h1 class="text-xl font-extrabold text-gray-900 mb-6">Test Coverage Dashboard</h1>
  <div class="grid grid-cols-2 md:grid-cols-4 gap-3 mb-6">
    <div class="bg-white rounded-2xl border p-4 text-center">
      <div class="text-3xl font-extrabold text-emerald-600 mb-1">94%</div>
      <p class="text-xs text-gray-500 font-medium">Statements</p>
    </div>
    <div class="bg-white rounded-2xl border p-4 text-center">
      <div class="text-3xl font-extrabold text-emerald-600 mb-1">89%</div>
      <p class="text-xs text-gray-500 font-medium">Branches</p>
    </div>
    <div class="bg-white rounded-2xl border p-4 text-center">
      <div class="text-3xl font-extrabold text-emerald-600 mb-1">96%</div>
      <p class="text-xs text-gray-500 font-medium">Functions</p>
    </div>
    <div class="bg-white rounded-2xl border p-4 text-center">
      <div class="text-3xl font-extrabold text-amber-500 mb-1">82%</div>
      <p class="text-xs text-gray-500 font-medium">Lines</p>
    </div>
  </div>
  <div class="bg-white rounded-2xl border overflow-hidden">
    <div class="px-5 py-3.5 border-b bg-gray-50 flex items-center justify-between">
      <p class="font-semibold text-gray-900 text-sm">Files</p>
      <div class="flex gap-4 text-xs font-semibold text-gray-500">
        <span>Stmts</span><span>Branch</span><span>Funcs</span><span>Lines</span>
      </div>
    </div>
    <div id="files-list"></div>
  </div>
</div>
<script>
  const files = [
    { path:'components/Button.tsx',    stmts:100, branch:100, funcs:100, lines:100 },
    { path:'components/Input.tsx',     stmts:96,  branch:87,  funcs:100, lines:95  },
    { path:'components/Modal.tsx',     stmts:90,  branch:85,  funcs:90,  lines:88  },
    { path:'hooks/useDebounce.ts',     stmts:100, branch:100, funcs:100, lines:100 },
    { path:'hooks/useLocalStorage.ts', stmts:88,  branch:75,  funcs:90,  lines:85  },
    { path:'utils/formatDate.ts',      stmts:100, branch:100, funcs:100, lines:100 },
  ];
  function pct(n) {
    const c = n >= 90 ? 'text-emerald-600' : n >= 75 ? 'text-amber-500' : 'text-red-600';
    return `<span class="${c} font-semibold">${n}%</span>`;
  }
  document.getElementById('files-list').innerHTML = files.map(f => `
    <div class="px-5 py-3 flex items-center justify-between border-b last:border-0 hover:bg-gray-50">
      <p class="text-sm text-gray-700 font-mono">${f.path}</p>
      <div class="flex gap-4 text-xs">${pct(f.stmts)}${pct(f.branch)}${pct(f.funcs)}${pct(f.lines)}</div>
    </div>`).join('');
</script>
</body>
</html>
```

---

## สรุป Part 85

| Step | เนื้อหา |
|------|---------|
| 841 | Vitest + Testing Library setup |
| 842 | Unit test: Button component (5 tests) |
| 843 | Unit test: Input component with accessibility |
| 844 | Hook testing: useDebounce with fake timers |
| 845 | Storybook — stories + autodocs |
| 846 | Visual regression — Chromatic/Percy |
| 847 | Playwright E2E — auth flows |
| 848 | Playwright — modal focus trap, form validation |
| 849 | CI/CD — GitHub Actions test pipeline |
| 850 | Workshop: Test coverage dashboard |

**Part ถัดไป:** Part 86 — Internationalization (i18n) (Steps 851–860)
