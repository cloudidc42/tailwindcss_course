# Part 100: World-Class Capstone Project

## เป้าหมาย
- รวมทุก concept ใน 1 project สมบูรณ์
- E-commerce platform ระดับ production
- World-class code quality
- Steps 991–1000

---

## Step 991: Capstone Project Overview

```
ShopAI — Full-Stack E-Commerce Platform
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

TECH STACK
  Frontend: Next.js 14 (App Router) + Tailwind CSS
  State:    Zustand (cart) + TanStack Query (server)
  Auth:     Auth.js v5 (GitHub/Google/Credentials)
  DB:       PostgreSQL + Prisma ORM
  Cache:    Upstash Redis (rate limit + data cache)
  AI:       Claude API (search + recommendations + chat)
  Deploy:   Vercel + Vercel Postgres + Upstash

FEATURES
  ✅ Product catalog with AI-powered semantic search
  ✅ Shopping cart with localStorage persistence (Zustand)
  ✅ User authentication with OAuth
  ✅ Order placement with inventory management
  ✅ Admin dashboard with real-time metrics (SSE)
  ✅ AI chat support assistant
  ✅ Dark mode + full accessibility (WCAG 2.1 AA)
  ✅ Internationalization (TH/EN)
  ✅ PWA support (offline access)
  ✅ CI/CD with GitHub Actions
```

---

## Step 992: Project Structure

```
shopai/
├── src/
│   ├── app/
│   │   ├── (marketing)/          ← Public pages
│   │   │   ├── layout.tsx        ← Marketing layout (header/footer)
│   │   │   ├── page.tsx          ← Homepage
│   │   │   ├── products/
│   │   │   │   ├── page.tsx      ← Product listing
│   │   │   │   └── [slug]/
│   │   │   │       └── page.tsx  ← Product detail
│   │   │   └── cart/page.tsx     ← Cart
│   │   │
│   │   ├── (app)/                ← Auth-required pages
│   │   │   ├── layout.tsx        ← App layout (sidebar)
│   │   │   ├── orders/page.tsx
│   │   │   ├── profile/page.tsx
│   │   │   └── settings/page.tsx
│   │   │
│   │   ├── admin/                ← Admin panel
│   │   │   ├── layout.tsx
│   │   │   ├── page.tsx          ← Dashboard
│   │   │   ├── products/page.tsx
│   │   │   └── orders/page.tsx
│   │   │
│   │   ├── api/
│   │   │   ├── auth/[...nextauth]/route.ts
│   │   │   ├── products/
│   │   │   │   ├── route.ts        ← GET/POST /api/products
│   │   │   │   └── [id]/route.ts   ← GET/PATCH/DELETE
│   │   │   ├── orders/route.ts
│   │   │   ├── chat/route.ts       ← AI streaming
│   │   │   └── health/route.ts
│   │   │
│   │   ├── layout.tsx            ← Root layout (Providers, fonts)
│   │   └── globals.css
│   │
│   ├── components/
│   │   ├── ui/                   ← Base design system
│   │   ├── product/              ← ProductCard, ProductGrid, etc.
│   │   ├── cart/                 ← CartDrawer, CartItem, etc.
│   │   └── layout/               ← Header, Sidebar, Footer
│   │
│   ├── domain/                   ← Entities + interfaces
│   ├── application/              ← Use cases
│   ├── infrastructure/           ← Prisma repos, email, storage
│   ├── stores/                   ← Zustand stores
│   ├── hooks/                    ← Custom React hooks
│   ├── lib/                      ← Utils, db client, env
│   └── types/                    ← Shared TypeScript types
│
├── prisma/
│   ├── schema.prisma
│   └── seed.ts
│
├── public/
│   ├── sw.js                     ← Service worker
│   └── manifest.json             ← PWA manifest
│
├── .github/workflows/
│   ├── ci.yml
│   └── deploy.yml
│
├── tailwind.config.ts
├── next.config.ts
└── docker-compose.yml
```

---

## Step 993: Root Layout + Providers

```tsx
// src/app/layout.tsx
import type { Metadata }      from 'next'
import { Inter, JetBrains_Mono } from 'next/font/google'
import { SessionProvider }    from 'next-auth/react'
import { Providers }          from '@/components/Providers'
import './globals.css'

const inter = Inter({ subsets: ['latin'], variable: '--font-sans', display: 'swap' })
const mono  = JetBrains_Mono({ subsets: ['latin'], variable: '--font-mono', display: 'swap' })

export const metadata: Metadata = {
  metadataBase: new URL(process.env.NEXT_PUBLIC_APP_URL!),
  title:        { template: '%s | ShopAI', default: 'ShopAI — AI-Powered Shopping' },
  description:  'Discover products with AI. Shop smarter.',
  openGraph:    { type: 'website', locale: 'th_TH', siteName: 'ShopAI' },
  manifest:     '/manifest.json',
  themeColor:   [{ media: '(prefers-color-scheme: light)', color: '#ffffff' }, { media: '(prefers-color-scheme: dark)', color: '#111827' }],
}

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="th" suppressHydrationWarning>
      <body className={`${inter.variable} ${mono.variable} font-sans antialiased bg-white dark:bg-gray-950 text-gray-900 dark:text-gray-100`}>
        <Providers>
          <a href="#main" className="sr-only focus:not-sr-only focus:fixed focus:top-2 focus:left-2 focus:z-50 focus:px-4 focus:py-2 focus:bg-indigo-600 focus:text-white focus:rounded-xl">
            Skip to main content
          </a>
          {children}
        </Providers>
      </body>
    </html>
  )
}
```

---

## Step 994: Homepage

```tsx
// src/app/(marketing)/page.tsx
import { Suspense }         from 'react'
import { HeroSection }      from '@/components/HeroSection'
import { FeaturedProducts } from '@/components/product/FeaturedProducts'
import { AiChatWidget }     from '@/components/AiChatWidget'
import { ProductSkeleton }  from '@/components/product/ProductSkeleton'
import type { Metadata }    from 'next'

export const metadata: Metadata = {
  title: 'ShopAI — Discover Products with AI',
}

export default function HomePage() {
  return (
    <main id="main">
      <HeroSection />

      <section className="max-w-7xl mx-auto px-4 py-16">
        <h2 className="text-2xl font-extrabold text-gray-900 dark:text-white mb-8">
          Featured Products
        </h2>
        <Suspense fallback={<ProductSkeleton count={8} />}>
          <FeaturedProducts />
        </Suspense>
      </section>

      <AiChatWidget />
    </main>
  )
}

// src/components/HeroSection.tsx
export function HeroSection() {
  return (
    <section className="relative bg-gradient-to-br from-indigo-950 via-indigo-900 to-violet-900 text-white overflow-hidden">
      <div className="absolute inset-0 opacity-20" style={{ backgroundImage: 'radial-gradient(circle at 30% 50%, #818CF8 0%, transparent 60%), radial-gradient(circle at 70% 50%, #A78BFA 0%, transparent 60%)' }} />
      <div className="relative max-w-4xl mx-auto px-4 py-24 text-center">
        <span className="inline-flex items-center gap-2 px-3 py-1 rounded-full bg-white/10 text-xs font-semibold mb-6 backdrop-blur-sm border border-white/20">
          <span className="w-1.5 h-1.5 rounded-full bg-emerald-400 animate-pulse" />
          AI-Powered Shopping
        </span>
        <h1 className="text-5xl md:text-6xl font-extrabold tracking-tight mb-6 leading-tight">
          Shop Smarter with{' '}
          <span className="bg-gradient-to-r from-indigo-300 to-violet-300 bg-clip-text text-transparent">
            AI
          </span>
        </h1>
        <p className="text-lg text-indigo-200 max-w-xl mx-auto mb-10">
          Describe what you're looking for in natural language. Our AI finds the perfect products for you.
        </p>
        <div className="flex flex-col sm:flex-row gap-3 justify-center">
          <a href="/products" className="px-8 py-3.5 bg-white text-indigo-900 font-bold rounded-2xl hover:bg-indigo-50 transition-colors">
            Browse Products
          </a>
          <a href="#chat" className="px-8 py-3.5 bg-white/10 text-white font-bold rounded-2xl hover:bg-white/20 transition-colors backdrop-blur-sm border border-white/20">
            Try AI Search ✨
          </a>
        </div>
      </div>
    </section>
  )
}
```

---

## Step 995: Cart Drawer

```tsx
// src/components/cart/CartDrawer.tsx
'use client'
import { Fragment }     from 'react'
import { useCart }      from '@/stores/cart'
import { CartItem }     from './CartItem'
import { CheckoutButton } from './CheckoutButton'

export function CartDrawer({ open, onClose }: { open: boolean; onClose: () => void }) {
  const { items, itemCount, total, clearCart } = useCart(state => ({
    items:     state.items,
    itemCount: state.itemCount(),
    total:     state.total(),
    clearCart: state.clearCart,
  }))

  return (
    <>
      {/* Backdrop */}
      <div
        className={`fixed inset-0 bg-black/40 backdrop-blur-sm z-40 transition-opacity ${open ? 'opacity-100' : 'opacity-0 pointer-events-none'}`}
        onClick={onClose}
      />

      {/* Drawer */}
      <aside
        role="dialog"
        aria-label="Shopping cart"
        aria-modal="true"
        className={`fixed top-0 right-0 h-full w-full max-w-sm bg-white dark:bg-gray-900 shadow-2xl z-50 flex flex-col transition-transform duration-300 ${open ? 'translate-x-0' : 'translate-x-full'}`}
      >
        <div className="flex items-center justify-between px-4 py-4 border-b dark:border-gray-800">
          <h2 className="font-extrabold text-gray-900 dark:text-white">Cart ({itemCount})</h2>
          <button onClick={onClose} className="p-2 rounded-xl hover:bg-gray-100 dark:hover:bg-gray-800 transition-colors" aria-label="Close cart">✕</button>
        </div>

        <div className="flex-1 overflow-y-auto px-4 py-3 space-y-3">
          {items.length === 0 ? (
            <div className="text-center py-16">
              <p className="text-4xl mb-3">🛒</p>
              <p className="text-gray-500 dark:text-gray-400 text-sm">Your cart is empty</p>
              <button onClick={onClose} className="mt-4 text-indigo-600 text-sm font-semibold hover:underline">Continue shopping →</button>
            </div>
          ) : (
            items.map(item => <CartItem key={item.id} item={item} />)
          )}
        </div>

        {items.length > 0 && (
          <div className="border-t dark:border-gray-800 p-4 space-y-3">
            <div className="flex justify-between font-bold text-lg">
              <span className="text-gray-900 dark:text-white">Total</span>
              <span className="text-indigo-600">${total.toFixed(2)}</span>
            </div>
            <CheckoutButton />
            <button onClick={clearCart} className="w-full text-xs text-gray-400 hover:text-gray-600 dark:hover:text-gray-200 transition-colors">
              Clear cart
            </button>
          </div>
        )}
      </aside>
    </>
  )
}
```

---

## Step 996: Admin Dashboard

```tsx
// src/app/admin/page.tsx — Real-time admin dashboard
import { Suspense }         from 'react'
import { auth }             from '@/auth'
import { redirect }         from 'next/navigation'
import { getDashboardStats } from '@/lib/analytics'
import { KpiCards }         from '@/components/admin/KpiCards'
import { RealtimeMetrics }  from '@/components/admin/RealtimeMetrics'
import { RecentOrders }     from '@/components/admin/RecentOrders'

export const metadata = { title: 'Dashboard' }

export default async function AdminDashboard() {
  const session = await auth()
  if ((session?.user as any)?.role !== 'ADMIN') redirect('/dashboard')

  const stats = await getDashboardStats()

  return (
    <div className="p-6 space-y-6">
      <div>
        <h1 className="text-2xl font-extrabold text-gray-900 dark:text-white">Dashboard</h1>
        <p className="text-gray-500 dark:text-gray-400 text-sm mt-1">
          Welcome back, {session?.user?.name}
        </p>
      </div>

      <KpiCards stats={stats} />

      {/* Real-time chart (client component) */}
      <Suspense fallback={<div className="h-48 bg-gray-100 dark:bg-gray-800 rounded-2xl animate-pulse" />}>
        <RealtimeMetrics />
      </Suspense>

      <Suspense fallback={<div className="h-64 bg-gray-100 dark:bg-gray-800 rounded-2xl animate-pulse" />}>
        <RecentOrders orders={stats.recentOrders} />
      </Suspense>
    </div>
  )
}
```

---

## Step 997: Full E2E Test

```ts
// tests/e2e/checkout.spec.ts
import { test, expect } from '@playwright/test'

test.describe('Checkout Flow', () => {
  test.beforeEach(async ({ page }) => {
    // Set up auth state
    await page.goto('/login')
    await page.fill('[name="email"]', 'user@test.com')
    await page.fill('[name="password"]', 'password123')
    await page.click('[type="submit"]')
    await page.waitForURL('/dashboard')
  })

  test('complete checkout flow', async ({ page }) => {
    // 1. Browse to product
    await page.goto('/products')
    await page.waitForSelector('[data-testid="product-card"]')

    // 2. Add to cart
    const firstProduct = page.locator('[data-testid="product-card"]').first()
    await firstProduct.locator('[data-testid="add-to-cart"]').click()

    // 3. Verify cart updated
    const cartCount = page.locator('[data-testid="cart-count"]')
    await expect(cartCount).toHaveText('1')

    // 4. Open cart
    await page.locator('[data-testid="cart-button"]').click()
    await expect(page.locator('[role="dialog"]')).toBeVisible()

    // 5. Proceed to checkout
    await page.locator('[data-testid="checkout-button"]').click()
    await page.waitForURL('/checkout')

    // 6. Fill shipping info
    await page.fill('[name="address"]', '123 Test Street')
    await page.fill('[name="city"]', 'Bangkok')
    await page.fill('[name="zip"]', '10110')

    // 7. Place order
    await page.locator('[data-testid="place-order"]').click()
    await page.waitForURL('/orders/**')

    // 8. Verify success
    await expect(page.locator('h1')).toContainText('Order placed')
  })

  test('AI search returns relevant results', async ({ page }) => {
    await page.goto('/products')
    await page.fill('[data-testid="search-input"]', 'wireless headphones under 100 dollars')
    await page.press('[data-testid="search-input"]', 'Enter')
    await page.waitForSelector('[data-testid="product-card"]')
    const products = await page.locator('[data-testid="product-card"]').count()
    expect(products).toBeGreaterThan(0)
  })
})
```

---

## Step 998: Performance Budget

```ts
// lighthouse.config.ts
export default {
  ci: {
    collect: {
      url: [
        'http://localhost:3000/',          // Homepage
        'http://localhost:3000/products',  // Product listing
        'http://localhost:3000/products/laptop-pro-15', // Product detail
      ],
      numberOfRuns: 3,
    },
    assert: {
      assertions: {
        'categories:performance':    ['error', { minScore: 0.9  }],
        'categories:accessibility':  ['error', { minScore: 0.95 }],
        'categories:best-practices': ['error', { minScore: 0.9  }],
        'categories:seo':            ['error', { minScore: 0.9  }],

        // Core Web Vitals
        'largest-contentful-paint':  ['error', { maxNumericValue: 2500  }],
        'first-contentful-paint':    ['error', { maxNumericValue: 1800  }],
        'cumulative-layout-shift':   ['error', { maxNumericValue: 0.1   }],
        'total-blocking-time':       ['error', { maxNumericValue: 200   }],

        // Bundle size
        'total-byte-weight':         ['warn',  { maxNumericValue: 200000 }], // 200KB
        'uses-optimized-images':     ['error', {}],
        'uses-webp-images':          ['warn',  {}],
        'render-blocking-resources': ['error', {}],
      }
    }
  }
}
```

---

## Step 999: Course Achievement Summary

```
🎓 Tailwind CSS Mastery — Complete Curriculum

FOUNDATION (Parts 1-30)
  ✅ Core utilities: spacing, colors, typography, flexbox, grid
  ✅ Responsive design: mobile-first breakpoints
  ✅ States: hover, focus, active, disabled, group-hover
  ✅ Dark mode: class strategy + CSS variables
  ✅ Tailwind configuration + custom themes

INTERMEDIATE (Parts 31-60)
  ✅ Complex layouts: sidebar, multi-column, card grids
  ✅ Animations: keyframes, transitions, transforms
  ✅ Forms: styling inputs, selects, checkboxes
  ✅ Typography: prose, fluid type with clamp()
  ✅ Component patterns: modals, dropdowns, tooltips

ADVANCED (Parts 61-80)
  ✅ Design systems: tokens, semantic colors, multi-brand
  ✅ Accessibility: WCAG 2.1 AA, ARIA, focus management
  ✅ Performance: JIT, critical CSS, Core Web Vitals
  ✅ Plugins: typography, forms, custom utilities
  ✅ Testing: Vitest, Storybook, Playwright, CI

PROFESSIONAL (Parts 81-90)
  ✅ Next.js App Router integration
  ✅ State management: Zustand, Jotai, TanStack Query
  ✅ API design: REST, validation, error handling
  ✅ Authentication: Auth.js, OAuth, RBAC
  ✅ i18n + RTL: react-i18next, logical CSS

WORLD-CLASS (Parts 91-100)
  ✅ Database: Prisma ORM, migrations, transactions
  ✅ Deployment: Vercel, Docker, GitHub Actions CI/CD
  ✅ Architecture: Clean Architecture, DDD, CQRS
  ✅ Monorepo: Turborepo, Module Federation
  ✅ Real-time: SSE, WebSocket, optimistic updates
  ✅ Performance: Redis, edge caching, virtualization
  ✅ Security: XSS, CSRF, CSP, RBAC, rate limiting
  ✅ AI: Claude API, streaming, RAG, moderation
  ✅ Capstone: Full production e-commerce platform

Steps completed: 1–1000 ✅
```

---

## Step 1000: Final Workshop — Full Landing Page

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ShopAI — Tailwind Course Capstone</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    @keyframes float    { 0%,100%{transform:translateY(0)}   50%{transform:translateY(-10px)} }
    @keyframes fade-up  { from{opacity:0;transform:translateY(20px)} to{opacity:1;transform:translateY(0)} }
    @keyframes slide-in { from{opacity:0;transform:translateX(-20px)} to{opacity:1;transform:translateX(0)} }
    .float    { animation: float  4s ease-in-out infinite }
    .fade-up  { animation: fade-up  0.6s ease both }
    .slide-in { animation: slide-in 0.5s ease both }
    .delay-1  { animation-delay: 0.1s }
    .delay-2  { animation-delay: 0.2s }
    .delay-3  { animation-delay: 0.3s }
  </style>
</head>
<body class="bg-white font-sans antialiased">

<!-- Nav -->
<nav class="sticky top-0 z-50 bg-white/80 backdrop-blur-lg border-b px-4 py-3 flex items-center justify-between">
  <div class="flex items-center gap-2">
    <div class="w-7 h-7 bg-indigo-600 rounded-lg flex items-center justify-center text-white text-xs font-black">S</div>
    <span class="font-extrabold text-gray-900">ShopAI</span>
  </div>
  <div class="hidden sm:flex items-center gap-6 text-sm font-medium text-gray-600">
    <a href="#features" class="hover:text-indigo-600 transition-colors">Features</a>
    <a href="#pricing"  class="hover:text-indigo-600 transition-colors">Pricing</a>
    <a href="#about"    class="hover:text-indigo-600 transition-colors">About</a>
  </div>
  <div class="flex gap-2">
    <button class="px-3 py-1.5 text-sm font-semibold text-gray-700 hover:text-gray-900 transition-colors">Sign in</button>
    <button class="px-4 py-1.5 bg-indigo-600 text-white text-sm font-semibold rounded-xl hover:bg-indigo-700 transition-colors">Get started</button>
  </div>
</nav>

<!-- Hero -->
<section class="max-w-5xl mx-auto px-4 pt-20 pb-16 text-center">
  <div class="fade-up inline-flex items-center gap-2 px-3 py-1 rounded-full bg-indigo-50 text-indigo-700 text-xs font-semibold border border-indigo-200 mb-6">
    <span class="w-1.5 h-1.5 rounded-full bg-indigo-500 animate-pulse"></span>
    AI-Powered E-Commerce Platform
  </div>
  <h1 class="fade-up delay-1 text-5xl sm:text-6xl font-extrabold text-gray-900 tracking-tight mb-6">
    Shop with the power of
    <span class="bg-gradient-to-r from-indigo-600 to-violet-600 bg-clip-text text-transparent"> AI</span>
  </h1>
  <p class="fade-up delay-2 text-xl text-gray-500 max-w-2xl mx-auto mb-10">
    Describe what you want in natural language. ShopAI finds the perfect products, answers your questions, and makes shopping effortless.
  </p>
  <div class="fade-up delay-3 flex flex-col sm:flex-row gap-3 justify-center">
    <button class="px-8 py-3.5 bg-indigo-600 text-white font-bold rounded-2xl hover:bg-indigo-700 transition-colors shadow-lg shadow-indigo-200">Start shopping free</button>
    <button class="px-8 py-3.5 bg-gray-100 text-gray-900 font-bold rounded-2xl hover:bg-gray-200 transition-colors">Watch demo ▶</button>
  </div>

  <!-- Floating UI preview -->
  <div class="float mt-16 mx-auto max-w-sm bg-white rounded-3xl border shadow-2xl p-5 text-left">
    <div class="flex items-center gap-2 mb-3">
      <div class="w-6 h-6 rounded-full bg-indigo-600 flex items-center justify-center text-white text-xs">AI</div>
      <span class="text-xs text-gray-500">AI Assistant</span>
      <span class="ml-auto flex items-center gap-1 text-xs text-emerald-600"><span class="w-1.5 h-1.5 rounded-full bg-emerald-500 animate-pulse"></span>Live</span>
    </div>
    <div class="space-y-2">
      <div class="flex justify-end"><div class="bg-indigo-600 text-white rounded-2xl rounded-br-sm px-3 py-2 text-sm max-w-[80%]">ต้องการหูฟัง ราคาไม่เกิน 2000 บาท</div></div>
      <div class="flex gap-2"><div class="w-5 h-5 rounded-full bg-indigo-100 flex-shrink-0"></div><div class="bg-gray-100 rounded-2xl rounded-bl-sm px-3 py-2 text-xs text-gray-700 max-w-[80%]">พบ 3 รายการที่ตรงกับความต้องการของคุณ! ราคา 890–1,990 บาท คะแนนรีวิวสูงสุด 4.8/5 ⭐</div></div>
    </div>
  </div>
</section>

<!-- Features -->
<section id="features" class="bg-gray-50 py-20">
  <div class="max-w-5xl mx-auto px-4">
    <h2 class="text-3xl font-extrabold text-gray-900 text-center mb-12">Everything you need to sell online</h2>
    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
      <div class="slide-in bg-white rounded-2xl border p-6 hover:shadow-lg transition-shadow">
        <div class="w-10 h-10 bg-indigo-100 rounded-xl flex items-center justify-center text-xl mb-4">🔍</div>
        <h3 class="font-bold text-gray-900 mb-2">AI-Powered Search</h3>
        <p class="text-sm text-gray-500">Natural language search that understands intent, not just keywords.</p>
      </div>
      <div class="slide-in delay-1 bg-white rounded-2xl border p-6 hover:shadow-lg transition-shadow">
        <div class="w-10 h-10 bg-violet-100 rounded-xl flex items-center justify-center text-xl mb-4">💬</div>
        <h3 class="font-bold text-gray-900 mb-2">24/7 AI Support</h3>
        <p class="text-sm text-gray-500">Instant answers to product questions, returns, and shipping inquiries.</p>
      </div>
      <div class="slide-in delay-2 bg-white rounded-2xl border p-6 hover:shadow-lg transition-shadow">
        <div class="w-10 h-10 bg-emerald-100 rounded-xl flex items-center justify-center text-xl mb-4">⚡</div>
        <h3 class="font-bold text-gray-900 mb-2">Lightning Fast</h3>
        <p class="text-sm text-gray-500">Edge-cached pages with 99ms response times and 99.9% uptime.</p>
      </div>
      <div class="slide-in delay-1 bg-white rounded-2xl border p-6 hover:shadow-lg transition-shadow">
        <div class="w-10 h-10 bg-amber-100 rounded-xl flex items-center justify-center text-xl mb-4">🔒</div>
        <h3 class="font-bold text-gray-900 mb-2">Enterprise Security</h3>
        <p class="text-sm text-gray-500">SOC 2 compliant, end-to-end encryption, and fraud prevention.</p>
      </div>
      <div class="slide-in delay-2 bg-white rounded-2xl border p-6 hover:shadow-lg transition-shadow">
        <div class="w-10 h-10 bg-rose-100 rounded-xl flex items-center justify-center text-xl mb-4">🌍</div>
        <h3 class="font-bold text-gray-900 mb-2">Global i18n</h3>
        <p class="text-sm text-gray-500">Full support for 40+ languages including RTL, Thai, Japanese.</p>
      </div>
      <div class="slide-in delay-3 bg-white rounded-2xl border p-6 hover:shadow-lg transition-shadow">
        <div class="w-10 h-10 bg-teal-100 rounded-xl flex items-center justify-center text-xl mb-4">📊</div>
        <h3 class="font-bold text-gray-900 mb-2">Real-Time Analytics</h3>
        <p class="text-sm text-gray-500">Live revenue, order tracking, and AI-generated business insights.</p>
      </div>
    </div>
  </div>
</section>

<!-- CTA -->
<section class="bg-indigo-600 py-20 text-white text-center">
  <div class="max-w-2xl mx-auto px-4">
    <h2 class="text-4xl font-extrabold mb-4">Ready to launch?</h2>
    <p class="text-indigo-200 mb-8">Join 10,000+ merchants who use ShopAI to grow their business.</p>
    <button class="px-10 py-4 bg-white text-indigo-600 font-extrabold rounded-2xl hover:bg-indigo-50 transition-colors shadow-xl">
      Start free trial — no credit card required
    </button>
  </div>
</section>

<!-- Footer -->
<footer class="border-t py-10 px-4">
  <div class="max-w-5xl mx-auto flex flex-col sm:flex-row items-center justify-between gap-4 text-sm text-gray-500">
    <div class="flex items-center gap-2">
      <div class="w-6 h-6 bg-indigo-600 rounded-md flex items-center justify-center text-white text-xs font-black">S</div>
      <span class="font-semibold text-gray-900">ShopAI</span>
    </div>
    <p>© 2025 ShopAI. Built with Next.js + Tailwind CSS + Claude AI</p>
    <div class="flex gap-4">
      <a href="#" class="hover:text-gray-900 transition-colors">Privacy</a>
      <a href="#" class="hover:text-gray-900 transition-colors">Terms</a>
    </div>
  </div>
</footer>

</body>
</html>
```

---

## สรุป Part 100 (Milestone: Step 1000!)

| Step | เนื้อหา |
|------|---------|
| 991 | Capstone project overview: ShopAI full-stack e-commerce |
| 992 | Complete project structure: app router + domain + infra layers |
| 993 | Root layout: fonts + providers + skip link |
| 994 | Homepage: hero section + Suspense + FeaturedProducts |
| 995 | Cart drawer: animated slide + accessibility + Zustand |
| 996 | Admin dashboard: Server Component + real-time metrics |
| 997 | E2E tests: checkout flow + AI search Playwright spec |
| 998 | Performance budget: Lighthouse CI thresholds + Core Web Vitals |
| 999 | Full curriculum summary: Foundation → Intermediate → Advanced → Professional → World-Class |
| **1000** | **Workshop: Complete SaaS landing page — ShopAI capstone** |

---

## 🎓 หลักสูตรจบสมบูรณ์!

**ยินดีด้วย! คุณเรียนจบหลักสูตร Tailwind CSS ระดับ World-Class แล้ว!**

ใน 100 Parts และ 1,000 Steps คุณได้เรียนรู้:

- **Tailwind CSS** ตั้งแต่พื้นฐานถึงเทคนิคขั้นสูง
- **Next.js 14** App Router, Server Components, Server Actions
- **Full-stack development**: API, Auth, Database, Caching
- **Architecture patterns**: Clean Architecture, DDD, Monorepo
- **Production skills**: CI/CD, Security, Performance, Monitoring
- **AI integration**: Claude API, RAG, streaming, moderation

พร้อมสำหรับการสร้าง production-ready applications ระดับโลกแล้ว! 🚀
