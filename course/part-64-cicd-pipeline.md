# Part 64: CI/CD Pipeline สำหรับ Tailwind Projects

## เป้าหมาย
- GitHub Actions สำหรับ build/test/deploy
- Tailwind CSS purge ใน production build
- Bundle analysis + performance checks
- Deploy to Vercel/Netlify
- Steps 631–640

---

## Step 631: Project Structure สำหรับ CI

```
my-tailwind-app/
├── .github/
│   └── workflows/
│       ├── ci.yml          ← test + lint + build
│       ├── deploy.yml      ← deploy to staging/prod
│       └── visual.yml      ← chromatic visual tests
├── src/
│   ├── app/
│   ├── components/
│   └── styles/
├── e2e/                    ← Playwright tests
├── public/
├── package.json
├── tailwind.config.ts
├── vite.config.ts / next.config.ts
└── playwright.config.ts
```

---

## Step 632: CI Workflow

`.github/workflows/ci.yml`:
```yaml
name: CI

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main, develop]

env:
  NODE_VERSION: '20'

jobs:
  lint:
    name: Lint & Type Check
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run ESLint
        run: npm run lint

      - name: Run TypeScript check
        run: npm run type-check

  test:
    name: Unit & Integration Tests
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run tests with coverage
        run: npm run test:coverage

      - name: Upload coverage to Codecov
        uses: codecov/codecov-action@v4
        with:
          token: ${{ secrets.CODECOV_TOKEN }}
          fail_ci_if_error: false

  build:
    name: Build
    runs-on: ubuntu-latest
    needs: [lint, test]
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Build production
        run: npm run build

      - name: Analyze bundle size
        run: |
          ls -lh dist/assets/*.js | awk '{print $5, $NF}' || true
          du -sh dist/ || true

      - name: Check CSS size
        run: |
          CSS_SIZE=$(ls -la dist/assets/*.css 2>/dev/null | awk '{sum += $5} END {print sum}')
          echo "CSS bundle size: ${CSS_SIZE} bytes"
          if [ "$CSS_SIZE" -gt 50000 ]; then
            echo "::warning::CSS bundle exceeds 50KB (${CSS_SIZE} bytes)"
          fi

      - name: Upload build artifacts
        uses: actions/upload-artifact@v4
        with:
          name: build-output
          path: dist/
          retention-days: 7

  e2e:
    name: E2E Tests
    runs-on: ubuntu-latest
    needs: build
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      - name: Download build artifacts
        uses: actions/download-artifact@v4
        with:
          name: build-output
          path: dist/

      - name: Start preview server
        run: npx serve dist -p 3000 &
        env:
          CI: true

      - name: Wait for server
        run: npx wait-on http://localhost:3000 --timeout 30000

      - name: Run E2E tests
        run: npx playwright test

      - name: Upload Playwright report
        if: failure()
        uses: actions/upload-artifact@v4
        with:
          name: playwright-report
          path: playwright-report/
          retention-days: 7
```

---

## Step 633: Deploy Workflow

`.github/workflows/deploy.yml`:
```yaml
name: Deploy

on:
  push:
    branches:
      - main      # deploy to production
      - develop   # deploy to staging

jobs:
  deploy:
    name: Deploy to Vercel
    runs-on: ubuntu-latest
    environment: ${{ github.ref == 'refs/heads/main' && 'production' || 'staging' }}

    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Build
        run: npm run build
        env:
          NODE_ENV: production

      - name: Deploy to Vercel (Production)
        if: github.ref == 'refs/heads/main'
        uses: amondnet/vercel-action@v25
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
          vercel-args: '--prod'

      - name: Deploy to Vercel (Staging)
        if: github.ref == 'refs/heads/develop'
        uses: amondnet/vercel-action@v25
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}

      - name: Comment PR with preview URL
        if: github.event_name == 'pull_request'
        uses: actions/github-script@v7
        with:
          script: |
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: '🚀 Preview deployed to: ${{ steps.deploy.outputs.preview-url }}'
            })
```

---

## Step 634: Tailwind Production Build Config

`tailwind.config.ts` — Production optimizations:
```ts
import type { Config } from 'tailwindcss'

const config: Config = {
  // Purge เฉพาะ files ที่ใช้จริง
  content: [
    './src/**/*.{ts,tsx}',
    // ระวัง: อย่า include *.css files เพราะจะทำให้ purge ไม่ถูกต้อง
  ],

  // ป้องกัน purge ไม่ถูกต้องสำหรับ dynamic classes
  safelist: [
    // Classes ที่ถูก generate จาก data (ต้อง whitelist ไว้)
    'bg-red-500', 'bg-green-500', 'bg-blue-500',
    { pattern: /bg-(red|green|blue|yellow|indigo)-(100|500|600|700)/ },
    { pattern: /text-(red|green|blue|yellow|indigo)-(600|700)/ },
  ],

  theme: { extend: {} },
  plugins: [],
}

export default config
```

`vite.config.ts` — Production build:
```ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  build: {
    // Code splitting
    rollupOptions: {
      output: {
        manualChunks: {
          vendor: ['react', 'react-dom'],
          ui: ['@radix-ui/react-dialog', '@radix-ui/react-dropdown-menu'],
        },
      },
    },
    // เพิ่ม sourcemap สำหรับ prod debugging (optional)
    sourcemap: false,
    // Target modern browsers เพื่อลด bundle size
    target: 'es2015',
    cssMinify: true,
    minify: 'esbuild',
  },
})
```

---

## Step 635: Bundle Analysis

```bash
# Vite bundle analyzer
npm install -D rollup-plugin-visualizer
```

`vite.config.ts`:
```ts
import { visualizer } from 'rollup-plugin-visualizer'

export default defineConfig({
  plugins: [
    react(),
    visualizer({
      filename: 'dist/stats.html',
      open: false,
      gzipSize: true,
    }),
  ],
})
```

`scripts/check-bundle.sh`:
```bash
#!/bin/bash
npm run build

echo "=== Bundle Sizes ==="
find dist/assets -name "*.js" -exec ls -lh {} \; | awk '{printf "%-60s %s\n", $NF, $5}'
find dist/assets -name "*.css" -exec ls -lh {} \; | awk '{printf "%-60s %s\n", $NF, $5}'

echo ""
echo "=== Total ==="
du -sh dist/

# ตรวจสอบ CSS ต้องน้อยกว่า 50KB
CSS_KB=$(find dist/assets -name "*.css" | xargs cat | wc -c | awk '{printf "%.1f", $1/1024}')
echo "CSS: ${CSS_KB}KB"
```

---

## Step 636: Netlify Deploy Config

`netlify.toml`:
```toml
[build]
  command = "npm run build"
  publish = "dist"

[build.environment]
  NODE_VERSION = "20"
  NPM_FLAGS = "--legacy-peer-deps"

[[headers]]
  for = "/assets/*"
  [headers.values]
    Cache-Control = "public, max-age=31536000, immutable"

[[headers]]
  for = "/*.html"
  [headers.values]
    Cache-Control = "public, max-age=0, must-revalidate"
    X-Frame-Options = "DENY"
    X-Content-Type-Options = "nosniff"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
  conditions = {Role = ["user"]}
```

---

## Step 637: Lighthouse CI

`.github/workflows/lighthouse.yml`:
```yaml
name: Lighthouse CI

on:
  pull_request:
    branches: [main]

jobs:
  lighthouse:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Build
        run: npm run build

      - name: Run Lighthouse CI
        uses: treosh/lighthouse-ci-action@v11
        with:
          urls: |
            http://localhost:3000/
            http://localhost:3000/users
          uploadArtifacts: true
          temporaryPublicStorage: true
          budgetPath: ./lighthouse-budget.json

  # lighthouse-budget.json:
  # [{ "path": "/*", "timings": [
  #   { "metric": "first-contentful-paint", "budget": 1800 },
  #   { "metric": "largest-contentful-paint", "budget": 2500 }
  # ], "resourceSizes": [
  #   { "resourceType": "stylesheet", "budget": 50 }
  # ]}]
```

---

## Steps 638–640: Pre-commit Hooks + Release + Workshop

`.husky/pre-commit`:
```bash
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

# Lint staged files
npx lint-staged

# Type check
npm run type-check --silent
```

`.lintstagedrc`:
```json
{
  "*.{ts,tsx}": ["eslint --fix", "prettier --write"],
  "*.{css,scss}": ["prettier --write"],
  "*.{json,md}": ["prettier --write"]
}
```

Setup:
```bash
npm install -D husky lint-staged
npx husky init
```

`package.json` scripts สมบูรณ์:
```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "type-check": "tsc --noEmit",
    "test": "vitest",
    "test:run": "vitest run",
    "test:coverage": "vitest run --coverage",
    "e2e": "playwright test",
    "e2e:ui": "playwright test --ui",
    "storybook": "storybook dev -p 6006",
    "build-storybook": "storybook build",
    "analyze": "ANALYZE=true npm run build"
  }
}
```

GitHub Actions full matrix:
```
PR Created → lint + type-check + unit tests
           → build + bundle size check
           → E2E tests
           → Lighthouse CI
           → Visual regression (Chromatic)
           → Preview deploy (Vercel)

Merge to main → production deploy
              → tag release
              → update changelog
```

---

## สรุป Part 64

| Step | เนื้อหา |
|------|---------|
| 631 | Project structure สำหรับ CI/CD |
| 632 | ci.yml — lint, test, build, E2E jobs |
| 633 | deploy.yml — Vercel production/staging |
| 634 | Tailwind production config — content, safelist, rollupOptions |
| 635 | Bundle analysis ด้วย rollup-plugin-visualizer |
| 636 | Netlify config — caching headers, redirects |
| 637 | Lighthouse CI — performance budget |
| 638 | Pre-commit hooks ด้วย Husky + lint-staged |
| 639 | Complete package.json scripts |
| 640 | Workshop: Full CI/CD pipeline diagram และ setup guide |

**Part ถัดไป:** Part 65 — Advanced Typography System (Steps 641–650)
