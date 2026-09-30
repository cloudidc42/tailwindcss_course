# Part 89: API Integration Patterns

## เป้าหมาย
- REST API best practices ใน Next.js
- Route Handlers (API Routes)
- Error handling + validation
- Rate limiting + middleware
- Steps 881–890

---

## Step 881: Route Handlers

```ts
// src/app/api/products/route.ts
import { NextRequest, NextResponse } from 'next/server'
import { z } from 'zod'

const CreateProductSchema = z.object({
  name:     z.string().min(1).max(100),
  price:    z.number().positive(),
  category: z.string(),
})

// GET /api/products
export async function GET(request: NextRequest) {
  const { searchParams } = request.nextUrl
  const page     = Number(searchParams.get('page') ?? 1)
  const limit    = Number(searchParams.get('limit') ?? 20)
  const category = searchParams.get('category') ?? undefined

  try {
    // const products = await db.product.findMany({ where: { category }, skip: (page-1)*limit, take: limit })
    const products: any[] = [] // placeholder
    return NextResponse.json({ data: products, page, hasNextPage: products.length === limit })
  } catch (err) {
    return NextResponse.json({ error: 'Failed to fetch products' }, { status: 500 })
  }
}

// POST /api/products
export async function POST(request: NextRequest) {
  try {
    const body = await request.json()
    const parsed = CreateProductSchema.safeParse(body)

    if (!parsed.success) {
      return NextResponse.json(
        { error: 'Validation failed', details: parsed.error.flatten() },
        { status: 400 }
      )
    }

    // const product = await db.product.create({ data: parsed.data })
    const product = { id: 1, ...parsed.data, createdAt: new Date().toISOString() }
    return NextResponse.json(product, { status: 201 })
  } catch (err) {
    return NextResponse.json({ error: 'Failed to create product' }, { status: 500 })
  }
}
```

---

## Step 882: Dynamic Route Handlers

```ts
// src/app/api/products/[id]/route.ts
import { NextRequest, NextResponse } from 'next/server'

type Params = { params: Promise<{ id: string }> }

// GET /api/products/:id
export async function GET(_req: NextRequest, { params }: Params) {
  const { id } = await params
  const numId = Number(id)
  if (!numId || isNaN(numId)) {
    return NextResponse.json({ error: 'Invalid ID' }, { status: 400 })
  }

  // const product = await db.product.findUnique({ where: { id: numId } })
  const product = null
  if (!product) return NextResponse.json({ error: 'Not found' }, { status: 404 })

  return NextResponse.json(product)
}

// PATCH /api/products/:id
export async function PATCH(request: NextRequest, { params }: Params) {
  const { id } = await params
  const body = await request.json()

  // const updated = await db.product.update({ where: { id: Number(id) }, data: body })
  return NextResponse.json({ id, ...body })
}

// DELETE /api/products/:id
export async function DELETE(_req: NextRequest, { params }: Params) {
  const { id } = await params
  // await db.product.delete({ where: { id: Number(id) } })
  return new NextResponse(null, { status: 204 })
}
```

---

## Step 883: API Client (fetch wrapper)

```ts
// src/lib/api.ts
class ApiError extends Error {
  constructor(public status: number, message: string, public data?: any) {
    super(message)
    this.name = 'ApiError'
  }
}

async function apiFetch<T>(url: string, options?: RequestInit): Promise<T> {
  const res = await fetch(url, {
    headers: { 'Content-Type': 'application/json', ...options?.headers },
    ...options,
  })

  if (!res.ok) {
    const errorData = await res.json().catch(() => null)
    throw new ApiError(res.status, errorData?.error ?? res.statusText, errorData)
  }

  if (res.status === 204) return null as T
  return res.json()
}

export const api = {
  get:    <T>(url: string, options?: RequestInit) => apiFetch<T>(url, options),
  post:   <T>(url: string, body: unknown) => apiFetch<T>(url, { method: 'POST', body: JSON.stringify(body) }),
  patch:  <T>(url: string, body: unknown) => apiFetch<T>(url, { method: 'PATCH', body: JSON.stringify(body) }),
  delete: <T>(url: string) => apiFetch<T>(url, { method: 'DELETE' }),
}

// Usage
// const products = await api.get<Product[]>('/api/products?page=1')
// const product = await api.post<Product>('/api/products', { name: 'New', price: 99 })
```

---

## Step 884: Middleware

```ts
// src/middleware.ts
import { NextRequest, NextResponse } from 'next/server'

// Rate limiting using Edge Runtime (simple in-memory, use Redis in production)
const rateLimit = new Map<string, { count: number; resetAt: number }>()

export function middleware(request: NextRequest) {
  const { pathname } = request.nextUrl

  // API rate limiting
  if (pathname.startsWith('/api/')) {
    const ip = request.ip ?? request.headers.get('x-forwarded-for') ?? 'unknown'
    const now = Date.now()
    const window = 60_000 // 1 minute
    const limit  = 100    // 100 req/min

    const entry = rateLimit.get(ip)
    if (entry && now < entry.resetAt) {
      if (entry.count >= limit) {
        return NextResponse.json({ error: 'Too many requests' }, {
          status: 429,
          headers: { 'Retry-After': String(Math.ceil((entry.resetAt - now) / 1000)) },
        })
      }
      entry.count++
    } else {
      rateLimit.set(ip, { count: 1, resetAt: now + window })
    }
  }

  // Auth protection
  if (pathname.startsWith('/dashboard') || pathname.startsWith('/settings')) {
    const token = request.cookies.get('session')?.value
    if (!token) {
      return NextResponse.redirect(new URL('/login', request.url))
    }
  }

  // CORS headers for API routes
  if (pathname.startsWith('/api/')) {
    const response = NextResponse.next()
    response.headers.set('Access-Control-Allow-Origin', process.env.ALLOWED_ORIGIN ?? '*')
    response.headers.set('Access-Control-Allow-Methods', 'GET,POST,PATCH,DELETE,OPTIONS')
    response.headers.set('Access-Control-Allow-Headers', 'Content-Type,Authorization')
    return response
  }

  return NextResponse.next()
}

export const config = {
  matcher: ['/api/:path*', '/dashboard/:path*', '/settings/:path*'],
}
```

---

## Step 885: Validation with Zod

```ts
// src/lib/schemas.ts
import { z } from 'zod'

export const ProductSchema = z.object({
  name:     z.string().min(1, 'ชื่อสินค้าต้องไม่ว่าง').max(100, 'ชื่อยาวเกิน 100 ตัวอักษร'),
  price:    z.number({ required_error: 'กรุณาระบุราคา' }).positive('ราคาต้องมากกว่า 0'),
  category: z.enum(['electronics', 'clothing', 'food'], { errorMap: () => ({ message: 'หมวดหมู่ไม่ถูกต้อง' }) }),
  stock:    z.number().int().min(0).default(0),
  images:   z.array(z.string().url()).min(1, 'ต้องมีรูปอย่างน้อย 1 รูป'),
  tags:     z.array(z.string()).optional().default([]),
})

export type Product = z.infer<typeof ProductSchema>

// Partial for PATCH
export const UpdateProductSchema = ProductSchema.partial()

// Form validation helper
export function validateForm<T>(schema: z.ZodSchema<T>, data: unknown): { success: true; data: T } | { success: false; errors: Record<string, string> } {
  const result = schema.safeParse(data)
  if (result.success) return { success: true, data: result.data }

  const errors: Record<string, string> = {}
  result.error.errors.forEach(err => {
    const path = err.path.join('.')
    errors[path] = err.message
  })
  return { success: false, errors }
}
```

---

## Step 886: Pagination Pattern

```ts
// src/lib/pagination.ts
interface PaginatedResponse<T> {
  data: T[]
  page: number
  pageSize: number
  totalCount: number
  totalPages: number
  hasNextPage: boolean
  hasPrevPage: boolean
}

export function paginate<T>(
  items: T[],
  page: number,
  pageSize = 20
): PaginatedResponse<T> {
  const totalCount = items.length
  const totalPages = Math.ceil(totalCount / pageSize)
  const start = (page - 1) * pageSize
  const data  = items.slice(start, start + pageSize)

  return {
    data,
    page,
    pageSize,
    totalCount,
    totalPages,
    hasNextPage: page < totalPages,
    hasPrevPage: page > 1,
  }
}

// Cursor-based pagination (better for large datasets)
export function cursorPaginate<T extends { id: number }>(
  items: T[],
  cursor: number | undefined,
  limit = 20
): { data: T[]; nextCursor: number | null } {
  const startIdx = cursor ? items.findIndex(i => i.id === cursor) + 1 : 0
  const data = items.slice(startIdx, startIdx + limit)
  const nextCursor = data.length === limit ? data[data.length - 1].id : null
  return { data, nextCursor }
}
```

---

## Step 887: API Integration with useInfiniteQuery

```tsx
// src/hooks/useInfiniteProducts.ts
import { useInfiniteQuery } from '@tanstack/react-query'
import { api } from '@/lib/api'

export function useInfiniteProducts(category?: string) {
  return useInfiniteQuery({
    queryKey: ['products', 'infinite', category],
    queryFn: ({ pageParam = 1 }) =>
      api.get<{ data: Product[]; hasNextPage: boolean; nextCursor: number | null }>(
        `/api/products?page=${pageParam}${category ? `&category=${category}` : ''}`
      ),
    initialPageParam: 1,
    getNextPageParam: (lastPage, allPages) =>
      lastPage.hasNextPage ? allPages.length + 1 : undefined,
  })
}

// Component with infinite scroll
export function InfiniteProductList() {
  const { data, fetchNextPage, hasNextPage, isFetchingNextPage } = useInfiniteProducts()

  // Intersection Observer for auto-load
  const observerRef = useRef<HTMLDivElement>(null)
  useEffect(() => {
    const el = observerRef.current
    if (!el) return
    const io = new IntersectionObserver(([entry]) => {
      if (entry.isIntersecting && hasNextPage && !isFetchingNextPage) fetchNextPage()
    }, { rootMargin: '200px' })
    io.observe(el)
    return () => io.disconnect()
  }, [hasNextPage, isFetchingNextPage, fetchNextPage])

  const allProducts = data?.pages.flatMap(p => p.data) ?? []

  return (
    <div>
      <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
        {allProducts.map(p => <ProductCard key={p.id} product={p} />)}
      </div>
      <div ref={observerRef} className="h-10 flex items-center justify-center mt-4">
        {isFetchingNextPage && <div className="w-6 h-6 border-2 border-indigo-600 border-t-transparent rounded-full animate-spin" />}
      </div>
    </div>
  )
}
```

---

## Step 888: Error Handling Strategy

```ts
// src/lib/errors.ts
export class AppError extends Error {
  constructor(
    public code: string,
    message: string,
    public statusCode = 500,
    public details?: any
  ) { super(message); this.name = 'AppError' }
}

export const Errors = {
  NotFound:      (resource: string) => new AppError('NOT_FOUND', `${resource} not found`, 404),
  Unauthorized:  ()                 => new AppError('UNAUTHORIZED', 'Authentication required', 401),
  Forbidden:     ()                 => new AppError('FORBIDDEN', 'Permission denied', 403),
  BadRequest:    (msg: string)      => new AppError('BAD_REQUEST', msg, 400),
  Conflict:      (msg: string)      => new AppError('CONFLICT', msg, 409),
  RateLimit:     ()                 => new AppError('RATE_LIMIT', 'Too many requests', 429),
}

// API error handler wrapper
export function withErrorHandler(
  handler: (req: NextRequest, ctx: any) => Promise<NextResponse>
) {
  return async (req: NextRequest, ctx: any) => {
    try {
      return await handler(req, ctx)
    } catch (err) {
      if (err instanceof AppError) {
        return NextResponse.json({ error: err.message, code: err.code, details: err.details }, { status: err.statusCode })
      }
      console.error('[API Error]', err)
      return NextResponse.json({ error: 'Internal server error' }, { status: 500 })
    }
  }
}

// Usage
export const GET = withErrorHandler(async (req) => {
  const product = await getProduct(1)
  if (!product) throw Errors.NotFound('Product')
  return NextResponse.json(product)
})
```

---

## Step 889: API Integration Demo

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>API Patterns Demo</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 min-h-screen p-6">
<div class="max-w-2xl mx-auto">
  <h1 class="text-2xl font-extrabold text-gray-900 mb-6">API Client Demo</h1>

  <div class="bg-white rounded-2xl border p-5 mb-4">
    <h2 class="font-bold text-gray-900 mb-3">Request Builder</h2>
    <div class="grid grid-cols-2 gap-3 mb-3">
      <select id="method" class="px-3 py-2 border rounded-xl text-sm bg-white focus:outline-none focus:ring-2 focus:ring-indigo-500">
        <option value="GET">GET</option>
        <option value="POST">POST</option>
        <option value="PATCH">PATCH</option>
        <option value="DELETE">DELETE</option>
      </select>
      <input id="url" value="https://jsonplaceholder.typicode.com/posts/1" class="px-3 py-2 border rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
    </div>
    <textarea id="body" placeholder='{"title": "New Post"}' rows="3" class="w-full px-3 py-2 border rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 resize-none mb-3 font-mono hidden"></textarea>
    <button onclick="sendRequest()" id="send-btn" class="px-4 py-2 bg-indigo-600 text-white text-sm font-semibold rounded-xl hover:bg-indigo-700 transition-colors flex items-center gap-2">
      <span id="spinner" class="w-4 h-4 border-2 border-white border-t-transparent rounded-full animate-spin hidden"></span>
      Send Request
    </button>
  </div>

  <div id="response-panel" class="hidden bg-white rounded-2xl border p-5">
    <div class="flex items-center gap-3 mb-3">
      <h2 class="font-bold text-gray-900">Response</h2>
      <span id="status-badge" class="px-2.5 py-0.5 text-xs rounded-full font-bold"></span>
      <span id="time-badge" class="text-xs text-gray-400"></span>
    </div>
    <pre id="response-body" class="text-xs bg-gray-50 rounded-xl p-4 overflow-x-auto font-mono text-gray-800 max-h-64 overflow-y-auto"></pre>
  </div>
</div>
<script>
  document.getElementById('method').addEventListener('change', function() {
    const body = document.getElementById('body')
    body.classList.toggle('hidden', ['GET','DELETE'].includes(this.value))
  })

  async function sendRequest() {
    const method = document.getElementById('method').value
    const url    = document.getElementById('url').value.trim()
    const bodyText = document.getElementById('body').value.trim()
    const btn    = document.getElementById('send-btn')
    const spinner = document.getElementById('spinner')

    btn.disabled = true
    spinner.classList.remove('hidden')

    const start = Date.now()
    try {
      const opts = { method, headers: { 'Content-Type': 'application/json' } }
      if (!['GET','DELETE'].includes(method) && bodyText) opts.body = bodyText

      const res = await fetch(url, opts)
      const elapsed = Date.now() - start
      const data = await res.json().catch(() => null)

      const statusEl = document.getElementById('status-badge')
      statusEl.textContent = res.status + ' ' + res.statusText
      statusEl.className = `px-2.5 py-0.5 text-xs rounded-full font-bold ${res.ok ? 'bg-emerald-100 text-emerald-700' : 'bg-red-100 text-red-700'}`
      document.getElementById('time-badge').textContent = elapsed + 'ms'
      document.getElementById('response-body').textContent = JSON.stringify(data, null, 2)
    } catch (err) {
      document.getElementById('response-body').textContent = 'Error: ' + err.message
    }

    btn.disabled = false
    spinner.classList.add('hidden')
    document.getElementById('response-panel').classList.remove('hidden')
  }
</script>
</body>
</html>
```

---

## Step 890: Workshop — API Checklist

```
✅ API Design Checklist

ROUTES
  ✅ RESTful naming: /api/resources (plural), /api/resources/:id
  ✅ HTTP methods ถูกต้อง: GET=read, POST=create, PATCH=update, DELETE=remove
  ✅ Status codes ถูกต้อง: 200/201/204/400/401/403/404/409/429/500

VALIDATION
  ✅ Input validation ด้วย Zod ก่อน process
  ✅ Return 400 พร้อม details เมื่อ validation fail
  ✅ Type-safe schemas แชร์ระหว่าง client/server

ERROR HANDLING
  ✅ Error handler wrapper ครอบทุก route
  ✅ ไม่ expose stack trace ใน production
  ✅ Consistent error format: { error, code, details }

SECURITY
  ✅ Authentication middleware บน protected routes
  ✅ Rate limiting บน API routes
  ✅ CORS headers configured
  ✅ Input sanitization (ป้องกัน XSS/injection)

PERFORMANCE
  ✅ Pagination (limit/offset หรือ cursor-based)
  ✅ Cache headers สำหรับ GET requests
  ✅ Database indexes บน columns ที่ query บ่อย
```

---

## สรุป Part 89

| Step | เนื้อหา |
|------|---------|
| 881 | Route Handlers GET/POST พร้อม Zod validation |
| 882 | Dynamic routes [id]: GET/PATCH/DELETE |
| 883 | API client wrapper ด้วย fetch + ApiError class |
| 884 | Middleware: rate limiting, auth protect, CORS |
| 885 | Zod schemas + validateForm helper |
| 886 | Pagination (offset-based + cursor-based) |
| 887 | useInfiniteQuery + IntersectionObserver scroll |
| 888 | Error handling strategy + withErrorHandler wrapper |
| 889 | Workshop: API request builder demo |
| 890 | API design checklist |

**Part ถัดไป:** Part 90 — Authentication & Authorization (Steps 891–900)
