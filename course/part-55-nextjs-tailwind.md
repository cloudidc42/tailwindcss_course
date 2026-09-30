# Part 55: Next.js + Tailwind Integration

## เป้าหมาย
- Next.js App Router + Tailwind CSS setup
- Server Components vs Client Components
- Layout, Metadata, Navigation patterns
- Steps 541–550

---

## Step 541: Next.js + Tailwind Setup

```bash
# สร้างโปรเจค Next.js พร้อม Tailwind
npx create-next-app@latest my-app --typescript --tailwind --app --eslint --src-dir

cd my-app
npm run dev
```

ไฟล์ที่ Tailwind ใช้ (`tailwind.config.ts`):
```ts
import type { Config } from 'tailwindcss'

const config: Config = {
  content: [
    './src/pages/**/*.{js,ts,jsx,tsx,mdx}',
    './src/components/**/*.{js,ts,jsx,tsx,mdx}',
    './src/app/**/*.{js,ts,jsx,tsx,mdx}',
  ],
  theme: {
    extend: {
      colors: {
        brand: {
          50:  '#eef2ff',
          100: '#e0e7ff',
          500: '#6366f1',
          600: '#4f46e5',
          700: '#4338ca',
        },
      },
    },
  },
  plugins: [],
}
export default config
```

`src/app/globals.css`:
```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  body {
    @apply bg-gray-50 text-gray-900 antialiased;
  }
}

@layer components {
  .btn {
    @apply inline-flex items-center gap-2 px-4 py-2.5 rounded-xl text-sm font-semibold transition-colors;
  }
  .btn-primary {
    @apply btn bg-brand-600 text-white hover:bg-brand-700;
  }
  .btn-outline {
    @apply btn border border-brand-600 text-brand-600 hover:bg-brand-50;
  }
  .card {
    @apply bg-white rounded-2xl border border-gray-200 shadow-sm;
  }
  .input {
    @apply w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-brand-500;
  }
}
```

---

## Step 542: App Router Layout

`src/app/layout.tsx`:
```tsx
import type { Metadata } from 'next'
import { Inter } from 'next/font/google'
import './globals.css'
import Sidebar from '@/components/Sidebar'
import Topbar from '@/components/Topbar'

const inter = Inter({ subsets: ['latin'] })

export const metadata: Metadata = {
  title: { template: '%s | MyApp', default: 'MyApp' },
  description: 'Next.js + Tailwind App',
}

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="th" className={inter.className}>
      <body>
        <div className="flex h-screen overflow-hidden">
          <Sidebar />
          <div className="flex-1 flex flex-col overflow-hidden">
            <Topbar />
            <main className="flex-1 overflow-y-auto p-6 bg-gray-50">
              {children}
            </main>
          </div>
        </div>
      </body>
    </html>
  )
}
```

---

## Step 543: Sidebar Component (Client)

`src/components/Sidebar.tsx`:
```tsx
'use client'
import Link from 'next/link'
import { usePathname } from 'next/navigation'
import { useState } from 'react'

const navItems = [
  { href: '/', label: 'Dashboard', icon: '🏠' },
  { href: '/users', label: 'Users', icon: '👥' },
  { href: '/products', label: 'Products', icon: '📦' },
  { href: '/settings', label: 'Settings', icon: '⚙️' },
]

export default function Sidebar() {
  const pathname = usePathname()
  const [collapsed, setCollapsed] = useState(false)

  return (
    <aside className={`flex-shrink-0 bg-white border-r flex flex-col transition-all duration-300 ${collapsed ? 'w-14' : 'w-52'}`}>
      <div className="h-14 border-b flex items-center px-3 overflow-hidden">
        <button
          onClick={() => setCollapsed(!collapsed)}
          className="w-8 h-8 flex items-center justify-center rounded-xl hover:bg-gray-100 transition-colors flex-shrink-0"
        >
          <svg className="w-5 h-5 text-gray-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M4 6h16M4 12h16M4 18h16" />
          </svg>
        </button>
        {!collapsed && <span className="ml-2 font-extrabold text-brand-600 whitespace-nowrap text-sm">MyApp</span>}
      </div>
      <nav className="p-2 space-y-0.5 flex-1">
        {navItems.map(item => {
          const active = pathname === item.href
          return (
            <Link key={item.href} href={item.href}
              className={`flex items-center gap-2.5 px-2 py-2 rounded-xl transition-colors text-sm ${
                active ? 'bg-brand-50 text-brand-700 font-semibold' : 'text-gray-600 hover:bg-gray-50'
              }`}>
              <span className="flex-shrink-0">{item.icon}</span>
              {!collapsed && <span className="whitespace-nowrap">{item.label}</span>}
            </Link>
          )
        })}
      </nav>
    </aside>
  )
}
```

---

## Step 544: Server Components + Data Fetching

`src/app/page.tsx` (Server Component):
```tsx
import KpiCard from '@/components/KpiCard'
import RecentTable from '@/components/RecentTable'

async function getStats() {
  // In real app: await fetch('/api/stats', { next: { revalidate: 60 } })
  return {
    revenue: '฿284,000',
    users: 1284,
    orders: 391,
    conversion: '3.2%',
  }
}

export default async function DashboardPage() {
  const stats = await getStats()

  return (
    <div className="space-y-6">
      <h1 className="text-xl font-extrabold text-gray-900">Dashboard</h1>

      <div className="grid grid-cols-2 xl:grid-cols-4 gap-4">
        <KpiCard label="Revenue" value={stats.revenue} change="+12%" up icon="💰" />
        <KpiCard label="Users" value={String(stats.users)} change="+8%" up icon="👥" />
        <KpiCard label="Orders" value={String(stats.orders)} change="-3%" up={false} icon="📦" />
        <KpiCard label="Conversion" value={stats.conversion} change="+0.5%" up icon="📈" />
      </div>

      <div className="grid grid-cols-1 lg:grid-cols-3 gap-6">
        <div className="lg:col-span-2 card p-5">
          <h2 className="font-bold text-sm mb-4">ยอดขายรายเดือน</h2>
          <div className="flex items-end gap-1.5 h-32">
            {[62,78,55,90,84,95,72,88,76,100,68,92].map((v, i) => (
              <div key={i} className={`flex-1 rounded-t-sm ${i === 11 ? 'bg-brand-600' : 'bg-brand-200'}`}
                style={{ height: `${v}%` }} />
            ))}
          </div>
        </div>
        <RecentTable />
      </div>
    </div>
  )
}
```

---

## Step 545: KpiCard Component

`src/components/KpiCard.tsx`:
```tsx
interface Props {
  label: string
  value: string
  change: string
  up: boolean
  icon: string
}

export default function KpiCard({ label, value, change, up, icon }: Props) {
  return (
    <div className="card p-4 hover:shadow-md transition-shadow cursor-pointer">
      <div className="flex items-center justify-between mb-3">
        <p className="text-xs text-gray-500 font-semibold">{label}</p>
        <span className="text-xl">{icon}</span>
      </div>
      <p className="text-2xl font-extrabold text-gray-900">{value}</p>
      <p className={`text-xs mt-1 ${up ? 'text-green-500' : 'text-red-500'}`}>
        {up ? '▲' : '▼'} {change}
      </p>
    </div>
  )
}
```

---

## Step 546: Users Page with Search (Client Component)

`src/app/users/page.tsx`:
```tsx
import type { Metadata } from 'next'
import UsersClient from '@/components/UsersClient'

export const metadata: Metadata = { title: 'Users' }

async function getUsers() {
  return [
    { id: 1, name: 'สมชาย เทคโน', email: 'sk@tech.co', role: 'admin', active: true },
    { id: 2, name: 'วิชัย ดีงาม', email: 'wd@corp.io', role: 'editor', active: true },
    { id: 3, name: 'นิดา สวยงาม', email: 'nd@mail.com', role: 'user', active: false },
  ]
}

export default async function UsersPage() {
  const users = await getUsers()
  return <UsersClient initialUsers={users} />
}
```

`src/components/UsersClient.tsx` — Client component with useState:
```tsx
'use client'
import { useState, useMemo } from 'react'

type User = { id: number; name: string; email: string; role: string; active: boolean }

const ROLE_COLORS: Record<string, string> = {
  admin: 'bg-red-100 text-red-700',
  editor: 'bg-brand-100 text-brand-700',
  user: 'bg-gray-100 text-gray-600',
}

export default function UsersClient({ initialUsers }: { initialUsers: User[] }) {
  const [users, setUsers] = useState(initialUsers)
  const [search, setSearch] = useState('')

  const filtered = useMemo(() =>
    search ? users.filter(u => u.name.toLowerCase().includes(search.toLowerCase())) : users,
    [users, search]
  )

  return (
    <div className="space-y-4">
      <div className="flex items-center justify-between">
        <h1 className="text-xl font-extrabold">Users ({filtered.length})</h1>
        <button className="btn-primary">+ เพิ่ม</button>
      </div>

      <div className="card overflow-hidden">
        <div className="p-4 border-b">
          <input value={search} onChange={e => setSearch(e.target.value)}
            placeholder="ค้นหาผู้ใช้..." className="input w-64" />
        </div>
        <table className="w-full text-sm">
          <thead className="bg-gray-50">
            <tr>
              {['ชื่อ','อีเมล','บทบาท','สถานะ',''].map(h => (
                <th key={h} className="px-5 py-3 text-left text-xs font-semibold text-gray-500">{h}</th>
              ))}
            </tr>
          </thead>
          <tbody className="divide-y">
            {filtered.map(user => (
              <tr key={user.id} className="hover:bg-gray-50 transition-colors">
                <td className="px-5 py-3">
                  <div className="flex items-center gap-2">
                    <div className="w-7 h-7 rounded-lg bg-brand-100 flex items-center justify-center text-xs font-bold text-brand-600">
                      {user.name[0]}
                    </div>
                    <span className="font-medium">{user.name}</span>
                  </div>
                </td>
                <td className="px-5 py-3 text-gray-500">{user.email}</td>
                <td className="px-5 py-3">
                  <span className={`text-xs font-semibold px-2.5 py-1 rounded-full ${ROLE_COLORS[user.role]}`}>
                    {user.role}
                  </span>
                </td>
                <td className="px-5 py-3">
                  <span className={`text-xs font-semibold px-2.5 py-1 rounded-full ${user.active ? 'bg-green-100 text-green-700' : 'bg-gray-100 text-gray-500'}`}>
                    {user.active ? 'active' : 'inactive'}
                  </span>
                </td>
                <td className="px-5 py-3">
                  <button onClick={() => setUsers(users.filter(u => u.id !== user.id))}
                    className="text-xs text-red-400 hover:text-red-600 transition-colors">ลบ</button>
                </td>
              </tr>
            ))}
          </tbody>
        </table>
      </div>
    </div>
  )
}
```

---

## Step 547: API Route + Form Action

`src/app/api/users/route.ts`:
```ts
import { NextResponse } from 'next/server'

const users = [
  { id: 1, name: 'สมชาย เทคโน', email: 'sk@tech.co', role: 'admin' },
]

export async function GET() {
  return NextResponse.json(users)
}

export async function POST(request: Request) {
  const body = await request.json()
  const newUser = { id: Date.now(), ...body }
  users.push(newUser)
  return NextResponse.json(newUser, { status: 201 })
}
```

Server Action (`src/app/actions.ts`):
```ts
'use server'
import { revalidatePath } from 'next/cache'

export async function createUser(formData: FormData) {
  const name = formData.get('name') as string
  const email = formData.get('email') as string

  // Save to DB here
  console.log('Create user:', { name, email })

  revalidatePath('/users')
}
```

Form Component using Server Action:
```tsx
import { createUser } from '@/app/actions'

export default function AddUserForm() {
  return (
    <form action={createUser} className="card p-5 max-w-sm space-y-3">
      <h3 className="font-bold">เพิ่มผู้ใช้ (Server Action)</h3>
      <input name="name" placeholder="ชื่อ" required className="input" />
      <input name="email" type="email" placeholder="อีเมล" required className="input" />
      <button type="submit" className="btn-primary w-full justify-center">บันทึก</button>
    </form>
  )
}
```

---

## Step 548: Loading + Error Boundaries

`src/app/users/loading.tsx`:
```tsx
export default function Loading() {
  return (
    <div className="space-y-4 animate-pulse">
      <div className="h-8 bg-gray-200 rounded-xl w-32" />
      <div className="card overflow-hidden">
        <div className="p-4 border-b">
          <div className="h-9 bg-gray-200 rounded-xl w-64" />
        </div>
        {[...Array(5)].map((_, i) => (
          <div key={i} className="flex items-center gap-3 px-5 py-3.5 border-b last:border-0">
            <div className="w-7 h-7 bg-gray-200 rounded-lg" />
            <div className="flex-1 space-y-1">
              <div className="h-3 bg-gray-200 rounded w-1/3" />
              <div className="h-3 bg-gray-100 rounded w-1/2" />
            </div>
          </div>
        ))}
      </div>
    </div>
  )
}
```

`src/app/users/error.tsx`:
```tsx
'use client'
export default function Error({ error, reset }: { error: Error; reset: () => void }) {
  return (
    <div className="card p-6 max-w-md text-center space-y-3">
      <div className="text-3xl">😞</div>
      <p className="font-bold text-gray-900">เกิดข้อผิดพลาด</p>
      <p className="text-sm text-gray-500">{error.message}</p>
      <button onClick={reset} className="btn-primary mx-auto">ลองใหม่</button>
    </div>
  )
}
```

---

## Step 549: Dark Mode with next-themes

```bash
npm install next-themes
```

`src/app/layout.tsx` — wrap with ThemeProvider:
```tsx
import { ThemeProvider } from 'next-themes'

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="th" suppressHydrationWarning>
      <body>
        <ThemeProvider attribute="class" defaultTheme="system" enableSystem>
          {/* layout content */}
          {children}
        </ThemeProvider>
      </body>
    </html>
  )
}
```

`tailwind.config.ts` — enable class-based dark mode:
```ts
const config: Config = {
  darkMode: 'class',
  // ...
}
```

Dark mode toggle component:
```tsx
'use client'
import { useTheme } from 'next-themes'
import { useEffect, useState } from 'react'

export default function ThemeToggle() {
  const { theme, setTheme } = useTheme()
  const [mounted, setMounted] = useState(false)
  useEffect(() => setMounted(true), [])

  if (!mounted) return <div className="w-9 h-9" />

  return (
    <button
      onClick={() => setTheme(theme === 'dark' ? 'light' : 'dark')}
      className="w-9 h-9 flex items-center justify-center rounded-xl hover:bg-gray-100 dark:hover:bg-gray-700 transition-colors"
      aria-label="Toggle theme"
    >
      {theme === 'dark' ? '☀️' : '🌙'}
    </button>
  )
}
```

---

## Step 550: Workshop — Next.js Full App Demo

```bash
# ไฟล์และโฟลเดอร์ที่ได้จาก workshop
src/
  app/
    layout.tsx          ← Root layout + ThemeProvider
    page.tsx            ← Dashboard (Server Component)
    globals.css         ← Tailwind base + @layer components
    loading.tsx         ← Global loading skeleton
    error.tsx           ← Global error boundary
    users/
      page.tsx          ← Users page (Server Component)
      loading.tsx       ← Users loading skeleton
      error.tsx         ← Users error boundary
    products/
      page.tsx          ← Products page
    api/
      users/route.ts    ← REST API route
    actions.ts          ← Server Actions
  components/
    Sidebar.tsx         ← Collapsible sidebar (Client)
    Topbar.tsx          ← Top nav with ThemeToggle (Client)
    KpiCard.tsx         ← KPI card (Server)
    RecentTable.tsx     ← Recent orders table (Server)
    UsersClient.tsx     ← Users table with search (Client)
    ThemeToggle.tsx     ← Dark/light toggle (Client)
    AddUserForm.tsx     ← Form with Server Action
```

สรุป Next.js patterns:
- **Server Component** — default, async, fetch data, no useState
- **Client Component** — `'use client'`, useState/useEffect, event handlers
- **Server Action** — `'use server'`, form action, revalidatePath
- **Route Handler** — `app/api/.../route.ts`, GET/POST/PUT/DELETE
- **Metadata** — `export const metadata: Metadata = { title, description }`
- **Loading/Error** — `loading.tsx` + `error.tsx` alongside `page.tsx`

---

## สรุป Part 55

| Step | เนื้อหา |
|------|---------|
| 541 | create-next-app + tailwind.config.ts + globals.css with @layer |
| 542 | App Router layout.tsx with Sidebar + Topbar |
| 543 | Sidebar client component with usePathname |
| 544 | Dashboard Server Component + async data fetching |
| 545 | KpiCard reusable component |
| 546 | Users page: Server + Client split pattern |
| 547 | API Route + Server Action + revalidatePath |
| 548 | Loading skeleton + Error boundary |
| 549 | Dark mode with next-themes + useTheme |
| 550 | Workshop: Full Next.js App architecture summary |

**Part ถัดไป:** Part 56 — Nuxt.js + Tailwind Integration (Steps 551–560)
