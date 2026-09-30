# Part 53: Vue 3 + Tailwind Integration

## เป้าหมาย
- Vue 3 Composition API + Tailwind CSS
- Reactive components พร้อม dynamic classes
- Vue directives + Tailwind patterns
- Steps 521–530

---

## Step 521: Vue 3 + Tailwind Setup

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Vue 3 + Tailwind - Step 521</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
</head>
<body class="bg-gray-50">
  <div id="app" class="p-8 max-w-4xl mx-auto space-y-8">

    <h1 class="text-2xl font-extrabold">Vue 3 + Tailwind</h1>

    <!-- Button Component via v-bind:class -->
    <section class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4">Dynamic Classes with :class</h3>
      <div class="flex flex-wrap gap-3">
        <button v-for="btn in buttons" :key="btn.id"
          :class="[
            'px-4 py-2 rounded-xl text-sm font-semibold transition-colors',
            btn.variant === 'primary' ? 'bg-indigo-600 text-white hover:bg-indigo-700' : '',
            btn.variant === 'outline' ? 'border-2 border-indigo-600 text-indigo-600 hover:bg-indigo-50' : '',
            btn.variant === 'ghost' ? 'bg-gray-100 text-gray-700 hover:bg-gray-200' : '',
            btn.variant === 'danger' ? 'bg-red-600 text-white hover:bg-red-700' : '',
            btn.loading ? 'opacity-50 cursor-not-allowed' : 'cursor-pointer',
          ]"
          :disabled="btn.loading"
          @click="handleBtn(btn)">
          <span v-if="btn.loading">⏳ Loading...</span>
          <span v-else>{{ btn.label }}</span>
        </button>
      </div>
    </section>

    <!-- Counter with reactive styles -->
    <section class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4">Reactive Counter</h3>
      <div class="flex items-center gap-4">
        <button @click="count--" class="w-9 h-9 bg-gray-100 rounded-xl flex items-center justify-center text-gray-700 hover:bg-gray-200 transition-colors text-xl font-bold">−</button>
        <div :class="['text-4xl font-extrabold transition-colors', count > 0 ? 'text-green-600' : count < 0 ? 'text-red-600' : 'text-gray-400']">{{ count }}</div>
        <button @click="count++" class="w-9 h-9 bg-indigo-600 rounded-xl flex items-center justify-center text-white hover:bg-indigo-700 transition-colors text-xl font-bold">+</button>
        <button @click="count = 0" class="text-xs text-gray-400 hover:text-gray-600 transition-colors ml-2">Reset</button>
      </div>
      <div class="mt-3 h-2 bg-gray-200 rounded-full overflow-hidden">
        <div class="h-full transition-all duration-300 rounded-full"
          :style="{ width: Math.abs(count) + '%', maxWidth: '100%' }"
          :class="count > 0 ? 'bg-green-500' : 'bg-red-500'">
        </div>
      </div>
    </section>

    <!-- Todo list -->
    <section class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4">Todo List</h3>
      <div class="flex gap-2 mb-4">
        <input v-model="newTodo" @keyup.enter="addTodo"
          class="flex-1 border border-gray-300 rounded-xl px-4 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500"
          placeholder="เพิ่มงาน...">
        <button @click="addTodo" class="bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">เพิ่ม</button>
      </div>
      <div v-if="todos.length === 0" class="text-center text-gray-400 text-sm py-4">ยังไม่มีงาน</div>
      <transition-group name="list" tag="div" class="space-y-2">
        <div v-for="todo in todos" :key="todo.id"
          :class="['flex items-center gap-3 p-3 rounded-xl transition-colors', todo.done ? 'bg-green-50' : 'bg-gray-50']">
          <input type="checkbox" v-model="todo.done" class="w-4 h-4 rounded text-indigo-600">
          <span :class="['flex-1 text-sm', todo.done ? 'line-through text-gray-400' : 'text-gray-700']">{{ todo.text }}</span>
          <button @click="removeTodo(todo.id)" class="text-red-400 hover:text-red-600 transition-colors text-sm">✕</button>
        </div>
      </transition-group>
      <div v-if="todos.length > 0" class="flex items-center justify-between mt-3 text-xs text-gray-400">
        <span>{{ todos.filter(t => t.done).length }} / {{ todos.length }} เสร็จแล้ว</span>
        <button @click="todos = todos.filter(t => !t.done)" class="hover:text-red-500 transition-colors">ลบที่เสร็จแล้ว</button>
      </div>
    </section>

  </div>

  <style>
    .list-enter-active, .list-leave-active { transition: all 0.3s ease; }
    .list-enter-from, .list-leave-to { opacity: 0; transform: translateY(-8px); }
  </style>

  <script>
    const { createApp, ref } = Vue;
    createApp({
      setup() {
        const count = ref(0);
        const newTodo = ref('');
        const todos = ref([
          { id: 1, text: 'เรียน Tailwind CSS', done: true },
          { id: 2, text: 'สร้าง Component Library', done: false },
        ]);
        const buttons = ref([
          { id: 1, label: 'Primary', variant: 'primary', loading: false },
          { id: 2, label: 'Outline', variant: 'outline', loading: false },
          { id: 3, label: 'Ghost', variant: 'ghost', loading: false },
          { id: 4, label: 'Danger', variant: 'danger', loading: false },
        ]);

        function handleBtn(btn) {
          btn.loading = true;
          setTimeout(() => btn.loading = false, 1500);
        }

        function addTodo() {
          if (!newTodo.value.trim()) return;
          todos.value.push({ id: Date.now(), text: newTodo.value.trim(), done: false });
          newTodo.value = '';
        }

        function removeTodo(id) {
          todos.value = todos.value.filter(t => t.id !== id);
        }

        return { count, newTodo, todos, buttons, handleBtn, addTodo, removeTodo };
      }
    }).mount('#app');
  </script>
</body>
</html>
```

---

## Steps 522–530: Vue Dashboard Workshop

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Vue Dashboard - Steps 522-530</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
</head>
<body class="bg-gray-100">
  <div id="vuedash">

    <!-- Sidebar + Main layout -->
    <div class="flex h-screen overflow-hidden">

      <!-- Sidebar -->
      <aside :class="['flex-shrink-0 bg-white border-r transition-all duration-300', sidebarOpen ? 'w-52' : 'w-14']">
        <div class="flex items-center h-14 border-b px-3 overflow-hidden">
          <button @click="sidebarOpen = !sidebarOpen" class="w-8 h-8 flex items-center justify-center rounded-xl hover:bg-gray-100 transition-colors flex-shrink-0">
            <svg class="w-5 h-5 text-gray-600" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"/></svg>
          </button>
          <span v-if="sidebarOpen" class="ml-2 font-extrabold text-indigo-600 whitespace-nowrap">VueDash</span>
        </div>
        <nav class="p-2 space-y-0.5">
          <button v-for="item in navItems" :key="item.id" @click="activePage = item.id"
            :class="['w-full flex items-center gap-2.5 px-2 py-2 rounded-xl transition-colors text-left',
              activePage === item.id ? 'bg-indigo-50 text-indigo-700' : 'text-gray-600 hover:bg-gray-50']">
            <span class="text-base flex-shrink-0">{{ item.icon }}</span>
            <span v-if="sidebarOpen" class="text-xs font-medium whitespace-nowrap">{{ item.label }}</span>
          </button>
        </nav>
      </aside>

      <!-- Main -->
      <div class="flex-1 flex flex-col overflow-hidden">
        <!-- Top bar -->
        <header class="bg-white border-b h-14 flex items-center justify-between px-6 flex-shrink-0">
          <div>
            <p class="font-bold text-sm">{{ navItems.find(n => n.id === activePage)?.label }}</p>
          </div>
          <div class="flex items-center gap-3">
            <!-- Search -->
            <div class="relative hidden md:block">
              <svg class="absolute left-3 top-1/2 -translate-y-1/2 w-3.5 h-3.5 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"/></svg>
              <input type="text" v-model="searchQ" placeholder="ค้นหา..." class="bg-gray-100 border-0 rounded-xl pl-8 pr-4 py-1.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-400 w-48">
            </div>
            <!-- Notification bell -->
            <button class="relative w-8 h-8 flex items-center justify-center rounded-xl hover:bg-gray-100 transition-colors">
              🔔
              <span v-if="notifications > 0" class="absolute top-0.5 right-0.5 w-4 h-4 bg-red-500 text-white text-[9px] font-bold rounded-full flex items-center justify-center">{{ notifications }}</span>
            </button>
            <!-- User -->
            <div class="flex items-center gap-2">
              <div class="w-7 h-7 bg-gradient-to-br from-indigo-400 to-purple-500 rounded-lg flex items-center justify-center text-white text-xs font-bold">SK</div>
            </div>
          </div>
        </header>

        <!-- Page content -->
        <main class="flex-1 overflow-y-auto p-6 space-y-6">

          <!-- Dashboard page -->
          <template v-if="activePage === 'dashboard'">
            <!-- KPIs -->
            <div class="grid grid-cols-2 xl:grid-cols-4 gap-4">
              <div v-for="kpi in kpis" :key="kpi.label"
                class="bg-white rounded-2xl border p-4 cursor-pointer hover:shadow-md transition-shadow">
                <div class="flex items-center justify-between mb-3">
                  <p class="text-xs text-gray-500 font-semibold">{{ kpi.label }}</p>
                  <span class="text-lg">{{ kpi.icon }}</span>
                </div>
                <p class="text-2xl font-extrabold text-gray-900">{{ kpi.value }}</p>
                <p :class="['text-xs mt-1', kpi.up ? 'text-green-500' : 'text-red-500']">
                  {{ kpi.up ? '▲' : '▼' }} {{ kpi.change }}
                </p>
              </div>
            </div>

            <!-- Chart + Table -->
            <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
              <div class="lg:col-span-2 bg-white rounded-2xl border p-5">
                <div class="flex items-center justify-between mb-4">
                  <p class="font-bold text-sm">ยอดขายรายเดือน</p>
                  <div class="flex gap-1">
                    <button v-for="r in ['3M','6M','12M']" :key="r" @click="chartRange = r"
                      :class="['text-xs px-2.5 py-1 rounded-lg transition-colors', chartRange === r ? 'bg-indigo-600 text-white' : 'bg-gray-100 text-gray-500 hover:bg-gray-200']">{{ r }}</button>
                  </div>
                </div>
                <div class="flex items-end gap-1.5 h-32">
                  <div v-for="(v, i) in chartData.slice(-parseInt(chartRange))" :key="i"
                    :style="{ height: (v / Math.max(...chartData) * 100) + '%' }"
                    :class="['flex-1 rounded-t-sm cursor-pointer hover:opacity-80 transition-opacity',
                      i === chartData.slice(-parseInt(chartRange)).length - 1 ? 'bg-indigo-600' : 'bg-indigo-200']"
                    :title="v + '%'">
                  </div>
                </div>
              </div>

              <div class="bg-white rounded-2xl border p-5">
                <p class="font-bold text-sm mb-3">Top Products</p>
                <div class="space-y-3">
                  <div v-for="(p, i) in topProducts" :key="i">
                    <div class="flex justify-between text-xs mb-1">
                      <span class="font-medium text-gray-700">{{ p.name }}</span>
                      <span class="text-gray-400">{{ p.pct }}%</span>
                    </div>
                    <div class="h-1.5 bg-gray-200 rounded-full overflow-hidden">
                      <div class="h-full bg-indigo-500 rounded-full transition-all duration-700" :style="{ width: p.pct + '%' }"></div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </template>

          <!-- Users page -->
          <template v-if="activePage === 'users'">
            <div class="bg-white rounded-2xl border overflow-hidden">
              <div class="px-5 py-4 border-b flex items-center justify-between">
                <p class="font-bold">ผู้ใช้ ({{ filteredUsers.length }})</p>
                <button @click="showAddUser = true" class="bg-indigo-600 text-white px-3 py-1.5 rounded-xl text-xs font-semibold hover:bg-indigo-700 transition-colors">+ เพิ่ม</button>
              </div>
              <div class="p-4">
                <input v-model="userSearch" placeholder="ค้นหาผู้ใช้..." class="w-full border border-gray-300 rounded-xl px-4 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 mb-4">
              </div>
              <table class="w-full text-sm">
                <thead class="bg-gray-50">
                  <tr>
                    <th class="px-5 py-3 text-left text-xs font-semibold text-gray-500">ชื่อ</th>
                    <th class="px-5 py-3 text-left text-xs font-semibold text-gray-500 hidden md:table-cell">อีเมล</th>
                    <th class="px-5 py-3 text-left text-xs font-semibold text-gray-500">สถานะ</th>
                    <th class="px-5 py-3"></th>
                  </tr>
                </thead>
                <tbody class="divide-y">
                  <tr v-for="user in filteredUsers" :key="user.id" class="hover:bg-gray-50 transition-colors">
                    <td class="px-5 py-3">
                      <div class="flex items-center gap-2">
                        <div class="w-7 h-7 rounded-lg bg-indigo-100 flex items-center justify-center text-xs font-bold text-indigo-600">{{ user.name[0] }}</div>
                        <span class="font-medium text-gray-900">{{ user.name }}</span>
                      </div>
                    </td>
                    <td class="px-5 py-3 text-gray-500 hidden md:table-cell">{{ user.email }}</td>
                    <td class="px-5 py-3">
                      <span :class="['text-xs font-semibold px-2.5 py-1 rounded-full',
                        user.status === 'active' ? 'bg-green-100 text-green-700' : 'bg-gray-100 text-gray-500']">
                        {{ user.status }}
                      </span>
                    </td>
                    <td class="px-5 py-3">
                      <button @click="removeUser(user.id)" class="text-xs text-red-400 hover:text-red-600 transition-colors">ลบ</button>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </template>

          <!-- Settings page -->
          <template v-if="activePage === 'settings'">
            <div class="bg-white rounded-2xl border p-5 space-y-4 max-w-lg">
              <h3 class="font-bold">การตั้งค่า</h3>
              <div v-for="setting in settings" :key="setting.key" class="flex items-center justify-between py-2 border-b last:border-0">
                <div>
                  <p class="text-sm font-medium">{{ setting.label }}</p>
                  <p class="text-xs text-gray-500">{{ setting.desc }}</p>
                </div>
                <button @click="setting.value = !setting.value"
                  :class="['relative w-11 h-6 rounded-full transition-colors', setting.value ? 'bg-indigo-600' : 'bg-gray-300']">
                  <span :class="['absolute w-5 h-5 bg-white rounded-full top-0.5 left-0.5 transition-transform shadow-sm', setting.value && 'translate-x-5']"></span>
                </button>
              </div>
            </div>
          </template>

        </main>
      </div>
    </div>

    <!-- Add user modal -->
    <div v-if="showAddUser" class="fixed inset-0 bg-black/50 z-50 flex items-center justify-center p-4" @click.self="showAddUser = false">
      <div class="bg-white rounded-2xl shadow-2xl w-full max-w-sm p-6">
        <h3 class="font-extrabold mb-4">เพิ่มผู้ใช้</h3>
        <div class="space-y-3">
          <input v-model="newUserName" placeholder="ชื่อ" class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
          <input v-model="newUserEmail" placeholder="อีเมล" type="email" class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
        </div>
        <div class="flex gap-3 mt-5">
          <button @click="showAddUser = false" class="flex-1 border border-gray-300 py-2.5 rounded-xl text-sm font-semibold hover:bg-gray-50 transition-colors">ยกเลิก</button>
          <button @click="addUser" class="flex-1 bg-indigo-600 text-white py-2.5 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">เพิ่ม</button>
        </div>
      </div>
    </div>

  </div>

  <script>
    const { createApp, ref, computed } = Vue;
    createApp({
      setup() {
        const sidebarOpen = ref(true);
        const activePage = ref('dashboard');
        const searchQ = ref('');
        const notifications = ref(3);
        const chartRange = ref('6M');
        const userSearch = ref('');
        const showAddUser = ref(false);
        const newUserName = ref('');
        const newUserEmail = ref('');

        const navItems = [
          { id: 'dashboard', label: 'Dashboard', icon: '🏠' },
          { id: 'users', label: 'Users', icon: '👥' },
          { id: 'analytics', label: 'Analytics', icon: '📊' },
          { id: 'settings', label: 'Settings', icon: '⚙️' },
        ];

        const kpis = ref([
          { label: 'ยอดขาย', value: '฿48,200', change: '12% วันนี้', up: true, icon: '💰' },
          { label: 'ผู้ใช้ใหม่', value: '284', change: '8% สัปดาห์', up: true, icon: '👥' },
          { label: 'Conversion', value: '3.2%', change: '0.5% เมื่อวาน', up: false, icon: '📈' },
          { label: 'Tickets', value: '17', change: '3 urgent', up: false, icon: '🎫' },
        ]);

        const chartData = [62, 78, 55, 90, 84, 95, 72, 88, 76, 100, 68, 92];

        const topProducts = ref([
          { name: 'Pro Plan', pct: 84 },
          { name: 'Enterprise', pct: 62 },
          { name: 'Free Plan', pct: 45 },
          { name: 'Add-ons', pct: 28 },
        ]);

        const users = ref([
          { id: 1, name: 'สมชาย เทคโน', email: 'sk@tech.co', status: 'active' },
          { id: 2, name: 'วิชัย ใจดี', email: 'wj@email.com', status: 'active' },
          { id: 3, name: 'มานี ดีมาก', email: 'mn@corp.io', status: 'inactive' },
        ]);

        const filteredUsers = computed(() =>
          users.value.filter(u =>
            !userSearch.value || u.name.toLowerCase().includes(userSearch.value.toLowerCase())
          )
        );

        function removeUser(id) { users.value = users.value.filter(u => u.id !== id); }
        function addUser() {
          if (!newUserName.value.trim()) return;
          users.value.push({ id: Date.now(), name: newUserName.value, email: newUserEmail.value, status: 'active' });
          newUserName.value = ''; newUserEmail.value = '';
          showAddUser.value = false;
        }

        const settings = ref([
          { key: 'email', label: 'การแจ้งเตือนอีเมล', desc: 'รับอีเมลเมื่อมีกิจกรรมใหม่', value: true },
          { key: 'push', label: 'Push Notification', desc: 'แจ้งเตือนผ่านเบราว์เซอร์', value: false },
          { key: 'dark', label: 'Dark Mode', desc: 'ธีมสีเข้ม', value: false },
          { key: 'twofa', label: 'Two-Factor Auth', desc: 'ความปลอดภัยเพิ่มเติม', value: true },
        ]);

        return {
          sidebarOpen, activePage, searchQ, notifications, chartRange,
          navItems, kpis, chartData, topProducts,
          users, filteredUsers, userSearch, removeUser,
          showAddUser, newUserName, newUserEmail, addUser,
          settings,
        };
      }
    }).mount('#vuedash');
  </script>
</body>
</html>
```

---

## สรุป Part 53

| Step | เนื้อหา |
|------|---------|
| 521 | Vue 3 setup, :class binding, reactive counter, animated todo list |
| 522 | Collapsible sidebar with transition |
| 523 | Top navigation bar with search + notification |
| 524 | Dashboard KPI cards |
| 525 | Bar chart with chartRange filter |
| 526 | Top products progress bars |
| 527 | Users table with v-for + computed filteredUsers |
| 528 | Settings toggles with v-model |
| 529 | Add user modal with v-if |
| 530 | Workshop: Complete Vue 3 Dashboard (sidebar + 3 pages) |

**Part ถัดไป:** Part 54 — Alpine.js + Tailwind (Steps 531–540)
