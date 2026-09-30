# Part 94: Monorepo & Micro-Frontends

## เป้าหมาย
- Turborepo monorepo setup
- Shared packages (UI, config, types)
- Module Federation concepts
- Steps 931–940

---

## Step 931: Turborepo Setup

```bash
npx create-turbo@latest my-monorepo
cd my-monorepo
```

```
my-monorepo/
├── apps/
│   ├── web/          ← Next.js main app
│   ├── admin/        ← Next.js admin panel
│   └── docs/         ← Next.js documentation site
├── packages/
│   ├── ui/           ← Shared React components + Tailwind
│   ├── config/       ← Shared Tailwind + TS config
│   ├── types/        ← Shared TypeScript types
│   └── utils/        ← Shared utilities
├── turbo.json
└── package.json
```

---

## Step 932: turbo.json Pipeline

```json
// turbo.json
{
  "$schema": "https://turbo.build/schema.json",
  "ui": "tui",
  "tasks": {
    "build": {
      "dependsOn": ["^build"],
      "inputs":    ["$TURBO_DEFAULT$", ".env*"],
      "outputs":   [".next/**", "dist/**"]
    },
    "test": {
      "dependsOn": ["^build"],
      "inputs":    ["src/**", "tests/**", "vitest.config.ts"]
    },
    "lint": {
      "inputs":    ["src/**", "*.config.*"]
    },
    "typecheck": {
      "dependsOn": ["^build"]
    },
    "dev": {
      "cache":     false,
      "persistent": true
    }
  }
}
```

```json
// package.json (root)
{
  "scripts": {
    "dev":       "turbo dev",
    "build":     "turbo build",
    "test":      "turbo test",
    "lint":      "turbo lint",
    "typecheck": "turbo typecheck",
    "build:web": "turbo build --filter=web"
  },
  "devDependencies": {
    "turbo": "latest"
  }
}
```

---

## Step 933: Shared UI Package

```
packages/ui/
├── src/
│   ├── components/
│   │   ├── Button.tsx
│   │   ├── Input.tsx
│   │   ├── Card.tsx
│   │   └── index.ts
│   └── index.ts
├── package.json
├── tailwind.config.ts
└── tsconfig.json
```

```json
// packages/ui/package.json
{
  "name": "@repo/ui",
  "version": "0.0.1",
  "exports": {
    ".": {
      "types":   "./src/index.ts",
      "default": "./src/index.ts"
    }
  },
  "peerDependencies": {
    "react": "^18",
    "react-dom": "^18"
  },
  "devDependencies": {
    "tailwindcss": "^3",
    "@repo/config": "*"
  }
}
```

```tsx
// packages/ui/src/components/Button.tsx
import type { ButtonHTMLAttributes, ReactNode } from 'react'

type Variant = 'primary' | 'secondary' | 'outline' | 'ghost' | 'destructive'
type Size    = 'sm' | 'md' | 'lg'

interface ButtonProps extends ButtonHTMLAttributes<HTMLButtonElement> {
  variant?:  Variant
  size?:     Size
  loading?:  boolean
  leftIcon?: ReactNode
  children:  ReactNode
}

const variants: Record<Variant, string> = {
  primary:     'bg-indigo-600 text-white hover:bg-indigo-700 border-transparent',
  secondary:   'bg-gray-100 text-gray-900 hover:bg-gray-200 border-transparent',
  outline:     'bg-transparent text-gray-900 hover:bg-gray-50 border-gray-300',
  ghost:       'bg-transparent text-gray-700 hover:bg-gray-100 border-transparent',
  destructive: 'bg-red-600 text-white hover:bg-red-700 border-transparent',
}

const sizes: Record<Size, string> = {
  sm: 'px-3 py-1.5 text-xs rounded-lg',
  md: 'px-4 py-2 text-sm rounded-xl',
  lg: 'px-5 py-2.5 text-base rounded-xl',
}

export function Button({ variant = 'primary', size = 'md', loading, leftIcon, children, disabled, className = '', ...props }: ButtonProps) {
  return (
    <button
      disabled={disabled || loading}
      className={`inline-flex items-center justify-center gap-2 font-semibold border transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-offset-2 focus-visible:ring-indigo-500 disabled:opacity-50 disabled:cursor-not-allowed ${variants[variant]} ${sizes[size]} ${className}`}
      {...props}
    >
      {loading && <span className="w-3.5 h-3.5 border-2 border-current border-t-transparent rounded-full animate-spin" />}
      {!loading && leftIcon}
      {children}
    </button>
  )
}

// packages/ui/src/index.ts
export { Button } from './components/Button'
export { Input }  from './components/Input'
export { Card }   from './components/Card'
```

---

## Step 934: Shared Config Package

```ts
// packages/config/tailwind.config.ts
import type { Config } from 'tailwindcss'

export const baseConfig: Partial<Config> = {
  darkMode: 'class',
  theme: {
    extend: {
      colors: {
        brand: {
          50:  'var(--brand-50,  #EEF2FF)',
          100: 'var(--brand-100, #E0E7FF)',
          500: 'var(--brand-500, #6366F1)',
          600: 'var(--brand-600, #4F46E5)',
          700: 'var(--brand-700, #4338CA)',
        },
      },
      fontFamily: {
        sans: ['var(--font-sans, Inter)', 'system-ui', 'sans-serif'],
        mono: ['var(--font-mono, JetBrains Mono)', 'monospace'],
      },
      animation: {
        'fade-in':    'fadeIn 0.3s ease',
        'slide-up':   'slideUp 0.3s ease',
        'slide-down': 'slideDown 0.3s ease',
      },
      keyframes: {
        fadeIn:    { from: { opacity: '0' },                        to: { opacity: '1' } },
        slideUp:   { from: { opacity: '0', transform: 'translateY(8px)' }, to: { opacity: '1', transform: 'translateY(0)' } },
        slideDown: { from: { opacity: '0', transform: 'translateY(-8px)' }, to: { opacity: '1', transform: 'translateY(0)' } },
      },
    },
  },
}
```

```ts
// apps/web/tailwind.config.ts
import type { Config } from 'tailwindcss'
import { baseConfig }  from '@repo/config/tailwind'

export default {
  ...baseConfig,
  content: [
    './src/**/*.{ts,tsx}',
    '../../packages/ui/src/**/*.{ts,tsx}',  // include shared UI
  ],
} satisfies Config
```

---

## Step 935: Shared Types Package

```ts
// packages/types/src/index.ts
export interface User {
  id:        number
  email:     string
  name:      string | null
  role:      'USER' | 'ADMIN' | 'SUPERADMIN'
  createdAt: string
}

export interface Product {
  id:          number
  name:        string
  description: string | null
  price:       number
  stock:       number
  categoryId:  number
  category?:   Category
  images:      string[]
  isActive:    boolean
  createdAt:   string
}

export interface Category {
  id:   number
  name: string
  slug: string
}

export interface Order {
  id:          number
  userId:      number
  user?:       User
  status:      OrderStatus
  totalPrice:  number
  items?:      OrderItem[]
  createdAt:   string
}

export type OrderStatus = 'PENDING' | 'PROCESSING' | 'SHIPPED' | 'DELIVERED' | 'CANCELLED'

export interface OrderItem {
  id:        number
  productId: number
  product?:  Product
  quantity:  number
  price:     number
}

// API response shapes
export interface ApiResponse<T> {
  data:       T
  message?:   string
}

export interface ApiError {
  error:    string
  code?:    string
  details?: Record<string, string[]>
}

export interface PaginatedResponse<T> {
  data:        T[]
  page:        number
  pageSize:    number
  totalCount:  number
  totalPages:  number
  hasNextPage: boolean
  hasPrevPage: boolean
}
```

---

## Step 936: Using Shared Packages in Apps

```tsx
// apps/web/src/app/products/page.tsx
import { Button }                from '@repo/ui'
import type { Product, PaginatedResponse } from '@repo/types'
import { formatCurrency }        from '@repo/utils'

async function getProducts(page: number): Promise<PaginatedResponse<Product>> {
  const res = await fetch(`/api/products?page=${page}`, { next: { revalidate: 60 } })
  return res.json()
}

export default async function ProductsPage() {
  const { data, totalCount } = await getProducts(1)

  return (
    <div className="p-6">
      <div className="flex justify-between items-center mb-6">
        <h1 className="text-2xl font-extrabold text-gray-900">Products ({totalCount})</h1>
        <Button variant="primary" size="sm">+ Add Product</Button>
      </div>
      <div className="grid grid-cols-3 gap-4">
        {data.map(product => (
          <div key={product.id} className="bg-white rounded-2xl border p-4">
            <h3 className="font-semibold text-gray-900">{product.name}</h3>
            <p className="text-indigo-600 font-bold mt-1">{formatCurrency(product.price)}</p>
          </div>
        ))}
      </div>
    </div>
  )
}
```

---

## Step 937: Running Monorepo

```bash
# Dev all apps
turbo dev

# Dev only web app
turbo dev --filter=web

# Build all
turbo build

# Build only changed packages (smart caching)
turbo build --filter=[HEAD^1]

# Test specific package
turbo test --filter=@repo/ui

# Add package to specific workspace
npm install lodash --workspace=apps/web

# Add shared package to app
npm install @repo/ui --workspace=apps/web
```

---

## Step 938: Module Federation (Micro-Frontends)

```ts
// next.config.mjs (host app)
import { NextFederationPlugin } from '@module-federation/nextjs-mf'

export default {
  webpack: (config, { isServer }) => {
    config.plugins.push(new NextFederationPlugin({
      name: 'host',
      remotes: {
        checkout: `checkout@http://localhost:3001/_next/static/chunks/remoteEntry.js`,
        catalog:  `catalog@http://localhost:3002/_next/static/chunks/remoteEntry.js`,
      },
      filename: 'static/chunks/remoteEntry.js',
      shared: {
        react:         { singleton: true },
        'react-dom':   { singleton: true },
        'next/router': { singleton: true },
      },
    }))
    return config
  },
}

// next.config.mjs (checkout remote)
new NextFederationPlugin({
  name: 'checkout',
  exposes: {
    './CheckoutFlow': './src/components/CheckoutFlow',
    './CartSummary':  './src/components/CartSummary',
  },
  filename: 'static/chunks/remoteEntry.js',
})

// Using remote component in host
import dynamic from 'next/dynamic'
const CheckoutFlow = dynamic(() => import('checkout/CheckoutFlow'), { ssr: false })
```

---

## Step 939: Versioning Strategy

```
Semantic Versioning สำหรับ shared packages:

MAJOR (1.0.0 → 2.0.0)
  - Breaking API changes
  - Removed exports
  - Changed component props (required → removed)

MINOR (1.0.0 → 1.1.0)
  - New components added
  - New optional props
  - New utilities

PATCH (1.0.0 → 1.0.1)
  - Bug fixes
  - Minor style tweaks
  - Performance improvements

Changeset workflow:
  npx changeset      ← describe change
  npx changeset version  ← bump versions
  npx changeset publish  ← publish to npm
```

---

## Step 940: Workshop — Monorepo Structure Visualizer

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Monorepo Visualizer</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 min-h-screen p-6">
<div class="max-w-2xl mx-auto">
  <h1 class="text-2xl font-extrabold text-gray-900 mb-2">Monorepo Dependency Graph</h1>
  <p class="text-sm text-gray-500 mb-6">arrows show "depends on" relationship</p>

  <div class="grid grid-cols-3 gap-4 mb-6">
    <!-- Apps column -->
    <div>
      <p class="text-xs font-bold text-gray-400 uppercase tracking-wide mb-2">Apps</p>
      <div class="space-y-2">
        <div class="bg-white rounded-2xl border-2 border-indigo-200 p-3 text-center">
          <p class="font-bold text-sm text-indigo-700">web</p>
          <p class="text-xs text-gray-500">Next.js</p>
        </div>
        <div class="bg-white rounded-2xl border-2 border-violet-200 p-3 text-center">
          <p class="font-bold text-sm text-violet-700">admin</p>
          <p class="text-xs text-gray-500">Next.js</p>
        </div>
        <div class="bg-white rounded-2xl border-2 border-teal-200 p-3 text-center">
          <p class="font-bold text-sm text-teal-700">docs</p>
          <p class="text-xs text-gray-500">Next.js</p>
        </div>
      </div>
    </div>

    <!-- Arrow -->
    <div class="flex flex-col items-center justify-center">
      <svg width="60" height="120" viewBox="0 0 60 120" class="text-gray-300">
        <defs><marker id="arrowhead" markerWidth="8" markerHeight="8" refX="8" refY="4" orient="auto"><polygon points="0 0, 8 4, 0 8" fill="#9CA3AF"/></marker></defs>
        <line x1="0" y1="30" x2="50" y2="30" stroke="#9CA3AF" stroke-width="1.5" marker-end="url(#arrowhead)"/>
        <line x1="0" y1="60" x2="50" y2="60" stroke="#9CA3AF" stroke-width="1.5" marker-end="url(#arrowhead)"/>
        <line x1="0" y1="90" x2="50" y2="90" stroke="#9CA3AF" stroke-width="1.5" marker-end="url(#arrowhead)"/>
      </svg>
    </div>

    <!-- Packages column -->
    <div>
      <p class="text-xs font-bold text-gray-400 uppercase tracking-wide mb-2">Packages</p>
      <div class="space-y-2">
        <div class="bg-white rounded-2xl border-2 border-emerald-200 p-3 text-center">
          <p class="font-bold text-sm text-emerald-700">@repo/ui</p>
          <p class="text-xs text-gray-500">Components</p>
        </div>
        <div class="bg-white rounded-2xl border-2 border-amber-200 p-3 text-center">
          <p class="font-bold text-sm text-amber-700">@repo/types</p>
          <p class="text-xs text-gray-500">TypeScript</p>
        </div>
        <div class="bg-white rounded-2xl border-2 border-rose-200 p-3 text-center">
          <p class="font-bold text-sm text-rose-700">@repo/config</p>
          <p class="text-xs text-gray-500">Tailwind + TS</p>
        </div>
      </div>
    </div>
  </div>

  <!-- Turbo cache benefits -->
  <div class="bg-white rounded-2xl border p-5">
    <h2 class="font-bold text-gray-900 mb-3">Turbo Remote Cache ประโยชน์</h2>
    <div class="space-y-2">
      <div class="flex items-center gap-3">
        <div class="w-20 text-xs text-gray-500 text-right">ไม่มี cache</div>
        <div class="flex-1 bg-red-100 rounded-full h-5 flex items-center px-2">
          <div class="bg-red-500 h-3 rounded-full" style="width:100%"></div>
        </div>
        <div class="w-16 text-xs font-bold text-gray-700">~4 นาที</div>
      </div>
      <div class="flex items-center gap-3">
        <div class="w-20 text-xs text-gray-500 text-right">Local cache</div>
        <div class="flex-1 bg-amber-100 rounded-full h-5 flex items-center px-2">
          <div class="bg-amber-500 h-3 rounded-full" style="width:30%"></div>
        </div>
        <div class="w-16 text-xs font-bold text-gray-700">~72 วินาที</div>
      </div>
      <div class="flex items-center gap-3">
        <div class="w-20 text-xs text-gray-500 text-right">Remote cache</div>
        <div class="flex-1 bg-emerald-100 rounded-full h-5 flex items-center px-2">
          <div class="bg-emerald-500 h-3 rounded-full" style="width:5%"></div>
        </div>
        <div class="w-16 text-xs font-bold text-gray-700">~5 วินาที</div>
      </div>
    </div>
  </div>
</div>
</body>
</html>
```

---

## สรุป Part 94

| Step | เนื้อหา |
|------|---------|
| 931 | Turborepo setup + monorepo directory structure |
| 932 | turbo.json pipeline: build/test/lint/dev tasks |
| 933 | @repo/ui: shared Button component with variants/sizes |
| 934 | @repo/config: shared Tailwind base config + animations |
| 935 | @repo/types: shared TypeScript interfaces |
| 936 | Using shared packages in Next.js apps |
| 937 | Turborepo CLI commands + smart caching |
| 938 | Module Federation: host + remote apps |
| 939 | Semantic versioning + Changeset workflow |
| 940 | Workshop: Monorepo dependency graph + cache visualizer |

**Part ถัดไป:** Part 95 — Real-Time Features (Steps 941–950)
