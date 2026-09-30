# Part 57: SvelteKit + Tailwind Integration

## เป้าหมาย
- SvelteKit + Tailwind CSS setup
- Svelte reactive patterns + Tailwind classes
- Load functions, form actions, endpoints
- Steps 561–570

---

## Step 561: SvelteKit + Tailwind Setup

```bash
npm create svelte@latest my-svelte-app
cd my-svelte-app

npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

`tailwind.config.js`:
```js
/** @type {import('tailwindcss').Config} */
export default {
  content: ['./src/**/*.{html,js,svelte,ts}'],
  theme: {
    extend: {
      colors: {
        brand: {
          50:  '#fdf4ff',
          100: '#fae8ff',
          500: '#a855f7',
          600: '#9333ea',
          700: '#7e22ce',
        },
      },
    },
  },
  plugins: [],
}
```

`src/app.css`:
```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer components {
  .btn {
    @apply inline-flex items-center gap-2 px-4 py-2.5 rounded-xl text-sm font-semibold transition-colors;
  }
  .btn-primary {
    @apply btn bg-brand-600 text-white hover:bg-brand-700;
  }
  .card {
    @apply bg-white rounded-2xl border border-gray-200 shadow-sm;
  }
  .input {
    @apply w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-brand-500;
  }
}
```

`src/routes/+layout.svelte`:
```svelte
<script>
  import '../app.css'
  import Sidebar from '$lib/components/Sidebar.svelte'
  import Topbar from '$lib/components/Topbar.svelte'
</script>

<div class="flex h-screen overflow-hidden">
  <Sidebar />
  <div class="flex-1 flex flex-col overflow-hidden">
    <Topbar />
    <main class="flex-1 overflow-y-auto p-6 bg-gray-50">
      <slot />
    </main>
  </div>
</div>
```

---

## Step 562: Svelte Components + Tailwind

`src/lib/components/Sidebar.svelte`:
```svelte
<script>
  import { page } from '$app/stores'
  let collapsed = false

  const nav = [
    { href: '/', label: 'Dashboard', icon: '🏠' },
    { href: '/users', label: 'Users', icon: '👥' },
    { href: '/products', label: 'Products', icon: '📦' },
    { href: '/settings', label: 'Settings', icon: '⚙️' },
  ]
</script>

<aside class="flex-shrink-0 bg-white border-r flex flex-col transition-all duration-300 {collapsed ? 'w-14' : 'w-52'}">
  <div class="h-14 border-b flex items-center px-3 overflow-hidden">
    <button on:click={() => collapsed = !collapsed}
      class="w-8 h-8 flex items-center justify-center rounded-xl hover:bg-gray-100 transition-colors flex-shrink-0">
      ☰
    </button>
    {#if !collapsed}
      <span class="ml-2 font-extrabold text-brand-600 whitespace-nowrap text-sm">SvelteApp</span>
    {/if}
  </div>
  <nav class="p-2 space-y-0.5 flex-1">
    {#each nav as item}
      <a href={item.href}
        class="flex items-center gap-2.5 px-2 py-2 rounded-xl transition-colors text-sm
          {$page.url.pathname === item.href ? 'bg-brand-50 text-brand-700 font-semibold' : 'text-gray-600 hover:bg-gray-50'}">
        <span class="flex-shrink-0">{item.icon}</span>
        {#if !collapsed}<span class="whitespace-nowrap">{item.label}</span>{/if}
      </a>
    {/each}
  </nav>
</aside>
```

---

## Step 563: Reactive Stores + Tailwind

`src/lib/stores/toast.ts`:
```ts
import { writable } from 'svelte/store'

export interface Toast {
  id: number
  message: string
  type: 'success' | 'error' | 'info'
}

export const toasts = writable<Toast[]>([])

export function showToast(message: string, type: Toast['type'] = 'info') {
  const id = Date.now()
  toasts.update(t => [...t, { id, message, type }])
  setTimeout(() => {
    toasts.update(t => t.filter(x => x.id !== id))
  }, 3500)
}
```

`src/lib/components/ToastContainer.svelte`:
```svelte
<script>
  import { toasts } from '$lib/stores/toast'
  import { fly } from 'svelte/transition'

  const typeClass: Record<string, string> = {
    success: 'bg-green-600',
    error: 'bg-red-600',
    info: 'bg-brand-600',
  }
  const typeIcon: Record<string, string> = {
    success: '✅', error: '❌', info: 'ℹ️',
  }
</script>

<div class="fixed bottom-4 right-4 z-50 space-y-2">
  {#each $toasts as t (t.id)}
    <div transition:fly={{ x: 16, duration: 250 }}
      class="flex items-center gap-3 px-4 py-3 rounded-xl shadow-lg text-sm font-medium text-white {typeClass[t.type]}">
      {typeIcon[t.type]} {t.message}
    </div>
  {/each}
</div>
```

---

## Step 564: +page.ts Load Function

`src/routes/+page.ts`:
```ts
import type { PageLoad } from './$types'

export const load: PageLoad = async ({ fetch }) => {
  const res = await fetch('/api/stats')
  const stats = await res.json()
  return { stats }
}
```

`src/routes/+page.svelte`:
```svelte
<script lang="ts">
  import type { PageData } from './$types'
  export let data: PageData
</script>

<svelte:head><title>Dashboard</title></svelte:head>

<div class="space-y-6">
  <h1 class="text-xl font-extrabold">Dashboard</h1>

  <div class="grid grid-cols-2 xl:grid-cols-4 gap-4">
    {#each data.stats.kpis as kpi}
      <div class="card p-4 hover:shadow-md transition-shadow">
        <div class="flex justify-between mb-3">
          <p class="text-xs text-gray-500 font-semibold">{kpi.label}</p>
          <span class="text-xl">{kpi.icon}</span>
        </div>
        <p class="text-2xl font-extrabold">{kpi.value}</p>
        <p class="text-xs mt-1 {kpi.up ? 'text-green-500' : 'text-red-500'}">
          {kpi.up ? '▲' : '▼'} {kpi.change}
        </p>
      </div>
    {/each}
  </div>
</div>
```

---

## Step 565: API Endpoint (+server.ts)

`src/routes/api/stats/+server.ts`:
```ts
import { json } from '@sveltejs/kit'
import type { RequestHandler } from './$types'

export const GET: RequestHandler = async () => {
  return json({
    kpis: [
      { label: 'Revenue', value: '฿284K', change: '+12%', up: true, icon: '💰' },
      { label: 'Users', value: '1,284', change: '+8%', up: true, icon: '👥' },
      { label: 'Orders', value: '391', change: '-3%', up: false, icon: '📦' },
      { label: 'Conversion', value: '3.2%', change: '+0.5%', up: true, icon: '📈' },
    ],
  })
}
```

`src/routes/api/users/+server.ts`:
```ts
import { json } from '@sveltejs/kit'
import type { RequestHandler } from './$types'

let users = [
  { id: 1, name: 'สมชาย เทคโน', email: 'sk@tech.co', role: 'admin', active: true },
  { id: 2, name: 'วิชัย ดีงาม', email: 'wd@corp.io', role: 'editor', active: true },
]

export const GET: RequestHandler = () => json(users)

export const POST: RequestHandler = async ({ request }) => {
  const body = await request.json()
  const newUser = { id: Date.now(), ...body, active: true }
  users = [...users, newUser]
  return json(newUser, { status: 201 })
}
```

---

## Steps 566–570: Users Page + Form Actions Workshop

`src/routes/users/+page.server.ts`:
```ts
import { fail, redirect } from '@sveltejs/kit'
import type { Actions, PageServerLoad } from './$types'

let users = [
  { id: 1, name: 'สมชาย เทคโน', email: 'sk@tech.co', role: 'admin', active: true },
  { id: 2, name: 'วิชัย ดีงาม', email: 'wd@corp.io', role: 'editor', active: true },
]

export const load: PageServerLoad = async () => {
  return { users }
}

export const actions: Actions = {
  add: async ({ request }) => {
    const data = await request.formData()
    const name = data.get('name') as string
    const email = data.get('email') as string
    const role = data.get('role') as string

    if (!name?.trim()) {
      return fail(400, { error: 'กรุณากรอกชื่อ' })
    }

    users = [...users, { id: Date.now(), name, email, role, active: true }]
    return { success: true }
  },

  delete: async ({ request }) => {
    const data = await request.formData()
    const id = Number(data.get('id'))
    users = users.filter(u => u.id !== id)
    return { success: true }
  },
}
```

`src/routes/users/+page.svelte`:
```svelte
<script lang="ts">
  import { enhance } from '$app/forms'
  import { showToast } from '$lib/stores/toast'
  import type { PageData, ActionData } from './$types'

  export let data: PageData
  export let form: ActionData

  let search = ''
  let showModal = false

  $: filteredUsers = search
    ? data.users.filter(u => u.name.toLowerCase().includes(search.toLowerCase()))
    : data.users

  $: if (form?.success) {
    showModal = false
    showToast('บันทึกสำเร็จ', 'success')
  }
</script>

<svelte:head><title>Users</title></svelte:head>

<div class="space-y-4">
  <div class="flex items-center justify-between">
    <h1 class="text-xl font-extrabold">Users ({filteredUsers.length})</h1>
    <button on:click={() => showModal = true} class="btn-primary">+ เพิ่ม</button>
  </div>

  <div class="card overflow-hidden">
    <div class="p-4 border-b">
      <input bind:value={search} placeholder="ค้นหา..." class="input w-64">
    </div>

    <table class="w-full text-sm">
      <thead class="bg-gray-50">
        <tr>
          <th class="px-5 py-3 text-left text-xs font-semibold text-gray-500">ชื่อ</th>
          <th class="px-5 py-3 text-left text-xs font-semibold text-gray-500 hidden md:table-cell">อีเมล</th>
          <th class="px-5 py-3 text-left text-xs font-semibold text-gray-500">บทบาท</th>
          <th class="px-5 py-3"></th>
        </tr>
      </thead>
      <tbody class="divide-y">
        {#each filteredUsers as user (user.id)}
          <tr class="hover:bg-gray-50 transition-colors">
            <td class="px-5 py-3">
              <div class="flex items-center gap-2">
                <div class="w-7 h-7 rounded-lg bg-brand-100 flex items-center justify-center text-xs font-bold text-brand-700">
                  {user.name[0]}
                </div>
                <span class="font-medium">{user.name}</span>
              </div>
            </td>
            <td class="px-5 py-3 text-gray-500 hidden md:table-cell">{user.email}</td>
            <td class="px-5 py-3">
              <span class="text-xs font-semibold px-2.5 py-1 rounded-full
                {user.role === 'admin' ? 'bg-red-100 text-red-700' : user.role === 'editor' ? 'bg-brand-100 text-brand-700' : 'bg-gray-100 text-gray-600'}">
                {user.role}
              </span>
            </td>
            <td class="px-5 py-3">
              <form method="POST" action="?/delete" use:enhance>
                <input type="hidden" name="id" value={user.id}>
                <button type="submit" class="text-xs text-red-400 hover:text-red-600 transition-colors">ลบ</button>
              </form>
            </td>
          </tr>
        {/each}
      </tbody>
    </table>
  </div>

  <!-- Modal -->
  {#if showModal}
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <div class="fixed inset-0 bg-black/50 z-50 flex items-center justify-center p-4" on:click|self={() => showModal = false}>
      <div class="bg-white rounded-2xl shadow-2xl w-full max-w-sm p-6">
        <h3 class="font-extrabold mb-4">เพิ่มผู้ใช้</h3>
        <form method="POST" action="?/add" use:enhance class="space-y-3">
          {#if form?.error}
            <p class="text-xs text-red-600 bg-red-50 px-3 py-2 rounded-xl">{form.error}</p>
          {/if}
          <input name="name" placeholder="ชื่อ" required class="input">
          <input name="email" type="email" placeholder="อีเมล" class="input">
          <select name="role" class="input">
            <option value="user">User</option>
            <option value="editor">Editor</option>
            <option value="admin">Admin</option>
          </select>
          <div class="flex gap-3 pt-2">
            <button type="button" on:click={() => showModal = false}
              class="flex-1 border border-gray-300 py-2.5 rounded-xl text-sm font-semibold hover:bg-gray-50 transition-colors">
              ยกเลิก
            </button>
            <button type="submit" class="flex-1 btn-primary justify-center">บันทึก</button>
          </div>
        </form>
      </div>
    </div>
  {/if}
</div>
```

---

## สรุป Part 57

| Step | เนื้อหา |
|------|---------|
| 561 | SvelteKit + Tailwind setup, tailwind.config, app.css, +layout.svelte |
| 562 | Sidebar.svelte — $page store, conditional classes, {#if}/{#each} |
| 563 | Svelte writable stores: useToast + ToastContainer with fly transition |
| 564 | +page.ts load function + PageData typing |
| 565 | API endpoints: +server.ts GET/POST |
| 566 | +page.server.ts: load + form actions (add/delete) |
| 567 | use:enhance for progressive enhancement |
| 568 | Reactive $: statements + bind:value |
| 569 | Modal with on:click|self |
| 570 | Workshop: Full SvelteKit app with form actions, stores, API routes |

**Part ถัดไป:** Part 58 — Headless UI + Tailwind (Steps 571–580)
