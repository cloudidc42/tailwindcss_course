# Part 92: Deployment & DevOps

## เป้าหมาย
- Vercel deployment
- Docker containerization
- CI/CD pipeline
- Environment variables
- Steps 911–920

---

## Step 911: Vercel Deployment

```bash
npm install -g vercel
vercel login

# Deploy to preview
vercel

# Deploy to production
vercel --prod
```

```json
// vercel.json
{
  "buildCommand": "npm run build",
  "outputDirectory": ".next",
  "framework": "nextjs",
  "regions": ["sin1"],
  "env": {
    "DATABASE_URL": "@database-url",
    "AUTH_SECRET": "@auth-secret",
    "GITHUB_ID": "@github-id",
    "GITHUB_SECRET": "@github-secret"
  },
  "headers": [
    {
      "source": "/api/(.*)",
      "headers": [
        { "key": "X-Frame-Options", "value": "DENY" },
        { "key": "X-Content-Type-Options", "value": "nosniff" }
      ]
    }
  ],
  "rewrites": [
    { "source": "/health", "destination": "/api/health" }
  ]
}
```

---

## Step 912: Environment Variables

```bash
# .env.local (development — never commit)
DATABASE_URL="postgresql://postgres:password@localhost:5432/myapp_dev"
AUTH_SECRET="dev-secret-replace-in-production"
NEXTAUTH_URL="http://localhost:3000"
GITHUB_ID="your-github-client-id"
GITHUB_SECRET="your-github-client-secret"
GOOGLE_ID="your-google-client-id"
GOOGLE_SECRET="your-google-client-secret"
NEXT_PUBLIC_APP_URL="http://localhost:3000"

# .env.example (commit — template for team)
DATABASE_URL=
AUTH_SECRET=
NEXTAUTH_URL=
GITHUB_ID=
GITHUB_SECRET=
GOOGLE_ID=
GOOGLE_SECRET=
NEXT_PUBLIC_APP_URL=
```

```ts
// src/lib/env.ts — type-safe env validation
import { z } from 'zod'

const EnvSchema = z.object({
  DATABASE_URL:       z.string().url(),
  AUTH_SECRET:        z.string().min(32),
  NEXTAUTH_URL:       z.string().url(),
  GITHUB_ID:          z.string(),
  GITHUB_SECRET:      z.string(),
  NODE_ENV:           z.enum(['development', 'test', 'production']).default('development'),
  NEXT_PUBLIC_APP_URL: z.string().url(),
})

const parsed = EnvSchema.safeParse(process.env)
if (!parsed.success) {
  console.error('❌ Invalid environment variables:\n', parsed.error.flatten().fieldErrors)
  throw new Error('Invalid environment configuration')
}

export const env = parsed.data
```

---

## Step 913: Dockerfile

```dockerfile
# Dockerfile
FROM node:20-alpine AS base
RUN apk add --no-cache libc6-compat

# Install dependencies
FROM base AS deps
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci --only=production

# Build
FROM base AS builder
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci
COPY . .
RUN npx prisma generate
ENV NEXT_TELEMETRY_DISABLED=1
RUN npm run build

# Production image
FROM base AS runner
WORKDIR /app
ENV NODE_ENV=production
ENV NEXT_TELEMETRY_DISABLED=1

RUN addgroup --system --gid 1001 nodejs
RUN adduser  --system --uid 1001 nextjs

COPY --from=builder /app/public ./public
COPY --from=builder --chown=nextjs:nodejs /app/.next/standalone ./
COPY --from=builder --chown=nextjs:nodejs /app/.next/static ./.next/static
COPY --from=builder /app/prisma ./prisma
COPY --from=builder /app/node_modules/.prisma ./node_modules/.prisma

USER nextjs
EXPOSE 3000
ENV PORT=3000
ENV HOSTNAME="0.0.0.0"

CMD ["node", "server.js"]
```

```yaml
# docker-compose.yml (development)
version: '3.9'
services:
  app:
    build: .
    ports: ['3000:3000']
    environment:
      DATABASE_URL: postgresql://postgres:postgres@db:5432/myapp
      AUTH_SECRET: dev-secret-at-least-32-chars-long
      NEXTAUTH_URL: http://localhost:3000
    depends_on: [db]
    volumes: ['.:/app', '/app/node_modules', '/app/.next']

  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: myapp
    ports: ['5432:5432']
    volumes: [pgdata:/var/lib/postgresql/data]

volumes:
  pgdata:
```

---

## Step 914: GitHub Actions CI

```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:    { branches: [main, develop] }
  pull_request: { branches: [main] }

jobs:
  test:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:16
        env:
          POSTGRES_USER: postgres
          POSTGRES_PASSWORD: postgres
          POSTGRES_DB: test
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports: ['5432:5432']

    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: '20', cache: 'npm' }
      - run: npm ci
      - run: npx prisma generate
      - run: npx prisma migrate deploy
        env:
          DATABASE_URL: postgresql://postgres:postgres@localhost:5432/test
      - run: npm test -- --coverage
        env:
          DATABASE_URL: postgresql://postgres:postgres@localhost:5432/test
          AUTH_SECRET: test-secret-minimum-32-characters
          NEXTAUTH_URL: http://localhost:3000
      - uses: codecov/codecov-action@v4

  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: '20', cache: 'npm' }
      - run: npm ci
      - run: npx prisma generate
      - run: npm run build
        env:
          DATABASE_URL: postgresql://placeholder:placeholder@localhost:5432/placeholder
          AUTH_SECRET: build-secret-minimum-32-characters
          NEXTAUTH_URL: http://localhost:3000
          NEXT_PUBLIC_APP_URL: http://localhost:3000

  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: '20', cache: 'npm' }
      - run: npm ci
      - run: npm run lint
      - run: npm run typecheck
```

---

## Step 915: CD Pipeline

```yaml
# .github/workflows/deploy.yml
name: Deploy to Production

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    needs: [test, build, lint]  # from ci.yml
    environment: production

    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: '20', cache: 'npm' }
      - run: npm ci
      - run: npm run build
        env:
          DATABASE_URL: ${{ secrets.DATABASE_URL }}
          AUTH_SECRET: ${{ secrets.AUTH_SECRET }}
          NEXTAUTH_URL: ${{ secrets.NEXTAUTH_URL }}

      # Deploy to Vercel
      - uses: amondnet/vercel-action@v25
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
          vercel-args: '--prod'

      # Run migrations
      - name: Run migrations
        run: npx prisma migrate deploy
        env:
          DATABASE_URL: ${{ secrets.DATABASE_URL }}

      # Notify
      - name: Notify Slack
        if: always()
        uses: rtCamp/action-slack-notify@v2
        env:
          SLACK_WEBHOOK: ${{ secrets.SLACK_WEBHOOK }}
          SLACK_MESSAGE: "Deploy ${{ job.status }}: ${{ github.sha }}"
```

---

## Step 916: Health Check Endpoint

```ts
// src/app/api/health/route.ts
import { db }            from '@/lib/db'
import { NextResponse }  from 'next/server'

export const dynamic = 'force-dynamic'

export async function GET() {
  const start = Date.now()
  const checks: Record<string, { status: 'ok'|'error'; latencyMs?: number; message?: string }> = {}

  // Database
  try {
    await db.$queryRaw`SELECT 1`
    checks.database = { status: 'ok', latencyMs: Date.now() - start }
  } catch (err: any) {
    checks.database = { status: 'error', message: err.message }
  }

  const healthy = Object.values(checks).every(c => c.status === 'ok')

  return NextResponse.json(
    { status: healthy ? 'healthy' : 'degraded', checks, version: process.env.npm_package_version, uptime: process.uptime() },
    { status: healthy ? 200 : 503 }
  )
}
```

---

## Step 917: Logging

```ts
// src/lib/logger.ts
type LogLevel = 'debug' | 'info' | 'warn' | 'error'

interface LogEntry {
  level:     LogLevel
  message:   string
  data?:     Record<string, unknown>
  timestamp: string
  requestId?: string
}

function log(level: LogLevel, message: string, data?: Record<string, unknown>) {
  const entry: LogEntry = {
    level,
    message,
    data,
    timestamp: new Date().toISOString(),
  }

  if (process.env.NODE_ENV === 'production') {
    // Structured JSON logging for log aggregators (Datadog, Logtail, etc.)
    process.stdout.write(JSON.stringify(entry) + '\n')
  } else {
    const colors = { debug:'\\x1b[34m', info:'\\x1b[32m', warn:'\\x1b[33m', error:'\\x1b[31m' }
    console[level](`${colors[level]}[${level.toUpperCase()}]\\x1b[0m ${message}`, data ?? '')
  }
}

export const logger = {
  debug: (msg: string, data?: Record<string, unknown>) => log('debug', msg, data),
  info:  (msg: string, data?: Record<string, unknown>) => log('info',  msg, data),
  warn:  (msg: string, data?: Record<string, unknown>) => log('warn',  msg, data),
  error: (msg: string, data?: Record<string, unknown>) => log('error', msg, data),
}
```

---

## Step 918: next.config.ts Production Settings

```ts
// next.config.ts
import type { NextConfig } from 'next'
import { env } from './src/lib/env'

const config: NextConfig = {
  output: 'standalone',  // for Docker

  images: {
    remotePatterns: [{ protocol: 'https', hostname: 'images.example.com' }],
    formats: ['image/avif', 'image/webp'],
  },

  // Security headers
  async headers() {
    return [
      {
        source: '/(.*)',
        headers: [
          { key: 'X-Frame-Options',        value: 'DENY' },
          { key: 'X-Content-Type-Options',  value: 'nosniff' },
          { key: 'Referrer-Policy',         value: 'strict-origin-when-cross-origin' },
          { key: 'Permissions-Policy',      value: 'camera=(), microphone=(), geolocation=()' },
          {
            key: 'Content-Security-Policy',
            value: [
              "default-src 'self'",
              "script-src 'self' 'unsafe-eval' 'unsafe-inline'",
              "style-src 'self' 'unsafe-inline' https://fonts.googleapis.com",
              "img-src 'self' data: https:",
              "font-src 'self' https://fonts.gstatic.com",
              "connect-src 'self' https:",
            ].join('; '),
          },
        ],
      },
    ]
  },

  // Bundle analysis
  ...(process.env.ANALYZE === 'true' ? {
    experimental: { bundlePagesRouterDependencies: true },
  } : {}),
}

export default config
```

---

## Step 919: Monitoring with Sentry

```bash
npm install @sentry/nextjs
npx @sentry/wizard@latest -i nextjs
```

```ts
// sentry.client.config.ts
import * as Sentry from '@sentry/nextjs'

Sentry.init({
  dsn: process.env.NEXT_PUBLIC_SENTRY_DSN,
  tracesSampleRate: 0.1,
  replaysOnErrorSampleRate: 1.0,
  replaysSessionSampleRate: 0.1,
  integrations: [Sentry.replayIntegration()],
  environment: process.env.NODE_ENV,
})

// sentry.server.config.ts
Sentry.init({
  dsn: process.env.NEXT_PUBLIC_SENTRY_DSN,
  tracesSampleRate: 0.1,
})

// Use in error boundaries
try {
  await riskyOperation()
} catch (err) {
  Sentry.captureException(err, { extra: { userId, action: 'createOrder' } })
  throw err
}
```

---

## Step 920: Workshop — Deployment Checklist

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Deployment Checklist</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 min-h-screen p-6">
<div class="max-w-2xl mx-auto">
  <h1 class="text-2xl font-extrabold text-gray-900 mb-6">Production Deployment Checklist</h1>
  <div id="checklist" class="space-y-4"></div>
  <div class="mt-6 bg-white rounded-2xl border p-4 text-center">
    <p class="text-sm text-gray-500 mb-1">Readiness Score</p>
    <p id="score" class="text-4xl font-extrabold text-gray-900">0%</p>
    <div class="w-full bg-gray-200 rounded-full h-2 mt-3">
      <div id="progress-bar" class="bg-indigo-600 h-2 rounded-full transition-all" style="width:0%"></div>
    </div>
  </div>
</div>
<script>
const SECTIONS = [
  { title: 'Security', items: ['AUTH_SECRET is strong (32+ chars)', 'No secrets in codebase (use env vars)', 'Security headers configured', 'CORS policy set', 'Rate limiting enabled'] },
  { title: 'Database',  items: ['DATABASE_URL uses connection pooling', 'Migrations run on deploy', 'Backup policy configured', 'Indexes on frequent query columns'] },
  { title: 'Performance', items: ['Images use next/image with optimization', 'Static assets cached', 'API responses cached where appropriate', 'Bundle size analyzed'] },
  { title: 'Monitoring', items: ['Error tracking (Sentry) configured', 'Health check endpoint /api/health', 'Uptime monitoring set', 'Log aggregation connected'] },
  { title: 'CI/CD', items: ['Tests pass in CI', 'Linting and type-checking in CI', 'Automated deployment on merge to main', 'Rollback strategy documented'] },
];

let checks = {};

function render() {
  const checklist = document.getElementById('checklist');
  checklist.innerHTML = SECTIONS.map(section => `
    <div class="bg-white rounded-2xl border p-5">
      <h2 class="font-bold text-gray-900 mb-3">${section.title}</h2>
      <div class="space-y-2">
        ${section.items.map(item => {
          const id = btoa(item);
          const checked = checks[id] || false;
          return `<label class="flex items-center gap-3 cursor-pointer select-none">
            <input type="checkbox" id="${id}" ${checked?'checked':''} onchange="toggle('${id}')"
              class="w-4 h-4 rounded accent-indigo-600">
            <span class="text-sm ${checked?'text-gray-900 line-through opacity-50':'text-gray-700'}">${item}</span>
          </label>`;
        }).join('')}
      </div>
    </div>`).join('');
  updateScore();
}

function toggle(id) {
  checks[id] = !checks[id];
  render();
}

function updateScore() {
  const all = SECTIONS.flatMap(s => s.items).length;
  const done = Object.values(checks).filter(Boolean).length;
  const pct = Math.round(done / all * 100);
  document.getElementById('score').textContent = pct + '%';
  document.getElementById('score').className = `text-4xl font-extrabold ${pct<50?'text-red-600':pct<80?'text-amber-600':'text-emerald-600'}`;
  document.getElementById('progress-bar').style.width = pct + '%';
  document.getElementById('progress-bar').className = `h-2 rounded-full transition-all ${pct<50?'bg-red-500':pct<80?'bg-amber-500':'bg-emerald-500'}`;
}

render();
</script>
</body>
</html>
```

---

## สรุป Part 92

| Step | เนื้อหา |
|------|---------|
| 911 | Vercel deployment: CLI + vercel.json config |
| 912 | Environment variables + type-safe validation with Zod |
| 913 | Dockerfile multi-stage + docker-compose.yml |
| 914 | GitHub Actions CI: test + build + lint + Postgres service |
| 915 | CD pipeline: deploy to Vercel + migrate + Slack notify |
| 916 | Health check endpoint `/api/health` |
| 917 | Structured logging (JSON production, colored dev) |
| 918 | next.config.ts: standalone output + security headers + CSP |
| 919 | Sentry error tracking: client + server config |
| 920 | Workshop: Interactive production readiness checklist |

**Part ถัดไป:** Part 93 — Advanced Patterns & Architecture (Steps 921–930)
