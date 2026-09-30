# Part 87: Next.js App Router Integration

## เป้าหมาย
- Next.js 14 App Router + Tailwind CSS
- Server Components vs Client Components
- Layout, loading, error pages
- Server Actions + Form
- Steps 861–870

---

## Step 861: Next.js + Tailwind Setup

```bash
npx create-next-app@latest my-app --typescript --tailwind --eslint --app --src-dir
cd my-app
npm run dev
```

```
my-app/
├── src/
│   ├── app/
│   │   ├── layout.tsx       ← Root layout
│   │   ├── page.tsx         ← Home page (/)
│   │   ├── globals.css      ← Global styles + Tailwind directives
│   │   ├── (auth)/
│   │   │   ├── login/page.tsx
│   │   │   └── register/page.tsx
│   │   ├── dashboard/
│   │   │   ├── layout.tsx   ← Dashboard layout
│   │   │   ├── page.tsx
│   │   │   └── loading.tsx  ← Loading UI
│   │   └── [...not-found]
│   └── components/
├── tailwind.config.ts
└── next.config.ts
```

---

## Step 862: Root Layout

```tsx
// src/app/layout.tsx
import type { Metadata } from 'next'
import { Inter } from 'next/font/google'
import './globals.css'

const inter = Inter({
  subsets: ['latin'],
  display: 'swap',   // font-display: swap
  variable: '--font-inter',
})

export const metadata: Metadata = {
  title: { template: '%s | MyApp', default: 'MyApp' },
  description: 'The best app for your workflow',
  openGraph: {
    type: 'website',
    locale: 'th_TH',
    siteName: 'MyApp',
  },
}

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="th" className={inter.variable}>
      <body className={`${inter.className} antialiased bg-gray-50 text-gray-900`}>
        {children}
      </body>
    </html>
  )
}
```

```css
/* src/app/globals.css */
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  :root {
    --font-inter: '';
  }
  * { @apply border-border; }
  body { @apply bg-background text-foreground; }
}
```

---

## Step 863: Server Component (Default)

```tsx
// src/app/dashboard/page.tsx — Server Component (no 'use client')
import { Suspense } from 'react'
import { KpiCards } from '@/components/KpiCards'
import { RecentOrders } from '@/components/RecentOrders'
import { KpiSkeleton, TableSkeleton } from '@/components/Skeletons'

// Server Component: เข้าถึง DB, filesystem, env variables ได้ตรงๆ
async function getStats() {
  // Direct DB fetch — no useEffect needed
  const stats = await fetch('https://api.example.com/stats', {
    next: { revalidate: 60 }, // ISR: revalidate ทุก 60s
  }).then(r => r.json())
  return stats
}

export const metadata = { title: 'Dashboard' }

export default async function DashboardPage() {
  const stats = await getStats()

  return (
    <main className="p-6 space-y-6">
      <div>
        <h1 className="text-2xl font-extrabold text-gray-900">Dashboard</h1>
        <p className="text-gray-500 text-sm mt-1">Overview of your business</p>
      </div>

      {/* Suspense สำหรับ async child components */}
      <Suspense fallback={<KpiSkeleton />}>
        <KpiCards stats={stats} />
      </Suspense>

      <Suspense fallback={<TableSkeleton />}>
        <RecentOrders />
      </Suspense>
    </main>
  )
}
```

---

## Step 864: Client Component

```tsx
// src/components/ThemeToggle.tsx — Client Component
'use client'

import { useState, useEffect } from 'react'

export function ThemeToggle() {
  const [dark, setDark] = useState(false)

  useEffect(() => {
    const saved = localStorage.getItem('theme')
    const prefersDark = matchMedia('(prefers-color-scheme: dark)').matches
    setDark(saved === 'dark' || (!saved && prefersDark))
  }, [])

  useEffect(() => {
    document.documentElement.classList.toggle('dark', dark)
    localStorage.setItem('theme', dark ? 'dark' : 'light')
  }, [dark])

  return (
    <button
      onClick={() => setDark(d => !d)}
      className="p-2 rounded-xl hover:bg-gray-100 dark:hover:bg-slate-800 transition-colors"
      aria-label={dark ? 'Switch to light mode' : 'Switch to dark mode'}
    >
      {dark ? '☀️' : '🌙'}
    </button>
  )
}
```

---

## Step 865: Loading + Error Pages

```tsx
// src/app/dashboard/loading.tsx — Skeleton loading
export default function Loading() {
  return (
    <div className="p-6 space-y-6 animate-pulse">
      {/* KPI skeleton */}
      <div className="grid grid-cols-4 gap-4">
        {[...Array(4)].map((_,i) => (
          <div key={i} className="bg-white rounded-2xl border p-5">
            <div className="h-3 bg-gray-200 rounded w-24 mb-3"></div>
            <div className="h-8 bg-gray-200 rounded w-16"></div>
          </div>
        ))}
      </div>
      {/* Table skeleton */}
      <div className="bg-white rounded-2xl border p-5">
        <div className="h-4 bg-gray-200 rounded w-32 mb-4"></div>
        {[...Array(5)].map((_,i) => (
          <div key={i} className="flex gap-4 py-3 border-t">
            <div className="h-3 bg-gray-200 rounded flex-1"></div>
            <div className="h-3 bg-gray-200 rounded w-24"></div>
            <div className="h-3 bg-gray-200 rounded w-16"></div>
          </div>
        ))}
      </div>
    </div>
  )
}
```

```tsx
// src/app/dashboard/error.tsx — Error boundary
'use client'
import { useEffect } from 'react'

export default function Error({ error, reset }: { error: Error & { digest?: string }, reset: () => void }) {
  useEffect(() => {
    // log to error service
    console.error(error)
  }, [error])

  return (
    <div className="flex flex-col items-center justify-center h-96 text-center p-6">
      <div className="w-16 h-16 bg-red-100 rounded-2xl flex items-center justify-center text-3xl mb-4">⚠️</div>
      <h2 className="text-xl font-extrabold text-gray-900 mb-2">Something went wrong</h2>
      <p className="text-gray-500 text-sm mb-6 max-w-sm">{error.message || 'An unexpected error occurred.'}</p>
      <button onClick={reset} className="px-5 py-2.5 bg-indigo-600 text-white text-sm font-semibold rounded-xl hover:bg-indigo-700 transition-colors">
        Try again
      </button>
    </div>
  )
}
```

---

## Step 866: not-found Page

```tsx
// src/app/not-found.tsx
import Link from 'next/link'

export default function NotFound() {
  return (
    <div className="min-h-screen flex items-center justify-center p-6">
      <div className="text-center max-w-md">
        <p className="text-8xl font-extrabold text-indigo-600 mb-4">404</p>
        <h2 className="text-2xl font-extrabold text-gray-900 mb-3">Page not found</h2>
        <p className="text-gray-500 mb-8">
          ไม่พบหน้าที่คุณต้องการ อาจถูกย้ายหรือลบไปแล้ว
        </p>
        <Link
          href="/"
          className="inline-flex items-center gap-2 px-6 py-3 bg-indigo-600 text-white font-semibold rounded-2xl hover:bg-indigo-700 transition-colors"
        >
          ← กลับหน้าแรก
        </Link>
      </div>
    </div>
  )
}
```

---

## Step 867: Server Actions

```tsx
// src/app/contact/page.tsx
import { ContactForm } from './ContactForm'

export default function ContactPage() {
  return (
    <main className="max-w-lg mx-auto p-6">
      <h1 className="text-2xl font-extrabold text-gray-900 mb-6">Contact Us</h1>
      <ContactForm />
    </main>
  )
}
```

```tsx
// src/app/contact/ContactForm.tsx
'use client'

import { useState } from 'react'
import { submitContact } from './actions'

export function ContactForm() {
  const [status, setStatus] = useState<'idle' | 'loading' | 'success' | 'error'>('idle')

  async function handleSubmit(e: React.FormEvent<HTMLFormElement>) {
    e.preventDefault()
    setStatus('loading')
    const formData = new FormData(e.currentTarget)
    const result = await submitContact(formData)
    setStatus(result.success ? 'success' : 'error')
  }

  return (
    <form onSubmit={handleSubmit} className="space-y-4 bg-white rounded-2xl border p-6">
      <div>
        <label htmlFor="name" className="block text-sm font-medium text-gray-700 mb-1.5">Name</label>
        <input id="name" name="name" required className="w-full px-3 py-2 border rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
      </div>
      <div>
        <label htmlFor="email" className="block text-sm font-medium text-gray-700 mb-1.5">Email</label>
        <input id="email" name="email" type="email" required className="w-full px-3 py-2 border rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
      </div>
      <div>
        <label htmlFor="message" className="block text-sm font-medium text-gray-700 mb-1.5">Message</label>
        <textarea id="message" name="message" rows={4} required className="w-full px-3 py-2 border rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 resize-none"></textarea>
      </div>

      {status === 'success' && (
        <div className="flex items-center gap-2 p-3 bg-emerald-50 border border-emerald-200 rounded-xl text-emerald-700 text-sm">
          ✅ ส่งข้อความสำเร็จ เราจะติดต่อกลับเร็วๆ นี้
        </div>
      )}

      <button
        type="submit"
        disabled={status === 'loading'}
        className="w-full py-2.5 bg-indigo-600 text-white text-sm font-semibold rounded-xl hover:bg-indigo-700 transition-colors disabled:opacity-50 flex items-center justify-center gap-2"
      >
        {status === 'loading' && <span className="w-4 h-4 border-2 border-white border-t-transparent rounded-full animate-spin" />}
        {status === 'loading' ? 'Sending...' : 'Send Message'}
      </button>
    </form>
  )
}
```

```ts
// src/app/contact/actions.ts — Server Action
'use server'

export async function submitContact(formData: FormData) {
  const name    = formData.get('name') as string
  const email   = formData.get('email') as string
  const message = formData.get('message') as string

  // Validate
  if (!name || !email || !message) return { success: false, error: 'Missing fields' }

  // Save to DB / send email
  try {
    // await db.contact.create({ data: { name, email, message } })
    // await sendEmail({ to: 'admin@example.com', subject: `Contact from ${name}`, body: message })
    return { success: true }
  } catch (err) {
    return { success: false, error: 'Failed to submit' }
  }
}
```

---

## Step 868: Route Groups + Parallel Routes

```
app/
├── (marketing)/          ← Route group (no URL segment)
│   ├── layout.tsx        ← Marketing layout
│   ├── page.tsx          ← /
│   ├── about/page.tsx    ← /about
│   └── pricing/page.tsx  ← /pricing
├── (app)/                ← App route group (requires auth)
│   ├── layout.tsx        ← App layout with sidebar
│   ├── dashboard/page.tsx
│   └── settings/page.tsx
└── @modal/               ← Parallel route (slot)
    └── (.)products/[id]/page.tsx  ← Intercepted route for modal
```

```tsx
// src/app/(app)/layout.tsx — Protected layout
import { redirect } from 'next/navigation'
import { getSession } from '@/lib/auth'
import { Sidebar } from '@/components/Sidebar'

export default async function AppLayout({ children }: { children: React.ReactNode }) {
  const session = await getSession()
  if (!session) redirect('/login')

  return (
    <div className="flex h-screen overflow-hidden">
      <Sidebar user={session.user} />
      <main className="flex-1 overflow-y-auto">
        {children}
      </main>
    </div>
  )
}
```

---

## Step 869: Image Optimization

```tsx
// ใช้ next/image — optimize + lazy load อัตโนมัติ
import Image from 'next/image'

// ✅ Static import — auto width/height
import heroImg from '@/assets/hero.jpg'

export function HeroImage() {
  return (
    <Image
      src={heroImg}
      alt="Hero"
      priority              // LCP image — preload
      className="w-full h-auto rounded-2xl"
      placeholder="blur"   // blur placeholder
    />
  )
}

// ✅ Remote image
export function ProductImage({ src, alt }: { src: string, alt: string }) {
  return (
    <Image
      src={src}
      alt={alt}
      width={400}
      height={400}
      loading="lazy"
      className="w-full aspect-square object-cover rounded-2xl"
    />
  )
}
```

```ts
// next.config.ts — allow remote images
import type { NextConfig } from 'next'
const config: NextConfig = {
  images: {
    remotePatterns: [
      { protocol: 'https', hostname: 'images.example.com' },
      { protocol: 'https', hostname: 'cdn.example.com' },
    ],
    formats: ['image/avif', 'image/webp'],
  },
}
export default config
```

---

## Step 870: Workshop — Next.js App Structure

```tsx
// tailwind.config.ts — Next.js optimized
import type { Config } from 'tailwindcss'

export default {
  content: [
    './src/pages/**/*.{js,ts,jsx,tsx,mdx}',
    './src/components/**/*.{js,ts,jsx,tsx,mdx}',
    './src/app/**/*.{js,ts,jsx,tsx,mdx}',
  ],
  darkMode: 'class',
  theme: {
    extend: {
      fontFamily: {
        sans: ['var(--font-inter)', 'system-ui', 'sans-serif'],
      },
      colors: {
        background: 'hsl(var(--background))',
        foreground:  'hsl(var(--foreground))',
        primary: {
          DEFAULT: 'hsl(var(--primary))',
          foreground: 'hsl(var(--primary-foreground))',
        },
      },
    },
  },
} satisfies Config
```

```
Next.js App Router Best Practices:
✅ Default to Server Components
✅ 'use client' เฉพาะ components ที่ต้องการ state/event
✅ ใช้ Suspense + loading.tsx สำหรับ async data
✅ Server Actions สำหรับ form mutations
✅ next/image สำหรับ image optimization
✅ next/font สำหรับ font optimization
✅ Route Groups สำหรับ layout sharing
✅ metadata export สำหรับ SEO
✅ generateStaticParams สำหรับ static generation
```

---

## สรุป Part 87

| Step | เนื้อหา |
|------|---------|
| 861 | Next.js + Tailwind setup + directory structure |
| 862 | Root layout + metadata + next/font |
| 863 | Server Component + ISR data fetching |
| 864 | Client Component ('use client') |
| 865 | loading.tsx skeleton + error.tsx boundary |
| 866 | not-found.tsx 404 page |
| 867 | Server Actions + form handling |
| 868 | Route Groups + parallel routes |
| 869 | next/image optimization |
| 870 | Workshop: Next.js best practices checklist |

**Part ถัดไป:** Part 88 — State Management Patterns (Steps 871–880)
