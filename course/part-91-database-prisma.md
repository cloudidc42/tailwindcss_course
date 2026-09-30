# Part 91: Database Integration with Prisma

## เป้าหมาย
- Prisma ORM setup + schema
- CRUD operations
- Relations + migrations
- Type-safe queries
- Steps 901–910

---

## Step 901: Prisma Setup

```bash
npm install prisma @prisma/client
npx prisma init --datasource-provider postgresql
```

```prisma
// prisma/schema.prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id           Int       @id @default(autoincrement())
  email        String    @unique
  name         String?
  passwordHash String?
  role         Role      @default(USER)
  createdAt    DateTime  @default(now())
  updatedAt    DateTime  @updatedAt
  orders       Order[]
  reviews      Review[]

  @@index([email])
  @@map("users")
}

enum Role { USER ADMIN SUPERADMIN }

model Product {
  id          Int       @id @default(autoincrement())
  name        String
  description String?
  price       Decimal   @db.Decimal(10,2)
  stock       Int       @default(0)
  categoryId  Int
  category    Category  @relation(fields: [categoryId], references: [id])
  images      String[]
  isActive    Boolean   @default(true)
  createdAt   DateTime  @default(now())
  updatedAt   DateTime  @updatedAt
  orderItems  OrderItem[]
  reviews     Review[]

  @@index([categoryId])
  @@index([isActive, createdAt(sort: Desc)])
  @@map("products")
}

model Category {
  id       Int       @id @default(autoincrement())
  name     String    @unique
  slug     String    @unique
  products Product[]
  @@map("categories")
}

model Order {
  id         Int         @id @default(autoincrement())
  userId     Int
  user       User        @relation(fields: [userId], references: [id])
  status     OrderStatus @default(PENDING)
  totalPrice Decimal     @db.Decimal(10,2)
  createdAt  DateTime    @default(now())
  items      OrderItem[]

  @@index([userId, createdAt(sort: Desc)])
  @@map("orders")
}

enum OrderStatus { PENDING PROCESSING SHIPPED DELIVERED CANCELLED }

model OrderItem {
  id        Int     @id @default(autoincrement())
  orderId   Int
  order     Order   @relation(fields: [orderId], references: [id], onDelete: Cascade)
  productId Int
  product   Product @relation(fields: [productId], references: [id])
  quantity  Int
  price     Decimal @db.Decimal(10,2)

  @@map("order_items")
}

model Review {
  id        Int      @id @default(autoincrement())
  userId    Int
  user      User     @relation(fields: [userId], references: [id])
  productId Int
  product   Product  @relation(fields: [productId], references: [id])
  rating    Int
  comment   String?
  createdAt DateTime @default(now())

  @@unique([userId, productId])
  @@map("reviews")
}
```

---

## Step 902: Prisma Client Singleton

```ts
// src/lib/db.ts
import { PrismaClient } from '@prisma/client'

const globalForPrisma = globalThis as unknown as { prisma: PrismaClient }

export const db = globalForPrisma.prisma ?? new PrismaClient({
  log: process.env.NODE_ENV === 'development' ? ['query', 'error', 'warn'] : ['error'],
})

if (process.env.NODE_ENV !== 'production') globalForPrisma.prisma = db
```

---

## Step 903: CRUD Operations

```ts
// src/lib/products.ts
import { db } from '@/lib/db'
import type { Prisma } from '@prisma/client'

// READ — paginated list
export async function getProducts({
  page = 1,
  pageSize = 20,
  categoryId,
  search,
  sortBy = 'createdAt',
  sortOrder = 'desc',
}: {
  page?: number
  pageSize?: number
  categoryId?: number
  search?: string
  sortBy?: 'name' | 'price' | 'createdAt'
  sortOrder?: 'asc' | 'desc'
}) {
  const where: Prisma.ProductWhereInput = {
    isActive: true,
    ...(categoryId && { categoryId }),
    ...(search && {
      OR: [
        { name:        { contains: search, mode: 'insensitive' } },
        { description: { contains: search, mode: 'insensitive' } },
      ],
    }),
  }

  const [products, totalCount] = await db.$transaction([
    db.product.findMany({
      where,
      skip:    (page - 1) * pageSize,
      take:    pageSize,
      orderBy: { [sortBy]: sortOrder },
      include: { category: { select: { name: true, slug: true } } },
    }),
    db.product.count({ where }),
  ])

  return {
    data: products,
    page,
    pageSize,
    totalCount,
    totalPages: Math.ceil(totalCount / pageSize),
    hasNextPage: page * pageSize < totalCount,
  }
}

// READ — single
export async function getProductById(id: number) {
  return db.product.findUnique({
    where:   { id },
    include: {
      category: true,
      reviews:  { include: { user: { select: { name: true } } }, orderBy: { createdAt: 'desc' }, take: 5 },
    },
  })
}

// CREATE
export async function createProduct(data: Prisma.ProductCreateInput) {
  return db.product.create({ data, include: { category: true } })
}

// UPDATE
export async function updateProduct(id: number, data: Prisma.ProductUpdateInput) {
  return db.product.update({ where: { id }, data, include: { category: true } })
}

// DELETE (soft delete)
export async function deleteProduct(id: number) {
  return db.product.update({ where: { id }, data: { isActive: false } })
}
```

---

## Step 904: Order with Transaction

```ts
// src/lib/orders.ts
import { db } from '@/lib/db'

interface CreateOrderInput {
  userId: number
  items: { productId: number; quantity: number }[]
}

export async function createOrder({ userId, items }: CreateOrderInput) {
  return db.$transaction(async (tx) => {
    // 1. Get products + check stock
    const products = await tx.product.findMany({
      where: { id: { in: items.map(i => i.productId) }, isActive: true },
    })

    for (const item of items) {
      const product = products.find(p => p.id === item.productId)
      if (!product) throw new Error(`Product ${item.productId} not found`)
      if (product.stock < item.quantity) throw new Error(`Insufficient stock for ${product.name}`)
    }

    // 2. Calculate total
    const totalPrice = items.reduce((sum, item) => {
      const product = products.find(p => p.id === item.productId)!
      return sum + Number(product.price) * item.quantity
    }, 0)

    // 3. Create order
    const order = await tx.order.create({
      data: {
        userId,
        totalPrice,
        items: {
          create: items.map(item => ({
            productId: item.productId,
            quantity:  item.quantity,
            price:     products.find(p => p.id === item.productId)!.price,
          })),
        },
      },
      include: { items: { include: { product: true } } },
    })

    // 4. Decrement stock
    await Promise.all(items.map(item =>
      tx.product.update({
        where: { id: item.productId },
        data:  { stock: { decrement: item.quantity } },
      })
    ))

    return order
  })
}
```

---

## Step 905: Migrations

```bash
# สร้าง migration ครั้งแรก
npx prisma migrate dev --name init

# หลัง schema เปลี่ยน
npx prisma migrate dev --name add_product_tags

# Production deploy
npx prisma migrate deploy

# Reset database (dev only)
npx prisma migrate reset

# Open Prisma Studio (GUI)
npx prisma studio
```

```prisma
// เพิ่ม field ใน schema
model Product {
  // ... existing fields ...
  tags      String[]    // array of tags
  seoTitle  String?
  seoDesc   String?
  weight    Float?
  published Boolean     @default(false)
}
```

---

## Step 906: Seed Data

```ts
// prisma/seed.ts
import { db } from '../src/lib/db'
import { hash } from 'bcryptjs'

async function main() {
  console.log('Seeding database...')

  // Categories
  const categories = await Promise.all([
    db.category.upsert({ where: { slug: 'electronics' }, update: {}, create: { name: 'Electronics', slug: 'electronics' } }),
    db.category.upsert({ where: { slug: 'clothing'    }, update: {}, create: { name: 'Clothing',    slug: 'clothing'    } }),
    db.category.upsert({ where: { slug: 'books'       }, update: {}, create: { name: 'Books',       slug: 'books'       } }),
  ])

  // Admin user
  const adminHash = await hash('adminpassword123', 12)
  await db.user.upsert({
    where:  { email: 'admin@example.com' },
    update: {},
    create: { email: 'admin@example.com', name: 'Admin', passwordHash: adminHash, role: 'ADMIN' },
  })

  // Sample products
  const electronics = categories[0]
  await db.product.createMany({
    data: [
      { name: 'Laptop Pro 15"', price: 1299.99, categoryId: electronics.id, stock: 10, images: ['https://placehold.co/400x400'] },
      { name: 'Wireless Mouse',  price:   39.99, categoryId: electronics.id, stock: 50, images: ['https://placehold.co/400x400'] },
    ],
    skipDuplicates: true,
  })

  console.log('Seeding done ✓')
}

main()
  .catch(console.error)
  .finally(() => db.$disconnect())
```

```json
// package.json
{
  "prisma": { "seed": "ts-node --compiler-options {\"module\":\"CommonJS\"} prisma/seed.ts" }
}
```

```bash
npx prisma db seed
```

---

## Step 907: Prisma in Next.js API Routes

```ts
// src/app/api/products/route.ts
import { NextRequest, NextResponse }  from 'next/server'
import { db }                         from '@/lib/db'
import { auth }                       from '@/auth'
import { ProductSchema }              from '@/lib/schemas'
import { getProducts, createProduct } from '@/lib/products'

export async function GET(request: NextRequest) {
  const { searchParams } = request.nextUrl
  const page       = Number(searchParams.get('page') ?? 1)
  const search     = searchParams.get('q') ?? undefined
  const categoryId = searchParams.get('category') ? Number(searchParams.get('category')) : undefined

  try {
    const result = await getProducts({ page, search, categoryId })
    return NextResponse.json(result, {
      headers: { 'Cache-Control': 'public, s-maxage=60, stale-while-revalidate=300' },
    })
  } catch {
    return NextResponse.json({ error: 'Failed to fetch' }, { status: 500 })
  }
}

export async function POST(request: NextRequest) {
  const session = await auth()
  if (!session) return NextResponse.json({ error: 'Unauthorized' }, { status: 401 })

  const body   = await request.json()
  const parsed = ProductSchema.safeParse(body)
  if (!parsed.success) return NextResponse.json({ error: 'Invalid data', details: parsed.error.flatten() }, { status: 400 })

  try {
    const product = await createProduct({ ...parsed.data, category: { connect: { id: (parsed.data as any).categoryId } } })
    return NextResponse.json(product, { status: 201 })
  } catch (err: any) {
    if (err.code === 'P2003') return NextResponse.json({ error: 'Category not found' }, { status: 400 })
    return NextResponse.json({ error: 'Failed to create' }, { status: 500 })
  }
}
```

---

## Step 908: Advanced Queries

```ts
// src/lib/analytics.ts
import { db } from '@/lib/db'

// Aggregate stats
export async function getDashboardStats() {
  const [totalOrders, totalRevenue, topProducts, recentOrders] = await db.$transaction([
    db.order.count({ where: { status: { not: 'CANCELLED' } } }),

    db.order.aggregate({
      where:   { status: { in: ['DELIVERED', 'SHIPPED'] } },
      _sum:    { totalPrice: true },
    }),

    db.orderItem.groupBy({
      by:      ['productId'],
      _sum:    { quantity: true },
      orderBy: { _sum: { quantity: 'desc' } },
      take:    5,
    }),

    db.order.findMany({
      take:    10,
      orderBy: { createdAt: 'desc' },
      include: { user: { select: { name: true, email: true } }, items: { select: { quantity: true } } },
    }),
  ])

  return {
    totalOrders,
    totalRevenue: Number(totalRevenue._sum.totalPrice ?? 0),
    topProducts,
    recentOrders,
  }
}

// Full-text search (PostgreSQL)
export async function searchProducts(query: string) {
  return db.$queryRaw<{ id: number; name: string; rank: number }[]>`
    SELECT id, name,
      ts_rank(to_tsvector('english', name || ' ' || COALESCE(description, '')),
              plainto_tsquery('english', ${query})) AS rank
    FROM products
    WHERE to_tsvector('english', name || ' ' || COALESCE(description, '')) @@ plainto_tsquery('english', ${query})
      AND "isActive" = true
    ORDER BY rank DESC
    LIMIT 20
  `
}
```

---

## Step 909: Error Codes

```ts
// Prisma error handling
import { Prisma } from '@prisma/client'

function handlePrismaError(err: unknown) {
  if (err instanceof Prisma.PrismaClientKnownRequestError) {
    switch (err.code) {
      case 'P2002': return { status: 409, message: 'Record already exists (unique constraint)' }
      case 'P2003': return { status: 400, message: 'Related record not found (foreign key)' }
      case 'P2025': return { status: 404, message: 'Record not found' }
      case 'P2016': return { status: 400, message: 'Query interpretation error' }
      default:      return { status: 500, message: `Database error: ${err.code}` }
    }
  }
  if (err instanceof Prisma.PrismaClientValidationError) {
    return { status: 400, message: 'Invalid data provided' }
  }
  return { status: 500, message: 'Unexpected database error' }
}
```

---

## Step 910: Workshop — Database Schema Visualizer

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Prisma Schema Visualizer</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 min-h-screen p-6">
<div class="max-w-4xl mx-auto">
  <h1 class="text-2xl font-extrabold text-gray-900 mb-6">Database Schema</h1>
  <div class="grid grid-cols-2 md:grid-cols-3 gap-4">
    <!-- User model -->
    <div class="bg-white rounded-2xl border-2 border-indigo-200 p-4">
      <div class="flex items-center gap-2 mb-3"><div class="w-2.5 h-2.5 rounded-full bg-indigo-500"></div><h3 class="font-bold text-gray-900">User</h3></div>
      <div class="space-y-1 text-xs">
        <div class="flex gap-2"><span class="text-amber-600 font-mono">PK</span><span class="text-gray-700">id: Int</span></div>
        <div class="flex gap-2"><span class="text-emerald-600 font-mono">UQ</span><span class="text-gray-700">email: String</span></div>
        <div class="flex gap-2"><span class="text-gray-300 font-mono">  </span><span class="text-gray-700">name: String?</span></div>
        <div class="flex gap-2"><span class="text-gray-300 font-mono">  </span><span class="text-gray-500">role: Role</span></div>
        <div class="flex gap-2"><span class="text-blue-600 font-mono">FK</span><span class="text-gray-500">orders[]</span></div>
        <div class="flex gap-2"><span class="text-blue-600 font-mono">FK</span><span class="text-gray-500">reviews[]</span></div>
      </div>
    </div>
    <!-- Product model -->
    <div class="bg-white rounded-2xl border-2 border-violet-200 p-4">
      <div class="flex items-center gap-2 mb-3"><div class="w-2.5 h-2.5 rounded-full bg-violet-500"></div><h3 class="font-bold text-gray-900">Product</h3></div>
      <div class="space-y-1 text-xs">
        <div class="flex gap-2"><span class="text-amber-600 font-mono">PK</span><span class="text-gray-700">id: Int</span></div>
        <div class="flex gap-2"><span class="text-gray-300 font-mono">  </span><span class="text-gray-700">name: String</span></div>
        <div class="flex gap-2"><span class="text-gray-300 font-mono">  </span><span class="text-gray-700">price: Decimal</span></div>
        <div class="flex gap-2"><span class="text-gray-300 font-mono">  </span><span class="text-gray-700">stock: Int</span></div>
        <div class="flex gap-2"><span class="text-blue-600 font-mono">FK</span><span class="text-gray-500">categoryId</span></div>
        <div class="flex gap-2"><span class="text-blue-600 font-mono">FK</span><span class="text-gray-500">orderItems[]</span></div>
      </div>
    </div>
    <!-- Order model -->
    <div class="bg-white rounded-2xl border-2 border-emerald-200 p-4">
      <div class="flex items-center gap-2 mb-3"><div class="w-2.5 h-2.5 rounded-full bg-emerald-500"></div><h3 class="font-bold text-gray-900">Order</h3></div>
      <div class="space-y-1 text-xs">
        <div class="flex gap-2"><span class="text-amber-600 font-mono">PK</span><span class="text-gray-700">id: Int</span></div>
        <div class="flex gap-2"><span class="text-blue-600 font-mono">FK</span><span class="text-gray-700">userId</span></div>
        <div class="flex gap-2"><span class="text-gray-300 font-mono">  </span><span class="text-gray-700">status: Enum</span></div>
        <div class="flex gap-2"><span class="text-gray-300 font-mono">  </span><span class="text-gray-700">totalPrice</span></div>
        <div class="flex gap-2"><span class="text-blue-600 font-mono">FK</span><span class="text-gray-500">items[]</span></div>
      </div>
    </div>
    <!-- Category model -->
    <div class="bg-white rounded-2xl border-2 border-amber-200 p-4">
      <div class="flex items-center gap-2 mb-3"><div class="w-2.5 h-2.5 rounded-full bg-amber-500"></div><h3 class="font-bold text-gray-900">Category</h3></div>
      <div class="space-y-1 text-xs">
        <div class="flex gap-2"><span class="text-amber-600 font-mono">PK</span><span class="text-gray-700">id: Int</span></div>
        <div class="flex gap-2"><span class="text-emerald-600 font-mono">UQ</span><span class="text-gray-700">name: String</span></div>
        <div class="flex gap-2"><span class="text-emerald-600 font-mono">UQ</span><span class="text-gray-700">slug: String</span></div>
        <div class="flex gap-2"><span class="text-blue-600 font-mono">FK</span><span class="text-gray-500">products[]</span></div>
      </div>
    </div>
    <!-- OrderItem model -->
    <div class="bg-white rounded-2xl border-2 border-rose-200 p-4">
      <div class="flex items-center gap-2 mb-3"><div class="w-2.5 h-2.5 rounded-full bg-rose-500"></div><h3 class="font-bold text-gray-900">OrderItem</h3></div>
      <div class="space-y-1 text-xs">
        <div class="flex gap-2"><span class="text-amber-600 font-mono">PK</span><span class="text-gray-700">id: Int</span></div>
        <div class="flex gap-2"><span class="text-blue-600 font-mono">FK</span><span class="text-gray-700">orderId</span></div>
        <div class="flex gap-2"><span class="text-blue-600 font-mono">FK</span><span class="text-gray-700">productId</span></div>
        <div class="flex gap-2"><span class="text-gray-300 font-mono">  </span><span class="text-gray-700">quantity: Int</span></div>
        <div class="flex gap-2"><span class="text-gray-300 font-mono">  </span><span class="text-gray-700">price: Decimal</span></div>
      </div>
    </div>
    <!-- Review model -->
    <div class="bg-white rounded-2xl border-2 border-teal-200 p-4">
      <div class="flex items-center gap-2 mb-3"><div class="w-2.5 h-2.5 rounded-full bg-teal-500"></div><h3 class="font-bold text-gray-900">Review</h3></div>
      <div class="space-y-1 text-xs">
        <div class="flex gap-2"><span class="text-amber-600 font-mono">PK</span><span class="text-gray-700">id: Int</span></div>
        <div class="flex gap-2"><span class="text-blue-600 font-mono">FK</span><span class="text-gray-700">userId</span></div>
        <div class="flex gap-2"><span class="text-blue-600 font-mono">FK</span><span class="text-gray-700">productId</span></div>
        <div class="flex gap-2"><span class="text-gray-300 font-mono">  </span><span class="text-gray-700">rating: Int</span></div>
        <div class="flex gap-2"><span class="text-emerald-600 font-mono">UQ</span><span class="text-gray-500">userId+productId</span></div>
      </div>
    </div>
  </div>
  <div class="flex gap-4 mt-4 text-xs text-gray-500">
    <span class="flex items-center gap-1"><span class="text-amber-600 font-mono font-bold">PK</span> Primary Key</span>
    <span class="flex items-center gap-1"><span class="text-emerald-600 font-mono font-bold">UQ</span> Unique</span>
    <span class="flex items-center gap-1"><span class="text-blue-600 font-mono font-bold">FK</span> Foreign Key</span>
  </div>
</div>
</body>
</html>
```

---

## สรุป Part 91

| Step | เนื้อหา |
|------|---------|
| 901 | Prisma schema: User/Product/Category/Order/OrderItem/Review |
| 902 | PrismaClient singleton สำหรับ Next.js |
| 903 | CRUD functions: getProducts/getProductById/createProduct/update/softDelete |
| 904 | Transaction: createOrder พร้อม stock check + atomic decrement |
| 905 | Migrations: dev/deploy/reset + Prisma Studio |
| 906 | Seed data: upsert categories/users/products |
| 907 | API Routes ใช้ Prisma + Auth + Zod + Error codes |
| 908 | Advanced: aggregate stats + full-text search (PostgreSQL) |
| 909 | Prisma error codes: P2002/P2003/P2025 handler |
| 910 | Workshop: Database schema visualizer |

**Part ถัดไป:** Part 92 — Deployment & DevOps (Steps 911–920)
