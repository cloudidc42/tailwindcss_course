# Part 42: Kanban Board & CRM

## เป้าหมาย
- สร้าง Kanban Board แบบ Drag-and-Drop UI
- CRM Pipeline, Lead Cards, Contact Detail
- Steps 411–420

---

## Step 411: Kanban Board Layout

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Kanban Board - Step 411</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 text-gray-900 h-screen overflow-hidden flex flex-col">

  <!-- Header -->
  <header class="bg-white border-b px-6 h-14 flex items-center justify-between flex-shrink-0">
    <div class="flex items-center gap-3">
      <h1 class="font-extrabold text-lg">Sprint 12 · TechShop</h1>
      <span class="text-xs bg-indigo-100 text-indigo-700 font-semibold px-2.5 py-1 rounded-full">20 งาน</span>
    </div>
    <div class="flex items-center gap-2">
      <div class="flex -space-x-2">
        <img src="https://i.pravatar.cc/28?img=1" class="w-7 h-7 rounded-full border-2 border-white" alt="">
        <img src="https://i.pravatar.cc/28?img=2" class="w-7 h-7 rounded-full border-2 border-white" alt="">
        <img src="https://i.pravatar.cc/28?img=3" class="w-7 h-7 rounded-full border-2 border-white" alt="">
        <div class="w-7 h-7 rounded-full border-2 border-white bg-gray-200 flex items-center justify-center text-[10px] font-bold text-gray-600">+4</div>
      </div>
      <button class="bg-indigo-600 text-white text-sm px-3 py-1.5 rounded-lg hover:bg-indigo-700 transition-colors">+ งานใหม่</button>
    </div>
  </header>

  <!-- Kanban Columns -->
  <div class="flex-1 overflow-x-auto p-5">
    <div class="flex gap-4 h-full min-w-max">

      <!-- Backlog -->
      <div class="w-72 flex flex-col bg-gray-200/50 rounded-2xl overflow-hidden flex-shrink-0">
        <div class="flex items-center justify-between px-4 py-3 bg-gray-200">
          <div class="flex items-center gap-2">
            <div class="w-2.5 h-2.5 bg-gray-500 rounded-full"></div>
            <span class="text-sm font-bold">Backlog</span>
            <span class="text-xs bg-gray-300 text-gray-600 font-semibold px-1.5 py-0.5 rounded-full">6</span>
          </div>
          <button class="text-gray-500 hover:text-gray-700 transition-colors text-sm">+</button>
        </div>
        <div class="flex-1 overflow-y-auto p-3 space-y-2.5" id="col-backlog">
          <!-- Cards -->
          <div class="bg-white rounded-xl p-3.5 shadow-sm border border-gray-100 cursor-pointer hover:shadow-md transition-shadow group">
            <div class="flex items-center gap-2 mb-2">
              <span class="bg-purple-100 text-purple-700 text-[10px] font-bold px-1.5 py-0.5 rounded">Feature</span>
              <span class="ml-auto text-[10px] text-gray-400 group-hover:text-gray-600">TS-48</span>
            </div>
            <p class="text-sm font-medium">เพิ่ม Dark Mode ให้ Dashboard</p>
            <div class="flex items-center justify-between mt-3">
              <div class="flex items-center gap-1 text-[10px] text-gray-400">
                <svg class="w-3 h-3" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 12h.01M12 12h.01M16 12h.01M21 12c0 4.418-4.03 8-9 8a9.863 9.863 0 01-4.255-.949L3 20l1.395-3.72C3.512 15.042 3 13.574 3 12c0-4.418 4.03-8 9-8s9 3.582 9 8z"/></svg>
                <span>3</span>
              </div>
              <div class="flex items-center gap-1 text-[10px] text-gray-400">
                <svg class="w-3 h-3" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2"/></svg>
                <span>2/5</span>
              </div>
              <img src="https://i.pravatar.cc/20?img=2" class="w-5 h-5 rounded-full" alt="">
            </div>
          </div>

          <div class="bg-white rounded-xl p-3.5 shadow-sm border border-gray-100 cursor-pointer hover:shadow-md transition-shadow">
            <div class="flex items-center gap-2 mb-2">
              <span class="bg-red-100 text-red-700 text-[10px] font-bold px-1.5 py-0.5 rounded">Bug</span>
              <span class="bg-red-500 text-white text-[10px] font-bold px-1.5 py-0.5 rounded ml-auto">ด่วน</span>
            </div>
            <p class="text-sm font-medium">แก้ bug Cart ไม่อัปเดตจำนวน</p>
            <div class="flex items-center justify-between mt-3">
              <div class="flex items-center gap-1 text-[10px] text-gray-400"><span>🗓</span><span>15 มิ.ย.</span></div>
              <img src="https://i.pravatar.cc/20?img=1" class="w-5 h-5 rounded-full" alt="">
            </div>
          </div>

          <div class="bg-white rounded-xl p-3.5 shadow-sm border-dashed border-2 border-gray-200 text-center cursor-pointer hover:border-indigo-300 hover:bg-indigo-50 transition-all">
            <p class="text-sm text-gray-400">+ เพิ่มงานใหม่</p>
          </div>
        </div>
      </div>

      <!-- In Progress -->
      <div class="w-72 flex flex-col bg-blue-50/60 rounded-2xl overflow-hidden flex-shrink-0">
        <div class="flex items-center justify-between px-4 py-3 bg-blue-100">
          <div class="flex items-center gap-2">
            <div class="w-2.5 h-2.5 bg-blue-500 rounded-full animate-pulse"></div>
            <span class="text-sm font-bold text-blue-800">In Progress</span>
            <span class="text-xs bg-blue-200 text-blue-700 font-semibold px-1.5 py-0.5 rounded-full">4</span>
          </div>
          <button class="text-blue-500 hover:text-blue-700 transition-colors text-sm">+</button>
        </div>
        <div class="flex-1 overflow-y-auto p-3 space-y-2.5">
          <div class="bg-white rounded-xl p-3.5 shadow-sm border border-blue-100 cursor-pointer hover:shadow-md transition-shadow">
            <div class="flex items-center gap-2 mb-2">
              <span class="bg-blue-100 text-blue-700 text-[10px] font-bold px-1.5 py-0.5 rounded">Dev</span>
            </div>
            <p class="text-sm font-medium">Redesign Product Detail Page</p>
            <div class="mt-2.5">
              <div class="flex justify-between text-[10px] text-gray-400 mb-1"><span>Progress</span><span>65%</span></div>
              <div class="h-1 bg-gray-100 rounded-full"><div class="h-1 bg-blue-500 rounded-full" style="width:65%"></div></div>
            </div>
            <div class="flex items-center justify-between mt-3">
              <div class="flex -space-x-1.5">
                <img src="https://i.pravatar.cc/20?img=1" class="w-5 h-5 rounded-full border border-white" alt="">
                <img src="https://i.pravatar.cc/20?img=3" class="w-5 h-5 rounded-full border border-white" alt="">
              </div>
              <div class="flex items-center gap-1 text-[10px] text-gray-400">🗓 18 มิ.ย.</div>
            </div>
          </div>

          <div class="bg-white rounded-xl p-3.5 shadow-sm border border-blue-100 cursor-pointer hover:shadow-md transition-shadow">
            <div class="flex items-center gap-2 mb-2">
              <span class="bg-green-100 text-green-700 text-[10px] font-bold px-1.5 py-0.5 rounded">API</span>
            </div>
            <p class="text-sm font-medium">Webhook Integration ส่ง Invoice</p>
            <div class="mt-2.5">
              <div class="flex justify-between text-[10px] text-gray-400 mb-1"><span>Progress</span><span>30%</span></div>
              <div class="h-1 bg-gray-100 rounded-full"><div class="h-1 bg-blue-500 rounded-full" style="width:30%"></div></div>
            </div>
            <div class="flex items-center justify-between mt-3">
              <img src="https://i.pravatar.cc/20?img=2" class="w-5 h-5 rounded-full" alt="">
              <div class="flex items-center gap-1 text-[10px] text-gray-400">🗓 22 มิ.ย.</div>
            </div>
          </div>
        </div>
      </div>

      <!-- Review -->
      <div class="w-72 flex flex-col bg-yellow-50/60 rounded-2xl overflow-hidden flex-shrink-0">
        <div class="flex items-center justify-between px-4 py-3 bg-yellow-100">
          <div class="flex items-center gap-2">
            <div class="w-2.5 h-2.5 bg-yellow-500 rounded-full"></div>
            <span class="text-sm font-bold text-yellow-800">Review</span>
            <span class="text-xs bg-yellow-200 text-yellow-700 font-semibold px-1.5 py-0.5 rounded-full">3</span>
          </div>
        </div>
        <div class="flex-1 overflow-y-auto p-3 space-y-2.5">
          <div class="bg-white rounded-xl p-3.5 shadow-sm border border-yellow-100 cursor-pointer hover:shadow-md transition-shadow">
            <div class="flex items-center gap-2 mb-2">
              <span class="bg-yellow-100 text-yellow-700 text-[10px] font-bold px-1.5 py-0.5 rounded">Design</span>
            </div>
            <p class="text-sm font-medium">Email Template สำหรับ Order Confirm</p>
            <div class="flex items-center justify-between mt-3">
              <div class="flex items-center gap-2 text-[10px] text-gray-400">
                <span class="bg-yellow-100 text-yellow-700 px-1.5 py-0.5 rounded font-medium">PR #42</span>
              </div>
              <img src="https://i.pravatar.cc/20?img=4" class="w-5 h-5 rounded-full" alt="">
            </div>
          </div>
        </div>
      </div>

      <!-- Done -->
      <div class="w-72 flex flex-col bg-green-50/60 rounded-2xl overflow-hidden flex-shrink-0">
        <div class="flex items-center justify-between px-4 py-3 bg-green-100">
          <div class="flex items-center gap-2">
            <div class="w-2.5 h-2.5 bg-green-500 rounded-full"></div>
            <span class="text-sm font-bold text-green-800">Done ✓</span>
            <span class="text-xs bg-green-200 text-green-700 font-semibold px-1.5 py-0.5 rounded-full">7</span>
          </div>
        </div>
        <div class="flex-1 overflow-y-auto p-3 space-y-2.5">
          <div class="bg-white rounded-xl p-3.5 shadow-sm border border-green-100 opacity-80 cursor-pointer hover:opacity-100 transition-opacity">
            <p class="text-sm font-medium line-through text-gray-400">สร้าง API สำหรับ Product Catalog</p>
            <div class="flex items-center justify-between mt-2">
              <span class="text-[10px] text-green-600 font-semibold">✓ เสร็จแล้ว</span>
              <img src="https://i.pravatar.cc/20?img=3" class="w-5 h-5 rounded-full" alt="">
            </div>
          </div>
          <div class="bg-white rounded-xl p-3.5 shadow-sm border border-green-100 opacity-80 cursor-pointer hover:opacity-100 transition-opacity">
            <p class="text-sm font-medium line-through text-gray-400">ตั้งค่า CI/CD Pipeline</p>
            <div class="flex items-center justify-between mt-2">
              <span class="text-[10px] text-green-600 font-semibold">✓ เสร็จแล้ว</span>
              <img src="https://i.pravatar.cc/20?img=1" class="w-5 h-5 rounded-full" alt="">
            </div>
          </div>
        </div>
      </div>

    </div>
  </div>

</body>
</html>
```

---

## Step 412: CRM Pipeline

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>CRM Pipeline - Step 412</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 text-gray-900 p-6">

  <div class="max-w-7xl mx-auto">
    <div class="flex items-center justify-between mb-5">
      <h2 class="text-xl font-extrabold">Sales Pipeline</h2>
      <div class="flex items-center gap-3">
        <span class="text-sm text-gray-500">มูลค่ารวม: <strong class="text-green-600">฿4.2M</strong></span>
        <button class="bg-indigo-600 text-white text-sm px-4 py-2 rounded-xl hover:bg-indigo-700 transition-colors">+ Deal ใหม่</button>
      </div>
    </div>

    <!-- Pipeline columns -->
    <div class="flex gap-4 overflow-x-auto pb-4">
      <!-- Stage: Lead -->
      <div class="w-64 flex-shrink-0">
        <div class="flex items-center justify-between mb-2.5">
          <div class="flex items-center gap-2">
            <div class="w-2 h-2 bg-gray-400 rounded-full"></div>
            <span class="text-xs font-bold text-gray-500 uppercase tracking-wide">Lead</span>
          </div>
          <span class="text-xs text-gray-400">6 deals · ฿890K</span>
        </div>
        <div class="space-y-2.5">
          <div class="bg-white rounded-xl border p-3.5 hover:shadow-md transition-shadow cursor-pointer">
            <div class="flex items-center justify-between mb-2">
              <p class="text-sm font-semibold truncate">บริษัท ABC จำกัด</p>
              <span class="text-xs text-gray-400">2 วัน</span>
            </div>
            <p class="text-xs text-gray-500 mb-2">Enterprise License · 50 users</p>
            <div class="flex items-center justify-between">
              <span class="text-sm font-bold text-green-600">฿120,000</span>
              <img src="https://i.pravatar.cc/20?img=2" class="w-5 h-5 rounded-full" alt="">
            </div>
          </div>
          <div class="bg-white rounded-xl border p-3.5 hover:shadow-md transition-shadow cursor-pointer">
            <div class="flex items-center justify-between mb-2">
              <p class="text-sm font-semibold truncate">สำนักงาน XYZ</p>
              <span class="text-xs text-gray-400">5 วัน</span>
            </div>
            <p class="text-xs text-gray-500 mb-2">Starter Plan · 20 users</p>
            <div class="flex items-center justify-between">
              <span class="text-sm font-bold text-green-600">฿48,000</span>
              <img src="https://i.pravatar.cc/20?img=3" class="w-5 h-5 rounded-full" alt="">
            </div>
          </div>
        </div>
      </div>

      <!-- Stage: Qualified -->
      <div class="w-64 flex-shrink-0">
        <div class="flex items-center justify-between mb-2.5">
          <div class="flex items-center gap-2">
            <div class="w-2 h-2 bg-blue-500 rounded-full"></div>
            <span class="text-xs font-bold text-gray-500 uppercase tracking-wide">Qualified</span>
          </div>
          <span class="text-xs text-gray-400">4 deals · ฿1.2M</span>
        </div>
        <div class="space-y-2.5">
          <div class="bg-white rounded-xl border border-blue-100 p-3.5 hover:shadow-md transition-shadow cursor-pointer">
            <div class="flex items-center justify-between mb-2">
              <p class="text-sm font-semibold truncate">ห้างเทคโนพาร์ค</p>
              <span class="text-xs bg-blue-100 text-blue-700 px-1.5 py-0.5 rounded font-medium">Hot</span>
            </div>
            <p class="text-xs text-gray-500 mb-2">Pro Plan · Annual · 200 users</p>
            <div class="mt-2 mb-2">
              <div class="flex justify-between text-[10px] text-gray-400 mb-1"><span>Probability</span><span>70%</span></div>
              <div class="h-1 bg-gray-100 rounded-full"><div class="h-1 bg-blue-500 rounded-full" style="width:70%"></div></div>
            </div>
            <div class="flex items-center justify-between">
              <span class="text-sm font-bold text-green-600">฿480,000</span>
              <img src="https://i.pravatar.cc/20?img=1" class="w-5 h-5 rounded-full" alt="">
            </div>
          </div>
        </div>
      </div>

      <!-- Stage: Proposal -->
      <div class="w-64 flex-shrink-0">
        <div class="flex items-center justify-between mb-2.5">
          <div class="flex items-center gap-2">
            <div class="w-2 h-2 bg-yellow-500 rounded-full"></div>
            <span class="text-xs font-bold text-gray-500 uppercase tracking-wide">Proposal</span>
          </div>
          <span class="text-xs text-gray-400">3 deals · ฿1.5M</span>
        </div>
        <div class="space-y-2.5">
          <div class="bg-white rounded-xl border border-yellow-100 p-3.5 hover:shadow-md transition-shadow cursor-pointer">
            <div class="flex items-center justify-between mb-2">
              <p class="text-sm font-semibold truncate">TechGiant Corp</p>
              <span class="text-xs bg-yellow-100 text-yellow-700 px-1.5 py-0.5 rounded font-medium">Due 20 มิ.ย.</span>
            </div>
            <p class="text-xs text-gray-500 mb-2">Enterprise · Custom + Support</p>
            <div class="mt-2 mb-2">
              <div class="flex justify-between text-[10px] text-gray-400 mb-1"><span>Probability</span><span>85%</span></div>
              <div class="h-1 bg-gray-100 rounded-full"><div class="h-1 bg-yellow-500 rounded-full" style="width:85%"></div></div>
            </div>
            <div class="flex items-center justify-between">
              <span class="text-sm font-bold text-green-600">฿960,000</span>
              <img src="https://i.pravatar.cc/20?img=4" class="w-5 h-5 rounded-full" alt="">
            </div>
          </div>
        </div>
      </div>

      <!-- Stage: Closed Won -->
      <div class="w-64 flex-shrink-0">
        <div class="flex items-center justify-between mb-2.5">
          <div class="flex items-center gap-2">
            <div class="w-2 h-2 bg-green-500 rounded-full"></div>
            <span class="text-xs font-bold text-gray-500 uppercase tracking-wide">Closed Won ✓</span>
          </div>
          <span class="text-xs text-gray-400">8 deals · ฿600K</span>
        </div>
        <div class="space-y-2.5">
          <div class="bg-green-50 rounded-xl border border-green-200 p-3.5 cursor-pointer hover:shadow-md transition-shadow">
            <div class="flex items-center justify-between mb-2">
              <p class="text-sm font-semibold truncate text-green-800">StartupHub TH</p>
              <span class="text-xs text-green-600 font-semibold">✓ ปิดแล้ว</span>
            </div>
            <p class="text-xs text-gray-500 mb-2">Pro Plan · Monthly</p>
            <div class="flex items-center justify-between">
              <span class="text-sm font-bold text-green-600">฿72,000</span>
              <img src="https://i.pravatar.cc/20?img=2" class="w-5 h-5 rounded-full" alt="">
            </div>
          </div>
          <div class="bg-green-50 rounded-xl border border-green-200 p-3.5 cursor-pointer hover:shadow-md transition-shadow">
            <div class="flex items-center justify-between mb-2">
              <p class="text-sm font-semibold truncate text-green-800">Digital Agency BKK</p>
              <span class="text-xs text-green-600 font-semibold">✓ ปิดแล้ว</span>
            </div>
            <p class="text-xs text-gray-500 mb-2">Starter Annual</p>
            <div class="flex items-center justify-between">
              <span class="text-sm font-bold text-green-600">฿36,000</span>
              <img src="https://i.pravatar.cc/20?img=3" class="w-5 h-5 rounded-full" alt="">
            </div>
          </div>
        </div>
      </div>

    </div>
  </div>

</body>
</html>
```

---

## Steps 413–419: Contact Detail, Activity Timeline, Filters

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>CRM Contact - Steps 413-419</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-6">
  <div class="max-w-5xl mx-auto grid grid-cols-1 lg:grid-cols-[1fr_320px] gap-6">

    <!-- Main: Contact List -->
    <div class="space-y-4">
      <!-- Search & Filter Bar -->
      <div class="flex items-center gap-3">
        <div class="relative flex-1">
          <svg class="absolute left-3 top-1/2 -translate-y-1/2 w-4 h-4 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"/></svg>
          <input type="text" placeholder="ค้นหา contact..." class="w-full border border-gray-300 rounded-xl pl-9 pr-4 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
        </div>
        <select class="border border-gray-300 rounded-xl px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
          <option>ทั้งหมด</option><option>Lead</option><option>Customer</option><option>Churned</option>
        </select>
        <button class="bg-indigo-600 text-white text-sm px-4 py-2 rounded-xl hover:bg-indigo-700 transition-colors">+ เพิ่ม</button>
      </div>

      <!-- Contact Cards -->
      <div class="bg-white rounded-2xl border overflow-hidden">
        <div class="divide-y">
          <div onclick="openContact()" class="flex items-center gap-4 px-5 py-3.5 hover:bg-indigo-50 cursor-pointer transition-colors group">
            <img src="https://i.pravatar.cc/40?img=1" class="w-10 h-10 rounded-full flex-shrink-0" alt="">
            <div class="flex-1 min-w-0">
              <p class="text-sm font-semibold group-hover:text-indigo-700 transition-colors">สมชาย เทคโน</p>
              <p class="text-xs text-gray-500 truncate">CTO · TechGiant Corp · somchai@techgiant.co.th</p>
            </div>
            <div class="flex items-center gap-2 flex-shrink-0">
              <span class="bg-blue-100 text-blue-700 text-[10px] font-semibold px-2 py-0.5 rounded-full">Proposal</span>
              <span class="text-xs font-bold text-green-600">฿960K</span>
            </div>
          </div>

          <div class="flex items-center gap-4 px-5 py-3.5 hover:bg-indigo-50 cursor-pointer transition-colors group">
            <img src="https://i.pravatar.cc/40?img=2" class="w-10 h-10 rounded-full flex-shrink-0" alt="">
            <div class="flex-1 min-w-0">
              <p class="text-sm font-semibold group-hover:text-indigo-700 transition-colors">วิไล รัตนมงคล</p>
              <p class="text-xs text-gray-500 truncate">CEO · StartupHub TH · wilai@startuphub.th</p>
            </div>
            <div class="flex items-center gap-2 flex-shrink-0">
              <span class="bg-green-100 text-green-700 text-[10px] font-semibold px-2 py-0.5 rounded-full">Customer</span>
              <span class="text-xs font-bold text-green-600">฿72K</span>
            </div>
          </div>

          <div class="flex items-center gap-4 px-5 py-3.5 hover:bg-indigo-50 cursor-pointer transition-colors group">
            <img src="https://i.pravatar.cc/40?img=3" class="w-10 h-10 rounded-full flex-shrink-0" alt="">
            <div class="flex-1 min-w-0">
              <p class="text-sm font-semibold group-hover:text-indigo-700 transition-colors">ธนพล สว่าง</p>
              <p class="text-xs text-gray-500 truncate">CFO · ห้างเทคโนพาร์ค · thanapol@technopark.co.th</p>
            </div>
            <div class="flex items-center gap-2 flex-shrink-0">
              <span class="bg-yellow-100 text-yellow-700 text-[10px] font-semibold px-2 py-0.5 rounded-full">Hot Lead</span>
              <span class="text-xs font-bold text-green-600">฿480K</span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Right: Contact Detail Panel -->
    <div id="contact-panel" class="hidden lg:block space-y-4">
      <div class="bg-white rounded-2xl border p-5 text-center">
        <img src="https://i.pravatar.cc/72?img=1" class="w-18 h-18 rounded-full mx-auto mb-3" alt="">
        <p class="font-extrabold text-lg">สมชาย เทคโน</p>
        <p class="text-sm text-gray-500">CTO · TechGiant Corp</p>
        <span class="inline-block mt-2 bg-blue-100 text-blue-700 text-xs font-semibold px-3 py-1 rounded-full">Proposal Stage</span>
        <div class="grid grid-cols-3 gap-2 mt-4 text-center">
          <button class="flex flex-col items-center gap-1 p-2 hover:bg-indigo-50 rounded-xl transition-colors">
            <span class="text-indigo-600">📞</span><span class="text-[10px] text-gray-500">โทร</span>
          </button>
          <button class="flex flex-col items-center gap-1 p-2 hover:bg-indigo-50 rounded-xl transition-colors">
            <span class="text-indigo-600">✉️</span><span class="text-[10px] text-gray-500">Email</span>
          </button>
          <button class="flex flex-col items-center gap-1 p-2 hover:bg-indigo-50 rounded-xl transition-colors">
            <span class="text-indigo-600">📅</span><span class="text-[10px] text-gray-500">นัด</span>
          </button>
        </div>
      </div>

      <!-- Activity Timeline -->
      <div class="bg-white rounded-2xl border p-4">
        <h4 class="font-semibold text-sm mb-3">Activity</h4>
        <div class="relative space-y-3">
          <div class="absolute left-2.5 top-0 bottom-0 w-px bg-gray-200"></div>
          <div class="flex gap-3 relative">
            <div class="w-5 h-5 bg-blue-100 rounded-full flex items-center justify-center flex-shrink-0 z-10 mt-0.5">📞</div>
            <div>
              <p class="text-xs font-medium">โทรหาลูกค้า</p>
              <p class="text-[11px] text-gray-400">วันนี้ 10:30 · โดย ปิยะ</p>
              <p class="text-[11px] text-gray-600 mt-0.5">พูดคุย requirements เพิ่มเติม</p>
            </div>
          </div>
          <div class="flex gap-3 relative">
            <div class="w-5 h-5 bg-green-100 rounded-full flex items-center justify-center flex-shrink-0 z-10 mt-0.5">✉️</div>
            <div>
              <p class="text-xs font-medium">ส่ง Proposal</p>
              <p class="text-[11px] text-gray-400">เมื่อวาน 15:42</p>
            </div>
          </div>
          <div class="flex gap-3 relative">
            <div class="w-5 h-5 bg-purple-100 rounded-full flex items-center justify-center flex-shrink-0 z-10 mt-0.5">📅</div>
            <div>
              <p class="text-xs font-medium">Demo Meeting</p>
              <p class="text-[11px] text-gray-400">15 มิ.ย. 14:00</p>
            </div>
          </div>
        </div>

        <!-- Add Note -->
        <div class="mt-4 pt-3 border-t">
          <textarea rows="2" placeholder="เพิ่ม note..." class="w-full border border-gray-200 rounded-xl px-3 py-2 text-xs focus:outline-none focus:ring-2 focus:ring-indigo-500 resize-none"></textarea>
          <button class="mt-2 w-full bg-indigo-600 text-white text-xs py-1.5 rounded-lg hover:bg-indigo-700 transition-colors">บันทึก</button>
        </div>
      </div>
    </div>
  </div>

  <script>
    function openContact() {
      document.getElementById('contact-panel').classList.remove('hidden');
    }
  </script>
</body>
</html>
```

---

## Step 420: Workshop — Full Kanban + CRM App

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Kanban CRM Workshop - Step 420</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 text-gray-900 h-screen flex flex-col overflow-hidden">

  <!-- Header -->
  <header class="bg-white border-b px-5 h-14 flex items-center justify-between flex-shrink-0">
    <div class="flex bg-gray-100 rounded-xl p-1 text-sm">
      <button onclick="switchView('kanban')" id="btn-kanban" class="px-4 py-1.5 rounded-lg bg-white shadow text-gray-800 font-medium text-sm transition-all">📋 Kanban</button>
      <button onclick="switchView('crm')" id="btn-crm" class="px-4 py-1.5 rounded-lg text-gray-500 hover:text-gray-800 text-sm transition-all">👥 CRM</button>
    </div>
    <div class="flex items-center gap-3">
      <span class="text-xs text-gray-500 hidden md:block">Sprint 12 · 18 มิ.ย. – 2 ก.ค. 2024</span>
      <button onclick="addCard()" class="bg-indigo-600 text-white text-xs px-3 py-1.5 rounded-lg hover:bg-indigo-700 transition-colors">+ เพิ่ม</button>
    </div>
  </header>

  <!-- Kanban View -->
  <div id="view-kanban" class="flex-1 overflow-x-auto p-4">
    <div class="flex gap-4 h-full min-w-max" id="kanban-board"></div>
  </div>

  <!-- CRM View -->
  <div id="view-crm" class="hidden flex-1 overflow-y-auto p-5">
    <div class="max-w-4xl mx-auto space-y-3" id="crm-list"></div>
  </div>

  <!-- Add Card Modal -->
  <div id="add-modal" class="hidden fixed inset-0 bg-black/40 flex items-center justify-center z-50 p-4">
    <div class="bg-white rounded-2xl p-5 w-full max-w-sm shadow-xl">
      <h3 class="font-extrabold mb-4">เพิ่มงานใหม่</h3>
      <div class="space-y-3">
        <input id="card-title" type="text" placeholder="ชื่องาน..." class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
        <select id="card-col" class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
          <option value="0">Backlog</option>
          <option value="1">In Progress</option>
          <option value="2">Review</option>
          <option value="3">Done</option>
        </select>
        <div class="flex gap-3">
          <button onclick="document.getElementById('add-modal').classList.add('hidden')" class="flex-1 border border-gray-300 py-2.5 rounded-xl text-sm">ยกเลิก</button>
          <button onclick="saveCard()" class="flex-1 bg-indigo-600 text-white py-2.5 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">บันทึก</button>
        </div>
      </div>
    </div>
  </div>

  <script>
    const columns = [
      { title: 'Backlog', color: 'bg-gray-200', dot: 'bg-gray-500', cards: ['เพิ่ม Dark Mode', 'แก้ Cart bug'] },
      { title: 'In Progress', color: 'bg-blue-100', dot: 'bg-blue-500', cards: ['Redesign Product Page', 'Webhook Integration'] },
      { title: 'Review', color: 'bg-yellow-100', dot: 'bg-yellow-500', cards: ['Email Template'] },
      { title: 'Done ✓', color: 'bg-green-100', dot: 'bg-green-500', cards: ['API Catalog', 'CI/CD Setup', 'Auth Flow'] },
    ];
    const tags = ['Feature','Bug','Dev','Design','API'];
    const tagColors = ['bg-purple-100 text-purple-700','bg-red-100 text-red-700','bg-blue-100 text-blue-700','bg-pink-100 text-pink-700','bg-green-100 text-green-700'];

    function renderKanban() {
      const board = document.getElementById('kanban-board');
      board.innerHTML = columns.map((col, ci) => `
        <div class="w-68 flex flex-col ${col.color}/40 rounded-2xl overflow-hidden flex-shrink-0" style="min-width:272px">
          <div class="flex items-center justify-between px-4 py-3 ${col.color}">
            <div class="flex items-center gap-2">
              <div class="w-2.5 h-2.5 ${col.dot} rounded-full"></div>
              <span class="text-sm font-bold">${col.title}</span>
              <span class="text-xs bg-white/60 px-1.5 py-0.5 rounded-full font-semibold">${col.cards.length}</span>
            </div>
          </div>
          <div class="flex-1 p-3 space-y-2 overflow-y-auto" style="max-height:400px">
            ${col.cards.map((c, ci2) => {
              const tagIdx = (ci + ci2) % 5;
              return `<div class="bg-white rounded-xl p-3 shadow-sm border cursor-pointer hover:shadow-md transition-shadow text-sm">
                <span class="text-[10px] ${tagColors[tagIdx]} px-1.5 py-0.5 rounded font-semibold">${tags[tagIdx]}</span>
                <p class="font-medium mt-1.5">${c}</p>
                <div class="flex items-center justify-between mt-2.5">
                  <img src="https://i.pravatar.cc/18?img=${(ci+ci2+1)%10}" class="w-4 h-4 rounded-full" alt="">
                  <span class="text-[10px] text-gray-400">TS-${40+ci*5+ci2}</span>
                </div>
              </div>`;
            }).join('')}
          </div>
        </div>
      `).join('');
    }

    const crmData = [
      { name:'สมชาย เทคโน', company:'TechGiant Corp', stage:'Proposal', deal:'฿960K', img:1 },
      { name:'วิไล รัตนมงคล', company:'StartupHub TH', stage:'Customer', deal:'฿72K', img:2 },
      { name:'ธนพล สว่าง', company:'ห้างเทคโนพาร์ค', stage:'Hot Lead', deal:'฿480K', img:3 },
    ];
    const stageBg = {'Proposal':'bg-blue-100 text-blue-700','Customer':'bg-green-100 text-green-700','Hot Lead':'bg-yellow-100 text-yellow-700'};

    function renderCRM() {
      document.getElementById('crm-list').innerHTML = `
        <div class="flex items-center justify-between mb-2">
          <h3 class="font-extrabold text-lg">Contacts</h3>
          <span class="text-sm text-gray-500">${crmData.length} รายการ</span>
        </div>
        ${crmData.map(c => `
          <div class="bg-white rounded-2xl border p-4 flex items-center gap-4 hover:shadow-md transition-shadow cursor-pointer">
            <img src="https://i.pravatar.cc/48?img=${c.img}" class="w-12 h-12 rounded-full flex-shrink-0" alt="">
            <div class="flex-1 min-w-0">
              <p class="font-semibold">${c.name}</p>
              <p class="text-sm text-gray-500 truncate">${c.company}</p>
            </div>
            <div class="flex items-center gap-3">
              <span class="${stageBg[c.stage]||'bg-gray-100 text-gray-600'} text-xs font-semibold px-2 py-1 rounded-full">${c.stage}</span>
              <span class="text-sm font-bold text-green-600">${c.deal}</span>
            </div>
          </div>
        `).join('')}
      `;
    }

    function switchView(v) {
      document.getElementById('view-kanban').classList.toggle('hidden', v !== 'kanban');
      document.getElementById('view-crm').classList.toggle('hidden', v !== 'crm');
      document.getElementById('btn-kanban').className = `px-4 py-1.5 rounded-lg ${v==='kanban'?'bg-white shadow text-gray-800 font-medium':'text-gray-500 hover:text-gray-800'} text-sm transition-all`;
      document.getElementById('btn-crm').className = `px-4 py-1.5 rounded-lg ${v==='crm'?'bg-white shadow text-gray-800 font-medium':'text-gray-500 hover:text-gray-800'} text-sm transition-all`;
    }

    function addCard() { document.getElementById('add-modal').classList.remove('hidden'); }

    function saveCard() {
      const title = document.getElementById('card-title').value.trim();
      const colIdx = parseInt(document.getElementById('card-col').value);
      if (!title) return;
      columns[colIdx].cards.unshift(title);
      document.getElementById('card-title').value = '';
      document.getElementById('add-modal').classList.add('hidden');
      renderKanban();
    }

    renderKanban();
    renderCRM();
  </script>
</body>
</html>
```

---

## สรุป Part 42

| Step | เนื้อหา |
|------|---------|
| 411 | Kanban Board: 4 columns, colored headers, progress bars, badges |
| 412 | CRM Pipeline: stage columns with deal probability |
| 413–415 | Contact list with search, filter, hover states |
| 416–418 | Contact detail panel with activity timeline |
| 419 | Add note form inline |
| 420 | Workshop: Kanban + CRM toggle app with add card modal |

**Part ถัดไป:** Part 43 — Chat & Messaging UI (Steps 421–430)
