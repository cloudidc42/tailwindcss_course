# Part 99: Production Patterns & Final Review

## เป้าหมาย
- Production-ready patterns รวมทุก concept
- Error recovery strategies
- Code review checklist
- Full-stack workflow summary
- Steps 981–990

---

## Step 981: Error Boundary Strategy

```tsx
// src/components/ErrorBoundary.tsx — Class component (required for error boundaries)
import React, { Component, type ReactNode } from 'react'
import * as Sentry from '@sentry/nextjs'

interface Props { children: ReactNode; fallback?: ReactNode; onReset?: () => void }
interface State { hasError: boolean; error: Error | null }

export class ErrorBoundary extends Component<Props, State> {
  state: State = { hasError: false, error: null }

  static getDerivedStateFromError(error: Error): State {
    return { hasError: true, error }
  }

  componentDidCatch(error: Error, info: React.ErrorInfo) {
    Sentry.captureException(error, { extra: { componentStack: info.componentStack } })
    console.error('[ErrorBoundary]', error)
  }

  reset = () => {
    this.setState({ hasError: false, error: null })
    this.props.onReset?.()
  }

  render() {
    if (!this.state.hasError) return this.props.children

    if (this.props.fallback) return this.props.fallback

    return (
      <div className="flex flex-col items-center justify-center p-8 text-center" role="alert">
        <div className="w-14 h-14 bg-red-100 rounded-2xl flex items-center justify-center text-2xl mb-4">⚠️</div>
        <h2 className="text-lg font-bold text-gray-900 mb-2">Something went wrong</h2>
        <p className="text-sm text-gray-500 mb-6 max-w-sm">
          {this.state.error?.message || 'An unexpected error occurred.'}
        </p>
        <button onClick={this.reset} className="px-5 py-2.5 bg-indigo-600 text-white text-sm font-semibold rounded-xl hover:bg-indigo-700 transition-colors">
          Try again
        </button>
      </div>
    )
  }
}

// Usage: wrap sections, not entire pages
// <ErrorBoundary>
//   <ProductGrid />
// </ErrorBoundary>
```

---

## Step 982: Graceful Degradation

```tsx
// Components that degrade gracefully when features are unavailable

// Feature detection
const supportsIntersectionObserver = typeof IntersectionObserver !== 'undefined'
const supportsWebWorker             = typeof Worker !== 'undefined'
const supportsWebSocket             = typeof WebSocket !== 'undefined'

// Progressive enhancement: basic → enhanced
export function LazySection({ children }: { children: React.ReactNode }) {
  const [visible, setVisible] = useState(!supportsIntersectionObserver)
  const ref = useRef<HTMLDivElement>(null)

  useEffect(() => {
    if (!supportsIntersectionObserver) return
    const io = new IntersectionObserver(([entry]) => {
      if (entry.isIntersecting) { setVisible(true); io.disconnect() }
    }, { rootMargin: '100px' })
    if (ref.current) io.observe(ref.current)
    return () => io.disconnect()
  }, [])

  return <div ref={ref}>{visible ? children : <div className="h-40 bg-gray-100 rounded-2xl animate-pulse" />}</div>
}

// Offline-aware component
export function OfflineAware({ children }: { children: React.ReactNode }) {
  const [offline, setOffline] = useState(!navigator?.onLine)

  useEffect(() => {
    const go = () => setOffline(false)
    const off = () => setOffline(true)
    window.addEventListener('online',  go)
    window.addEventListener('offline', off)
    return () => { window.removeEventListener('online', go); window.removeEventListener('offline', off) }
  }, [])

  return (
    <>
      {offline && (
        <div className="fixed top-0 inset-x-0 z-50 bg-amber-500 text-white text-xs font-semibold text-center py-2">
          You are offline — some features may be unavailable
        </div>
      )}
      {children}
    </>
  )
}
```

---

## Step 983: Service Worker & PWA

```ts
// public/sw.js — basic service worker for offline support
const CACHE_NAME = 'myapp-v1'
const PRECACHE   = ['/', '/offline']

self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open(CACHE_NAME).then(cache => cache.addAll(PRECACHE))
  )
})

self.addEventListener('fetch', (event) => {
  // Cache-first for static assets
  if (event.request.destination === 'image' || event.request.url.includes('/_next/static/')) {
    event.respondWith(
      caches.match(event.request).then(cached => cached ?? fetch(event.request).then(response => {
        const clone = response.clone()
        caches.open(CACHE_NAME).then(cache => cache.put(event.request, clone))
        return response
      }))
    )
    return
  }

  // Network-first for API calls
  if (event.request.url.includes('/api/')) {
    event.respondWith(
      fetch(event.request).catch(() =>
        new Response(JSON.stringify({ error: 'Offline' }), {
          headers: { 'Content-Type': 'application/json' },
          status:  503,
        })
      )
    )
    return
  }
})
```

```json
// public/manifest.json
{
  "name":             "MyApp",
  "short_name":       "MyApp",
  "description":      "Your best shopping experience",
  "start_url":        "/",
  "display":          "standalone",
  "background_color": "#ffffff",
  "theme_color":      "#4F46E5",
  "icons": [
    { "src": "/icon-192.png", "sizes": "192x192", "type": "image/png" },
    { "src": "/icon-512.png", "sizes": "512x512", "type": "image/png", "purpose": "maskable" }
  ]
}
```

---

## Step 984: Data Fetching Patterns Summary

```ts
// Choose the right data fetching approach:

// 1. Server Component (default — no client JS) ✅
async function ProductList() {
  const products = await db.product.findMany() // direct DB access
  return <div>{products.map(p => <ProductCard key={p.id} product={p} />)}</div>
}

// 2. Server Component with ISR ✅
async function PopularProducts() {
  const data = await fetch('/api/products?popular=true', {
    next: { revalidate: 300, tags: ['products'] }, // 5min ISR
  }).then(r => r.json())
  return <ProductGrid products={data} />
}

// 3. Client Component with TanStack Query ✅ (user-specific / real-time)
'use client'
function UserOrders() {
  const { data, isLoading } = useQuery({
    queryKey: ['orders', 'mine'],
    queryFn: () => fetch('/api/orders/mine').then(r => r.json()),
  })
  if (isLoading) return <OrderSkeleton />
  return <OrderList orders={data} />
}

// 4. Server Action (mutations) ✅
async function createProduct(formData: FormData) {
  'use server'
  const data = Object.fromEntries(formData)
  // validate → save → revalidate
  revalidateTag('products')
}
```

---

## Step 985: Component Design Principles

```
Component Design Checklist:

SINGLE RESPONSIBILITY
  ✅ Each component does ONE thing
  ✅ Presentational vs. Container separation
  ✅ Custom hooks extract logic

COMPOSITION OVER INHERITANCE
  ✅ Use children prop for flexibility
  ✅ Compound components (Select.Option, Tab.Panel)
  ✅ Render props / slot pattern when needed

ACCESSIBILITY FIRST
  ✅ Semantic HTML (button, nav, main, etc.)
  ✅ ARIA roles + labels where HTML isn't enough
  ✅ Keyboard navigable
  ✅ Focus visible ring

PERFORMANCE
  ✅ Server Component by default
  ✅ Minimal 'use client' boundaries
  ✅ Memoize expensive computations (useMemo/useCallback)
  ✅ Dynamic import heavy components

TAILWIND
  ✅ Mobile-first responsive classes
  ✅ Semantic color tokens (not raw colors)
  ✅ dark: variant for dark mode
  ✅ focus-visible: for keyboard
```

---

## Step 986: Git Workflow

```bash
# Feature branch workflow
git checkout -b feature/product-search
# make changes
git add -p  # interactive staging (review each hunk)
git commit -m "feat: add product search with AI intent extraction"
git push -u origin feature/product-search

# Commit message format (Conventional Commits)
# feat:     new feature
# fix:      bug fix
# docs:     documentation only
# style:    formatting, no logic change
# refactor: restructure without behavior change
# perf:     performance improvement
# test:     add/update tests
# chore:    build, tooling, deps

# Pre-commit hooks (lint-staged)
# .husky/pre-commit
#!/bin/sh
npx lint-staged

# package.json
{
  "lint-staged": {
    "*.{ts,tsx}": ["eslint --fix", "prettier --write"],
    "*.{css,json,md}": "prettier --write"
  }
}
```

---

## Step 987: Code Review Checklist

```
Pull Request Checklist:

FUNCTIONALITY
  ✅ Feature works as described in the ticket
  ✅ Edge cases handled (empty state, error state, loading)
  ✅ No console.log / debug code left in

PERFORMANCE
  ✅ No unnecessary re-renders
  ✅ Images optimized with next/image
  ✅ No N+1 database queries
  ✅ API responses appropriately cached

SECURITY
  ✅ User input validated server-side
  ✅ Auth + authorization checked
  ✅ No secrets in code
  ✅ Dependencies checked for vulnerabilities (npm audit)

ACCESSIBILITY
  ✅ Keyboard navigable
  ✅ Screen reader labels present
  ✅ Color contrast ≥ 4.5:1
  ✅ Focus management correct

CODE QUALITY
  ✅ TypeScript types correct (no 'any')
  ✅ Tests cover happy path + key error paths
  ✅ No code duplication (DRY where it makes sense)
  ✅ Components are focused and small
```

---

## Step 988: Production Launch Checklist

```
Launch Day Checklist:

PRE-LAUNCH (1 week before)
  ✅ Load testing completed (k6, Locust)
  ✅ Security audit passed
  ✅ Accessibility audit passed (Lighthouse ≥90)
  ✅ All tests green in CI
  ✅ Staging environment smoke tested

LAUNCH DAY
  ✅ Database backup taken
  ✅ Rollback plan documented
  ✅ On-call engineer assigned
  ✅ Error tracking (Sentry) verified working
  ✅ Monitoring dashboards set up
  ✅ CDN configured and cache warmed

POST-LAUNCH (first 24 hours)
  ✅ Error rate < 0.1%
  ✅ P95 response time < 500ms
  ✅ Core Web Vitals passing
  ✅ Real user monitoring active
  ✅ Customer support briefed
```

---

## Step 989: Full-Stack Architecture Summary

```
Tailwind CSS Full-Stack Architecture:

┌─────────────────────────────────────────────────────┐
│                    PRESENTATION                       │
│  Next.js App Router                                  │
│  ├── Server Components (default)                    │
│  │   └── Direct DB access, ISR, metadata API        │
│  ├── Client Components ('use client')               │
│  │   └── State, events, browser APIs                │
│  └── Tailwind CSS                                   │
│      └── Mobile-first, tokens, dark mode, a11y      │
├─────────────────────────────────────────────────────┤
│                    APPLICATION                        │
│  ├── Server Actions (mutations)                     │
│  ├── API Route Handlers (REST API)                  │
│  ├── TanStack Query (client state)                  │
│  ├── Zustand/Jotai (global UI state)               │
│  └── Auth.js (authentication)                       │
├─────────────────────────────────────────────────────┤
│                   INFRASTRUCTURE                      │
│  ├── Prisma ORM → PostgreSQL                        │
│  ├── Redis (caching, sessions, rate limit)          │
│  ├── Socket.io (real-time)                          │
│  ├── Resend/SendGrid (email)                        │
│  └── Cloudflare R2/Vercel Blob (file storage)       │
├─────────────────────────────────────────────────────┤
│                    PLATFORM                           │
│  ├── Vercel (hosting, CDN, edge functions)          │
│  ├── GitHub Actions (CI/CD)                         │
│  ├── Sentry (error tracking)                        │
│  └── Upstash (Redis managed)                        │
└─────────────────────────────────────────────────────┘
```

---

## Step 990: Workshop — Production Status Board

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Production Status Board</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-950 min-h-screen p-6 text-white">
<div class="max-w-3xl mx-auto">
  <div class="flex items-center gap-3 mb-8">
    <div class="w-3 h-3 rounded-full bg-emerald-500 animate-pulse"></div>
    <h1 class="text-2xl font-extrabold">Production Status</h1>
    <span class="ml-auto text-xs text-gray-500" id="time"></span>
  </div>

  <!-- Services -->
  <div class="grid grid-cols-2 md:grid-cols-3 gap-3 mb-6" id="services"></div>

  <!-- Metrics -->
  <div class="grid grid-cols-4 gap-3 mb-6">
    <div class="bg-gray-900 rounded-2xl p-4 text-center">
      <p class="text-xs text-gray-500 mb-1">Uptime</p>
      <p class="text-2xl font-extrabold text-emerald-400">99.9%</p>
    </div>
    <div class="bg-gray-900 rounded-2xl p-4 text-center">
      <p class="text-xs text-gray-500 mb-1">Req/min</p>
      <p class="text-2xl font-extrabold text-indigo-400" id="req-rate">—</p>
    </div>
    <div class="bg-gray-900 rounded-2xl p-4 text-center">
      <p class="text-xs text-gray-500 mb-1">Error Rate</p>
      <p class="text-2xl font-extrabold text-emerald-400">0.01%</p>
    </div>
    <div class="bg-gray-900 rounded-2xl p-4 text-center">
      <p class="text-xs text-gray-500 mb-1">P95 Latency</p>
      <p class="text-2xl font-extrabold text-amber-400" id="latency">—</p>
    </div>
  </div>

  <!-- Deployments -->
  <div class="bg-gray-900 rounded-2xl p-5">
    <h2 class="font-bold text-gray-300 mb-3 text-sm">Recent Deployments</h2>
    <div class="space-y-2">
      <div class="flex items-center gap-3 text-sm">
        <span class="w-2 h-2 rounded-full bg-emerald-500 flex-shrink-0"></span>
        <span class="text-gray-300 font-mono text-xs">v2.5.1</span>
        <span class="text-gray-500 text-xs">feat: AI product search</span>
        <span class="ml-auto text-gray-600 text-xs">2 hrs ago</span>
      </div>
      <div class="flex items-center gap-3 text-sm">
        <span class="w-2 h-2 rounded-full bg-emerald-500 flex-shrink-0"></span>
        <span class="text-gray-300 font-mono text-xs">v2.5.0</span>
        <span class="text-gray-500 text-xs">feat: real-time notifications</span>
        <span class="ml-auto text-gray-600 text-xs">1 day ago</span>
      </div>
      <div class="flex items-center gap-3 text-sm">
        <span class="w-2 h-2 rounded-full bg-red-500 flex-shrink-0"></span>
        <span class="text-gray-300 font-mono text-xs">v2.4.9</span>
        <span class="text-gray-500 text-xs">fix: cart persistence bug</span>
        <span class="ml-auto text-gray-600 text-xs">3 days ago</span>
      </div>
    </div>
  </div>
</div>
<script>
const SERVICES = [
  { name:'Web App',    status:'operational' },
  { name:'API',        status:'operational' },
  { name:'Database',   status:'operational' },
  { name:'Redis',      status:'operational' },
  { name:'CDN',        status:'operational' },
  { name:'Email',      status:'degraded'    },
];

const COLORS = { operational:'bg-emerald-500', degraded:'bg-amber-500', outage:'bg-red-500' };
const LABELS = { operational:'Operational', degraded:'Degraded', outage:'Outage' };

function render(){
  document.getElementById('services').innerHTML=SERVICES.map(s=>`
    <div class="bg-gray-900 rounded-2xl p-4 flex items-center gap-3">
      <div class="w-2.5 h-2.5 rounded-full ${COLORS[s.status]} ${s.status==='operational'?'':'animate-pulse'} flex-shrink-0"></div>
      <div>
        <p class="text-sm font-semibold">${s.name}</p>
        <p class="text-xs ${s.status==='operational'?'text-emerald-400':'text-amber-400'}">${LABELS[s.status]}</p>
      </div>
    </div>`).join('');
}

function updateMetrics(){
  document.getElementById('req-rate').textContent = (850+Math.floor(Math.random()*200)).toLocaleString();
  document.getElementById('latency').textContent  = (140+Math.floor(Math.random()*60))+'ms';
  document.getElementById('time').textContent     = 'Updated '+new Date().toLocaleTimeString();
}

render(); updateMetrics();
setInterval(updateMetrics, 5000);
</script>
</body>
</html>
```

---

## สรุป Part 99

| Step | เนื้อหา |
|------|---------|
| 981 | Error Boundary class component + Sentry integration |
| 982 | Graceful degradation: IntersectionObserver + offline detection |
| 983 | Service Worker caching strategies + PWA manifest.json |
| 984 | Data fetching patterns: Server Component / ISR / Query / Action |
| 985 | Component design principles: SRP, composition, a11y, performance |
| 986 | Git workflow: Conventional Commits + lint-staged + husky |
| 987 | Code review checklist: functionality/performance/security/a11y |
| 988 | Production launch checklist: pre-launch/launch-day/post-launch |
| 989 | Full-stack architecture diagram: Presentation→Application→Infrastructure |
| 990 | Workshop: Production status board (dark theme + live metrics) |

**Part ถัดไป:** Part 100 — World-Class Capstone Project (Steps 991–1000)
