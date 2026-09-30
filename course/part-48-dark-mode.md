# Part 48: Dark Mode Implementation

## เป้าหมาย
- Dark mode ด้วย `dark:` prefix
- Toggle switch ที่บันทึกค่าใน localStorage
- สร้าง component ที่รองรับ dark/light อัตโนมัติ
- Steps 471–480

---

## Step 471: Dark Mode Basics

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Dark Mode Basics - Step 471</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    // Load saved theme before render to avoid flash
    if (localStorage.theme === 'dark' || (!('theme' in localStorage) && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
      document.documentElement.classList.add('dark');
    }
  </script>
</head>
<body class="bg-white dark:bg-gray-900 text-gray-900 dark:text-white transition-colors duration-300 p-8">

  <div class="max-w-3xl mx-auto space-y-6">
    <div class="flex items-center justify-between">
      <h2 class="text-xl font-extrabold">Dark Mode Basics</h2>
      <!-- Toggle -->
      <button onclick="toggleDark()" id="dark-btn" class="flex items-center gap-2 bg-gray-100 dark:bg-gray-800 border dark:border-gray-700 px-4 py-2 rounded-xl text-sm font-medium transition-colors">
        <span id="dark-icon">🌙</span>
        <span id="dark-label">Dark Mode</span>
      </button>
    </div>

    <!-- Cards -->
    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
      <div class="bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 rounded-2xl p-5 shadow-sm">
        <h3 class="font-bold text-gray-900 dark:text-white">Card Component</h3>
        <p class="text-gray-600 dark:text-gray-300 text-sm mt-1">การ์ดที่รองรับ dark mode</p>
        <button class="mt-3 bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">Action</button>
      </div>
      <div class="bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 rounded-2xl p-5">
        <div class="flex items-center gap-3 mb-3">
          <div class="w-8 h-8 bg-indigo-100 dark:bg-indigo-900 rounded-lg flex items-center justify-center text-indigo-600 dark:text-indigo-400">📊</div>
          <div>
            <p class="text-sm font-semibold text-gray-900 dark:text-white">Statistics</p>
            <p class="text-xs text-gray-500 dark:text-gray-400">วันนี้</p>
          </div>
        </div>
        <p class="text-2xl font-extrabold text-gray-900 dark:text-white">1,284</p>
        <p class="text-xs text-green-500 mt-1">▲ 12% จากเมื่อวาน</p>
      </div>
    </div>

    <!-- Form -->
    <div class="bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 rounded-2xl p-5">
      <h3 class="font-bold mb-4 text-gray-900 dark:text-white">Form Components</h3>
      <div class="space-y-4">
        <div>
          <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1.5">ชื่อผู้ใช้</label>
          <input type="text" placeholder="กรอกชื่อ..." class="w-full border border-gray-300 dark:border-gray-600 bg-white dark:bg-gray-700 text-gray-900 dark:text-white placeholder-gray-400 dark:placeholder-gray-500 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 transition-colors">
        </div>
        <div>
          <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1.5">เลือกแผนก</label>
          <select class="w-full border border-gray-300 dark:border-gray-600 bg-white dark:bg-gray-700 text-gray-900 dark:text-white rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 transition-colors">
            <option>เลือก...</option>
            <option>ฝ่ายพัฒนา</option>
            <option>ฝ่ายการตลาด</option>
          </select>
        </div>
      </div>
    </div>

    <!-- Table -->
    <div class="bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 rounded-2xl overflow-hidden">
      <table class="w-full text-sm">
        <thead class="bg-gray-50 dark:bg-gray-700/50">
          <tr>
            <th class="px-4 py-3 text-left text-xs font-semibold text-gray-500 dark:text-gray-400">ชื่อ</th>
            <th class="px-4 py-3 text-left text-xs font-semibold text-gray-500 dark:text-gray-400">บทบาท</th>
            <th class="px-4 py-3 text-left text-xs font-semibold text-gray-500 dark:text-gray-400">สถานะ</th>
          </tr>
        </thead>
        <tbody class="divide-y divide-gray-200 dark:divide-gray-700">
          <tr class="hover:bg-gray-50 dark:hover:bg-gray-700/50 transition-colors">
            <td class="px-4 py-3 text-gray-900 dark:text-white">สมชาย เทคโน</td>
            <td class="px-4 py-3 text-gray-600 dark:text-gray-300">Developer</td>
            <td class="px-4 py-3"><span class="bg-green-100 dark:bg-green-900/30 text-green-700 dark:text-green-400 text-xs px-2 py-0.5 rounded-full">Active</span></td>
          </tr>
          <tr class="hover:bg-gray-50 dark:hover:bg-gray-700/50 transition-colors">
            <td class="px-4 py-3 text-gray-900 dark:text-white">วิชัย ใจดี</td>
            <td class="px-4 py-3 text-gray-600 dark:text-gray-300">Designer</td>
            <td class="px-4 py-3"><span class="bg-yellow-100 dark:bg-yellow-900/30 text-yellow-700 dark:text-yellow-400 text-xs px-2 py-0.5 rounded-full">Away</span></td>
          </tr>
        </tbody>
      </table>
    </div>

  </div>

  <script>
    function toggleDark() {
      const html = document.documentElement;
      const isDark = html.classList.toggle('dark');
      localStorage.theme = isDark ? 'dark' : 'light';
      document.getElementById('dark-icon').textContent = isDark ? '☀️' : '🌙';
      document.getElementById('dark-label').textContent = isDark ? 'Light Mode' : 'Dark Mode';
    }

    // Set initial icon
    const isDark = document.documentElement.classList.contains('dark');
    document.getElementById('dark-icon').textContent = isDark ? '☀️' : '🌙';
    document.getElementById('dark-label').textContent = isDark ? 'Light Mode' : 'Dark Mode';
  </script>

</body>
</html>
```

---

## Steps 472–479: Dark Mode Component Library

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Dark Mode Components - Steps 472-479</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    if (localStorage.theme === 'dark') document.documentElement.classList.add('dark');
  </script>
</head>
<body class="bg-gray-100 dark:bg-gray-950 text-gray-900 dark:text-white transition-colors duration-300">

  <!-- Header with dark toggle -->
  <header class="bg-white dark:bg-gray-900 border-b border-gray-200 dark:border-gray-800 px-6 h-14 flex items-center justify-between sticky top-0 z-20">
    <h1 class="font-extrabold text-indigo-600 dark:text-indigo-400">DarkKit</h1>
    <div class="flex items-center gap-3">
      <!-- Dark toggle switch -->
      <button onclick="toggleDark()" id="toggle-btn"
        class="relative w-12 h-6 rounded-full transition-colors duration-300 bg-gray-300 dark:bg-indigo-600">
        <span id="toggle-knob" class="absolute w-5 h-5 bg-white rounded-full top-0.5 left-0.5 dark:translate-x-6 transition-transform duration-300 shadow-sm flex items-center justify-center text-[10px]">🌙</span>
      </button>
    </div>
  </header>

  <main class="max-w-5xl mx-auto p-6 space-y-8">

    <!-- Buttons dark (Step 472) -->
    <section class="bg-white dark:bg-gray-800 rounded-2xl border border-gray-200 dark:border-gray-700 p-5">
      <h3 class="font-bold mb-4">Buttons</h3>
      <div class="flex flex-wrap gap-3">
        <button class="bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">Primary</button>
        <button class="border-2 border-indigo-600 dark:border-indigo-400 text-indigo-600 dark:text-indigo-400 px-4 py-2 rounded-xl text-sm font-semibold hover:bg-indigo-50 dark:hover:bg-indigo-900/20 transition-colors">Outline</button>
        <button class="bg-gray-100 dark:bg-gray-700 text-gray-700 dark:text-gray-300 px-4 py-2 rounded-xl text-sm font-semibold hover:bg-gray-200 dark:hover:bg-gray-600 transition-colors">Ghost</button>
        <button class="bg-red-600 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-red-700 transition-colors">Danger</button>
      </div>
    </section>

    <!-- Sidebar dark (Step 473) -->
    <section class="bg-white dark:bg-gray-800 rounded-2xl border border-gray-200 dark:border-gray-700 overflow-hidden">
      <div class="flex h-64">
        <aside class="w-44 bg-gray-50 dark:bg-gray-900 border-r border-gray-200 dark:border-gray-700 p-3 flex-shrink-0">
          <p class="text-[10px] font-bold text-gray-400 dark:text-gray-500 uppercase tracking-wider px-2 mb-2">Menu</p>
          <nav class="space-y-0.5">
            <a href="#" class="flex items-center gap-2 px-2 py-1.5 rounded-lg bg-indigo-50 dark:bg-indigo-900/40 text-indigo-700 dark:text-indigo-300 text-xs font-medium">🏠 หน้าแรก</a>
            <a href="#" class="flex items-center gap-2 px-2 py-1.5 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-700/50 text-gray-600 dark:text-gray-400 text-xs transition-colors">📊 Analytics</a>
            <a href="#" class="flex items-center gap-2 px-2 py-1.5 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-700/50 text-gray-600 dark:text-gray-400 text-xs transition-colors">⚙️ Settings</a>
          </nav>
        </aside>
        <div class="flex-1 p-5">
          <p class="font-bold text-sm text-gray-900 dark:text-white mb-1">Content Area</p>
          <p class="text-xs text-gray-500 dark:text-gray-400">Sidebar + content dark mode</p>
          <div class="mt-4 grid grid-cols-2 gap-3">
            <div class="bg-gray-50 dark:bg-gray-700/50 rounded-xl p-3 text-center">
              <p class="text-lg font-extrabold text-indigo-600 dark:text-indigo-400">84%</p>
              <p class="text-xs text-gray-500 dark:text-gray-400">Performance</p>
            </div>
            <div class="bg-gray-50 dark:bg-gray-700/50 rounded-xl p-3 text-center">
              <p class="text-lg font-extrabold text-green-600 dark:text-green-400">100%</p>
              <p class="text-xs text-gray-500 dark:text-gray-400">Uptime</p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Code block dark (Step 474) -->
    <section class="bg-white dark:bg-gray-800 rounded-2xl border border-gray-200 dark:border-gray-700 p-5">
      <div class="flex items-center justify-between mb-3">
        <h3 class="font-bold">Code Block</h3>
        <button onclick="navigator.clipboard.writeText(document.querySelector('code').textContent)" class="text-xs bg-gray-100 dark:bg-gray-700 px-3 py-1.5 rounded-lg hover:bg-gray-200 dark:hover:bg-gray-600 transition-colors text-gray-600 dark:text-gray-300">Copy</button>
      </div>
      <div class="bg-gray-900 rounded-xl overflow-hidden">
        <div class="flex items-center gap-2 px-4 py-2.5 bg-gray-800 border-b border-gray-700">
          <div class="w-3 h-3 rounded-full bg-red-500"></div>
          <div class="w-3 h-3 rounded-full bg-yellow-500"></div>
          <div class="w-3 h-3 rounded-full bg-green-500"></div>
          <span class="ml-2 text-xs text-gray-400">tailwind.html</span>
        </div>
        <pre class="p-4 text-xs text-green-400 overflow-x-auto"><code>&lt;div class="bg-white <span class="text-blue-400">dark:bg-gray-800</span>
     text-gray-900 <span class="text-blue-400">dark:text-white</span>
     border-gray-200 <span class="text-blue-400">dark:border-gray-700</span>"&gt;
  Dark mode ready component
&lt;/div&gt;</code></pre>
      </div>
    </section>

    <!-- Alert dark (Step 475) -->
    <section class="space-y-3">
      <div class="bg-blue-50 dark:bg-blue-900/20 border border-blue-200 dark:border-blue-800 rounded-xl p-4 flex gap-3">
        <span class="text-blue-500 dark:text-blue-400">ℹ️</span>
        <p class="text-sm text-blue-800 dark:text-blue-200">Info alert ที่รองรับ dark mode</p>
      </div>
      <div class="bg-green-50 dark:bg-green-900/20 border border-green-200 dark:border-green-800 rounded-xl p-4 flex gap-3">
        <span class="text-green-500 dark:text-green-400">✅</span>
        <p class="text-sm text-green-800 dark:text-green-200">Success alert พร้อม dark mode</p>
      </div>
      <div class="bg-red-50 dark:bg-red-900/20 border border-red-200 dark:border-red-800 rounded-xl p-4 flex gap-3">
        <span class="text-red-500 dark:text-red-400">❌</span>
        <p class="text-sm text-red-800 dark:text-red-200">Error alert dark mode support</p>
      </div>
    </section>

    <!-- Stats dark (Step 476) -->
    <section class="grid grid-cols-2 md:grid-cols-4 gap-4">
      <div class="bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 rounded-2xl p-4 text-center">
        <p class="text-2xl font-extrabold text-indigo-600 dark:text-indigo-400">12K</p>
        <p class="text-xs text-gray-500 dark:text-gray-400 mt-1">ผู้ใช้</p>
      </div>
      <div class="bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 rounded-2xl p-4 text-center">
        <p class="text-2xl font-extrabold text-green-600 dark:text-green-400">฿2.4M</p>
        <p class="text-xs text-gray-500 dark:text-gray-400 mt-1">รายได้</p>
      </div>
      <div class="bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 rounded-2xl p-4 text-center">
        <p class="text-2xl font-extrabold text-purple-600 dark:text-purple-400">98%</p>
        <p class="text-xs text-gray-500 dark:text-gray-400 mt-1">Uptime</p>
      </div>
      <div class="bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 rounded-2xl p-4 text-center">
        <p class="text-2xl font-extrabold text-orange-600 dark:text-orange-400">4.9</p>
        <p class="text-xs text-gray-500 dark:text-gray-400 mt-1">Rating</p>
      </div>
    </section>

    <!-- Modal dark (Step 477) -->
    <section class="bg-white dark:bg-gray-800 rounded-2xl border border-gray-200 dark:border-gray-700 p-5">
      <h3 class="font-bold mb-4">Modal</h3>
      <button onclick="document.getElementById('dark-modal').classList.remove('hidden')"
        class="bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">Open Modal</button>
    </section>

    <!-- Profile card dark (Step 478) -->
    <section class="grid grid-cols-1 md:grid-cols-3 gap-4">
      <div class="bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 rounded-2xl p-5">
        <div class="flex items-center gap-3 mb-4">
          <div class="w-12 h-12 bg-gradient-to-br from-indigo-400 to-purple-500 rounded-xl flex items-center justify-center text-white font-bold">SK</div>
          <div>
            <p class="font-bold text-gray-900 dark:text-white text-sm">สมชาย</p>
            <p class="text-xs text-gray-500 dark:text-gray-400">Developer</p>
          </div>
        </div>
        <div class="flex gap-2">
          <button class="flex-1 bg-indigo-600 text-white py-2 rounded-xl text-xs font-semibold hover:bg-indigo-700 transition-colors">Follow</button>
          <button class="flex-1 border border-gray-200 dark:border-gray-600 text-gray-700 dark:text-gray-300 py-2 rounded-xl text-xs font-semibold hover:bg-gray-50 dark:hover:bg-gray-700 transition-colors">Message</button>
        </div>
      </div>
      <div class="bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 rounded-2xl p-5">
        <div class="flex items-center gap-3 mb-4">
          <div class="w-12 h-12 bg-gradient-to-br from-green-400 to-teal-500 rounded-xl flex items-center justify-center text-white font-bold">VP</div>
          <div>
            <p class="font-bold text-gray-900 dark:text-white text-sm">วิชัย</p>
            <p class="text-xs text-gray-500 dark:text-gray-400">Designer</p>
          </div>
        </div>
        <div class="flex gap-2">
          <button class="flex-1 bg-gray-100 dark:bg-gray-700 text-gray-700 dark:text-gray-300 py-2 rounded-xl text-xs font-semibold hover:bg-gray-200 dark:hover:bg-gray-600 transition-colors">Following</button>
          <button class="flex-1 border border-gray-200 dark:border-gray-600 text-gray-700 dark:text-gray-300 py-2 rounded-xl text-xs font-semibold hover:bg-gray-50 dark:hover:bg-gray-700 transition-colors">Message</button>
        </div>
      </div>
      <div class="bg-gradient-to-br from-indigo-600 to-purple-600 rounded-2xl p-5 text-white">
        <p class="font-bold mb-1">Pro Plan</p>
        <p class="text-xs text-white/70 mb-4">เข้าถึง features ทั้งหมด</p>
        <button class="w-full bg-white text-indigo-700 py-2 rounded-xl text-xs font-bold hover:bg-white/90 transition-colors">Upgrade</button>
      </div>
    </section>

  </main>

  <!-- Dark modal -->
  <div id="dark-modal" class="hidden fixed inset-0 bg-black/60 z-50 flex items-center justify-center p-4">
    <div class="bg-white dark:bg-gray-800 rounded-2xl shadow-2xl w-full max-w-md border border-gray-200 dark:border-gray-700">
      <div class="flex items-center justify-between p-5 border-b border-gray-200 dark:border-gray-700">
        <h3 class="font-extrabold text-gray-900 dark:text-white">Dark Mode Modal</h3>
        <button onclick="document.getElementById('dark-modal').classList.add('hidden')" class="text-gray-400 hover:text-gray-600 dark:hover:text-gray-300 transition-colors">✕</button>
      </div>
      <div class="p-5">
        <p class="text-sm text-gray-600 dark:text-gray-300">Modal ที่รองรับ dark mode โดยใช้ dark: prefix</p>
      </div>
      <div class="flex gap-3 p-5 border-t border-gray-200 dark:border-gray-700">
        <button onclick="document.getElementById('dark-modal').classList.add('hidden')" class="flex-1 border border-gray-200 dark:border-gray-600 text-gray-700 dark:text-gray-300 py-2.5 rounded-xl text-sm font-semibold hover:bg-gray-50 dark:hover:bg-gray-700 transition-colors">ยกเลิก</button>
        <button onclick="document.getElementById('dark-modal').classList.add('hidden')" class="flex-1 bg-indigo-600 text-white py-2.5 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">ยืนยัน</button>
      </div>
    </div>
  </div>

  <script>
    function toggleDark() {
      const isDark = document.documentElement.classList.toggle('dark');
      localStorage.theme = isDark ? 'dark' : 'light';
      document.getElementById('toggle-knob').textContent = isDark ? '☀️' : '🌙';
    }
    // sync knob icon on load
    if (document.documentElement.classList.contains('dark')) {
      document.getElementById('toggle-knob').textContent = '☀️';
    }
  </script>

</body>
</html>
```

---

## Step 480: Workshop — Full Dark Mode Dashboard

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Dark Dashboard - Step 480</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    if (localStorage.theme === 'dark' || (!('theme' in localStorage) && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
      document.documentElement.classList.add('dark');
    }
  </script>
</head>
<body class="bg-gray-100 dark:bg-gray-950 text-gray-900 dark:text-white transition-colors duration-300 min-h-screen">

  <div class="flex h-screen overflow-hidden">

    <!-- Sidebar -->
    <aside class="w-56 flex-shrink-0 bg-white dark:bg-gray-900 border-r border-gray-200 dark:border-gray-800 flex flex-col">
      <div class="p-4 border-b border-gray-200 dark:border-gray-800">
        <div class="flex items-center gap-2.5">
          <div class="w-8 h-8 bg-indigo-600 rounded-lg flex items-center justify-center text-white font-bold text-sm">D</div>
          <div>
            <p class="font-extrabold text-sm text-gray-900 dark:text-white">DarkDash</p>
            <p class="text-[10px] text-gray-500 dark:text-gray-400">v2.0</p>
          </div>
        </div>
      </div>

      <nav class="flex-1 p-3 space-y-0.5 overflow-y-auto">
        <p class="text-[10px] font-bold text-gray-400 dark:text-gray-500 uppercase tracking-wider px-2 pt-1 pb-1">Main</p>
        <a href="#" class="flex items-center gap-2.5 px-2 py-2 rounded-xl bg-indigo-50 dark:bg-indigo-900/40 text-indigo-700 dark:text-indigo-300 text-xs font-semibold">🏠 <span>Dashboard</span></a>
        <a href="#" class="flex items-center gap-2.5 px-2 py-2 rounded-xl hover:bg-gray-100 dark:hover:bg-gray-700/50 text-gray-600 dark:text-gray-400 text-xs transition-colors">📊 <span>Analytics</span></a>
        <a href="#" class="flex items-center gap-2.5 px-2 py-2 rounded-xl hover:bg-gray-100 dark:hover:bg-gray-700/50 text-gray-600 dark:text-gray-400 text-xs transition-colors">👥 <span>Users</span></a>
        <a href="#" class="flex items-center gap-2.5 px-2 py-2 rounded-xl hover:bg-gray-100 dark:hover:bg-gray-700/50 text-gray-600 dark:text-gray-400 text-xs transition-colors">📦 <span>Products</span></a>
        <a href="#" class="flex items-center gap-2.5 px-2 py-2 rounded-xl hover:bg-gray-100 dark:hover:bg-gray-700/50 text-gray-600 dark:text-gray-400 text-xs transition-colors">💳 <span>Billing</span></a>
        <p class="text-[10px] font-bold text-gray-400 dark:text-gray-500 uppercase tracking-wider px-2 pt-3 pb-1">System</p>
        <a href="#" class="flex items-center gap-2.5 px-2 py-2 rounded-xl hover:bg-gray-100 dark:hover:bg-gray-700/50 text-gray-600 dark:text-gray-400 text-xs transition-colors">⚙️ <span>Settings</span></a>
      </nav>

      <div class="p-3 border-t border-gray-200 dark:border-gray-800">
        <div class="flex items-center gap-2">
          <div class="w-8 h-8 bg-gradient-to-br from-indigo-400 to-purple-500 rounded-lg flex items-center justify-center text-white text-xs font-bold flex-shrink-0">SK</div>
          <div class="flex-1 min-w-0">
            <p class="text-xs font-semibold text-gray-900 dark:text-white truncate">สมชาย เทคโน</p>
            <p class="text-[10px] text-gray-500 dark:text-gray-400 truncate">admin@tech.co</p>
          </div>
          <!-- Dark toggle here -->
          <button onclick="toggleDark()" class="w-7 h-7 flex-shrink-0 flex items-center justify-center rounded-lg hover:bg-gray-100 dark:hover:bg-gray-700 transition-colors" id="side-dark-btn">
            <span id="side-dark-icon" class="text-sm">🌙</span>
          </button>
        </div>
      </div>
    </aside>

    <!-- Content -->
    <div class="flex-1 flex flex-col overflow-hidden">
      <!-- Top bar -->
      <header class="bg-white dark:bg-gray-900 border-b border-gray-200 dark:border-gray-800 px-6 h-14 flex items-center justify-between flex-shrink-0">
        <div>
          <p class="font-bold text-sm text-gray-900 dark:text-white">Dashboard</p>
          <p class="text-xs text-gray-500 dark:text-gray-400">ยินดีต้อนรับกลับ, สมชาย 👋</p>
        </div>
        <div class="flex items-center gap-3">
          <button class="w-9 h-9 bg-gray-100 dark:bg-gray-800 rounded-xl flex items-center justify-center text-sm hover:bg-gray-200 dark:hover:bg-gray-700 transition-colors relative">
            🔔
            <span class="absolute top-1 right-1 w-2 h-2 bg-red-500 rounded-full"></span>
          </button>
          <button class="bg-indigo-600 text-white px-3 py-1.5 rounded-xl text-xs font-semibold hover:bg-indigo-700 transition-colors">+ New</button>
        </div>
      </header>

      <!-- Scrollable content -->
      <main class="flex-1 overflow-y-auto p-6 space-y-6">

        <!-- KPIs -->
        <div class="grid grid-cols-2 xl:grid-cols-4 gap-4">
          <div class="bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 rounded-2xl p-5">
            <div class="flex items-center justify-between mb-3">
              <p class="text-xs font-semibold text-gray-500 dark:text-gray-400">ยอดขายวันนี้</p>
              <span class="text-lg">💰</span>
            </div>
            <p class="text-2xl font-extrabold text-gray-900 dark:text-white">฿48,250</p>
            <p class="text-xs text-green-500 mt-1">▲ 12% จากเมื่อวาน</p>
          </div>
          <div class="bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 rounded-2xl p-5">
            <div class="flex items-center justify-between mb-3">
              <p class="text-xs font-semibold text-gray-500 dark:text-gray-400">ผู้ใช้ใหม่</p>
              <span class="text-lg">👥</span>
            </div>
            <p class="text-2xl font-extrabold text-gray-900 dark:text-white">284</p>
            <p class="text-xs text-green-500 mt-1">▲ 8% สัปดาห์นี้</p>
          </div>
          <div class="bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 rounded-2xl p-5">
            <div class="flex items-center justify-between mb-3">
              <p class="text-xs font-semibold text-gray-500 dark:text-gray-400">Conversion</p>
              <span class="text-lg">📈</span>
            </div>
            <p class="text-2xl font-extrabold text-gray-900 dark:text-white">3.2%</p>
            <p class="text-xs text-red-500 mt-1">▼ 0.5% จากเมื่อวาน</p>
          </div>
          <div class="bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 rounded-2xl p-5">
            <div class="flex items-center justify-between mb-3">
              <p class="text-xs font-semibold text-gray-500 dark:text-gray-400">Tickets เปิด</p>
              <span class="text-lg">🎫</span>
            </div>
            <p class="text-2xl font-extrabold text-gray-900 dark:text-white">17</p>
            <p class="text-xs text-yellow-500 mt-1">3 urgent</p>
          </div>
        </div>

        <!-- Chart + Activity -->
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
          <!-- Bar chart -->
          <div class="lg:col-span-2 bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 rounded-2xl p-5">
            <div class="flex items-center justify-between mb-4">
              <p class="font-bold text-sm text-gray-900 dark:text-white">ยอดขายรายเดือน</p>
              <select class="text-xs border border-gray-200 dark:border-gray-600 bg-white dark:bg-gray-700 text-gray-600 dark:text-gray-300 rounded-lg px-2 py-1 focus:outline-none">
                <option>2024</option>
                <option>2023</option>
              </select>
            </div>
            <div id="dark-chart" class="flex items-end gap-1.5 h-32"></div>
            <div class="flex justify-between text-[10px] text-gray-400 dark:text-gray-500 mt-2">
              <span>ม.ค.</span><span>ก.พ.</span><span>มี.ค.</span><span>เม.ย.</span><span>พ.ค.</span><span>มิ.ย.</span>
              <span>ก.ค.</span><span>ส.ค.</span><span>ก.ย.</span><span>ต.ค.</span><span>พ.ย.</span><span>ธ.ค.</span>
            </div>
          </div>

          <!-- Activity feed -->
          <div class="bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 rounded-2xl p-5">
            <p class="font-bold text-sm text-gray-900 dark:text-white mb-4">กิจกรรมล่าสุด</p>
            <div class="space-y-3" id="activity-feed">
              <div class="flex gap-2 text-xs">
                <div class="w-6 h-6 bg-green-100 dark:bg-green-900/30 rounded-full flex items-center justify-center text-sm flex-shrink-0">✅</div>
                <div><p class="font-medium text-gray-900 dark:text-white">Order #1024 completed</p><p class="text-gray-400 dark:text-gray-500">2 นาทีที่แล้ว</p></div>
              </div>
              <div class="flex gap-2 text-xs">
                <div class="w-6 h-6 bg-blue-100 dark:bg-blue-900/30 rounded-full flex items-center justify-center text-sm flex-shrink-0">👤</div>
                <div><p class="font-medium text-gray-900 dark:text-white">ผู้ใช้ใหม่ลงทะเบียน</p><p class="text-gray-400 dark:text-gray-500">5 นาทีที่แล้ว</p></div>
              </div>
              <div class="flex gap-2 text-xs">
                <div class="w-6 h-6 bg-yellow-100 dark:bg-yellow-900/30 rounded-full flex items-center justify-center text-sm flex-shrink-0">⚠️</div>
                <div><p class="font-medium text-gray-900 dark:text-white">Server load สูง</p><p class="text-gray-400 dark:text-gray-500">12 นาทีที่แล้ว</p></div>
              </div>
            </div>
          </div>
        </div>

        <!-- Users table -->
        <div class="bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 rounded-2xl overflow-hidden">
          <div class="px-5 py-4 border-b border-gray-200 dark:border-gray-700 flex items-center justify-between">
            <p class="font-bold text-sm text-gray-900 dark:text-white">ผู้ใช้ล่าสุด</p>
            <button class="text-xs text-indigo-600 dark:text-indigo-400 hover:underline">ดูทั้งหมด</button>
          </div>
          <table class="w-full text-sm">
            <thead class="bg-gray-50 dark:bg-gray-700/50">
              <tr>
                <th class="px-5 py-3 text-left text-xs font-semibold text-gray-500 dark:text-gray-400">ผู้ใช้</th>
                <th class="px-5 py-3 text-left text-xs font-semibold text-gray-500 dark:text-gray-400 hidden md:table-cell">อีเมล</th>
                <th class="px-5 py-3 text-left text-xs font-semibold text-gray-500 dark:text-gray-400">Plan</th>
                <th class="px-5 py-3 text-left text-xs font-semibold text-gray-500 dark:text-gray-400">สถานะ</th>
              </tr>
            </thead>
            <tbody class="divide-y divide-gray-200 dark:divide-gray-700">
              <tr class="hover:bg-gray-50 dark:hover:bg-gray-700/30 transition-colors">
                <td class="px-5 py-3"><div class="flex items-center gap-2"><div class="w-7 h-7 bg-indigo-100 dark:bg-indigo-900/40 rounded-lg flex items-center justify-center text-xs font-bold text-indigo-600 dark:text-indigo-400">SK</div><span class="font-medium text-gray-900 dark:text-white text-xs">สมชาย</span></div></td>
                <td class="px-5 py-3 text-xs text-gray-500 dark:text-gray-400 hidden md:table-cell">somchai@tech.co</td>
                <td class="px-5 py-3"><span class="bg-indigo-100 dark:bg-indigo-900/30 text-indigo-700 dark:text-indigo-300 text-xs px-2 py-0.5 rounded-full font-medium">Pro</span></td>
                <td class="px-5 py-3"><span class="bg-green-100 dark:bg-green-900/30 text-green-700 dark:text-green-400 text-xs px-2 py-0.5 rounded-full">Active</span></td>
              </tr>
              <tr class="hover:bg-gray-50 dark:hover:bg-gray-700/30 transition-colors">
                <td class="px-5 py-3"><div class="flex items-center gap-2"><div class="w-7 h-7 bg-green-100 dark:bg-green-900/40 rounded-lg flex items-center justify-center text-xs font-bold text-green-600 dark:text-green-400">VP</div><span class="font-medium text-gray-900 dark:text-white text-xs">วิชัย</span></div></td>
                <td class="px-5 py-3 text-xs text-gray-500 dark:text-gray-400 hidden md:table-cell">wichai@email.com</td>
                <td class="px-5 py-3"><span class="bg-gray-100 dark:bg-gray-700 text-gray-600 dark:text-gray-300 text-xs px-2 py-0.5 rounded-full font-medium">Free</span></td>
                <td class="px-5 py-3"><span class="bg-yellow-100 dark:bg-yellow-900/30 text-yellow-700 dark:text-yellow-400 text-xs px-2 py-0.5 rounded-full">Trial</span></td>
              </tr>
            </tbody>
          </table>
        </div>

      </main>
    </div>
  </div>

  <script>
    // Dark toggle
    function toggleDark() {
      const isDark = document.documentElement.classList.toggle('dark');
      localStorage.theme = isDark ? 'dark' : 'light';
      syncIcon();
    }
    function syncIcon() {
      const isDark = document.documentElement.classList.contains('dark');
      document.getElementById('side-dark-icon').textContent = isDark ? '☀️' : '🌙';
    }
    syncIcon();

    // Build chart
    const months = [62,78,55,90,84,95,72,88,76,100,68,92];
    const max = Math.max(...months);
    const chart = document.getElementById('dark-chart');
    months.forEach((v, i) => {
      const bar = document.createElement('div');
      const isHighest = v === max;
      bar.className = `flex-1 rounded-t-sm cursor-pointer hover:opacity-80 transition-opacity ${isHighest ? 'bg-indigo-600' : 'bg-indigo-200 dark:bg-indigo-800'}`;
      bar.style.height = (v / max * 100) + '%';
      bar.title = `${v}%`;
      chart.appendChild(bar);
    });
  </script>

</body>
</html>
```

---

## สรุป Part 48

| Step | เนื้อหา |
|------|---------|
| 471 | Dark mode basics: `dark:` prefix, localStorage toggle, no-flash script |
| 472 | Buttons dark mode variants |
| 473 | Sidebar + content dark mode |
| 474 | Code block dark |
| 475 | Alert components dark |
| 476 | Stats cards dark |
| 477 | Modal dark mode |
| 478 | Profile cards dark |
| 479 | Component patterns: `dark:bg-gray-800`, `dark:border-gray-700`, `dark:text-gray-300` |
| 480 | Workshop: Full dark mode dashboard with toggle, chart, and table |

**Part ถัดไป:** Part 49 — Custom Tailwind Config & Theming (Steps 481–490)
