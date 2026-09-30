# Part 25: Dashboard Layout
## Steps 241–250: สร้าง Dashboard Layout ระดับมืออาชีพ

---

## 🎯 เป้าหมายของ Part นี้

- Fixed sidebar + main content layout
- Collapsible sidebar
- Sticky topbar
- Responsive dashboard grid
- Stat cards, charts placeholders
- Scrollable content area

---

## Step 241: Dashboard Shell Layout

```html
<!-- Structure: sidebar-left + topbar + content -->
<div class="flex h-screen bg-gray-50 overflow-hidden">
  <!-- Sidebar -->
  <aside id="sidebar" class="w-64 flex-none bg-white border-r border-gray-200 flex flex-col transition-all duration-300">
    <!-- Logo -->
    <div class="h-16 flex items-center px-5 border-b border-gray-100">
      <div class="flex items-center gap-2.5">
        <div class="size-8 rounded-xl bg-indigo-600 flex items-center justify-center">
          <span class="text-white font-black text-sm">T</span>
        </div>
        <span class="font-bold text-gray-900">TailwindPro</span>
      </div>
    </div>
    <!-- Nav will go here -->
    <nav class="flex-1 overflow-y-auto p-3 space-y-1">
    </nav>
    <!-- User area -->
    <div class="p-3 border-t border-gray-100">
      <div class="flex items-center gap-3 px-3 py-2 rounded-xl hover:bg-gray-100 cursor-pointer transition-colors">
        <div class="size-8 rounded-full bg-gradient-to-br from-indigo-400 to-purple-500 flex-none"></div>
        <div class="flex-1 min-w-0">
          <p class="font-medium text-sm text-gray-900 truncate">สมชาย ใจดี</p>
          <p class="text-xs text-gray-400 truncate">admin@example.com</p>
        </div>
        <span class="text-gray-300 text-sm">⋯</span>
      </div>
    </div>
  </aside>

  <!-- Main Column -->
  <div class="flex-1 flex flex-col overflow-hidden">
    <!-- Topbar -->
    <header class="h-16 bg-white border-b border-gray-200 flex items-center px-6 gap-4 flex-none">
      <button onclick="document.getElementById('sidebar').classList.toggle('w-64'); document.getElementById('sidebar').classList.toggle('w-0')" class="text-gray-500 hover:text-gray-900 transition-colors">☰</button>
      <div class="flex-1 relative max-w-sm">
        <span class="absolute left-3 top-1/2 -translate-y-1/2 text-gray-400 text-sm">🔍</span>
        <input class="w-full pl-9 pr-4 py-2 text-sm bg-gray-100 rounded-xl border-0 focus:outline-none focus:ring-2 focus:ring-indigo-200 placeholder:text-gray-400" placeholder="ค้นหา...">
      </div>
      <div class="ml-auto flex items-center gap-3">
        <button class="relative size-9 flex items-center justify-center text-gray-500 hover:text-gray-900 hover:bg-gray-100 rounded-xl transition-colors">
          🔔
          <span class="absolute top-1.5 right-1.5 size-2 bg-red-500 rounded-full"></span>
        </button>
        <div class="size-9 rounded-full bg-gradient-to-br from-indigo-400 to-purple-500 cursor-pointer"></div>
      </div>
    </header>

    <!-- Scrollable Content -->
    <main class="flex-1 overflow-y-auto p-6">
      <!-- content here -->
    </main>
  </div>
</div>
```

---

## Step 242: Sidebar Navigation Items

```html
<!-- Sidebar nav items with active state -->
<nav class="flex-1 overflow-y-auto p-3 space-y-0.5">
  <!-- Section label -->
  <p class="px-3 py-2 text-[11px] font-semibold tracking-widest text-gray-400 uppercase">หลัก</p>
  
  <!-- Nav Item — Active -->
  <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-xl bg-indigo-50 text-indigo-600 font-medium text-sm">
    <span class="text-base">🏠</span>
    <span>Dashboard</span>
    <span class="ml-auto text-xs bg-indigo-100 text-indigo-600 px-2 py-0.5 rounded-full font-bold">3</span>
  </a>
  
  <!-- Nav Item — Normal -->
  <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-xl hover:bg-gray-100 text-gray-600 hover:text-gray-900 font-medium text-sm transition-colors">
    <span class="text-base">📚</span>
    <span>คอร์สเรียน</span>
  </a>
  
  <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-xl hover:bg-gray-100 text-gray-600 hover:text-gray-900 font-medium text-sm transition-colors">
    <span class="text-base">👥</span>
    <span>นักเรียน</span>
  </a>
  
  <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-xl hover:bg-gray-100 text-gray-600 hover:text-gray-900 font-medium text-sm transition-colors">
    <span class="text-base">📊</span>
    <span>รายงาน</span>
  </a>

  <p class="px-3 py-2 mt-3 text-[11px] font-semibold tracking-widest text-gray-400 uppercase">การตั้งค่า</p>
  
  <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-xl hover:bg-gray-100 text-gray-600 hover:text-gray-900 font-medium text-sm transition-colors">
    <span class="text-base">⚙️</span>
    <span>ตั้งค่าระบบ</span>
  </a>
  
  <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-xl hover:bg-gray-100 text-gray-600 hover:text-gray-900 font-medium text-sm transition-colors">
    <span class="text-base">🔐</span>
    <span>ความปลอดภัย</span>
  </a>

  <!-- Collapsible Group -->
  <div>
    <button 
      onclick="const c = this.nextElementSibling; c.style.display = c.style.display === 'none' ? 'block' : 'none'"
      class="w-full flex items-center gap-3 px-3 py-2.5 rounded-xl hover:bg-gray-100 text-gray-600 hover:text-gray-900 font-medium text-sm transition-colors"
    >
      <span class="text-base">🧩</span>
      <span class="flex-1 text-left">ปลั๊กอิน</span>
      <span class="text-xs text-gray-400">▼</span>
    </button>
    <div class="ml-9 mt-0.5 space-y-0.5">
      <a href="#" class="block px-3 py-2 rounded-xl text-sm text-gray-500 hover:text-gray-900 hover:bg-gray-100 transition-colors">Analytics</a>
      <a href="#" class="block px-3 py-2 rounded-xl text-sm text-gray-500 hover:text-gray-900 hover:bg-gray-100 transition-colors">SEO Tools</a>
    </div>
  </div>
</nav>
```

---

## Step 243: KPI / Stat Cards Grid

```html
<!-- Stats overview grid -->
<div class="grid grid-cols-1 sm:grid-cols-2 xl:grid-cols-4 gap-5 mb-6">

  <!-- Card 1: Revenue -->
  <div class="bg-white rounded-2xl p-5 border border-gray-200 relative overflow-hidden">
    <div class="absolute inset-0 bg-gradient-to-br from-indigo-50 to-transparent opacity-60"></div>
    <div class="relative">
      <div class="flex items-start justify-between">
        <div>
          <p class="text-xs font-medium text-gray-500 mb-1">รายได้รวม</p>
          <p class="text-2xl font-black text-gray-900">฿284,500</p>
        </div>
        <span class="text-2xl">💰</span>
      </div>
      <div class="mt-3 flex items-center gap-2">
        <span class="text-xs font-semibold text-green-600 bg-green-50 px-2 py-0.5 rounded-full">+12.5%</span>
        <span class="text-xs text-gray-400">vs เดือนก่อน</span>
      </div>
    </div>
  </div>

  <!-- Card 2: Students -->
  <div class="bg-white rounded-2xl p-5 border border-gray-200 relative overflow-hidden">
    <div class="absolute inset-0 bg-gradient-to-br from-purple-50 to-transparent opacity-60"></div>
    <div class="relative">
      <div class="flex items-start justify-between">
        <div>
          <p class="text-xs font-medium text-gray-500 mb-1">นักเรียน</p>
          <p class="text-2xl font-black text-gray-900">1,247</p>
        </div>
        <span class="text-2xl">🎓</span>
      </div>
      <div class="mt-3 flex items-center gap-2">
        <span class="text-xs font-semibold text-green-600 bg-green-50 px-2 py-0.5 rounded-full">+8.3%</span>
        <span class="text-xs text-gray-400">vs เดือนก่อน</span>
      </div>
    </div>
  </div>

  <!-- Card 3: Courses -->
  <div class="bg-white rounded-2xl p-5 border border-gray-200 relative overflow-hidden">
    <div class="absolute inset-0 bg-gradient-to-br from-amber-50 to-transparent opacity-60"></div>
    <div class="relative">
      <div class="flex items-start justify-between">
        <div>
          <p class="text-xs font-medium text-gray-500 mb-1">คอร์สทั้งหมด</p>
          <p class="text-2xl font-black text-gray-900">48</p>
        </div>
        <span class="text-2xl">📚</span>
      </div>
      <div class="mt-3 flex items-center gap-2">
        <span class="text-xs font-semibold text-indigo-600 bg-indigo-50 px-2 py-0.5 rounded-full">+3 ใหม่</span>
        <span class="text-xs text-gray-400">เดือนนี้</span>
      </div>
    </div>
  </div>

  <!-- Card 4: Completion Rate -->
  <div class="bg-white rounded-2xl p-5 border border-gray-200 relative overflow-hidden">
    <div class="absolute inset-0 bg-gradient-to-br from-green-50 to-transparent opacity-60"></div>
    <div class="relative">
      <div class="flex items-start justify-between">
        <div>
          <p class="text-xs font-medium text-gray-500 mb-1">อัตราจบ</p>
          <p class="text-2xl font-black text-gray-900">73%</p>
        </div>
        <span class="text-2xl">✅</span>
      </div>
      <div class="mt-3">
        <div class="h-1.5 bg-gray-100 rounded-full overflow-hidden">
          <div class="h-full w-[73%] bg-green-500 rounded-full"></div>
        </div>
      </div>
    </div>
  </div>

</div>
```

---

## Step 244: Chart Placeholder + Recent Activity

```html
<!-- Charts + Activity 2-col layout -->
<div class="grid grid-cols-1 xl:grid-cols-3 gap-5 mb-6">

  <!-- Chart placeholder (2 cols) -->
  <div class="xl:col-span-2 bg-white rounded-2xl border border-gray-200 p-5">
    <div class="flex items-center justify-between mb-5">
      <div>
        <h3 class="font-bold text-gray-900">รายได้รายเดือน</h3>
        <p class="text-xs text-gray-400">6 เดือนล่าสุด</p>
      </div>
      <select class="text-xs border border-gray-200 rounded-lg px-3 py-1.5 text-gray-500 focus:outline-none focus:ring-2 focus:ring-indigo-200">
        <option>6 เดือน</option>
        <option>12 เดือน</option>
      </select>
    </div>
    
    <!-- Bar chart (CSS) -->
    <div class="flex items-end gap-3 h-40">
      <div class="flex-1 flex flex-col items-center gap-1">
        <div class="w-full bg-indigo-200 rounded-t-lg" style="height:60%"></div>
        <span class="text-[10px] text-gray-400">เม.ย.</span>
      </div>
      <div class="flex-1 flex flex-col items-center gap-1">
        <div class="w-full bg-indigo-200 rounded-t-lg" style="height:75%"></div>
        <span class="text-[10px] text-gray-400">พ.ค.</span>
      </div>
      <div class="flex-1 flex flex-col items-center gap-1">
        <div class="w-full bg-indigo-200 rounded-t-lg" style="height:50%"></div>
        <span class="text-[10px] text-gray-400">มิ.ย.</span>
      </div>
      <div class="flex-1 flex flex-col items-center gap-1">
        <div class="w-full bg-indigo-400 rounded-t-lg" style="height:85%"></div>
        <span class="text-[10px] text-gray-400">ก.ค.</span>
      </div>
      <div class="flex-1 flex flex-col items-center gap-1">
        <div class="w-full bg-indigo-500 rounded-t-lg" style="height:70%"></div>
        <span class="text-[10px] text-gray-400">ส.ค.</span>
      </div>
      <div class="flex-1 flex flex-col items-center gap-1">
        <div class="w-full bg-indigo-700 rounded-t-lg" style="height:100%"></div>
        <span class="text-[10px] text-gray-400">ก.ย.</span>
      </div>
    </div>
  </div>

  <!-- Recent activity (1 col) -->
  <div class="bg-white rounded-2xl border border-gray-200 p-5">
    <h3 class="font-bold text-gray-900 mb-4">กิจกรรมล่าสุด</h3>
    <div class="space-y-4">
      <div class="flex gap-3">
        <div class="size-8 rounded-full bg-green-100 flex items-center justify-center text-sm flex-none">✅</div>
        <div class="flex-1 min-w-0">
          <p class="text-xs font-medium text-gray-900 truncate">สมใจ จบ Part 20</p>
          <p class="text-[11px] text-gray-400">2 นาทีที่แล้ว</p>
        </div>
      </div>
      <div class="flex gap-3">
        <div class="size-8 rounded-full bg-blue-100 flex items-center justify-center text-sm flex-none">🆕</div>
        <div class="flex-1 min-w-0">
          <p class="text-xs font-medium text-gray-900 truncate">มีผู้เรียนใหม่ลงทะเบียน</p>
          <p class="text-[11px] text-gray-400">15 นาทีที่แล้ว</p>
        </div>
      </div>
      <div class="flex gap-3">
        <div class="size-8 rounded-full bg-yellow-100 flex items-center justify-center text-sm flex-none">⭐</div>
        <div class="flex-1 min-w-0">
          <p class="text-xs font-medium text-gray-900 truncate">รีวิวใหม่ 5 ดาว</p>
          <p class="text-[11px] text-gray-400">1 ชั่วโมงที่แล้ว</p>
        </div>
      </div>
      <div class="flex gap-3">
        <div class="size-8 rounded-full bg-red-100 flex items-center justify-center text-sm flex-none">💳</div>
        <div class="flex-1 min-w-0">
          <p class="text-xs font-medium text-gray-900 truncate">ชำระเงินสำเร็จ ฿2,490</p>
          <p class="text-[11px] text-gray-400">2 ชั่วโมงที่แล้ว</p>
        </div>
      </div>
    </div>
  </div>

</div>
```

---

## Step 245–250: Workshop — Full Dashboard Page

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Dashboard — TailwindPro</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50">

<div class="flex h-screen overflow-hidden">

  <!-- Sidebar -->
  <aside id="sidebar" class="w-64 flex-none bg-white border-r border-gray-200 flex flex-col transition-all duration-300 overflow-hidden">
    <div class="h-16 flex items-center px-5 border-b border-gray-100 flex-none">
      <div class="flex items-center gap-2.5">
        <div class="size-8 rounded-xl bg-gradient-to-br from-indigo-600 to-purple-600 flex items-center justify-center flex-none">
          <span class="text-white font-black text-sm">T</span>
        </div>
        <span class="font-bold text-gray-900 whitespace-nowrap">TailwindPro</span>
      </div>
    </div>
    
    <nav class="flex-1 overflow-y-auto p-3">
      <p class="px-3 py-2 text-[10px] font-bold tracking-widest text-gray-400 uppercase">หลัก</p>
      <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-xl bg-indigo-50 text-indigo-600 font-semibold text-sm mb-0.5">
        <span>🏠</span><span class="whitespace-nowrap">Dashboard</span>
        <span class="ml-auto text-xs bg-indigo-600 text-white px-1.5 py-0.5 rounded-full">3</span>
      </a>
      <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-xl hover:bg-gray-100 text-gray-600 font-medium text-sm transition-colors mb-0.5">
        <span>📚</span><span class="whitespace-nowrap">คอร์สเรียน</span>
      </a>
      <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-xl hover:bg-gray-100 text-gray-600 font-medium text-sm transition-colors mb-0.5">
        <span>👥</span><span class="whitespace-nowrap">นักเรียน</span>
      </a>
      <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-xl hover:bg-gray-100 text-gray-600 font-medium text-sm transition-colors mb-0.5">
        <span>📊</span><span class="whitespace-nowrap">รายงาน</span>
      </a>
      <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-xl hover:bg-gray-100 text-gray-600 font-medium text-sm transition-colors">
        <span>⚙️</span><span class="whitespace-nowrap">ตั้งค่า</span>
      </a>
    </nav>
    
    <div class="p-3 border-t border-gray-100">
      <div class="flex items-center gap-3 px-3 py-2 rounded-xl hover:bg-gray-100 cursor-pointer transition-colors">
        <div class="size-8 rounded-full bg-gradient-to-br from-indigo-400 to-purple-500 flex-none"></div>
        <div class="flex-1 min-w-0">
          <p class="font-semibold text-sm text-gray-900 truncate whitespace-nowrap">สมชาย ใจดี</p>
          <p class="text-xs text-gray-400 truncate whitespace-nowrap">admin@tailwindpro.co</p>
        </div>
      </div>
    </div>
  </aside>

  <!-- Main -->
  <div class="flex-1 flex flex-col overflow-hidden">
    <!-- Topbar -->
    <header class="h-16 bg-white border-b border-gray-200 flex items-center px-5 gap-4 flex-none">
      <button
        onclick="const s = document.getElementById('sidebar'); s.classList.toggle('w-64'); s.classList.toggle('w-0');"
        class="size-9 flex items-center justify-center rounded-xl hover:bg-gray-100 text-gray-500 transition-colors"
      >☰</button>
      
      <div class="flex-1 relative max-w-xs">
        <span class="absolute left-3 top-1/2 -translate-y-1/2 text-gray-400 text-xs">🔍</span>
        <input class="w-full pl-8 pr-4 py-2 text-sm bg-gray-100 rounded-xl border-0 focus:outline-none focus:ring-2 focus:ring-indigo-200" placeholder="ค้นหา...">
      </div>
      
      <div class="ml-auto flex items-center gap-2">
        <button class="relative size-9 flex items-center justify-center rounded-xl hover:bg-gray-100 text-gray-500 transition-colors text-sm">
          🔔
          <span class="absolute top-1.5 right-1.5 size-2 bg-red-500 rounded-full border-2 border-white"></span>
        </button>
        <div class="size-9 rounded-full bg-gradient-to-br from-indigo-400 to-purple-500 cursor-pointer"></div>
      </div>
    </header>

    <!-- Content -->
    <main class="flex-1 overflow-y-auto p-6">
      <!-- Header -->
      <div class="mb-6">
        <h1 class="text-2xl font-black text-gray-900">Dashboard</h1>
        <p class="text-gray-500 text-sm mt-1">ยินดีต้อนรับกลับ! นี่คือสรุปภาพรวมของระบบ</p>
      </div>

      <!-- Stats -->
      <div class="grid grid-cols-2 xl:grid-cols-4 gap-4 mb-6">
        <div class="bg-white rounded-2xl p-5 border border-gray-200">
          <p class="text-xs text-gray-400 font-medium">รายได้รวม</p>
          <p class="text-2xl font-black text-gray-900 mt-1">฿284,500</p>
          <p class="text-xs text-green-600 mt-2 font-semibold">↑ 12.5% vs เดือนก่อน</p>
        </div>
        <div class="bg-white rounded-2xl p-5 border border-gray-200">
          <p class="text-xs text-gray-400 font-medium">นักเรียน</p>
          <p class="text-2xl font-black text-gray-900 mt-1">1,247</p>
          <p class="text-xs text-green-600 mt-2 font-semibold">↑ 8.3% vs เดือนก่อน</p>
        </div>
        <div class="bg-white rounded-2xl p-5 border border-gray-200">
          <p class="text-xs text-gray-400 font-medium">คอร์สทั้งหมด</p>
          <p class="text-2xl font-black text-gray-900 mt-1">48</p>
          <p class="text-xs text-indigo-600 mt-2 font-semibold">+3 ใหม่เดือนนี้</p>
        </div>
        <div class="bg-white rounded-2xl p-5 border border-gray-200">
          <p class="text-xs text-gray-400 font-medium">อัตราจบ</p>
          <p class="text-2xl font-black text-gray-900 mt-1">73%</p>
          <div class="h-1.5 bg-gray-100 rounded-full mt-2"><div class="h-full w-[73%] bg-green-500 rounded-full"></div></div>
        </div>
      </div>

      <!-- Charts Row -->
      <div class="grid xl:grid-cols-3 gap-5 mb-6">
        <div class="xl:col-span-2 bg-white rounded-2xl border border-gray-200 p-5">
          <h3 class="font-bold text-gray-900 mb-4">รายได้ 6 เดือนล่าสุด</h3>
          <div class="flex items-end gap-3 h-32">
            <div class="flex-1 flex flex-col items-center gap-1"><div class="w-full rounded-t-md bg-indigo-200" style="height:60%"></div><span class="text-[10px] text-gray-400">เม.ย.</span></div>
            <div class="flex-1 flex flex-col items-center gap-1"><div class="w-full rounded-t-md bg-indigo-200" style="height:75%"></div><span class="text-[10px] text-gray-400">พ.ค.</span></div>
            <div class="flex-1 flex flex-col items-center gap-1"><div class="w-full rounded-t-md bg-indigo-200" style="height:50%"></div><span class="text-[10px] text-gray-400">มิ.ย.</span></div>
            <div class="flex-1 flex flex-col items-center gap-1"><div class="w-full rounded-t-md bg-indigo-400" style="height:85%"></div><span class="text-[10px] text-gray-400">ก.ค.</span></div>
            <div class="flex-1 flex flex-col items-center gap-1"><div class="w-full rounded-t-md bg-indigo-500" style="height:70%"></div><span class="text-[10px] text-gray-400">ส.ค.</span></div>
            <div class="flex-1 flex flex-col items-center gap-1"><div class="w-full rounded-t-md bg-indigo-700" style="height:100%"></div><span class="text-[10px] text-gray-400">ก.ย.</span></div>
          </div>
        </div>
        
        <div class="bg-white rounded-2xl border border-gray-200 p-5">
          <h3 class="font-bold text-gray-900 mb-4">กิจกรรมล่าสุด</h3>
          <div class="space-y-3.5">
            <div class="flex gap-3"><div class="size-8 rounded-full bg-green-100 flex items-center justify-center text-xs flex-none">✅</div><div><p class="text-xs font-medium text-gray-900">สมใจ จบ Part 20</p><p class="text-[11px] text-gray-400">2 นาทีที่แล้ว</p></div></div>
            <div class="flex gap-3"><div class="size-8 rounded-full bg-blue-100 flex items-center justify-center text-xs flex-none">🆕</div><div><p class="text-xs font-medium text-gray-900">ผู้เรียนใหม่ลงทะเบียน</p><p class="text-[11px] text-gray-400">15 นาทีที่แล้ว</p></div></div>
            <div class="flex gap-3"><div class="size-8 rounded-full bg-yellow-100 flex items-center justify-center text-xs flex-none">⭐</div><div><p class="text-xs font-medium text-gray-900">รีวิวใหม่ 5 ดาว</p><p class="text-[11px] text-gray-400">1 ชั่วโมงที่แล้ว</p></div></div>
            <div class="flex gap-3"><div class="size-8 rounded-full bg-red-100 flex items-center justify-center text-xs flex-none">💳</div><div><p class="text-xs font-medium text-gray-900">ชำระเงิน ฿2,490</p><p class="text-[11px] text-gray-400">2 ชั่วโมงที่แล้ว</p></div></div>
          </div>
        </div>
      </div>

      <!-- Recent Courses Table -->
      <div class="bg-white rounded-2xl border border-gray-200 p-5">
        <div class="flex items-center justify-between mb-4">
          <h3 class="font-bold text-gray-900">คอร์สยอดนิยม</h3>
          <a href="#" class="text-indigo-600 text-xs font-medium hover:text-indigo-800">ดูทั้งหมด →</a>
        </div>
        <div class="overflow-x-auto">
          <table class="w-full text-sm">
            <thead>
              <tr class="border-b border-gray-100">
                <th class="text-left py-3 px-3 text-xs text-gray-400 font-semibold">คอร์ส</th>
                <th class="text-right py-3 px-3 text-xs text-gray-400 font-semibold">นักเรียน</th>
                <th class="text-right py-3 px-3 text-xs text-gray-400 font-semibold">รายได้</th>
                <th class="text-center py-3 px-3 text-xs text-gray-400 font-semibold">Status</th>
              </tr>
            </thead>
            <tbody class="divide-y divide-gray-50">
              <tr class="hover:bg-gray-50 transition-colors">
                <td class="py-3.5 px-3"><span class="font-medium text-gray-900 text-xs">Tailwind CSS Mastery</span></td>
                <td class="py-3.5 px-3 text-right text-xs text-gray-600">842</td>
                <td class="py-3.5 px-3 text-right text-xs text-gray-900 font-semibold">฿125,800</td>
                <td class="py-3.5 px-3 text-center"><span class="text-[11px] bg-green-100 text-green-700 px-2.5 py-1 rounded-full font-semibold">เผยแพร่</span></td>
              </tr>
              <tr class="hover:bg-gray-50 transition-colors">
                <td class="py-3.5 px-3"><span class="font-medium text-gray-900 text-xs">React with Tailwind</span></td>
                <td class="py-3.5 px-3 text-right text-xs text-gray-600">405</td>
                <td class="py-3.5 px-3 text-right text-xs text-gray-900 font-semibold">฿86,250</td>
                <td class="py-3.5 px-3 text-center"><span class="text-[11px] bg-green-100 text-green-700 px-2.5 py-1 rounded-full font-semibold">เผยแพร่</span></td>
              </tr>
              <tr class="hover:bg-gray-50 transition-colors">
                <td class="py-3.5 px-3"><span class="font-medium text-gray-900 text-xs">Design System Pro</span></td>
                <td class="py-3.5 px-3 text-right text-xs text-gray-600">0</td>
                <td class="py-3.5 px-3 text-right text-xs text-gray-900 font-semibold">—</td>
                <td class="py-3.5 px-3 text-center"><span class="text-[11px] bg-yellow-100 text-yellow-700 px-2.5 py-1 rounded-full font-semibold">ร่าง</span></td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </main>
  </div>

</div>

</body>
</html>
```

---

## 📝 สรุป Part 25

| Concept | Class ที่ใช้ |
|---------|------------|
| Layout shell | `flex h-screen overflow-hidden` |
| Sidebar toggle | `transition-all w-64 / w-0` |
| Topbar | `h-16 flex items-center sticky` |
| Scrollable content | `flex-1 overflow-y-auto` |
| Stat cards | `grid grid-cols-2 xl:grid-cols-4` |

---

*Part 25 — Dashboard Layout | Steps 241–250 จาก 1,000 Steps*
