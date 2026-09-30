# Part 93: Advanced Architecture Patterns

## เป้าหมาย
- Clean Architecture ใน Next.js
- Repository Pattern
- Service Layer
- Domain-Driven Design concepts
- Steps 921–930

---

## Step 921: Clean Architecture Layers

```
src/
├── domain/                  ← Business rules (no dependencies)
│   ├── entities/
│   │   ├── Product.ts
│   │   └── Order.ts
│   └── repositories/        ← Interfaces (contracts)
│       ├── IProductRepository.ts
│       └── IOrderRepository.ts
│
├── application/             ← Use cases (depends only on domain)
│   ├── products/
│   │   ├── CreateProduct.ts
│   │   ├── GetProducts.ts
│   │   └── DeleteProduct.ts
│   └── orders/
│       └── PlaceOrder.ts
│
├── infrastructure/          ← DB, email, external APIs
│   ├── repositories/
│   │   ├── PrismaProductRepository.ts
│   │   └── PrismaOrderRepository.ts
│   └── email/
│       └── ResendEmailService.ts
│
└── presentation/            ← Next.js: routes, components, server actions
    ├── app/
    └── components/
```

---

## Step 922: Domain Entities

```ts
// src/domain/entities/Product.ts
export interface ProductProps {
  id:          number
  name:        string
  price:       number
  stock:       number
  categoryId:  number
  isActive:    boolean
}

export class Product {
  constructor(private props: ProductProps) {}

  get id()         { return this.props.id }
  get name()       { return this.props.name }
  get price()      { return this.props.price }
  get stock()      { return this.props.stock }
  get isActive()   { return this.props.isActive }

  // Business rules live in the entity
  isInStock(quantity: number): boolean {
    return this.props.stock >= quantity
  }

  canBeDeleted(): boolean {
    return this.props.isActive  // can only soft-delete active products
  }

  withUpdatedPrice(newPrice: number): Product {
    if (newPrice <= 0) throw new Error('Price must be positive')
    return new Product({ ...this.props, price: newPrice })
  }

  toJSON(): ProductProps {
    return { ...this.props }
  }

  static create(props: Omit<ProductProps, 'id' | 'isActive'>): Omit<ProductProps, 'id'> {
    if (!props.name.trim()) throw new Error('Product name cannot be empty')
    if (props.price <= 0)   throw new Error('Price must be positive')
    return { ...props, isActive: true }
  }
}
```

---

## Step 923: Repository Interfaces

```ts
// src/domain/repositories/IProductRepository.ts
import type { Product } from '../entities/Product'

export interface PaginatedResult<T> {
  data:       T[]
  totalCount: number
  page:       number
  totalPages: number
  hasNextPage: boolean
}

export interface ProductFilters {
  search?:    string
  categoryId?: number
  page?:      number
  pageSize?:  number
  sortBy?:    'name' | 'price' | 'createdAt'
  sortOrder?: 'asc' | 'desc'
}

export interface IProductRepository {
  findById(id: number):                          Promise<Product | null>
  findMany(filters: ProductFilters):             Promise<PaginatedResult<Product>>
  create(data: Omit<Product['toJSON'], 'id'>):   Promise<Product>
  update(id: number, data: Partial<ProductFilters>): Promise<Product>
  softDelete(id: number):                        Promise<void>
  exists(id: number):                            Promise<boolean>
}
```

---

## Step 924: Infrastructure Implementation

```ts
// src/infrastructure/repositories/PrismaProductRepository.ts
import { db }    from '@/lib/db'
import { Product } from '@/domain/entities/Product'
import type { IProductRepository, ProductFilters, PaginatedResult } from '@/domain/repositories/IProductRepository'
import type { Prisma } from '@prisma/client'

export class PrismaProductRepository implements IProductRepository {
  async findById(id: number): Promise<Product | null> {
    const row = await db.product.findUnique({ where: { id, isActive: true } })
    if (!row) return null
    return new Product({ ...row, price: Number(row.price) })
  }

  async findMany(filters: ProductFilters): Promise<PaginatedResult<Product>> {
    const { search, categoryId, page = 1, pageSize = 20, sortBy = 'createdAt', sortOrder = 'desc' } = filters

    const where: Prisma.ProductWhereInput = {
      isActive: true,
      ...(categoryId && { categoryId }),
      ...(search && { OR: [
        { name:        { contains: search, mode: 'insensitive' } },
        { description: { contains: search, mode: 'insensitive' } },
      ]}),
    }

    const [rows, totalCount] = await db.$transaction([
      db.product.findMany({ where, skip: (page-1)*pageSize, take: pageSize, orderBy: { [sortBy]: sortOrder } }),
      db.product.count({ where }),
    ])

    return {
      data:       rows.map(r => new Product({ ...r, price: Number(r.price) })),
      totalCount,
      page,
      totalPages: Math.ceil(totalCount / pageSize),
      hasNextPage: page * pageSize < totalCount,
    }
  }

  async create(data: any): Promise<Product> {
    const row = await db.product.create({ data })
    return new Product({ ...row, price: Number(row.price) })
  }

  async update(id: number, data: any): Promise<Product> {
    const row = await db.product.update({ where: { id }, data })
    return new Product({ ...row, price: Number(row.price) })
  }

  async softDelete(id: number): Promise<void> {
    await db.product.update({ where: { id }, data: { isActive: false } })
  }

  async exists(id: number): Promise<boolean> {
    const count = await db.product.count({ where: { id, isActive: true } })
    return count > 0
  }
}
```

---

## Step 925: Use Cases (Application Layer)

```ts
// src/application/products/CreateProduct.ts
import type { IProductRepository } from '@/domain/repositories/IProductRepository'
import { Product } from '@/domain/entities/Product'

interface CreateProductDTO {
  name:       string
  price:      number
  stock:      number
  categoryId: number
}

export class CreateProductUseCase {
  constructor(private productRepo: IProductRepository) {}

  async execute(dto: CreateProductDTO): Promise<Product> {
    // Validate via domain entity
    const validData = Product.create(dto)

    // Business rule: check for duplicate name
    // (would need IProductRepository.findByName in real impl)

    return this.productRepo.create(validData as any)
  }
}

// src/application/products/GetProducts.ts
export class GetProductsUseCase {
  constructor(private productRepo: IProductRepository) {}

  async execute(filters: ProductFilters) {
    return this.productRepo.findMany(filters)
  }
}

// src/application/products/DeleteProduct.ts
export class DeleteProductUseCase {
  constructor(private productRepo: IProductRepository) {}

  async execute(id: number, requestedBy: { role: string }) {
    if (requestedBy.role !== 'ADMIN' && requestedBy.role !== 'SUPERADMIN') {
      throw new Error('FORBIDDEN: Only admins can delete products')
    }

    const product = await this.productRepo.findById(id)
    if (!product)              throw new Error('NOT_FOUND: Product not found')
    if (!product.canBeDeleted()) throw new Error('CONFLICT: Product cannot be deleted')

    await this.productRepo.softDelete(id)
  }
}
```

---

## Step 926: Dependency Injection

```ts
// src/lib/container.ts — simple DI container
import { PrismaProductRepository } from '@/infrastructure/repositories/PrismaProductRepository'
import { CreateProductUseCase }    from '@/application/products/CreateProduct'
import { GetProductsUseCase }      from '@/application/products/GetProducts'
import { DeleteProductUseCase }    from '@/application/products/DeleteProduct'

// Singleton instances
const productRepo = new PrismaProductRepository()

export const useCases = {
  createProduct: new CreateProductUseCase(productRepo),
  getProducts:   new GetProductsUseCase(productRepo),
  deleteProduct: new DeleteProductUseCase(productRepo),
}

// Usage in API route
// import { useCases } from '@/lib/container'
// const products = await useCases.getProducts.execute({ page: 1, search: 'laptop' })
```

---

## Step 927: Event-Driven Pattern

```ts
// src/lib/events.ts — simple event bus
type EventHandler<T> = (payload: T) => void | Promise<void>

class EventBus {
  private handlers = new Map<string, EventHandler<any>[]>()

  on<T>(event: string, handler: EventHandler<T>) {
    if (!this.handlers.has(event)) this.handlers.set(event, [])
    this.handlers.get(event)!.push(handler)
  }

  async emit<T>(event: string, payload: T) {
    const handlers = this.handlers.get(event) ?? []
    await Promise.all(handlers.map(h => h(payload)))
  }
}

export const eventBus = new EventBus()

// Domain events
export interface OrderPlacedEvent {
  orderId:    number
  userId:     number
  totalPrice: number
  items:      { productId: number; quantity: number }[]
}

// Register handlers at app startup
eventBus.on<OrderPlacedEvent>('order.placed', async ({ orderId, userId }) => {
  // await sendOrderConfirmationEmail(userId, orderId)
  console.log(`[Event] order.placed: order ${orderId} for user ${userId}`)
})

eventBus.on<OrderPlacedEvent>('order.placed', async ({ items }) => {
  // Update product analytics
  console.log(`[Event] updating analytics for ${items.length} products`)
})

// Emit after successful order creation
// await eventBus.emit('order.placed', { orderId, userId, totalPrice, items })
```

---

## Step 928: CQRS-lite Pattern

```ts
// Command-Query Responsibility Segregation (simplified)

// COMMANDS — write side (state changes)
interface Command<T = void> { execute(): Promise<T> }

class PlaceOrderCommand implements Command<Order> {
  constructor(private data: CreateOrderInput, private repos: Repos) {}
  async execute() {
    // validate → create → emit events
    return createOrder(this.data)
  }
}

class CancelOrderCommand implements Command<void> {
  constructor(private orderId: number, private userId: number) {}
  async execute() {
    // check ownership → update status → emit events
  }
}

// QUERIES — read side (no state changes)
class GetOrderQuery {
  constructor(private orderId: number) {}
  async execute() {
    // return read-optimized view (can use different DB/cache)
    return db.order.findUnique({
      where:   { id: this.orderId },
      include: { items: { include: { product: { select: { name: true, images: true } } } }, user: { select: { name: true, email: true } } },
    })
  }
}

class GetUserOrdersQuery {
  constructor(private userId: number, private page = 1) {}
  async execute() {
    return db.order.findMany({
      where:   { userId: this.userId },
      skip:    (this.page - 1) * 10,
      take:    10,
      orderBy: { createdAt: 'desc' },
      select:  { id: true, status: true, totalPrice: true, createdAt: true, _count: { select: { items: true } } },
    })
  }
}
```

---

## Step 929: Feature Flags

```ts
// src/lib/flags.ts — feature flags system
interface FeatureFlag {
  enabled:     boolean
  rolloutPct?: number  // 0-100: gradual rollout
  allowList?:  string[] // specific user IDs
}

const FLAGS: Record<string, FeatureFlag> = {
  'new-checkout-flow':  { enabled: true,  rolloutPct: 50 },
  'ai-recommendations': { enabled: false },
  'bulk-upload':        { enabled: true,  allowList: ['admin@example.com'] },
  'dark-mode':          { enabled: true },
}

export function isEnabled(flag: string, context?: { userId?: string; email?: string }): boolean {
  const config = FLAGS[flag]
  if (!config || !config.enabled) return false

  // Check allow list
  if (config.allowList && context?.email) {
    return config.allowList.includes(context.email)
  }

  // Gradual rollout (deterministic based on userId)
  if (config.rolloutPct !== undefined && context?.userId) {
    const hash = context.userId.split('').reduce((acc, c) => acc + c.charCodeAt(0), 0)
    return (hash % 100) < config.rolloutPct
  }

  return config.enabled
}

// Usage
// if (isEnabled('new-checkout-flow', { userId: session.user.id })) { ... }
```

---

## Step 930: Workshop — Architecture Decision Record

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Architecture Decision</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 min-h-screen p-6">
<div class="max-w-2xl mx-auto">
  <h1 class="text-2xl font-extrabold text-gray-900 mb-2">Architecture Patterns</h1>
  <p class="text-gray-500 text-sm mb-6">เลือก pattern ตามขนาดและความซับซ้อนของ project</p>

  <div class="space-y-4">
    <div class="bg-white rounded-2xl border-l-4 border-emerald-500 p-5">
      <div class="flex items-center gap-2 mb-2">
        <span class="px-2 py-0.5 bg-emerald-100 text-emerald-700 text-xs rounded-full font-bold">SMALL</span>
        <h2 class="font-bold text-gray-900">Simple MVC</h2>
      </div>
      <p class="text-sm text-gray-600 mb-3">Route handlers + Direct DB calls + Basic components</p>
      <div class="grid grid-cols-2 gap-2 text-xs">
        <div><p class="font-semibold text-gray-700 mb-1">✅ ข้อดี</p><ul class="text-gray-600 space-y-0.5"><li>• เริ่มต้นเร็ว</li><li>• Code น้อย</li><li>• ง่ายต่อการ debug</li></ul></div>
        <div><p class="font-semibold text-gray-700 mb-1">⚠️ ข้อเสีย</p><ul class="text-gray-600 space-y-0.5"><li>• Scale ยาก</li><li>• Logic กระจาย</li><li>• Test ยาก</li></ul></div>
      </div>
    </div>

    <div class="bg-white rounded-2xl border-l-4 border-indigo-500 p-5">
      <div class="flex items-center gap-2 mb-2">
        <span class="px-2 py-0.5 bg-indigo-100 text-indigo-700 text-xs rounded-full font-bold">MEDIUM</span>
        <h2 class="font-bold text-gray-900">Service Layer + Repository</h2>
      </div>
      <p class="text-sm text-gray-600 mb-3">Routes → Services → Repositories → DB</p>
      <div class="grid grid-cols-2 gap-2 text-xs">
        <div><p class="font-semibold text-gray-700 mb-1">✅ ข้อดี</p><ul class="text-gray-600 space-y-0.5"><li>• Business logic รวมศูนย์</li><li>• Mock-able สำหรับ tests</li><li>• เหมาะสำหรับ team</li></ul></div>
        <div><p class="font-semibold text-gray-700 mb-1">⚠️ ข้อเสีย</p><ul class="text-gray-600 space-y-0.5"><li>• Boilerplate มากขึ้น</li><li>• Learning curve</li></ul></div>
      </div>
    </div>

    <div class="bg-white rounded-2xl border-l-4 border-violet-500 p-5">
      <div class="flex items-center gap-2 mb-2">
        <span class="px-2 py-0.5 bg-violet-100 text-violet-700 text-xs rounded-full font-bold">LARGE</span>
        <h2 class="font-bold text-gray-900">Clean Architecture + DDD</h2>
      </div>
      <p class="text-sm text-gray-600 mb-3">Domain → Application → Infrastructure → Presentation</p>
      <div class="grid grid-cols-2 gap-2 text-xs">
        <div><p class="font-semibold text-gray-700 mb-1">✅ ข้อดี</p><ul class="text-gray-600 space-y-0.5"><li>• Highly testable</li><li>• Framework-agnostic core</li><li>• Maintainable long-term</li></ul></div>
        <div><p class="font-semibold text-gray-700 mb-1">⚠️ ข้อเสีย</p><ul class="text-gray-600 space-y-0.5"><li>• Over-engineering สำหรับ small apps</li><li>• Steep learning curve</li></ul></div>
      </div>
    </div>
  </div>

  <div class="mt-6 bg-amber-50 border border-amber-200 rounded-2xl p-4">
    <p class="text-sm font-semibold text-amber-800 mb-1">คำแนะนำ</p>
    <p class="text-sm text-amber-700">เริ่มต้นด้วย Simple MVC → เมื่อ codebase โตขึ้นค่อยย้ายไป Service Layer → ใช้ Clean Architecture เฉพาะเมื่อ domain logic ซับซ้อนมาก (E-commerce ขนาดใหญ่, Fintech, Healthcare)</p>
  </div>
</div>
</body>
</html>
```

---

## สรุป Part 93

| Step | เนื้อหา |
|------|---------|
| 921 | Clean Architecture layer structure |
| 922 | Domain Entities: Product class + business rules |
| 923 | Repository interfaces (contracts) |
| 924 | Infrastructure: PrismaProductRepository implementation |
| 925 | Use Cases: Create/Get/Delete product |
| 926 | Dependency injection container |
| 927 | Event-driven pattern: EventBus + domain events |
| 928 | CQRS-lite: Command + Query separation |
| 929 | Feature flags with rollout percentage + allow list |
| 930 | Workshop: Architecture decision guide |

**Part ถัดไป:** Part 94 — Monorepo & Micro-Frontends (Steps 931–940)
