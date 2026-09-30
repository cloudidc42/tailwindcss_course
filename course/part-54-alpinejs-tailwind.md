# Part 54: Alpine.js + Tailwind Integration

## เป้าหมาย
- Alpine.js x-data, x-bind, x-show, x-transition
- Tailwind + Alpine reactive patterns
- Interactive components โดยไม่ต้องเขียน JavaScript แยก
- Steps 531–540

---

## Step 531: Alpine.js พื้นฐาน

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Alpine.js + Tailwind - Step 531</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script defer src="https://unpkg.com/alpinejs@3.x.x/dist/cdn.min.js"></script>
</head>
<body class="bg-gray-50 p-8">
  <div class="max-w-3xl mx-auto space-y-6">

    <h1 class="text-2xl font-extrabold">Alpine.js + Tailwind</h1>

    <!-- Counter: x-data + x-on -->
    <div class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4">Counter</h3>
      <div x-data="{ n: 0 }" class="flex items-center gap-4">
        <button @click="n--" class="w-9 h-9 bg-gray-100 rounded-xl text-gray-700 hover:bg-gray-200 transition-colors font-bold text-xl">−</button>
        <span class="text-3xl font-extrabold w-12 text-center" :class="n > 0 ? 'text-green-600' : n < 0 ? 'text-red-500' : 'text-gray-400'">
          <span x-text="n"></span>
        </span>
        <button @click="n++" class="w-9 h-9 bg-indigo-600 text-white rounded-xl hover:bg-indigo-700 transition-colors font-bold text-xl">+</button>
        <button @click="n = 0" class="text-xs text-gray-400 hover:text-gray-600 ml-2">Reset</button>
      </div>
    </div>

    <!-- Toggle: x-show + x-transition -->
    <div class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4">x-show + x-transition</h3>
      <div x-data="{ open: false }">
        <button @click="open = !open" class="bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">
          <span x-text="open ? 'ซ่อน' : 'แสดง'"></span>
        </button>
        <div x-show="open"
          x-transition:enter="transition ease-out duration-200"
          x-transition:enter-start="opacity-0 translate-y-2"
          x-transition:enter-end="opacity-100 translate-y-0"
          x-transition:leave="transition ease-in duration-150"
          x-transition:leave-start="opacity-100 translate-y-0"
          x-transition:leave-end="opacity-0 translate-y-2"
          class="mt-4 bg-indigo-50 border border-indigo-200 rounded-xl p-4 text-indigo-700 text-sm">
          ✨ ข้อความที่ซ่อนอยู่ — แสดงด้วย Alpine.js transition
        </div>
      </div>
    </div>

    <!-- x-model: two-way binding -->
    <div class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4">x-model (Two-way binding)</h3>
      <div x-data="{ msg: '', darkMode: false, size: '16' }">
        <div class="space-y-3">
          <input x-model="msg" placeholder="พิมพ์ข้อความ..."
            class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
          <div class="flex gap-6">
            <label class="flex items-center gap-2 text-sm cursor-pointer">
              <input type="checkbox" x-model="darkMode" class="w-4 h-4 rounded text-indigo-600">
              Dark Mode
            </label>
          </div>
          <input type="range" x-model="size" min="12" max="32" class="w-full">
          <div :class="['rounded-xl p-4 transition-colors', darkMode ? 'bg-gray-800 text-white' : 'bg-gray-100 text-gray-800']"
            :style="'font-size:' + size + 'px'">
            <span x-text="msg || 'ตัวอย่างข้อความ'"></span>
          </div>
        </div>
      </div>
    </div>

    <!-- x-for: list rendering -->
    <div class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4">x-for + CRUD</h3>
      <div x-data="{ items: ['Tailwind CSS', 'Alpine.js', 'Vue 3'], newItem: '' }">
        <div class="flex gap-2 mb-3">
          <input x-model="newItem" @keyup.enter="if(newItem.trim()) { items.push(newItem.trim()); newItem=''; }"
            placeholder="เพิ่มรายการ..."
            class="flex-1 border border-gray-300 rounded-xl px-4 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
          <button @click="if(newItem.trim()) { items.push(newItem.trim()); newItem=''; }"
            class="bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm hover:bg-indigo-700 transition-colors">เพิ่ม</button>
        </div>
        <ul class="space-y-2">
          <template x-for="(item, i) in items" :key="i">
            <li class="flex items-center justify-between bg-gray-50 rounded-xl px-4 py-2.5">
              <span class="text-sm text-gray-700" x-text="item"></span>
              <button @click="items.splice(i, 1)" class="text-red-400 hover:text-red-600 text-sm transition-colors">✕</button>
            </li>
          </template>
        </ul>
        <p class="text-xs text-gray-400 mt-2" x-text="items.length + ' รายการ'"></p>
      </div>
    </div>

  </div>
</body>
</html>
```

---

## Steps 532–540: Alpine.js Full App Workshop

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Alpine.js App - Steps 532-540</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script defer src="https://unpkg.com/alpinejs@3.x.x/dist/cdn.min.js"></script>
</head>
<body class="bg-gray-100 min-h-screen" x-data="app()" x-init="init()">

  <!-- Toast notifications -->
  <div class="fixed bottom-4 right-4 z-50 space-y-2">
    <template x-for="toast in toasts" :key="toast.id">
      <div x-show="toast.show"
        x-transition:enter="transition ease-out duration-300"
        x-transition:enter-start="opacity-0 translate-x-4"
        x-transition:enter-end="opacity-100 translate-x-0"
        x-transition:leave="transition ease-in duration-200"
        x-transition:leave-start="opacity-100 translate-x-0"
        x-transition:leave-end="opacity-0 translate-x-4"
        :class="['flex items-center gap-3 px-4 py-3 rounded-xl shadow-lg text-sm font-medium text-white',
          toast.type === 'success' ? 'bg-green-600' : toast.type === 'error' ? 'bg-red-600' : 'bg-indigo-600']">
        <span x-text="toast.type === 'success' ? '✅' : toast.type === 'error' ? '❌' : 'ℹ️'"></span>
        <span x-text="toast.message"></span>
      </div>
    </template>
  </div>

  <!-- Modal backdrop -->
  <div x-show="modal.open" x-transition.opacity
    class="fixed inset-0 bg-black/50 z-40 flex items-center justify-center p-4"
    @click.self="modal.open = false">
    <div x-show="modal.open"
      x-transition:enter="transition ease-out duration-200"
      x-transition:enter-start="opacity-0 scale-95"
      x-transition:enter-end="opacity-100 scale-100"
      x-transition:leave="transition ease-in duration-150"
      x-transition:leave-start="opacity-100 scale-100"
      x-transition:leave-end="opacity-0 scale-95"
      class="bg-white rounded-2xl shadow-2xl w-full max-w-sm p-6"
      @click.stop>
      <h3 class="font-extrabold text-gray-900 mb-4" x-text="modal.title"></h3>
      <div class="space-y-3">
        <input x-model="modal.form.name" placeholder="ชื่อ"
          class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
        <input x-model="modal.form.email" placeholder="อีเมล" type="email"
          class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
        <select x-model="modal.form.role"
          class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
          <option value="user">User</option>
          <option value="admin">Admin</option>
          <option value="editor">Editor</option>
        </select>
      </div>
      <div class="flex gap-3 mt-5">
        <button @click="modal.open = false" class="flex-1 border border-gray-300 py-2.5 rounded-xl text-sm font-semibold hover:bg-gray-50 transition-colors">ยกเลิก</button>
        <button @click="saveUser()" class="flex-1 bg-indigo-600 text-white py-2.5 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">บันทึก</button>
      </div>
    </div>
  </div>

  <!-- Main Layout -->
  <div class="flex h-screen overflow-hidden">

    <!-- Sidebar -->
    <aside class="w-52 bg-white border-r flex flex-col flex-shrink-0">
      <div class="h-14 border-b flex items-center px-4">
        <span class="font-extrabold text-indigo-600 text-sm">⛰ AlpineDash</span>
      </div>
      <nav class="p-2 space-y-0.5 flex-1">
        <template x-for="item in nav" :key="item.id">
          <button @click="page = item.id"
            :class="['w-full flex items-center gap-2.5 px-3 py-2 rounded-xl transition-colors text-sm text-left',
              page === item.id ? 'bg-indigo-50 text-indigo-700 font-semibold' : 'text-gray-600 hover:bg-gray-50']">
            <span x-text="item.icon"></span>
            <span x-text="item.label"></span>
          </button>
        </template>
      </nav>
      <div class="p-3 border-t">
        <div class="flex items-center gap-2">
          <div class="w-7 h-7 bg-indigo-100 rounded-lg flex items-center justify-center text-xs font-bold text-indigo-600">A</div>
          <span class="text-xs font-medium text-gray-700">Admin User</span>
        </div>
      </div>
    </aside>

    <!-- Main -->
    <div class="flex-1 flex flex-col overflow-hidden">
      <header class="bg-white border-b h-14 px-6 flex items-center justify-between flex-shrink-0">
        <p class="font-bold text-sm" x-text="nav.find(n => n.id === page)?.label"></p>
        <div class="flex items-center gap-3">
          <span class="text-xs text-gray-400" x-text="new Date().toLocaleDateString('th-TH')"></span>
        </div>
      </header>

      <main class="flex-1 overflow-y-auto p-6">

        <!-- Dashboard page -->
        <div x-show="page === 'dashboard'" class="space-y-6">
          <div class="grid grid-cols-2 xl:grid-cols-4 gap-4">
            <template x-for="kpi in kpis" :key="kpi.label">
              <div class="bg-white rounded-2xl border p-4 hover:shadow-md transition-shadow cursor-pointer">
                <div class="flex justify-between items-start mb-2">
                  <p class="text-xs text-gray-500 font-semibold" x-text="kpi.label"></p>
                  <span x-text="kpi.icon" class="text-xl"></span>
                </div>
                <p class="text-2xl font-extrabold text-gray-900" x-text="kpi.value"></p>
                <p class="text-xs mt-1" :class="kpi.up ? 'text-green-500' : 'text-red-500'"
                  x-text="(kpi.up ? '▲ ' : '▼ ') + kpi.change"></p>
              </div>
            </template>
          </div>

          <!-- Tabs -->
          <div class="bg-white rounded-2xl border overflow-hidden">
            <div class="flex border-b">
              <template x-for="tab in tabs" :key="tab">
                <button @click="activeTab = tab"
                  :class="['px-5 py-3 text-sm font-semibold border-b-2 transition-colors',
                    activeTab === tab ? 'border-indigo-600 text-indigo-600' : 'border-transparent text-gray-500 hover:text-gray-700']"
                  x-text="tab">
                </button>
              </template>
            </div>
            <div class="p-5">
              <div x-show="activeTab === 'ภาพรวม'" class="space-y-3">
                <div class="flex items-end gap-1.5 h-28">
                  <template x-for="v in chartVals" :key="v">
                    <div class="flex-1 bg-indigo-200 rounded-t-sm hover:bg-indigo-400 transition-colors cursor-pointer"
                      :style="'height:' + v + '%'"></div>
                  </template>
                </div>
              </div>
              <div x-show="activeTab === 'ยอดขาย'" class="text-sm text-gray-500">ข้อมูลยอดขายรายละเอียด...</div>
              <div x-show="activeTab === 'รายงาน'" class="text-sm text-gray-500">ดาวน์โหลดรายงานได้ที่นี่</div>
            </div>
          </div>
        </div>

        <!-- Users page -->
        <div x-show="page === 'users'" class="space-y-4">
          <div class="bg-white rounded-2xl border overflow-hidden">
            <div class="px-5 py-4 border-b flex items-center justify-between">
              <p class="font-bold">ผู้ใช้ (<span x-text="filteredUsers().length"></span>)</p>
              <button @click="openAdd()" class="bg-indigo-600 text-white px-3 py-1.5 rounded-xl text-xs font-semibold hover:bg-indigo-700 transition-colors">+ เพิ่ม</button>
            </div>
            <div class="px-5 pt-4">
              <input x-model="userSearch" placeholder="ค้นหา..." class="border border-gray-300 rounded-xl px-4 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 w-64 mb-4">
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
                <template x-for="user in filteredUsers()" :key="user.id">
                  <tr class="hover:bg-gray-50 transition-colors">
                    <td class="px-5 py-3">
                      <div class="flex items-center gap-2">
                        <div class="w-7 h-7 rounded-lg bg-indigo-100 flex items-center justify-center text-xs font-bold text-indigo-700"
                          x-text="user.name[0]"></div>
                        <span class="font-medium text-gray-900" x-text="user.name"></span>
                      </div>
                    </td>
                    <td class="px-5 py-3 text-gray-500 hidden md:table-cell" x-text="user.email"></td>
                    <td class="px-5 py-3">
                      <span :class="['text-xs font-semibold px-2.5 py-1 rounded-full',
                        user.role === 'admin' ? 'bg-red-100 text-red-700' :
                        user.role === 'editor' ? 'bg-indigo-100 text-indigo-700' :
                        'bg-gray-100 text-gray-600']" x-text="user.role">
                      </span>
                    </td>
                    <td class="px-5 py-3">
                      <div class="flex gap-2">
                        <button @click="openEdit(user)" class="text-xs text-indigo-500 hover:text-indigo-700 transition-colors">แก้ไข</button>
                        <button @click="deleteUser(user.id)" class="text-xs text-red-400 hover:text-red-600 transition-colors">ลบ</button>
                      </div>
                    </td>
                  </tr>
                </template>
              </tbody>
            </table>
          </div>
        </div>

        <!-- Settings page -->
        <div x-show="page === 'settings'" class="max-w-md space-y-4">
          <div class="bg-white rounded-2xl border p-5">
            <h3 class="font-bold mb-4">การตั้งค่าระบบ</h3>
            <div class="space-y-4">
              <template x-for="s in settingsList" :key="s.key">
                <div class="flex items-center justify-between py-2 border-b last:border-0">
                  <div>
                    <p class="text-sm font-medium text-gray-900" x-text="s.label"></p>
                    <p class="text-xs text-gray-500" x-text="s.desc"></p>
                  </div>
                  <button @click="s.val = !s.val"
                    :class="['relative w-11 h-6 rounded-full transition-colors', s.val ? 'bg-indigo-600' : 'bg-gray-300']">
                    <span :class="['absolute w-5 h-5 bg-white rounded-full top-0.5 left-0.5 transition-transform shadow-sm', s.val && 'translate-x-5']"></span>
                  </button>
                </div>
              </template>
            </div>
          </div>
        </div>

      </main>
    </div>
  </div>

  <script>
    function app() {
      return {
        page: 'dashboard',
        nav: [
          { id: 'dashboard', label: 'Dashboard', icon: '🏠' },
          { id: 'users', label: 'Users', icon: '👥' },
          { id: 'settings', label: 'Settings', icon: '⚙️' },
        ],
        kpis: [
          { label: 'ยอดขาย', value: '฿52K', change: '+14%', up: true, icon: '💰' },
          { label: 'ผู้ใช้', value: '1,284', change: '+8%', up: true, icon: '👥' },
          { label: 'Orders', value: '391', change: '-3%', up: false, icon: '📦' },
          { label: 'Revenue', value: '฿8.4M', change: '+22%', up: true, icon: '📈' },
        ],
        activeTab: 'ภาพรวม',
        tabs: ['ภาพรวม', 'ยอดขาย', 'รายงาน'],
        chartVals: [60, 80, 55, 90, 70, 95, 75, 88, 65, 100, 80, 92],
        userSearch: '',
        users: [
          { id: 1, name: 'สมชาย เทคโน', email: 'sk@tech.co', role: 'admin' },
          { id: 2, name: 'วิชัย ดีงาม', email: 'wd@corp.io', role: 'editor' },
          { id: 3, name: 'นิดา สวยงาม', email: 'nd@mail.com', role: 'user' },
        ],
        modal: { open: false, title: '', form: { name: '', email: '', role: 'user' }, editId: null },
        toasts: [],
        settingsList: [
          { key: 'email', label: 'Email Notifications', desc: 'รับการแจ้งเตือนทางอีเมล', val: true },
          { key: 'push', label: 'Push Notifications', desc: 'แจ้งเตือนผ่านเบราว์เซอร์', val: false },
          { key: 'twofa', label: 'Two-Factor Auth', desc: 'เพิ่มความปลอดภัย', val: true },
        ],
        init() {},
        filteredUsers() {
          const q = this.userSearch.toLowerCase();
          return q ? this.users.filter(u => u.name.toLowerCase().includes(q)) : this.users;
        },
        openAdd() {
          this.modal = { open: true, title: 'เพิ่มผู้ใช้', form: { name: '', email: '', role: 'user' }, editId: null };
        },
        openEdit(user) {
          this.modal = { open: true, title: 'แก้ไขผู้ใช้', form: { ...user }, editId: user.id };
        },
        saveUser() {
          if (!this.modal.form.name.trim()) return;
          if (this.modal.editId) {
            const u = this.users.find(u => u.id === this.modal.editId);
            Object.assign(u, this.modal.form);
            this.toast('แก้ไขผู้ใช้สำเร็จ', 'success');
          } else {
            this.users.push({ id: Date.now(), ...this.modal.form });
            this.toast('เพิ่มผู้ใช้สำเร็จ', 'success');
          }
          this.modal.open = false;
        },
        deleteUser(id) {
          this.users = this.users.filter(u => u.id !== id);
          this.toast('ลบผู้ใช้แล้ว', 'error');
        },
        toast(message, type = 'info') {
          const t = { id: Date.now(), message, type, show: true };
          this.toasts.push(t);
          setTimeout(() => { t.show = false; setTimeout(() => { this.toasts = this.toasts.filter(x => x.id !== t.id); }, 300); }, 3000);
        },
      };
    }
  </script>
</body>
</html>
```

---

## สรุป Part 54

| Step | เนื้อหา |
|------|---------|
| 531 | x-data, x-on, x-show, x-transition, x-model, x-for พื้นฐาน |
| 532 | Collapsible sidebar ด้วย Alpine |
| 533 | Dashboard KPI cards |
| 534 | Tab component ด้วย activeTab |
| 535 | Bar chart reactive |
| 536 | Users table + filteredUsers() |
| 537 | Add/Edit user modal |
| 538 | Delete + Toast notifications |
| 539 | Settings toggles |
| 540 | Workshop: Alpine.js Full Dashboard (3 pages + CRUD + Toasts) |

**Part ถัดไป:** Part 55 — Next.js + Tailwind Integration (Steps 541–550)
