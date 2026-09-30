# Part 63: Testing Tailwind Components

## เป้าหมาย
- Unit testing ด้วย Vitest + Testing Library
- Visual regression testing ด้วย Storybook + Chromatic
- E2E testing ด้วย Playwright + Tailwind
- Accessibility testing
- Steps 621–630

---

## Step 621: Testing Setup

```bash
# Vitest + Testing Library
npm install -D vitest @testing-library/react @testing-library/user-event @testing-library/jest-dom
npm install -D jsdom

# vite.config.ts
```

`vite.config.ts`:
```ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  test: {
    globals: true,
    environment: 'jsdom',
    setupFiles: ['./src/test/setup.ts'],
    css: true,
  },
})
```

`src/test/setup.ts`:
```ts
import '@testing-library/jest-dom'
```

---

## Step 622: Unit Tests — Button Component

`src/components/Button/Button.test.tsx`:
```tsx
import { render, screen, fireEvent } from '@testing-library/react'
import userEvent from '@testing-library/user-event'
import { describe, it, expect, vi } from 'vitest'
import { Button } from './Button'

describe('Button', () => {
  it('renders with correct text', () => {
    render(<Button>Click me</Button>)
    expect(screen.getByRole('button', { name: /click me/i })).toBeInTheDocument()
  })

  it('applies primary variant classes by default', () => {
    render(<Button>Test</Button>)
    const btn = screen.getByRole('button')
    expect(btn).toHaveClass('bg-indigo-600')
    expect(btn).toHaveClass('text-white')
  })

  it('applies outline variant classes', () => {
    render(<Button variant="outline">Test</Button>)
    const btn = screen.getByRole('button')
    expect(btn).toHaveClass('border-2')
    expect(btn).toHaveClass('border-indigo-600')
  })

  it('applies size classes', () => {
    render(<Button size="lg">Test</Button>)
    expect(screen.getByRole('button')).toHaveClass('h-10')
  })

  it('shows loading spinner and disables button', () => {
    render(<Button loading>Save</Button>)
    const btn = screen.getByRole('button')
    expect(btn).toBeDisabled()
    expect(btn.querySelector('svg')).toHaveClass('animate-spin')
  })

  it('is disabled when disabled prop is true', () => {
    render(<Button disabled>Test</Button>)
    expect(screen.getByRole('button')).toBeDisabled()
  })

  it('calls onClick when clicked', async () => {
    const user = userEvent.setup()
    const handleClick = vi.fn()
    render(<Button onClick={handleClick}>Click</Button>)
    await user.click(screen.getByRole('button'))
    expect(handleClick).toHaveBeenCalledOnce()
  })

  it('does not call onClick when disabled', async () => {
    const user = userEvent.setup()
    const handleClick = vi.fn()
    render(<Button disabled onClick={handleClick}>Click</Button>)
    await user.click(screen.getByRole('button'))
    expect(handleClick).not.toHaveBeenCalled()
  })

  it('merges className with default classes', () => {
    render(<Button className="mt-4">Test</Button>)
    expect(screen.getByRole('button')).toHaveClass('mt-4')
  })
})
```

---

## Step 623: Unit Tests — Input Component

`src/components/Input/Input.test.tsx`:
```tsx
import { render, screen } from '@testing-library/react'
import userEvent from '@testing-library/user-event'
import { describe, it, expect, vi } from 'vitest'
import { Input } from './Input'

describe('Input', () => {
  it('renders with label', () => {
    render(<Input label="Email" />)
    expect(screen.getByLabelText('Email')).toBeInTheDocument()
  })

  it('renders placeholder', () => {
    render(<Input placeholder="กรอกอีเมล" />)
    expect(screen.getByPlaceholderText('กรอกอีเมล')).toBeInTheDocument()
  })

  it('shows error message', () => {
    render(<Input error="ข้อมูลไม่ถูกต้อง" />)
    expect(screen.getByText('ข้อมูลไม่ถูกต้อง')).toBeInTheDocument()
  })

  it('applies error border class when error prop is set', () => {
    render(<Input error="Error" />)
    expect(screen.getByRole('textbox')).toHaveClass('border-red-400')
  })

  it('shows hint text', () => {
    render(<Input hint="ใช้อีเมลที่ลงทะเบียน" />)
    expect(screen.getByText('ใช้อีเมลที่ลงทะเบียน')).toBeInTheDocument()
  })

  it('is disabled when disabled prop is true', () => {
    render(<Input disabled />)
    expect(screen.getByRole('textbox')).toBeDisabled()
  })

  it('calls onChange when typing', async () => {
    const user = userEvent.setup()
    const handleChange = vi.fn()
    render(<Input onChange={handleChange} />)
    await user.type(screen.getByRole('textbox'), 'hello')
    expect(handleChange).toHaveBeenCalled()
  })

  it('has aria-describedby pointing to error element', () => {
    render(<Input label="Email" error="ผิดพลาด" />)
    const input = screen.getByRole('textbox')
    const errorEl = screen.getByText('ผิดพลาด')
    expect(input).toHaveAttribute('aria-describedby', errorEl.id)
  })
})
```

---

## Step 624: Unit Tests — Modal Component

`src/components/Modal/Modal.test.tsx`:
```tsx
import { render, screen } from '@testing-library/react'
import userEvent from '@testing-library/user-event'
import { describe, it, expect, vi } from 'vitest'
import { Modal } from './Modal'

describe('Modal', () => {
  const defaultProps = {
    isOpen: true,
    onClose: vi.fn(),
    title: 'Test Modal',
    children: <p>Modal content</p>,
  }

  it('renders when isOpen is true', () => {
    render(<Modal {...defaultProps} />)
    expect(screen.getByRole('dialog')).toBeInTheDocument()
    expect(screen.getByText('Test Modal')).toBeInTheDocument()
    expect(screen.getByText('Modal content')).toBeInTheDocument()
  })

  it('does not render when isOpen is false', () => {
    render(<Modal {...defaultProps} isOpen={false} />)
    expect(screen.queryByRole('dialog')).not.toBeInTheDocument()
  })

  it('calls onClose when close button is clicked', async () => {
    const user = userEvent.setup()
    const onClose = vi.fn()
    render(<Modal {...defaultProps} onClose={onClose} />)
    await user.click(screen.getByRole('button', { name: /ปิด/i }))
    expect(onClose).toHaveBeenCalledOnce()
  })

  it('calls onClose when Escape key is pressed', async () => {
    const user = userEvent.setup()
    const onClose = vi.fn()
    render(<Modal {...defaultProps} onClose={onClose} />)
    await user.keyboard('{Escape}')
    expect(onClose).toHaveBeenCalledOnce()
  })

  it('has role=dialog and aria-modal', () => {
    render(<Modal {...defaultProps} />)
    const dialog = screen.getByRole('dialog')
    expect(dialog).toHaveAttribute('aria-modal', 'true')
  })

  it('has aria-labelledby pointing to title', () => {
    render(<Modal {...defaultProps} />)
    const dialog = screen.getByRole('dialog')
    const title = screen.getByText('Test Modal')
    expect(dialog.getAttribute('aria-labelledby')).toBe(title.id)
  })
})
```

---

## Step 625: Integration Tests — Form

`src/components/UserForm/UserForm.test.tsx`:
```tsx
import { render, screen, waitFor } from '@testing-library/react'
import userEvent from '@testing-library/user-event'
import { describe, it, expect, vi } from 'vitest'
import { UserForm } from './UserForm'

describe('UserForm', () => {
  it('shows validation errors on empty submit', async () => {
    const user = userEvent.setup()
    render(<UserForm onSubmit={vi.fn()} />)
    await user.click(screen.getByRole('button', { name: /บันทึก/i }))
    expect(await screen.findByText('กรุณากรอกชื่อ')).toBeInTheDocument()
    expect(screen.getByText('กรุณากรอกอีเมล')).toBeInTheDocument()
  })

  it('calls onSubmit with form data when valid', async () => {
    const user = userEvent.setup()
    const onSubmit = vi.fn()
    render(<UserForm onSubmit={onSubmit} />)

    await user.type(screen.getByLabelText('ชื่อ'), 'สมชาย')
    await user.type(screen.getByLabelText('อีเมล'), 'sk@test.com')
    await user.click(screen.getByRole('button', { name: /บันทึก/i }))

    await waitFor(() => {
      expect(onSubmit).toHaveBeenCalledWith({ name: 'สมชาย', email: 'sk@test.com' })
    })
  })

  it('shows invalid email error', async () => {
    const user = userEvent.setup()
    render(<UserForm onSubmit={vi.fn()} />)
    await user.type(screen.getByLabelText('อีเมล'), 'notanemail')
    await user.click(screen.getByRole('button', { name: /บันทึก/i }))
    expect(await screen.findByText('รูปแบบอีเมลไม่ถูกต้อง')).toBeInTheDocument()
  })
})
```

---

## Step 626: Accessibility Testing

```tsx
import { render } from '@testing-library/react'
import { axe, toHaveNoViolations } from 'jest-axe'
import { expect } from 'vitest'

// Setup
expect.extend(toHaveNoViolations)

// Install: npm install -D jest-axe

describe('Accessibility', () => {
  it('Button has no accessibility violations', async () => {
    const { container } = render(<Button>Click me</Button>)
    const results = await axe(container)
    expect(results).toHaveNoViolations()
  })

  it('Input with label has no violations', async () => {
    const { container } = render(<Input label="Email" />)
    const results = await axe(container)
    expect(results).toHaveNoViolations()
  })

  it('Modal has no violations when open', async () => {
    const { container } = render(
      <Modal isOpen title="Test" onClose={vi.fn()}>Content</Modal>
    )
    const results = await axe(container)
    expect(results).toHaveNoViolations()
  })
})
```

---

## Step 627: Playwright E2E Tests

```bash
npm install -D @playwright/test
npx playwright install --with-deps chromium
```

`playwright.config.ts`:
```ts
import { defineConfig } from '@playwright/test'

export default defineConfig({
  testDir: './e2e',
  use: {
    baseURL: 'http://localhost:3000',
    screenshot: 'only-on-failure',
    trace: 'on-first-retry',
  },
  projects: [
    { name: 'chromium', use: { ...devices['Desktop Chrome'] } },
    { name: 'mobile', use: { ...devices['iPhone 14'] } },
  ],
})
```

`e2e/dashboard.spec.ts`:
```ts
import { test, expect } from '@playwright/test'

test.describe('Dashboard', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('/')
  })

  test('shows KPI cards', async ({ page }) => {
    await expect(page.getByText('Revenue')).toBeVisible()
    await expect(page.getByText('Users')).toBeVisible()
  })

  test('sidebar navigation works', async ({ page }) => {
    await page.click('text=Users')
    await expect(page).toHaveURL('/users')
    await expect(page.getByRole('heading', { name: 'Users' })).toBeVisible()
  })

  test('dark mode toggle works', async ({ page }) => {
    const html = page.locator('html')
    await expect(html).not.toHaveClass(/dark/)
    await page.click('[aria-label="Toggle theme"]')
    await expect(html).toHaveClass(/dark/)
  })

  test('mobile hamburger menu works', async ({ page }) => {
    await page.setViewportSize({ width: 375, height: 812 })
    const sidebar = page.getByRole('navigation')
    await expect(sidebar).not.toBeVisible()
    await page.click('[aria-label="Open menu"]')
    await expect(sidebar).toBeVisible()
  })
})
```

`e2e/users.spec.ts`:
```ts
import { test, expect } from '@playwright/test'

test.describe('Users page', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('/users')
  })

  test('search filters users', async ({ page }) => {
    const rows = page.locator('tbody tr')
    const initial = await rows.count()

    await page.fill('[placeholder="ค้นหา..."]', 'สมชาย')
    const filtered = await rows.count()
    expect(filtered).toBeLessThan(initial)
  })

  test('add user modal opens', async ({ page }) => {
    await page.click('button:has-text("เพิ่ม")')
    await expect(page.getByRole('dialog')).toBeVisible()
    await expect(page.getByText('เพิ่มผู้ใช้')).toBeVisible()
  })

  test('add user flow completes', async ({ page }) => {
    await page.click('button:has-text("เพิ่ม")')
    await page.fill('[placeholder="ชื่อ"]', 'ทดสอบ User')
    await page.fill('[placeholder="อีเมล"]', 'test@test.com')
    await page.click('button:has-text("บันทึก")')
    await expect(page.getByRole('dialog')).not.toBeVisible()
    await expect(page.getByText('ทดสอบ User')).toBeVisible()
  })

  test('delete user removes row', async ({ page }) => {
    const rows = page.locator('tbody tr')
    const initial = await rows.count()
    await page.click('tbody tr:first-child button:has-text("ลบ")')
    await expect(rows).toHaveCount(initial - 1)
  })
})
```

---

## Steps 628–630: Visual Regression + Coverage + Workshop

```ts
// Visual regression with Playwright
test('dashboard screenshot', async ({ page }) => {
  await page.goto('/')
  await page.waitForLoadState('networkidle')
  await expect(page).toHaveScreenshot('dashboard.png', {
    maxDiffPixelRatio: 0.01,
  })
})

// Storybook + Chromatic visual testing
// package.json
{
  "scripts": {
    "chromatic": "npx chromatic --project-token=<token>"
  }
}
// Chromatic captures snapshots of every story and alerts on visual changes
```

`vitest.config.ts` — coverage:
```ts
export default defineConfig({
  test: {
    coverage: {
      provider: 'v8',
      reporter: ['text', 'html', 'lcov'],
      include: ['src/components/**'],
      thresholds: {
        lines: 80,
        branches: 70,
        functions: 80,
      },
    },
  },
})
```

```bash
# Run tests
npm run test                    # watch mode
npm run test -- --run           # single run
npm run test -- --coverage      # with coverage

# Run E2E
npx playwright test
npx playwright test --ui        # visual UI mode
npx playwright show-report      # view HTML report
```

---

## สรุป Part 63

| Step | เนื้อหา |
|------|---------|
| 621 | Vitest + Testing Library setup, globals, jsdom |
| 622 | Button unit tests — render, classes, events, loading, disabled |
| 623 | Input unit tests — label, error, hint, disabled, onChange |
| 624 | Modal unit tests — isOpen, onClose, Escape, ARIA |
| 625 | Integration tests — form validation + submission flow |
| 626 | Accessibility testing ด้วย jest-axe |
| 627 | Playwright E2E — navigation, dark mode, mobile |
| 628 | Users E2E — search, add, delete flows |
| 629 | Visual regression — Playwright screenshots + Chromatic |
| 630 | Coverage thresholds + test run commands |

**Part ถัดไป:** Part 64 — CI/CD Pipeline สำหรับ Tailwind Projects (Steps 631–640)
