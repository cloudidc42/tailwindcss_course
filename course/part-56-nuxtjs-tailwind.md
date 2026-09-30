# Part 56: Nuxt.js + Tailwind Integration

## เป้าหมาย
- Nuxt 3 + Tailwind CSS (@nuxtjs/tailwindcss module)
- Auto-imports, composables, server routes
- SSR vs SPA mode
- Steps 551–560

---

## Step 551: Nuxt 3 + Tailwind Setup

```bash
npx nuxi@latest init my-nuxt-app
cd my-nuxt-app

# ติดตั้ง Tailwind module
npm install -D @nuxtjs/tailwindcss
```

`nuxt.config.ts`:
```ts
export default defineNuxtConfig({
  devtools: { enabled: true },
  modules: ['@nuxtjs/tailwindcss'],
  tailwindcss: {
    cssPath: '~/assets/css/tailwind.css',
    configPath: 'tailwind.config',
  },
})
```

`tailwind.config.js`:
```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    './components/**/*.{vue,js,ts}',
    './layouts/**/*.vue',
    './pages/**/*.vue',
    './composables/**/*.{js,ts}',
    './plugins/**/*.{js,ts}',
    './app.vue',
  ],
  theme: {
    extend: {
      colors: {
        primary: {
          50:  '#f0f9ff',
          100: '#e0f2fe',
          500: '#0ea5e9',
          600: '#0284c7',
          700: '#0369a1',
        },
      },
    },
  },
  plugins: [],
}
```

`assets/css/tailwind.css`:
```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer components {
  .btn {
    @apply inline-flex items-center gap-2 px-4 py-2.5 rounded-xl text-sm font-semibold transition-colors;
  }
  .btn-primary {
    @apply btn bg-primary-600 text-white hover:bg-primary-700;
  }
  .card {
    @apply bg-white rounded-2xl border border-gray-200 shadow-sm;
  }
}
```

---

## Step 552: Pages + Layouts

`layouts/default.vue`:
```vue
<template>
  <div class="flex h-screen overflow-hidden">
    <AppSidebar />
    <div class="flex-1 flex flex-col overflow-hidden">
      <AppTopbar />
      <main class="flex-1 overflow-y-auto p-6 bg-gray-50">
        <slot />
      </main>
    </div>
  </div>
</template>
```

`pages/index.vue`:
```vue
<script setup lang="ts">
definePageMeta({ layout: 'default' })
useHead({ title: 'Dashboard' })

const { data: stats } = await useFetch('/api/stats')
</script>

<template>
  <div class="space-y-6">
    <h1 class="text-xl font-extrabold">Dashboard</h1>

    <div class="grid grid-cols-2 xl:grid-cols-4 gap-4">
      <KpiCard v-for="kpi in stats?.kpis" :key="kpi.label" v-bind="kpi" />
    </div>
  </div>
</template>
```

---

## Step 553: Auto-imported Components

Nuxt auto-imports components ใน `components/` โดยอัตโนมัติ:

`components/KpiCard.vue`:
```vue
<script setup lang="ts">
defineProps<{
  label: string
  value: string
  change: string
  up: boolean
  icon: string
}>()
</script>

<template>
  <div class="card p-4 hover:shadow-md transition-shadow">
    <div class="flex justify-between items-start mb-3">
      <p class="text-xs text-gray-500 font-semibold">{{ label }}</p>
      <span class="text-xl">{{ icon }}</span>
    </div>
    <p class="text-2xl font-extrabold text-gray-900">{{ value }}</p>
    <p :class="['text-xs mt-1', up ? 'text-green-500' : 'text-red-500']">
      {{ up ? '▲' : '▼' }} {{ change }}
    </p>
  </div>
</template>
```

`components/AppSidebar.vue`:
```vue
<script setup lang="ts">
const route = useRoute()
const collapsed = ref(false)

const navItems = [
  { href: '/', label: 'Dashboard', icon: '🏠' },
  { href: '/users', label: 'Users', icon: '👥' },
  { href: '/products', label: 'Products', icon: '📦' },
  { href: '/settings', label: 'Settings', icon: '⚙️' },
]
</script>

<template>
  <aside :class="['flex-shrink-0 bg-white border-r flex flex-col transition-all duration-300', collapsed ? 'w-14' : 'w-52']">
    <div class="h-14 border-b flex items-center px-3 overflow-hidden">
      <button @click="collapsed = !collapsed" class="w-8 h-8 flex items-center justify-center rounded-xl hover:bg-gray-100 transition-colors flex-shrink-0">
        <svg class="w-5 h-5 text-gray-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
        </svg>
      </button>
      <span v-if="!collapsed" class="ml-2 font-extrabold text-primary-600 whitespace-nowrap text-sm">NuxtApp</span>
    </div>
    <nav class="p-2 space-y-0.5 flex-1">
      <NuxtLink v-for="item in navItems" :key="item.href" :to="item.href"
        :class="['flex items-center gap-2.5 px-2 py-2 rounded-xl transition-colors text-sm',
          route.path === item.href ? 'bg-primary-50 text-primary-700 font-semibold' : 'text-gray-600 hover:bg-gray-50']">
        <span class="flex-shrink-0">{{ item.icon }}</span>
        <span v-if="!collapsed" class="whitespace-nowrap">{{ item.label }}</span>
      </NuxtLink>
    </nav>
  </aside>
</template>
```

---

## Step 554: Composables (Auto-imported)

`composables/useUsers.ts`:
```ts
export interface User {
  id: number
  name: string
  email: string
  role: 'admin' | 'editor' | 'user'
  active: boolean
}

export function useUsers() {
  const users = ref<User[]>([])
  const search = ref('')
  const loading = ref(false)

  const filteredUsers = computed(() =>
    search.value
      ? users.value.filter(u => u.name.toLowerCase().includes(search.value.toLowerCase()))
      : users.value
  )

  async function fetchUsers() {
    loading.value = true
    const data = await $fetch<User[]>('/api/users')
    users.value = data
    loading.value = false
  }

  function removeUser(id: number) {
    users.value = users.value.filter(u => u.id !== id)
  }

  return { users, search, loading, filteredUsers, fetchUsers, removeUser }
}
```

`composables/useToast.ts`:
```ts
interface Toast {
  id: number
  message: string
  type: 'success' | 'error' | 'info'
}

const toasts = ref<Toast[]>([])

export function useToast() {
  function show(message: string, type: Toast['type'] = 'info') {
    const id = Date.now()
    toasts.value.push({ id, message, type })
    setTimeout(() => {
      toasts.value = toasts.value.filter(t => t.id !== id)
    }, 3500)
  }

  return { toasts, show }
}
```

---

## Step 555: Server Routes (API)

`server/api/stats.get.ts`:
```ts
export default defineEventHandler(async () => {
  return {
    kpis: [
      { label: 'Revenue', value: '฿284K', change: '+12%', up: true, icon: '💰' },
      { label: 'Users', value: '1,284', change: '+8%', up: true, icon: '👥' },
      { label: 'Orders', value: '391', change: '-3%', up: false, icon: '📦' },
      { label: 'Conversion', value: '3.2%', change: '+0.5%', up: true, icon: '📈' },
    ],
  }
})
```

`server/api/users/index.get.ts`:
```ts
const users = [
  { id: 1, name: 'สมชาย เทคโน', email: 'sk@tech.co', role: 'admin', active: true },
  { id: 2, name: 'วิชัย ดีงาม', email: 'wd@corp.io', role: 'editor', active: true },
]

export default defineEventHandler(() => users)
```

`server/api/users/index.post.ts`:
```ts
export default defineEventHandler(async event => {
  const body = await readBody(event)
  const newUser = { id: Date.now(), ...body, active: true }
  // Save to DB
  return newUser
})
```

`server/api/users/[id].delete.ts`:
```ts
export default defineEventHandler(event => {
  const id = getRouterParam(event, 'id')
  // Delete from DB
  return { success: true, id }
})
```

---

## Step 556: Users Page

`pages/users.vue`:
```vue
<script setup lang="ts">
definePageMeta({ layout: 'default' })
useHead({ title: 'Users' })

const { users, search, loading, filteredUsers, fetchUsers, removeUser } = useUsers()
const { show: showToast } = useToast()

await fetchUsers()

const showModal = ref(false)
const form = reactive({ name: '', email: '', role: 'user' })

async function addUser() {
  if (!form.name.trim()) return
  await $fetch('/api/users', { method: 'POST', body: form })
  await fetchUsers()
  showModal.value = false
  showToast('เพิ่มผู้ใช้สำเร็จ', 'success')
}

async function deleteUser(id: number) {
  await $fetch(`/api/users/${id}`, { method: 'DELETE' })
  removeUser(id)
  showToast('ลบผู้ใช้แล้ว', 'error')
}
</script>

<template>
  <div class="space-y-4">
    <div class="flex items-center justify-between">
      <h1 class="text-xl font-extrabold">Users ({{ filteredUsers.length }})</h1>
      <button @click="showModal = true" class="btn-primary">+ เพิ่ม</button>
    </div>

    <div class="card overflow-hidden">
      <div class="p-4 border-b">
        <input v-model="search" placeholder="ค้นหา..." class="border border-gray-300 rounded-xl px-4 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-primary-500 w-64">
      </div>

      <div v-if="loading" class="p-8 text-center text-gray-400 text-sm animate-pulse">กำลังโหลด...</div>

      <table v-else class="w-full text-sm">
        <thead class="bg-gray-50">
          <tr>
            <th class="px-5 py-3 text-left text-xs font-semibold text-gray-500">ชื่อ</th>
            <th class="px-5 py-3 text-left text-xs font-semibold text-gray-500 hidden md:table-cell">อีเมล</th>
            <th class="px-5 py-3 text-left text-xs font-semibold text-gray-500">บทบาท</th>
            <th class="px-5 py-3"></th>
          </tr>
        </thead>
        <tbody class="divide-y">
          <tr v-for="user in filteredUsers" :key="user.id" class="hover:bg-gray-50 transition-colors">
            <td class="px-5 py-3">
              <div class="flex items-center gap-2">
                <div class="w-7 h-7 rounded-lg bg-primary-100 flex items-center justify-center text-xs font-bold text-primary-700">
                  {{ user.name[0] }}
                </div>
                <span class="font-medium">{{ user.name }}</span>
              </div>
            </td>
            <td class="px-5 py-3 text-gray-500 hidden md:table-cell">{{ user.email }}</td>
            <td class="px-5 py-3">
              <span :class="['text-xs font-semibold px-2.5 py-1 rounded-full',
                user.role === 'admin' ? 'bg-red-100 text-red-700' :
                user.role === 'editor' ? 'bg-primary-100 text-primary-700' :
                'bg-gray-100 text-gray-600']">
                {{ user.role }}
              </span>
            </td>
            <td class="px-5 py-3">
              <button @click="deleteUser(user.id)" class="text-xs text-red-400 hover:text-red-600 transition-colors">ลบ</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <!-- Modal -->
    <Teleport to="body">
      <div v-if="showModal" class="fixed inset-0 bg-black/50 z-50 flex items-center justify-center p-4" @click.self="showModal = false">
        <div class="bg-white rounded-2xl shadow-2xl w-full max-w-sm p-6">
          <h3 class="font-extrabold mb-4">เพิ่มผู้ใช้</h3>
          <div class="space-y-3">
            <input v-model="form.name" placeholder="ชื่อ" class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-primary-500">
            <input v-model="form.email" type="email" placeholder="อีเมล" class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-primary-500">
            <select v-model="form.role" class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-primary-500">
              <option value="user">User</option>
              <option value="editor">Editor</option>
              <option value="admin">Admin</option>
            </select>
          </div>
          <div class="flex gap-3 mt-5">
            <button @click="showModal = false" class="flex-1 border border-gray-300 py-2.5 rounded-xl text-sm font-semibold hover:bg-gray-50 transition-colors">ยกเลิก</button>
            <button @click="addUser" class="flex-1 btn-primary justify-center">บันทึก</button>
          </div>
        </div>
      </div>
    </Teleport>
  </div>
</template>
```

---

## Steps 557–560: Toast + Dark Mode + Workshop

`components/ToastContainer.vue`:
```vue
<script setup>
const { toasts } = useToast()
</script>

<template>
  <div class="fixed bottom-4 right-4 z-50 space-y-2">
    <TransitionGroup
      enter-active-class="transition duration-300 ease-out"
      enter-from-class="opacity-0 translate-x-4"
      enter-to-class="opacity-100 translate-x-0"
      leave-active-class="transition duration-200 ease-in"
      leave-from-class="opacity-100 translate-x-0"
      leave-to-class="opacity-0 translate-x-4">
      <div v-for="t in toasts" :key="t.id"
        :class="['flex items-center gap-3 px-4 py-3 rounded-xl shadow-lg text-sm font-medium text-white',
          t.type === 'success' ? 'bg-green-600' : t.type === 'error' ? 'bg-red-600' : 'bg-primary-600']">
        {{ t.type === 'success' ? '✅' : t.type === 'error' ? '❌' : 'ℹ️' }}
        {{ t.message }}
      </div>
    </TransitionGroup>
  </div>
</template>
```

Dark mode (`nuxt.config.ts`):
```ts
export default defineNuxtConfig({
  modules: ['@nuxtjs/tailwindcss', '@nuxtjs/color-mode'],
  colorMode: {
    classSuffix: '',   // ใช้ class="dark" ไม่ใช่ class="dark-mode"
  },
})
```

Dark toggle component:
```vue
<script setup>
const colorMode = useColorMode()
</script>

<template>
  <button @click="colorMode.preference = colorMode.value === 'dark' ? 'light' : 'dark'"
    class="w-9 h-9 flex items-center justify-center rounded-xl hover:bg-gray-100 dark:hover:bg-gray-700 transition-colors">
    {{ colorMode.value === 'dark' ? '☀️' : '🌙' }}
  </button>
</template>
```

---

## สรุป Part 56

| Step | เนื้อหา |
|------|---------|
| 551 | Nuxt 3 setup + @nuxtjs/tailwindcss + tailwind.config + globals |
| 552 | Layouts + pages + NuxtLink navigation |
| 553 | Auto-imported components: KpiCard, AppSidebar |
| 554 | Composables: useUsers, useToast (auto-import) |
| 555 | Server routes: API GET/POST/DELETE with defineEventHandler |
| 556 | Users page: useFetch + $fetch + modal with Teleport |
| 557 | ToastContainer + TransitionGroup animation |
| 558 | Dark mode with @nuxtjs/color-mode + useColorMode |
| 559 | SSR vs SPA: `ssr: false` in nuxt.config for SPA mode |
| 560 | Workshop: Full Nuxt 3 app — pages, composables, API routes, dark mode |

**Part ถัดไป:** Part 57 — SvelteKit + Tailwind Integration (Steps 561–570)
