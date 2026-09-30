# Part 96: Performance at Scale

## เป้าหมาย
- Caching strategies (Redis, Edge)
- Database optimization
- CDN and edge computing
- Monitoring & profiling
- Steps 951–960

---

## Step 951: Redis Caching

```bash
npm install ioredis
```

```ts
// src/lib/redis.ts
import Redis from 'ioredis'

const globalForRedis = globalThis as unknown as { redis: Redis }

export const redis = globalForRedis.redis ?? new Redis(process.env.REDIS_URL!, {
  maxRetriesPerRequest: 3,
  lazyConnect: true,
})

if (process.env.NODE_ENV !== 'production') globalForRedis.redis = redis

// Cache helper
export async function withCache<T>(
  key: string,
  ttlSeconds: number,
  fetchFn: () => Promise<T>
): Promise<T> {
  const cached = await redis.get(key)
  if (cached) return JSON.parse(cached)

  const fresh = await fetchFn()
  await redis.setex(key, ttlSeconds, JSON.stringify(fresh))
  return fresh
}

// Cache invalidation helper
export async function invalidatePattern(pattern: string) {
  const keys = await redis.keys(pattern)
  if (keys.length > 0) await redis.del(...keys)
}

// Usage
const products = await withCache(
  `products:page:${page}:category:${category}`,
  300, // 5 minutes
  () => db.product.findMany({ /* ... */ })
)

// Invalidate on mutation
await invalidatePattern('products:*')
```

---

## Step 952: Edge Caching with Next.js

```ts
// Stale-While-Revalidate at the edge
export async function GET(request: NextRequest) {
  const data = await getProducts()
  return NextResponse.json(data, {
    headers: {
      // Cache at CDN for 60s, serve stale for 10min while revalidating
      'Cache-Control': 'public, s-maxage=60, stale-while-revalidate=600',
      // Vary by Accept-Language for i18n pages
      'Vary': 'Accept-Language',
    },
  })
}

// Route Segment Config (App Router)
// src/app/products/page.tsx
export const revalidate   = 60   // ISR: revalidate every 60s
export const dynamic      = 'force-static' // always static
export const dynamicParams = true  // allow unknown params

// On-demand revalidation
// src/app/api/revalidate/route.ts
import { revalidatePath, revalidateTag } from 'next/cache'

export async function POST(req: NextRequest) {
  const { secret, path, tag } = await req.json()
  if (secret !== process.env.REVALIDATE_SECRET) {
    return NextResponse.json({ error: 'Unauthorized' }, { status: 401 })
  }
  if (path) revalidatePath(path)
  if (tag)  revalidateTag(tag)
  return NextResponse.json({ revalidated: true })
}

// Tag data fetches
async function getProduct(id: number) {
  return fetch(`/api/products/${id}`, {
    next: { tags: [`product-${id}`, 'products'] }
  }).then(r => r.json())
}
```

---

## Step 953: Database Query Optimization

```ts
// src/lib/query-optimization.ts
import { db } from '@/lib/db'

// ❌ N+1 problem
async function badGetOrders() {
  const orders = await db.order.findMany()
  for (const order of orders) {
    order.user = await db.user.findUnique({ where: { id: order.userId } }) as any
    // N extra queries!
  }
  return orders
}

// ✅ Include relations in one query
async function goodGetOrders() {
  return db.order.findMany({
    include: {
      user:  { select: { name: true, email: true } },
      items: { include: { product: { select: { name: true, price: true, images: true } } }, take: 5 },
    },
    orderBy: { createdAt: 'desc' },
    take:    20,
  })
}

// ✅ Select only needed fields (projection)
async function getOrderSummaries(userId: number) {
  return db.order.findMany({
    where:  { userId },
    select: {
      id:         true,
      status:     true,
      totalPrice: true,
      createdAt:  true,
      _count:     { select: { items: true } },
    },
    orderBy: { createdAt: 'desc' },
  })
}

// ✅ Batch similar queries
async function getMultipleProducts(ids: number[]) {
  return db.product.findMany({
    where: { id: { in: ids } },
  })
}

// ✅ Connection pool (Prisma Accelerate or PgBouncer in production)
// DATABASE_URL="prisma://accelerate.prisma-data.net/?api_key=..."
```

---

## Step 954: Image Optimization

```tsx
// Full image optimization strategy

// 1. next/image with proper sizing
import Image from 'next/image'

export function OptimizedProductImage({ src, name }: { src: string; name: string }) {
  return (
    <Image
      src={src}
      alt={name}
      width={400}
      height={400}
      sizes="(max-width: 640px) 100vw, (max-width: 1024px) 50vw, 33vw"
      className="w-full aspect-square object-cover"
      placeholder="blur"
      blurDataURL="data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQABAAD/2wBDAAYEBQYFBAYGBQYHBwYIChAKCgkJChQODwwQFxQYGBcUFhYaHSUfGhsjHBYWICwgIyYnKSopGR8tMC0oMCUoKSj/2wBDAQcHBwoIChMKChMoGhYaKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCj/wAARCAABAAEDASIAAhEBAxEB/8QAFAABAAAAAAAAAAAAAAAAAAAACf/EABQQAQAAAAAAAAAAAAAAAAAAAAD/xAAUAQEAAAAAAAAAAAAAAAAAAAAA/8QAFBEBAAAAAAAAAAAAAAAAAAAAAP/aAAwDAQACEQMRAD8AJQAB/9k="
    />
  )
}

// 2. Responsive art direction
export function HeroImage() {
  return (
    <picture>
      <source media="(max-width: 640px)"  srcSet="/hero-mobile.webp" type="image/webp" />
      <source media="(min-width: 641px)"  srcSet="/hero-desktop.webp" type="image/webp" />
      <img src="/hero-desktop.jpg" alt="Hero" className="w-full h-auto" loading="eager" fetchPriority="high" />
    </picture>
  )
}

// 3. Lazy loading with LQIP
export function LazyImage({ src, alt, lqip }: { src: string; alt: string; lqip: string }) {
  return (
    <div className="relative overflow-hidden aspect-square bg-gray-100">
      <img src={lqip} alt="" aria-hidden className="absolute inset-0 w-full h-full object-cover blur-xl scale-110" />
      <Image src={src} alt={alt} fill className="object-cover" loading="lazy" />
    </div>
  )
}
```

---

## Step 955: Code Splitting

```tsx
// Dynamic imports with loading states
import dynamic from 'next/dynamic'

// Heavy components load only when needed
const RichTextEditor = dynamic(
  () => import('@/components/RichTextEditor'),
  {
    ssr: false,
    loading: () => <div className="h-48 bg-gray-100 rounded-2xl animate-pulse" />
  }
)

const DataTable = dynamic(
  () => import('@/components/DataTable').then(m => ({ default: m.DataTable })),
  { loading: () => <div className="h-64 bg-gray-100 rounded-2xl animate-pulse" /> }
)

const Charts = dynamic(() => import('@/components/Charts'), {
  ssr: false,
  loading: () => <div className="h-48 bg-gray-100 rounded-2xl animate-pulse" />,
})

// Route-level code splitting is automatic in Next.js App Router
// Each page.tsx is its own chunk
```

---

## Step 956: Web Worker for Heavy Computation

```ts
// public/workers/filter.worker.ts
self.onmessage = ({ data: { items, filters } }) => {
  const result = items.filter((item: any) => {
    if (filters.search) {
      const query = filters.search.toLowerCase()
      if (!item.name.toLowerCase().includes(query)) return false
    }
    if (filters.minPrice && item.price < filters.minPrice) return false
    if (filters.maxPrice && item.price > filters.maxPrice) return false
    if (filters.category && item.categoryId !== filters.category) return false
    return true
  })
  self.postMessage(result)
}

// Hook to use the worker
import { useEffect, useRef, useState } from 'react'

export function useWorkerFilter<T>(items: T[], filters: object) {
  const [filtered, setFiltered] = useState<T[]>(items)
  const workerRef = useRef<Worker>()

  useEffect(() => {
    workerRef.current = new Worker(new URL('/workers/filter.worker.ts', import.meta.url))
    workerRef.current.onmessage = ({ data }) => setFiltered(data)
    return () => workerRef.current?.terminate()
  }, [])

  useEffect(() => {
    workerRef.current?.postMessage({ items, filters })
  }, [items, filters])

  return filtered
}
```

---

## Step 957: Virtualized Lists

```tsx
// Large list rendering with react-window (virtualization)
import { FixedSizeList } from 'react-window'

interface Product { id: number; name: string; price: number; category: string }

export function VirtualProductList({ products }: { products: Product[] }) {
  return (
    <FixedSizeList
      height={600}
      itemCount={products.length}
      itemSize={72}   // row height in px
      width="100%"
    >
      {({ index, style }) => {
        const product = products[index]
        return (
          <div style={style} className="flex items-center gap-3 px-4 border-b">
            <div className="w-10 h-10 bg-gray-100 rounded-xl flex-shrink-0" />
            <div className="flex-1 min-w-0">
              <p className="font-semibold text-sm text-gray-900 truncate">{product.name}</p>
              <p className="text-xs text-gray-500">{product.category}</p>
            </div>
            <p className="text-sm font-bold text-indigo-600">${product.price}</p>
          </div>
        )
      }}
    </FixedSizeList>
  )
}
// Renders only ~10 rows in DOM regardless of 100k items
```

---

## Step 958: Performance Monitoring

```ts
// src/lib/performance.ts
export function measureApiCall(endpoint: string) {
  const start = performance.now()
  return {
    end: (status: number) => {
      const duration = performance.now() - start
      // Send to analytics
      if (typeof window !== 'undefined') {
        gtag?.('event', 'api_call', { endpoint, duration, status })
      }
      // Log slow requests (>1s)
      if (duration > 1000) {
        console.warn(`[Performance] Slow API call: ${endpoint} took ${Math.round(duration)}ms`)
      }
      return duration
    }
  }
}

// Core Web Vitals reporting
export function reportWebVitals({ id, name, value }: { id: string; name: string; value: number }) {
  const metrics: Record<string, string> = {
    LCP: 'Largest Contentful Paint',
    FID: 'First Input Delay',
    CLS: 'Cumulative Layout Shift',
    FCP: 'First Contentful Paint',
    TTFB: 'Time to First Byte',
  }

  console.log(`[Web Vital] ${metrics[name] ?? name}: ${Math.round(value)}${name === 'CLS' ? '' : 'ms'}`)
  // Send to Vercel Analytics, Google Analytics, or DataDog
}
```

---

## Step 959: Bundle Size Audit

```bash
# Analyze bundle
npm install --save-dev @next/bundle-analyzer
ANALYZE=true npm run build

# Common bundle size targets
# - First load JS: < 80KB gzip
# - Route-specific chunks: < 30KB
# - Third-party libs: measure impact before adding

# Lighthouse CI
npm install --save-dev @lhci/cli
```

```json
// lighthouserc.json
{
  "ci": {
    "collect": { "url": ["http://localhost:3000"], "numberOfRuns": 3 },
    "assert": {
      "assertions": {
        "categories:performance":   ["error", { "minScore": 0.9 }],
        "categories:accessibility": ["error", { "minScore": 0.9 }],
        "categories:seo":           ["warn",  { "minScore": 0.9 }],
        "first-contentful-paint":   ["error", { "maxNumericValue": 2000 }],
        "largest-contentful-paint": ["error", { "maxNumericValue": 3000 }],
        "cumulative-layout-shift":  ["error", { "maxNumericValue": 0.1  }]
      }
    }
  }
}
```

---

## Step 960: Workshop — Performance Dashboard

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Performance Dashboard</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 min-h-screen p-6">
<div class="max-w-2xl mx-auto">
  <h1 class="text-2xl font-extrabold text-gray-900 mb-6">Performance Metrics</h1>

  <!-- Core Web Vitals -->
  <div class="bg-white rounded-2xl border p-5 mb-4">
    <h2 class="font-bold text-gray-900 mb-4">Core Web Vitals</h2>
    <div class="grid grid-cols-3 gap-4" id="vitals"></div>
  </div>

  <!-- Cache hit rate simulator -->
  <div class="bg-white rounded-2xl border p-5">
    <h2 class="font-bold text-gray-900 mb-4">Cache Performance Simulator</h2>
    <div class="space-y-3">
      <div class="flex justify-between text-sm">
        <span class="text-gray-600">Requests</span>
        <span id="req-count" class="font-bold tabular-nums">0</span>
      </div>
      <div class="flex justify-between text-sm">
        <span class="text-gray-600">Cache Hits</span>
        <span id="hit-count" class="font-bold text-emerald-600 tabular-nums">0</span>
      </div>
      <div class="flex justify-between text-sm">
        <span class="text-gray-600">Cache Misses</span>
        <span id="miss-count" class="font-bold text-red-600 tabular-nums">0</span>
      </div>
      <div class="flex justify-between text-sm font-bold">
        <span>Hit Rate</span>
        <span id="hit-rate" class="tabular-nums">—</span>
      </div>
      <div class="w-full bg-gray-200 rounded-full h-2">
        <div id="hit-bar" class="bg-emerald-500 h-2 rounded-full transition-all" style="width:0%"></div>
      </div>
      <div class="grid grid-cols-2 gap-2 pt-2">
        <button onclick="simulateRequest(true)"  class="py-2 bg-emerald-600 text-white text-sm font-semibold rounded-xl hover:bg-emerald-700">Cache Hit</button>
        <button onclick="simulateRequest(false)" class="py-2 bg-red-500    text-white text-sm font-semibold rounded-xl hover:bg-red-600">Cache Miss</button>
      </div>
      <button onclick="reset()" class="w-full py-1.5 text-xs text-gray-500 hover:text-gray-700">Reset</button>
    </div>
  </div>
</div>
<script>
  const VITALS = [
    { name:'LCP', value:1.8, unit:'s', threshold:[2.5,4], label:'Largest Contentful Paint' },
    { name:'FID', value:12,  unit:'ms', threshold:[100,300], label:'First Input Delay' },
    { name:'CLS', value:0.05, unit:'', threshold:[0.1,0.25], label:'Cumulative Layout Shift' },
  ];

  function renderVitals() {
    document.getElementById('vitals').innerHTML = VITALS.map(v => {
      const good = v.value <= v.threshold[0];
      const poor = v.value >  v.threshold[1];
      const color = good ? 'text-emerald-600 bg-emerald-50' : poor ? 'text-red-600 bg-red-50' : 'text-amber-600 bg-amber-50';
      const badge = good ? '✅ Good' : poor ? '❌ Poor' : '⚠️ Needs improvement';
      return `<div class="text-center p-3 rounded-xl ${color}">
        <p class="text-xs font-bold opacity-60 mb-1">${v.name}</p>
        <p class="text-2xl font-extrabold tabular-nums">${v.value}${v.unit}</p>
        <p class="text-[10px] mt-1 font-semibold">${badge}</p>
      </div>`;
    }).join('');
  }

  let reqs=0, hits=0;
  function simulateRequest(isHit) {
    reqs++; if(isHit) hits++;
    const misses = reqs-hits;
    const rate = Math.round(hits/reqs*100);
    document.getElementById('req-count').textContent  = reqs;
    document.getElementById('hit-count').textContent  = hits;
    document.getElementById('miss-count').textContent = misses;
    document.getElementById('hit-rate').textContent   = rate+'%';
    document.getElementById('hit-rate').className = `tabular-nums font-bold ${rate>=80?'text-emerald-600':rate>=50?'text-amber-600':'text-red-600'}`;
    document.getElementById('hit-bar').style.width = rate+'%';
    document.getElementById('hit-bar').className = `h-2 rounded-full transition-all ${rate>=80?'bg-emerald-500':rate>=50?'bg-amber-500':'bg-red-500'}`;
  }

  function reset() { reqs=0; hits=0; simulateRequest=simulateRequest; ['req-count','hit-count','miss-count'].forEach(id=>document.getElementById(id).textContent='0'); document.getElementById('hit-rate').textContent='—'; document.getElementById('hit-bar').style.width='0%'; }

  renderVitals();
</script>
</body>
</html>
```

---

## สรุป Part 96

| Step | เนื้อหา |
|------|---------|
| 951 | Redis caching: withCache helper + invalidatePattern |
| 952 | Edge caching: s-maxage + stale-while-revalidate + revalidateTag |
| 953 | DB optimization: N+1 fix, select projection, batch queries |
| 954 | Image optimization: sizes, LQIP, art direction |
| 955 | Code splitting: dynamic import + loading skeleton |
| 956 | Web Worker: offload heavy filtering to background thread |
| 957 | Virtualized lists: react-window FixedSizeList |
| 958 | Performance monitoring: API timing + Web Vitals reporting |
| 959 | Bundle size audit: @next/bundle-analyzer + Lighthouse CI |
| 960 | Workshop: Core Web Vitals + cache hit rate simulator |

**Part ถัดไป:** Part 97 — Security Hardening (Steps 961–970)
