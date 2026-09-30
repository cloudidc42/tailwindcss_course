# Part 88: State Management Patterns

## เป้าหมาย
- Zustand — simple global state
- Jotai — atomic state
- React Query (TanStack Query) — server state
- Context + Reducer patterns
- Steps 871–880

---

## Step 871: Zustand Setup

```bash
npm install zustand
```

```ts
// src/stores/cart.ts
import { create } from 'zustand'
import { persist } from 'zustand/middleware'

interface Product { id: number; name: string; price: number; image: string }
interface CartItem extends Product { qty: number }

interface CartStore {
  items: CartItem[]
  addItem:    (product: Product) => void
  removeItem: (id: number) => void
  updateQty:  (id: number, qty: number) => void
  clearCart:  () => void
  total:      () => number
  itemCount:  () => number
}

export const useCart = create<CartStore>()(
  persist(
    (set, get) => ({
      items: [],

      addItem: (product) => set(state => {
        const existing = state.items.find(i => i.id === product.id)
        if (existing) {
          return { items: state.items.map(i => i.id === product.id ? { ...i, qty: i.qty + 1 } : i) }
        }
        return { items: [...state.items, { ...product, qty: 1 }] }
      }),

      removeItem: (id) => set(state => ({ items: state.items.filter(i => i.id !== id) })),

      updateQty: (id, qty) => set(state => ({
        items: qty <= 0
          ? state.items.filter(i => i.id !== id)
          : state.items.map(i => i.id === id ? { ...i, qty } : i),
      })),

      clearCart: () => set({ items: [] }),

      total: () => get().items.reduce((sum, i) => sum + i.price * i.qty, 0),
      itemCount: () => get().items.reduce((sum, i) => sum + i.qty, 0),
    }),
    { name: 'cart-storage' } // persist to localStorage
  )
)
```

---

## Step 872: Using Zustand Store

```tsx
// src/components/CartButton.tsx
import { useCart } from '@/stores/cart'

export function CartButton() {
  const itemCount = useCart(state => state.itemCount())

  return (
    <button className="relative p-2 rounded-xl hover:bg-gray-100 transition-colors">
      🛒
      {itemCount > 0 && (
        <span className="absolute -top-1 -right-1 w-5 h-5 bg-red-500 text-white text-xs rounded-full flex items-center justify-center font-bold">
          {itemCount > 99 ? '99+' : itemCount}
        </span>
      )}
    </button>
  )
}

// src/components/ProductCard.tsx
import { useCart } from '@/stores/cart'

export function ProductCard({ product }: { product: Product }) {
  const addItem = useCart(state => state.addItem)
  const items   = useCart(state => state.items)
  const inCart  = items.some(i => i.id === product.id)

  return (
    <div className="bg-white rounded-2xl border p-4">
      <div className="aspect-square bg-gray-100 rounded-xl mb-3" />
      <h3 className="font-semibold text-gray-900 text-sm">{product.name}</h3>
      <p className="text-indigo-600 font-bold mt-1">${product.price}</p>
      <button
        onClick={() => addItem(product)}
        className={`mt-3 w-full py-2 rounded-xl text-sm font-semibold transition-colors ${
          inCart
            ? 'bg-emerald-50 text-emerald-700 border border-emerald-300'
            : 'bg-indigo-600 text-white hover:bg-indigo-700'
        }`}
      >
        {inCart ? '✓ In cart' : 'Add to cart'}
      </button>
    </div>
  )
}
```

---

## Step 873: Jotai Atoms

```bash
npm install jotai
```

```ts
// src/atoms/ui.ts
import { atom } from 'jotai'
import { atomWithStorage } from 'jotai/utils'

// Simple atoms
export const sidebarOpenAtom    = atom(false)
export const searchQueryAtom    = atom('')
export const selectedTabAtom    = atom<'all' | 'active' | 'archived'>('all')

// Persist to localStorage
export const themeAtom = atomWithStorage<'light' | 'dark' | 'system'>('theme', 'system')

// Derived atom (read-only computed)
export const filteredItemsAtom = atom(get => {
  const query  = get(searchQueryAtom).toLowerCase()
  const tab    = get(selectedTabAtom)
  // items would come from another atom
  return [] // filtered result
})

// Write-only atom (action)
export const toggleSidebarAtom = atom(null, (get, set) => {
  set(sidebarOpenAtom, !get(sidebarOpenAtom))
})
```

```tsx
// Using atoms
import { useAtom, useAtomValue, useSetAtom } from 'jotai'
import { sidebarOpenAtom, themeAtom, toggleSidebarAtom } from '@/atoms/ui'

function Sidebar() {
  const [open] = useAtom(sidebarOpenAtom)
  return <aside className={open ? 'block' : 'hidden'}>...</aside>
}

function ToggleButton() {
  const toggle = useSetAtom(toggleSidebarAtom)
  return <button onClick={toggle}>☰</button>
}

function ThemeSelector() {
  const [theme, setTheme] = useAtom(themeAtom)
  return (
    <select value={theme} onChange={e => setTheme(e.target.value as any)}>
      <option value="light">Light</option>
      <option value="dark">Dark</option>
      <option value="system">System</option>
    </select>
  )
}
```

---

## Step 874: TanStack Query Setup

```bash
npm install @tanstack/react-query @tanstack/react-query-devtools
```

```tsx
// src/app/providers.tsx
'use client'
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'
import { ReactQueryDevtools } from '@tanstack/react-query-devtools'
import { useState } from 'react'

export function Providers({ children }: { children: React.ReactNode }) {
  const [queryClient] = useState(() => new QueryClient({
    defaultOptions: {
      queries: {
        staleTime: 5 * 60 * 1000,  // 5 minutes
        retry: 2,
        refetchOnWindowFocus: false,
      },
    },
  }))

  return (
    <QueryClientProvider client={queryClient}>
      {children}
      <ReactQueryDevtools initialIsOpen={false} />
    </QueryClientProvider>
  )
}
```

---

## Step 875: useQuery + useMutation

```tsx
// src/hooks/useProducts.ts
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query'

const api = {
  getProducts: (page = 1) =>
    fetch(`/api/products?page=${page}`).then(r => r.json()),
  createProduct: (data: Partial<Product>) =>
    fetch('/api/products', { method: 'POST', body: JSON.stringify(data), headers: {'Content-Type':'application/json'} }).then(r => r.json()),
  deleteProduct: (id: number) =>
    fetch(`/api/products/${id}`, { method: 'DELETE' }).then(r => r.json()),
}

export function useProducts(page: number) {
  return useQuery({
    queryKey: ['products', page],
    queryFn: () => api.getProducts(page),
    placeholderData: (prev) => prev, // keep previous data while fetching
  })
}

export function useCreateProduct() {
  const qc = useQueryClient()
  return useMutation({
    mutationFn: api.createProduct,
    onSuccess: () => {
      qc.invalidateQueries({ queryKey: ['products'] })
    },
    onError: (error) => console.error('Failed to create:', error),
  })
}

export function useDeleteProduct() {
  const qc = useQueryClient()
  return useMutation({
    mutationFn: api.deleteProduct,
    // Optimistic update
    onMutate: async (id) => {
      await qc.cancelQueries({ queryKey: ['products'] })
      const previous = qc.getQueryData(['products'])
      qc.setQueryData(['products'], (old: any) => ({
        ...old,
        data: old.data.filter((p: Product) => p.id !== id),
      }))
      return { previous }
    },
    onError: (_err, _id, ctx) => {
      if (ctx?.previous) qc.setQueryData(['products'], ctx.previous)
    },
    onSettled: () => qc.invalidateQueries({ queryKey: ['products'] }),
  })
}
```

---

## Step 876: Products Page with TanStack Query

```tsx
// src/app/products/page.tsx (Client Component)
'use client'
import { useState } from 'react'
import { useProducts, useDeleteProduct } from '@/hooks/useProducts'

export default function ProductsPage() {
  const [page, setPage] = useState(1)
  const { data, isLoading, isError, isFetching } = useProducts(page)
  const deleteMutation = useDeleteProduct()

  if (isLoading) return (
    <div className="grid grid-cols-3 gap-4 p-6 animate-pulse">
      {[...Array(6)].map((_,i) => <div key={i} className="h-48 bg-gray-200 rounded-2xl"></div>)}
    </div>
  )

  if (isError) return (
    <div className="flex flex-col items-center justify-center h-64 text-center p-6">
      <p className="text-red-600 font-semibold mb-3">Failed to load products</p>
      <button onClick={() => window.location.reload()} className="px-4 py-2 bg-red-600 text-white rounded-xl text-sm">Retry</button>
    </div>
  )

  return (
    <div className="p-6">
      {isFetching && <div className="text-xs text-indigo-600 text-center mb-2">Updating...</div>}
      <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
        {data?.data?.map((product: Product) => (
          <div key={product.id} className="bg-white rounded-2xl border p-4">
            <div className="aspect-square bg-gray-100 rounded-xl mb-3"></div>
            <h3 className="font-semibold text-gray-900 text-sm">{product.name}</h3>
            <p className="text-indigo-600 font-bold mt-1">${product.price}</p>
            <button
              onClick={() => deleteMutation.mutate(product.id)}
              disabled={deleteMutation.isPending}
              className="mt-3 w-full py-1.5 text-sm text-red-600 border border-red-200 rounded-xl hover:bg-red-50 disabled:opacity-50 transition-colors"
            >
              {deleteMutation.isPending ? 'Deleting...' : 'Delete'}
            </button>
          </div>
        ))}
      </div>
      {/* Pagination */}
      <div className="flex justify-center gap-2 mt-6">
        <button onClick={() => setPage(p => Math.max(1, p-1))} disabled={page === 1} className="px-4 py-2 border rounded-xl text-sm disabled:opacity-50">← Prev</button>
        <span className="px-4 py-2 text-sm font-medium">Page {page}</span>
        <button onClick={() => setPage(p => p+1)} disabled={!data?.hasNextPage} className="px-4 py-2 border rounded-xl text-sm disabled:opacity-50">Next →</button>
      </div>
    </div>
  )
}
```

---

## Step 877: Context + Reducer Pattern

```tsx
// src/contexts/notifications.tsx
import { createContext, useContext, useReducer, useCallback } from 'react'

interface Notification { id: string; message: string; type: 'success'|'error'|'info'; }
interface State { notifications: Notification[] }
type Action = { type: 'ADD'; payload: Notification } | { type: 'REMOVE'; id: string } | { type: 'CLEAR' }

function reducer(state: State, action: Action): State {
  switch (action.type) {
    case 'ADD':    return { notifications: [...state.notifications, action.payload] }
    case 'REMOVE': return { notifications: state.notifications.filter(n => n.id !== action.id) }
    case 'CLEAR':  return { notifications: [] }
    default:       return state
  }
}

const NotificationCtx = createContext<{ state: State; dispatch: React.Dispatch<Action>; toast: (msg: string, type?: Notification['type']) => void } | null>(null)

export function NotificationProvider({ children }: { children: React.ReactNode }) {
  const [state, dispatch] = useReducer(reducer, { notifications: [] })

  const toast = useCallback((message: string, type: Notification['type'] = 'info') => {
    const id = Math.random().toString(36).slice(2)
    dispatch({ type: 'ADD', payload: { id, message, type } })
    setTimeout(() => dispatch({ type: 'REMOVE', id }), 4000)
  }, [])

  return <NotificationCtx.Provider value={{ state, dispatch, toast }}>{children}</NotificationCtx.Provider>
}

export const useNotifications = () => {
  const ctx = useContext(NotificationCtx)
  if (!ctx) throw new Error('useNotifications must be inside NotificationProvider')
  return ctx
}
```

---

## Step 878: URL State with nuqs

```bash
npm install nuqs
```

```tsx
// URL state — shareable, browser history, SSR-compatible
import { useQueryState } from 'nuqs'

export function ProductFilters() {
  const [search, setSearch]   = useQueryState('q', { defaultValue: '' })
  const [category, setCategory] = useQueryState('cat', { defaultValue: 'all' })
  const [sort, setSort]       = useQueryState('sort', { defaultValue: 'newest' })
  const [page, setPage]       = useQueryState('page', { defaultValue: 1, parse: Number, serialize: String })

  return (
    <div className="flex gap-3 flex-wrap">
      <input
        value={search}
        onChange={e => { setSearch(e.target.value || null); setPage(1); }}
        placeholder="Search..."
        className="px-3 py-2 border rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500"
      />
      <select
        value={category}
        onChange={e => setCategory(e.target.value)}
        className="px-3 py-2 border rounded-xl text-sm focus:outline-none bg-white"
      >
        <option value="all">All categories</option>
        <option value="electronics">Electronics</option>
        <option value="clothing">Clothing</option>
      </select>
    </div>
  )
}
// URL: /products?q=phone&cat=electronics&sort=newest&page=2
```

---

## Step 879: State Management Decision Tree

```
ตัดสินใจเลือก state management:

LOCAL UI STATE
  useState / useReducer
  ✅ ข้อมูล component เดียว: toggle, form values, modal open/closed
  ✅ ไม่ต้องแชร์ข้ามหน้า

SERVER STATE
  TanStack Query / SWR
  ✅ ข้อมูลจาก API: products, users, orders
  ✅ ต้องการ cache, loading, error states
  ✅ refetch, pagination, mutations

GLOBAL UI STATE
  Zustand / Jotai / Context
  ✅ Theme, sidebar open/closed, cart count
  ✅ ข้ามหลาย components แต่ไม่ใช่ server data
  Zustand: ง่าย, devtools, persist
  Jotai: atomic, เหมาะกับ fine-grained reactivity
  Context: simple cases, ไม่ต้องการ library

URL STATE
  nuqs / searchParams
  ✅ Filters, pagination, tabs — ที่ควร bookmark/share ได้
  ✅ Back button ทำงานถูกต้อง
```

---

## Step 880: Workshop — Full State Example

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>State Workshop</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    @keyframes slide-up { from{transform:translateY(12px);opacity:0} to{transform:translateY(0);opacity:1} }
    .toast-enter { animation: slide-up 0.3s ease; }
  </style>
</head>
<body class="bg-gray-100 min-h-screen p-6">
<div class="max-w-xl mx-auto">
  <h1 class="text-xl font-extrabold text-gray-900 mb-6">State Management Demo</h1>

  <!-- Cart state -->
  <div class="bg-white rounded-2xl border p-5 mb-4">
    <div class="flex items-center justify-between mb-4">
      <h2 class="font-bold text-gray-900">Shopping Cart</h2>
      <span class="px-2.5 py-0.5 bg-indigo-100 text-indigo-700 text-xs rounded-full font-bold" id="cart-count">0 items</span>
    </div>
    <div class="grid grid-cols-3 gap-2 mb-4">
      <button onclick="addToCart('Product A', 49)" class="p-3 bg-gray-50 border rounded-xl text-center hover:bg-indigo-50 hover:border-indigo-300 transition-all">
        <p class="text-sm font-semibold">Product A</p><p class="text-xs text-indigo-600">$49</p>
      </button>
      <button onclick="addToCart('Product B', 89)" class="p-3 bg-gray-50 border rounded-xl text-center hover:bg-indigo-50 hover:border-indigo-300 transition-all">
        <p class="text-sm font-semibold">Product B</p><p class="text-xs text-indigo-600">$89</p>
      </button>
      <button onclick="addToCart('Product C', 29)" class="p-3 bg-gray-50 border rounded-xl text-center hover:bg-indigo-50 hover:border-indigo-300 transition-all">
        <p class="text-sm font-semibold">Product C</p><p class="text-xs text-indigo-600">$29</p>
      </button>
    </div>
    <div id="cart-items" class="space-y-2 mb-3"></div>
    <div class="flex justify-between items-center pt-3 border-t">
      <p class="font-bold text-gray-900">Total: <span id="cart-total">$0</span></p>
      <button onclick="clearCart()" class="text-xs text-red-600 hover:text-red-700">Clear cart</button>
    </div>
  </div>

  <!-- Toast stack -->
  <div id="toasts" class="fixed bottom-4 right-4 flex flex-col gap-2 z-50"></div>
</div>
<script>
  let cart = JSON.parse(localStorage.getItem('demo-cart') || '[]');

  function save() { localStorage.setItem('demo-cart', JSON.stringify(cart)); }

  function addToCart(name, price) {
    const existing = cart.find(i => i.name === name);
    if (existing) existing.qty++;
    else cart.push({ name, price, qty: 1 });
    save(); renderCart();
    toast(`Added ${name} to cart`, 'success');
  }

  function removeItem(name) {
    cart = cart.filter(i => i.name !== name);
    save(); renderCart();
  }

  function clearCart() { cart = []; save(); renderCart(); toast('Cart cleared', 'info'); }

  function renderCart() {
    const count = cart.reduce((s,i) => s+i.qty, 0);
    const total = cart.reduce((s,i) => s+i.price*i.qty, 0);
    document.getElementById('cart-count').textContent = count + ' item' + (count!==1?'s':'');
    document.getElementById('cart-total').textContent = '$' + total;
    document.getElementById('cart-items').innerHTML = cart.map(i => `
      <div class="flex items-center justify-between text-sm">
        <span class="text-gray-700">${i.name} × ${i.qty}</span>
        <div class="flex items-center gap-2">
          <span class="font-semibold text-gray-900">$${i.price * i.qty}</span>
          <button onclick="removeItem('${i.name}')" class="text-gray-300 hover:text-red-500">✕</button>
        </div>
      </div>`).join('') || '<p class="text-gray-400 text-sm text-center py-2">Cart is empty</p>';
  }

  function toast(msg, type='info') {
    const colors = { success:'bg-emerald-600', error:'bg-red-600', info:'bg-indigo-600' };
    const el = document.createElement('div');
    el.className = `toast-enter ${colors[type]} text-white px-4 py-2.5 rounded-xl text-sm font-medium shadow-lg`;
    el.textContent = msg;
    document.getElementById('toasts').prepend(el);
    setTimeout(() => el.remove(), 3000);
  }

  renderCart();
</script>
</body>
</html>
```

---

## สรุป Part 88

| Step | เนื้อหา |
|------|---------|
| 871 | Zustand store — cart with persist middleware |
| 872 | ใช้ Zustand ใน CartButton + ProductCard |
| 873 | Jotai atoms — simple, derived, write-only |
| 874 | TanStack Query setup + QueryClientProvider |
| 875 | useQuery + useMutation + optimistic update |
| 876 | Products page ด้วย React Query |
| 877 | Context + useReducer pattern สำหรับ notifications |
| 878 | URL state ด้วย nuqs |
| 879 | Decision tree: เลือก state management ถูกประเภท |
| 880 | Workshop: Cart + toasts state demo |

**Part ถัดไป:** Part 89 — API Integration Patterns (Steps 881–890)
