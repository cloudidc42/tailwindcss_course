# Part 18: Navigation และ Header
## Steps 171–180: Navigation Patterns

---

## 🎯 เป้าหมายของ Part นี้

- Top navigation bar patterns
- Sidebar navigation
- Breadcrumbs
- Tabs component
- Dropdown menus
- Footer

---

## Step 171: Basic Navbar

```html
<!-- Minimal navbar -->
<nav class="bg-white border-b border-gray-200">
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 flex items-center justify-between h-16">
    <!-- Logo -->
    <div class="font-black text-xl text-gray-900">TailwindPro</div>
    
    <!-- Links -->
    <div class="flex items-center gap-6">
      <a href="#" class="text-sm text-gray-600 hover:text-gray-900 transition-colors">หน้าแรก</a>
      <a href="#" class="text-sm text-indigo-600 font-medium">หลักสูตร</a>
      <a href="#" class="text-sm text-gray-600 hover:text-gray-900 transition-colors">บล็อก</a>
      <a href="#" class="text-sm text-gray-600 hover:text-gray-900 transition-colors">เกี่ยวกับ</a>
    </div>
    
    <!-- CTA -->
    <div class="flex items-center gap-3">
      <a href="#" class="text-sm text-gray-600 hover:text-gray-900">เข้าสู่ระบบ</a>
      <a href="#" class="bg-indigo-600 hover:bg-indigo-700 text-white text-sm font-medium px-4 py-2 rounded-lg transition-colors">สมัครสมาชิก</a>
    </div>
  </div>
</nav>
```

---

## Step 172: Sticky Navbar with Logo

```html
<!-- Sticky with backdrop blur on scroll -->
<nav class="sticky top-0 z-40 bg-white/80 backdrop-blur-md border-b border-gray-200/80">
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
    <div class="flex items-center h-16">
      <!-- Logo with icon -->
      <a href="#" class="flex items-center gap-2.5 font-black text-gray-900 mr-10 flex-none">
        <div class="size-8 bg-gradient-to-br from-indigo-500 to-purple-600 rounded-lg flex items-center justify-center text-white font-black text-sm">T</div>
        <span>TailwindPro</span>
      </a>
      
      <!-- Primary nav -->
      <div class="flex-1 hidden md:flex items-center gap-1">
        <a href="#" class="px-3 py-2 rounded-lg text-sm text-gray-700 hover:bg-gray-100 hover:text-gray-900 transition-colors font-medium">
          หลักสูตร
        </a>
        <a href="#" class="px-3 py-2 rounded-lg text-sm text-indigo-600 bg-indigo-50 font-medium">
          โปรเจกต์
        </a>
        <a href="#" class="px-3 py-2 rounded-lg text-sm text-gray-700 hover:bg-gray-100 hover:text-gray-900 transition-colors font-medium">
          ชุมชน
        </a>
        <a href="#" class="px-3 py-2 rounded-lg text-sm text-gray-700 hover:bg-gray-100 hover:text-gray-900 transition-colors font-medium">
          บล็อก
        </a>
      </div>
      
      <!-- Right actions -->
      <div class="flex items-center gap-3 ml-auto">
        <button class="hidden md:flex items-center gap-1.5 text-sm text-gray-500 hover:text-gray-700 bg-gray-100 hover:bg-gray-200 px-3 py-1.5 rounded-lg transition-colors">
          <span>🔍</span>
          <span>ค้นหา</span>
          <kbd class="text-xs bg-white border border-gray-300 px-1.5 py-0.5 rounded ml-2">⌘K</kbd>
        </button>
        
        <button class="relative size-9 flex items-center justify-center text-gray-500 hover:text-gray-900 hover:bg-gray-100 rounded-lg transition-colors">
          🔔
          <span class="absolute top-1 right-1 size-2 bg-red-500 rounded-full"></span>
        </button>
        
        <div class="flex items-center gap-2">
          <div class="size-8 rounded-full bg-indigo-600 flex items-center justify-center text-white text-sm font-bold">S</div>
          <span class="hidden lg:block text-sm font-medium text-gray-700">สมชาย</span>
        </div>
      </div>
      
      <!-- Mobile hamburger -->
      <button class="md:hidden size-9 flex flex-col justify-center items-center gap-1.5 ml-3">
        <span class="w-5 h-0.5 bg-gray-600 rounded"></span>
        <span class="w-5 h-0.5 bg-gray-600 rounded"></span>
        <span class="w-5 h-0.5 bg-gray-600 rounded"></span>
      </button>
    </div>
  </div>
</nav>
```

---

## Step 173: Sidebar Navigation

```html
<!-- Left sidebar nav -->
<aside class="w-64 flex-none h-screen sticky top-0 bg-white border-r border-gray-200 flex flex-col py-6 px-3">
  
  <!-- Logo -->
  <div class="flex items-center gap-2.5 px-3 mb-8">
    <div class="size-8 bg-indigo-600 rounded-lg flex items-center justify-center text-white font-bold text-sm">T</div>
    <span class="font-black text-gray-900">TailwindPro</span>
  </div>
  
  <!-- Nav items -->
  <nav class="flex-1 space-y-1">
    <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-xl text-sm font-medium bg-indigo-50 text-indigo-700">
      <span class="text-base">🏠</span>
      <span>ภาพรวม</span>
    </a>
    <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-xl text-sm font-medium text-gray-600 hover:bg-gray-100 hover:text-gray-900 transition-colors">
      <span class="text-base">📚</span>
      <span>หลักสูตรของฉัน</span>
    </a>
    <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-xl text-sm font-medium text-gray-600 hover:bg-gray-100 hover:text-gray-900 transition-colors">
      <span class="text-base">📊</span>
      <span>Progress</span>
      <span class="ml-auto text-xs bg-green-100 text-green-700 font-bold px-1.5 py-0.5 rounded-full">87%</span>
    </a>
    <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-xl text-sm font-medium text-gray-600 hover:bg-gray-100 hover:text-gray-900 transition-colors">
      <span class="text-base">🏆</span>
      <span>ความสำเร็จ</span>
      <span class="ml-auto text-xs bg-yellow-100 text-yellow-700 font-bold px-1.5 py-0.5 rounded-full">3 ใหม่</span>
    </a>
    <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-xl text-sm font-medium text-gray-600 hover:bg-gray-100 hover:text-gray-900 transition-colors">
      <span class="text-base">🗂️</span>
      <span>โปรเจกต์</span>
    </a>
    
    <!-- Divider -->
    <div class="my-3 border-t border-gray-100"></div>
    
    <p class="px-3 text-xs font-semibold text-gray-400 uppercase tracking-wider mb-1">จัดการ</p>
    <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-xl text-sm font-medium text-gray-600 hover:bg-gray-100 transition-colors">
      <span class="text-base">⚙</span>
      <span>ตั้งค่า</span>
    </a>
    <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-xl text-sm font-medium text-gray-600 hover:bg-gray-100 transition-colors">
      <span class="text-base">❓</span>
      <span>ช่วยเหลือ</span>
    </a>
  </nav>
  
  <!-- User section -->
  <div class="border-t border-gray-100 pt-4 mt-4 px-2">
    <div class="flex items-center gap-3 p-2 rounded-xl hover:bg-gray-50 cursor-pointer transition-colors">
      <div class="size-9 rounded-xl bg-indigo-600 flex items-center justify-center text-white font-bold text-sm flex-none">S</div>
      <div class="flex-1 min-w-0">
        <p class="text-sm font-semibold text-gray-900 truncate">สมชาย ใจดี</p>
        <p class="text-xs text-gray-500 truncate">Pro Plan</p>
      </div>
      <span class="text-gray-400 text-xs">⋯</span>
    </div>
  </div>
  
</aside>
```

---

## Step 174: Breadcrumbs

```html
<!-- Basic breadcrumb -->
<nav class="flex items-center gap-2 text-sm text-gray-500">
  <a href="#" class="hover:text-gray-700 transition-colors">หน้าแรก</a>
  <span>/</span>
  <a href="#" class="hover:text-gray-700 transition-colors">หลักสูตร</a>
  <span>/</span>
  <span class="text-gray-900 font-medium">Tailwind CSS</span>
</nav>

<!-- With icons -->
<nav class="flex items-center gap-1.5 text-sm">
  <a href="#" class="flex items-center gap-1 text-gray-500 hover:text-indigo-600 transition-colors">
    <span>🏠</span>
    <span>Home</span>
  </a>
  <span class="text-gray-300">›</span>
  <a href="#" class="text-gray-500 hover:text-indigo-600 transition-colors">Courses</a>
  <span class="text-gray-300">›</span>
  <a href="#" class="text-gray-500 hover:text-indigo-600 transition-colors">Tailwind CSS</a>
  <span class="text-gray-300">›</span>
  <span class="text-gray-900 font-medium">Part 18</span>
</nav>

<!-- Rounded pills style -->
<nav class="flex items-center gap-1 text-sm">
  <a href="#" class="px-2.5 py-1 rounded-md text-gray-500 hover:bg-gray-100 hover:text-gray-700 transition-colors">Home</a>
  <span class="text-gray-300">›</span>
  <a href="#" class="px-2.5 py-1 rounded-md text-gray-500 hover:bg-gray-100 hover:text-gray-700 transition-colors">Courses</a>
  <span class="text-gray-300">›</span>
  <span class="px-2.5 py-1 rounded-md bg-indigo-50 text-indigo-700 font-medium">Part 18</span>
</nav>
```

---

## Step 175: Tabs Component

```html
<!-- Underline tabs -->
<div>
  <div class="border-b border-gray-200">
    <nav class="flex gap-1 -mb-px px-4">
      <a href="#" class="px-4 py-3 text-sm font-medium border-b-2 border-indigo-600 text-indigo-600">ภาพรวม</a>
      <a href="#" class="px-4 py-3 text-sm font-medium border-b-2 border-transparent text-gray-500 hover:text-gray-700 hover:border-gray-300 transition-colors">หลักสูตร</a>
      <a href="#" class="px-4 py-3 text-sm font-medium border-b-2 border-transparent text-gray-500 hover:text-gray-700 hover:border-gray-300 transition-colors">
        รีวิว
        <span class="ml-1.5 text-xs bg-gray-100 text-gray-600 px-1.5 py-0.5 rounded-full">48</span>
      </a>
      <a href="#" class="px-4 py-3 text-sm font-medium border-b-2 border-transparent text-gray-500 hover:text-gray-700 hover:border-gray-300 transition-colors">คำถาม</a>
    </nav>
  </div>
  <div class="p-4">
    <p class="text-gray-700 text-sm">Content for Overview tab</p>
  </div>
</div>

<!-- Pill tabs -->
<div>
  <div class="flex gap-1 p-1 bg-gray-100 rounded-xl w-fit">
    <button class="px-4 py-2 text-sm font-medium rounded-lg bg-white text-gray-900 shadow-sm">วันนี้</button>
    <button class="px-4 py-2 text-sm font-medium rounded-lg text-gray-500 hover:text-gray-700 transition-colors">สัปดาห์นี้</button>
    <button class="px-4 py-2 text-sm font-medium rounded-lg text-gray-500 hover:text-gray-700 transition-colors">เดือนนี้</button>
  </div>
</div>

<!-- Vertical tabs -->
<div class="flex gap-4">
  <div class="flex-none w-44 space-y-1">
    <button class="w-full text-left px-4 py-2.5 text-sm font-medium rounded-xl bg-indigo-50 text-indigo-700">บัญชีผู้ใช้</button>
    <button class="w-full text-left px-4 py-2.5 text-sm font-medium rounded-xl text-gray-600 hover:bg-gray-100 transition-colors">การชำระเงิน</button>
    <button class="w-full text-left px-4 py-2.5 text-sm font-medium rounded-xl text-gray-600 hover:bg-gray-100 transition-colors">การแจ้งเตือน</button>
    <button class="w-full text-left px-4 py-2.5 text-sm font-medium rounded-xl text-gray-600 hover:bg-gray-100 transition-colors">ความเป็นส่วนตัว</button>
  </div>
  <div class="flex-1 bg-white rounded-xl p-6 border border-gray-200">
    <h3 class="font-bold text-gray-900 mb-4">บัญชีผู้ใช้</h3>
    <p class="text-gray-500 text-sm">ตั้งค่าข้อมูลบัญชีของคุณ</p>
  </div>
</div>
```

---

## Step 176: Dropdown Menu

```html
<!-- Dropdown (requires JS to toggle) -->
<div class="relative inline-block">
  <button 
    onclick="this.nextElementSibling.classList.toggle('hidden')"
    class="flex items-center gap-2 px-4 py-2.5 text-sm font-medium text-gray-700 bg-white border border-gray-300 rounded-xl hover:bg-gray-50 transition-colors"
  >
    <div class="size-6 rounded-full bg-indigo-600 flex items-center justify-center text-white text-xs font-bold">S</div>
    <span>สมชาย</span>
    <span class="text-gray-400 text-xs">▼</span>
  </button>
  
  <div class="hidden absolute right-0 top-full mt-2 w-56 bg-white border border-gray-200 rounded-2xl shadow-xl py-2 z-50">
    <!-- User info -->
    <div class="px-4 py-3 border-b border-gray-100">
      <p class="font-semibold text-gray-900 text-sm">สมชาย ใจดี</p>
      <p class="text-xs text-gray-500">somchai@email.com</p>
    </div>
    
    <!-- Items -->
    <div class="py-1">
      <a href="#" class="flex items-center gap-3 px-4 py-2.5 text-sm text-gray-700 hover:bg-gray-50 transition-colors">
        <span>👤</span> โปรไฟล์
      </a>
      <a href="#" class="flex items-center gap-3 px-4 py-2.5 text-sm text-gray-700 hover:bg-gray-50 transition-colors">
        <span>⚙</span> ตั้งค่า
      </a>
      <a href="#" class="flex items-center gap-3 px-4 py-2.5 text-sm text-gray-700 hover:bg-gray-50 transition-colors">
        <span>🏆</span> ความสำเร็จ
        <span class="ml-auto text-xs bg-yellow-100 text-yellow-700 px-1.5 py-0.5 rounded-full">3 ใหม่</span>
      </a>
    </div>
    
    <div class="border-t border-gray-100 pt-1 pb-1">
      <a href="#" class="flex items-center gap-3 px-4 py-2.5 text-sm text-red-600 hover:bg-red-50 transition-colors">
        <span>🚪</span> ออกจากระบบ
      </a>
    </div>
  </div>
</div>
```

---

## Step 177: Mobile Menu

```html
<!-- Mobile slide-down menu -->
<div id="mobile-menu" class="hidden md:hidden bg-white border-t border-gray-200">
  <div class="px-4 py-3 space-y-1">
    <a href="#" class="block px-3 py-2.5 rounded-xl text-sm font-medium text-indigo-600 bg-indigo-50">หน้าแรก</a>
    <a href="#" class="block px-3 py-2.5 rounded-xl text-sm font-medium text-gray-700 hover:bg-gray-100 transition-colors">หลักสูตร</a>
    <a href="#" class="block px-3 py-2.5 rounded-xl text-sm font-medium text-gray-700 hover:bg-gray-100 transition-colors">โปรเจกต์</a>
    <a href="#" class="block px-3 py-2.5 rounded-xl text-sm font-medium text-gray-700 hover:bg-gray-100 transition-colors">บล็อก</a>
    <div class="pt-3 pb-2 border-t border-gray-100 mt-2 flex flex-col gap-2">
      <a href="#" class="block px-4 py-2.5 text-sm font-medium text-gray-700 bg-gray-100 hover:bg-gray-200 rounded-xl text-center transition-colors">เข้าสู่ระบบ</a>
      <a href="#" class="block px-4 py-2.5 text-sm font-medium text-white bg-indigo-600 hover:bg-indigo-700 rounded-xl text-center transition-colors">สมัครสมาชิกฟรี</a>
    </div>
  </div>
</div>
```

---

## Step 178: Footer

```html
<footer class="bg-gray-900 text-white pt-16 pb-8">
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
    
    <!-- Top grid -->
    <div class="grid grid-cols-2 md:grid-cols-4 gap-8 pb-10 border-b border-gray-800">
      
      <!-- Brand -->
      <div class="col-span-2 md:col-span-1">
        <div class="flex items-center gap-2 mb-4">
          <div class="size-8 bg-indigo-600 rounded-lg flex items-center justify-center font-black text-sm">T</div>
          <span class="font-black text-xl">TailwindPro</span>
        </div>
        <p class="text-gray-400 text-sm leading-relaxed">
          เรียน Tailwind CSS อย่างเป็นระบบ ตั้งแต่พื้นฐานสู่ระดับโลก
        </p>
        <div class="flex gap-3 mt-4">
          <a href="#" class="size-9 bg-gray-800 hover:bg-indigo-600 rounded-lg flex items-center justify-center text-sm transition-colors">𝕏</a>
          <a href="#" class="size-9 bg-gray-800 hover:bg-blue-600 rounded-lg flex items-center justify-center text-sm transition-colors">📘</a>
          <a href="#" class="size-9 bg-gray-800 hover:bg-pink-600 rounded-lg flex items-center justify-center text-sm transition-colors">📸</a>
        </div>
      </div>
      
      <!-- Links -->
      <div>
        <h4 class="font-semibold text-sm uppercase tracking-wider text-gray-400 mb-4">หลักสูตร</h4>
        <ul class="space-y-2.5">
          <li><a href="#" class="text-gray-400 hover:text-white text-sm transition-colors">พื้นฐาน</a></li>
          <li><a href="#" class="text-gray-400 hover:text-white text-sm transition-colors">ระดับกลาง</a></li>
          <li><a href="#" class="text-gray-400 hover:text-white text-sm transition-colors">ขั้นสูง</a></li>
          <li><a href="#" class="text-gray-400 hover:text-white text-sm transition-colors">React + Tailwind</a></li>
        </ul>
      </div>
      
      <div>
        <h4 class="font-semibold text-sm uppercase tracking-wider text-gray-400 mb-4">บริษัท</h4>
        <ul class="space-y-2.5">
          <li><a href="#" class="text-gray-400 hover:text-white text-sm transition-colors">เกี่ยวกับเรา</a></li>
          <li><a href="#" class="text-gray-400 hover:text-white text-sm transition-colors">บล็อก</a></li>
          <li><a href="#" class="text-gray-400 hover:text-white text-sm transition-colors">ร่วมงาน</a></li>
          <li><a href="#" class="text-gray-400 hover:text-white text-sm transition-colors">ติดต่อ</a></li>
        </ul>
      </div>
      
      <div>
        <h4 class="font-semibold text-sm uppercase tracking-wider text-gray-400 mb-4">Newsletter</h4>
        <p class="text-gray-400 text-sm mb-3">รับข่าวสารใหม่ทุกสัปดาห์</p>
        <div class="flex gap-2">
          <input type="email" placeholder="your@email.com" class="flex-1 bg-gray-800 border border-gray-700 text-white text-sm rounded-lg px-3 py-2 focus:outline-none focus:border-indigo-500 placeholder:text-gray-600">
          <button class="bg-indigo-600 hover:bg-indigo-700 text-white text-sm px-3 py-2 rounded-lg transition-colors flex-none">→</button>
        </div>
      </div>
    </div>
    
    <!-- Bottom -->
    <div class="pt-6 flex flex-col sm:flex-row items-center justify-between gap-4">
      <p class="text-gray-500 text-sm">© 2025 TailwindPro. All rights reserved.</p>
      <div class="flex gap-6">
        <a href="#" class="text-gray-500 hover:text-gray-300 text-sm transition-colors">Privacy</a>
        <a href="#" class="text-gray-500 hover:text-gray-300 text-sm transition-colors">Terms</a>
        <a href="#" class="text-gray-500 hover:text-gray-300 text-sm transition-colors">Cookies</a>
      </div>
    </div>
    
  </div>
</footer>
```

---

## Step 179: Pagination

```html
<!-- Basic pagination -->
<div class="flex items-center gap-1">
  <button class="size-9 flex items-center justify-center text-sm text-gray-400 hover:bg-gray-100 rounded-lg transition-colors" disabled>
    ←
  </button>
  <button class="size-9 flex items-center justify-center text-sm font-medium bg-indigo-600 text-white rounded-lg">1</button>
  <button class="size-9 flex items-center justify-center text-sm text-gray-700 hover:bg-gray-100 rounded-lg transition-colors">2</button>
  <button class="size-9 flex items-center justify-center text-sm text-gray-700 hover:bg-gray-100 rounded-lg transition-colors">3</button>
  <span class="size-9 flex items-center justify-center text-sm text-gray-400">…</span>
  <button class="size-9 flex items-center justify-center text-sm text-gray-700 hover:bg-gray-100 rounded-lg transition-colors">10</button>
  <button class="size-9 flex items-center justify-center text-sm text-gray-700 hover:bg-gray-100 rounded-lg transition-colors">
    →
  </button>
</div>
```

---

## Step 180: Workshop — Full Dashboard Header + Sidebar

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Dashboard Navigation Workshop</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 min-h-screen">

<div class="flex h-screen overflow-hidden">
  <!-- Sidebar -->
  <aside class="w-60 flex-none bg-white border-r border-gray-200 flex flex-col py-5 px-3 hidden md:flex">
    <div class="flex items-center gap-2.5 px-3 mb-6">
      <div class="size-8 bg-gradient-to-br from-indigo-500 to-purple-600 rounded-lg flex items-center justify-center text-white font-black text-sm">T</div>
      <span class="font-black text-gray-900">TailwindPro</span>
    </div>
    
    <nav class="flex-1 space-y-1">
      <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-xl text-sm font-medium bg-indigo-50 text-indigo-700">
        <span>🏠</span> ภาพรวม
      </a>
      <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-xl text-sm font-medium text-gray-600 hover:bg-gray-100 transition-colors">
        <span>📚</span> หลักสูตร
        <span class="ml-auto text-xs bg-indigo-100 text-indigo-600 px-1.5 py-0.5 rounded-full font-bold">12</span>
      </a>
      <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-xl text-sm font-medium text-gray-600 hover:bg-gray-100 transition-colors">
        <span>📊</span> Progress
      </a>
      <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-xl text-sm font-medium text-gray-600 hover:bg-gray-100 transition-colors">
        <span>🏆</span> ความสำเร็จ
      </a>
      <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-xl text-sm font-medium text-gray-600 hover:bg-gray-100 transition-colors">
        <span>🗂️</span> โปรเจกต์
      </a>
      <div class="pt-3 mt-3 border-t border-gray-100">
        <p class="px-3 text-xs font-semibold text-gray-400 uppercase mb-2">ชุมชน</p>
        <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-xl text-sm font-medium text-gray-600 hover:bg-gray-100 transition-colors">
          <span>💬</span> ถามตอบ
        </a>
        <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-xl text-sm font-medium text-gray-600 hover:bg-gray-100 transition-colors">
          <span>👥</span> ชุมชน
        </a>
      </div>
    </nav>
    
    <div class="border-t border-gray-100 pt-4 mt-4">
      <div class="flex items-center gap-3 p-2.5 rounded-xl hover:bg-gray-50 cursor-pointer">
        <div class="size-8 rounded-xl bg-indigo-600 flex items-center justify-center text-white font-bold text-xs flex-none">S</div>
        <div class="flex-1 min-w-0">
          <p class="text-sm font-semibold text-gray-900 truncate">สมชาย</p>
          <p class="text-xs text-gray-400 truncate">Pro Plan</p>
        </div>
        <button class="text-gray-400 hover:text-gray-600 text-xs">⋯</button>
      </div>
    </div>
  </aside>
  
  <!-- Main content -->
  <div class="flex-1 flex flex-col overflow-hidden">
    <!-- Top bar -->
    <header class="bg-white border-b border-gray-200 px-4 sm:px-6 flex items-center h-14 gap-4">
      <div class="flex-1">
        <div class="flex items-center gap-2 text-sm text-gray-500">
          <a href="#" class="hover:text-gray-700">หน้าแรก</a>
          <span>/</span>
          <span class="text-gray-900 font-medium">ภาพรวม</span>
        </div>
      </div>
      <button class="flex items-center gap-2 text-sm text-gray-500 bg-gray-100 hover:bg-gray-200 px-3 py-1.5 rounded-lg transition-colors">
        <span>🔍</span> ค้นหา
      </button>
      <button class="relative size-9 flex items-center justify-center text-gray-500 hover:text-gray-900 hover:bg-gray-100 rounded-lg transition-colors">
        🔔
        <span class="absolute top-1.5 right-1.5 size-2 bg-red-500 rounded-full"></span>
      </button>
      <div class="size-8 rounded-xl bg-indigo-600 flex items-center justify-center text-white text-xs font-bold cursor-pointer">S</div>
    </header>
    
    <!-- Page content -->
    <main class="flex-1 overflow-y-auto p-4 sm:p-6">
      <div class="mb-6">
        <h1 class="text-2xl font-bold text-gray-900">สวัสดี สมชาย 👋</h1>
        <p class="text-gray-500 text-sm mt-1">ดูความคืบหน้าของวันนี้</p>
      </div>
      
      <!-- Stats cards -->
      <div class="grid grid-cols-2 lg:grid-cols-4 gap-4 mb-6">
        <div class="bg-white rounded-xl p-4 border border-gray-200">
          <p class="text-xs text-gray-500 mb-1">บทที่เรียน</p>
          <p class="text-2xl font-black text-gray-900">42</p>
          <p class="text-xs text-green-600 mt-1">↑ +3 วันนี้</p>
        </div>
        <div class="bg-white rounded-xl p-4 border border-gray-200">
          <p class="text-xs text-gray-500 mb-1">Streak</p>
          <p class="text-2xl font-black text-orange-600">7🔥</p>
          <p class="text-xs text-gray-500 mt-1">วันติดต่อกัน</p>
        </div>
        <div class="bg-white rounded-xl p-4 border border-gray-200">
          <p class="text-xs text-gray-500 mb-1">XP ทั้งหมด</p>
          <p class="text-2xl font-black text-purple-600">2,450</p>
          <p class="text-xs text-gray-500 mt-1">Level 8</p>
        </div>
        <div class="bg-white rounded-xl p-4 border border-gray-200">
          <p class="text-xs text-gray-500 mb-1">โปรเจกต์</p>
          <p class="text-2xl font-black text-gray-900">6</p>
          <p class="text-xs text-indigo-600 mt-1">2 กำลังทำ</p>
        </div>
      </div>
      
      <!-- Progress section -->
      <div class="bg-white rounded-xl p-5 border border-gray-200">
        <h3 class="font-bold text-gray-900 mb-4">Progress รวม</h3>
        <div class="space-y-4">
          <div>
            <div class="flex justify-between text-sm mb-1.5">
              <span class="font-medium text-gray-700">Level 1: พื้นฐาน</span>
              <span class="text-indigo-600 font-bold">100%</span>
            </div>
            <div class="h-2 bg-gray-100 rounded-full">
              <div class="h-full w-full bg-green-500 rounded-full"></div>
            </div>
          </div>
          <div>
            <div class="flex justify-between text-sm mb-1.5">
              <span class="font-medium text-gray-700">Level 2: กลาง</span>
              <span class="text-indigo-600 font-bold">42%</span>
            </div>
            <div class="h-2 bg-gray-100 rounded-full">
              <div class="h-full w-[42%] bg-indigo-500 rounded-full"></div>
            </div>
          </div>
          <div>
            <div class="flex justify-between text-sm mb-1.5">
              <span class="font-medium text-gray-700">Level 3: ขั้นสูง</span>
              <span class="text-gray-400 font-bold">0%</span>
            </div>
            <div class="h-2 bg-gray-100 rounded-full"></div>
          </div>
        </div>
      </div>
    </main>
  </div>
</div>

</body>
</html>
```

---

## 📝 สรุป Part 18

| Component | Pattern หลัก |
|-----------|-------------|
| Sticky Navbar | `sticky top-0 z-40 bg-white/80 backdrop-blur-md` |
| Active nav item | `bg-indigo-50 text-indigo-700` |
| Sidebar | `w-64 h-screen sticky top-0 border-r` |
| Tab underline | `border-b-2 border-indigo-600` |
| Breadcrumb | `flex items-center gap-2 text-sm` |
| Dropdown | `absolute top-full right-0 bg-white shadow-xl rounded-2xl z-50` |
| Footer | `bg-gray-900 grid-cols-4 pt-16 pb-8` |

---

*Part 18 — จาก 100 Parts | Steps 171–180 จาก 1,000 Steps*
