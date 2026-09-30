# Part 19: Cards และ Content Blocks
## Steps 181–190: Content Component Patterns

---

## 🎯 เป้าหมายของ Part นี้

- Card variants (basic, image, horizontal, pricing)
- Avatar & profile components
- Stat cards & KPI widgets
- List items & table rows
- Modal dialog
- Tooltip & Popover

---

## Step 181: Basic Cards

```html
<!-- Simple card -->
<div class="bg-white rounded-2xl border border-gray-200 p-6 shadow-sm hover:shadow-md transition-shadow">
  <h3 class="font-bold text-gray-900 mb-2">Card Title</h3>
  <p class="text-gray-500 text-sm leading-relaxed">Card content goes here. Any text or components inside.</p>
  <div class="mt-4 flex items-center gap-2">
    <button class="text-indigo-600 text-sm font-medium hover:text-indigo-800 transition-colors">อ่านเพิ่ม →</button>
  </div>
</div>

<!-- Card with header accent -->
<div class="bg-white rounded-2xl border border-gray-200 overflow-hidden shadow-sm">
  <div class="h-1.5 bg-gradient-to-r from-indigo-500 to-purple-500"></div>
  <div class="p-6">
    <h3 class="font-bold text-gray-900 mb-2">Accent Card</h3>
    <p class="text-gray-500 text-sm">Content with top accent bar</p>
  </div>
</div>

<!-- Colored card -->
<div class="bg-indigo-600 text-white rounded-2xl p-6">
  <div class="size-10 bg-white/20 rounded-xl flex items-center justify-center text-xl mb-4">🎨</div>
  <h3 class="font-bold text-lg mb-2">Colored Card</h3>
  <p class="text-indigo-200 text-sm">White text on colored background</p>
</div>

<!-- Gradient card -->
<div class="bg-gradient-to-br from-indigo-600 to-purple-700 text-white rounded-2xl p-6 shadow-xl shadow-indigo-500/25">
  <h3 class="font-bold text-xl mb-2">Premium Feature</h3>
  <p class="text-white/80 text-sm mb-4">Unlock all features with Pro plan</p>
  <button class="bg-white text-indigo-700 font-semibold text-sm px-4 py-2 rounded-xl hover:bg-indigo-50 transition-colors">
    Upgrade Now
  </button>
</div>
```

---

## Step 182: Image Card

```html
<!-- Vertical image card -->
<div class="bg-white rounded-2xl overflow-hidden shadow-sm hover:shadow-xl transition-all duration-300 group cursor-pointer">
  <!-- Image -->
  <div class="relative overflow-hidden">
    <div class="aspect-video bg-gradient-to-br from-blue-400 to-indigo-600 group-hover:scale-105 transition-transform duration-500">
      <div class="w-full h-full flex items-center justify-center text-5xl">🎨</div>
    </div>
    <!-- Overlay badge -->
    <div class="absolute top-3 left-3">
      <span class="text-xs font-bold bg-indigo-600 text-white px-2.5 py-1 rounded-full">พื้นฐาน</span>
    </div>
    <!-- Duration badge -->
    <div class="absolute bottom-3 right-3 bg-black/60 text-white text-xs px-2 py-1 rounded-lg">
      2:30 ชั่วโมง
    </div>
  </div>
  
  <!-- Content -->
  <div class="p-5">
    <div class="flex items-center gap-2 text-xs text-gray-400 mb-2">
      <span>Part 01</span>
      <span>•</span>
      <span>10 Steps</span>
    </div>
    <h3 class="font-bold text-gray-900 group-hover:text-indigo-600 transition-colors leading-snug">
      Introduction to Tailwind CSS
    </h3>
    <p class="text-gray-500 text-sm mt-2 line-clamp-2 leading-relaxed">
      เรียนรู้ Tailwind CSS ตั้งแต่พื้นฐาน utility-first approach
    </p>
    
    <!-- Meta -->
    <div class="mt-4 pt-4 border-t border-gray-100 flex items-center justify-between">
      <div class="flex items-center gap-2">
        <div class="size-6 rounded-full bg-indigo-600 flex items-center justify-center text-white text-xs font-bold">A</div>
        <span class="text-xs text-gray-500">อาจารย์สมชาย</span>
      </div>
      <div class="flex items-center gap-1 text-xs text-yellow-500 font-medium">
        ★ <span class="text-gray-600">4.9</span>
        <span class="text-gray-400">(48)</span>
      </div>
    </div>
  </div>
</div>

<!-- Horizontal image card -->
<div class="bg-white rounded-2xl overflow-hidden shadow-sm hover:shadow-md transition-shadow flex">
  <div class="w-32 flex-none bg-gradient-to-br from-green-400 to-emerald-600 flex items-center justify-center text-3xl">📐</div>
  <div class="p-4 flex-1 min-w-0">
    <span class="text-xs bg-green-100 text-green-700 px-2 py-0.5 rounded-full font-medium">กลาง</span>
    <h3 class="font-bold text-gray-900 mt-1.5 text-sm truncate">Flexbox & Grid Mastery</h3>
    <p class="text-gray-500 text-xs mt-1 line-clamp-2">Layout ทุกรูปแบบด้วย Flexbox และ CSS Grid</p>
    <div class="flex items-center gap-3 mt-3">
      <span class="text-xs text-gray-400">12 Steps</span>
      <span class="text-yellow-500 text-xs">★ 4.8</span>
    </div>
  </div>
</div>
```

---

## Step 183: Profile & Avatar

```html
<!-- Avatar sizes -->
<div class="flex items-center gap-4">
  <!-- XS -->
  <div class="size-6 rounded-full bg-indigo-600 flex items-center justify-center text-white text-xs font-bold">S</div>
  <!-- SM -->
  <div class="size-8 rounded-full bg-indigo-600 flex items-center justify-center text-white text-sm font-bold">S</div>
  <!-- MD -->
  <div class="size-10 rounded-full bg-indigo-600 flex items-center justify-center text-white font-bold">S</div>
  <!-- LG -->
  <div class="size-12 rounded-full bg-indigo-600 flex items-center justify-center text-white text-lg font-bold">S</div>
  <!-- XL -->
  <div class="size-16 rounded-full bg-indigo-600 flex items-center justify-center text-white text-2xl font-bold">S</div>
</div>

<!-- Avatar with status -->
<div class="relative inline-block">
  <div class="size-10 rounded-full bg-indigo-600 flex items-center justify-center text-white font-bold">S</div>
  <span class="absolute bottom-0 right-0 size-3 bg-green-500 rounded-full border-2 border-white"></span>
</div>

<!-- Avatar stack -->
<div class="flex -space-x-3">
  <div class="size-9 rounded-full bg-blue-500 border-2 border-white flex items-center justify-center text-white text-xs font-bold">A</div>
  <div class="size-9 rounded-full bg-pink-500 border-2 border-white flex items-center justify-center text-white text-xs font-bold">B</div>
  <div class="size-9 rounded-full bg-green-500 border-2 border-white flex items-center justify-center text-white text-xs font-bold">C</div>
  <div class="size-9 rounded-full bg-gray-200 border-2 border-white flex items-center justify-center text-gray-600 text-xs font-bold">+5</div>
</div>

<!-- Profile card -->
<div class="bg-white rounded-2xl p-6 text-center border border-gray-200 shadow-sm max-w-xs">
  <div class="relative inline-block mb-4">
    <div class="size-20 rounded-2xl bg-gradient-to-br from-indigo-500 to-purple-600 flex items-center justify-center text-white text-3xl font-black">S</div>
    <span class="absolute -bottom-1 -right-1 size-5 bg-green-500 rounded-full border-2 border-white text-xs flex items-center justify-center text-white">✓</span>
  </div>
  <h3 class="font-bold text-gray-900">สมชาย ใจดี</h3>
  <p class="text-sm text-gray-500 mt-0.5">Frontend Developer</p>
  <div class="flex justify-center gap-1 mt-2">
    <span class="text-xs bg-indigo-100 text-indigo-700 px-2 py-0.5 rounded-full">React</span>
    <span class="text-xs bg-green-100 text-green-700 px-2 py-0.5 rounded-full">Tailwind</span>
    <span class="text-xs bg-purple-100 text-purple-700 px-2 py-0.5 rounded-full">TypeScript</span>
  </div>
  <div class="grid grid-cols-3 gap-3 mt-4 pt-4 border-t border-gray-100">
    <div class="text-center">
      <p class="font-bold text-gray-900 text-sm">42</p>
      <p class="text-xs text-gray-400">Parts</p>
    </div>
    <div class="text-center">
      <p class="font-bold text-gray-900 text-sm">6</p>
      <p class="text-xs text-gray-400">Projects</p>
    </div>
    <div class="text-center">
      <p class="font-bold text-gray-900 text-sm">2.4k</p>
      <p class="text-xs text-gray-400">XP</p>
    </div>
  </div>
  <button class="mt-4 w-full bg-indigo-600 hover:bg-indigo-700 text-white text-sm font-semibold py-2.5 rounded-xl transition-colors">
    Follow
  </button>
</div>
```

---

## Step 184: Stat Cards

```html
<!-- KPI cards -->
<div class="grid grid-cols-2 lg:grid-cols-4 gap-4">
  
  <div class="bg-white rounded-2xl p-5 border border-gray-200">
    <div class="flex items-center justify-between mb-3">
      <div class="size-10 bg-blue-100 rounded-xl flex items-center justify-center text-blue-600">👥</div>
      <span class="text-xs text-green-600 font-medium bg-green-50 px-2 py-0.5 rounded-full">↑ 12%</span>
    </div>
    <p class="text-2xl font-black text-gray-900">1,248</p>
    <p class="text-xs text-gray-500 mt-0.5">นักเรียนทั้งหมด</p>
  </div>
  
  <div class="bg-white rounded-2xl p-5 border border-gray-200">
    <div class="flex items-center justify-between mb-3">
      <div class="size-10 bg-indigo-100 rounded-xl flex items-center justify-center text-indigo-600">📚</div>
      <span class="text-xs text-green-600 font-medium bg-green-50 px-2 py-0.5 rounded-full">↑ 3</span>
    </div>
    <p class="text-2xl font-black text-gray-900">42</p>
    <p class="text-xs text-gray-500 mt-0.5">บทเรียนเสร็จแล้ว</p>
  </div>
  
  <div class="bg-white rounded-2xl p-5 border border-gray-200">
    <div class="flex items-center justify-between mb-3">
      <div class="size-10 bg-orange-100 rounded-xl flex items-center justify-center text-orange-600">🔥</div>
      <span class="text-xs text-orange-600 font-medium bg-orange-50 px-2 py-0.5 rounded-full">Streak</span>
    </div>
    <p class="text-2xl font-black text-gray-900">7</p>
    <p class="text-xs text-gray-500 mt-0.5">วันติดต่อกัน</p>
  </div>
  
  <div class="bg-white rounded-2xl p-5 border border-gray-200">
    <div class="flex items-center justify-between mb-3">
      <div class="size-10 bg-purple-100 rounded-xl flex items-center justify-center text-purple-600">⭐</div>
      <span class="text-xs text-gray-500">XP</span>
    </div>
    <p class="text-2xl font-black text-gray-900">2,450</p>
    <p class="text-xs text-gray-500 mt-0.5">คะแนนสะสม</p>
  </div>
  
</div>
```

---

## Step 185: Pricing Cards

```html
<!-- Pricing cards -->
<div class="grid grid-cols-1 md:grid-cols-3 gap-6 max-w-4xl mx-auto">

  <!-- Free -->
  <div class="bg-white rounded-2xl border-2 border-gray-200 p-6">
    <h3 class="font-bold text-gray-900 text-lg">ฟรี</h3>
    <p class="text-gray-500 text-sm mt-1 mb-5">สำหรับผู้เริ่มต้น</p>
    <div class="flex items-end gap-1 mb-6">
      <span class="text-4xl font-black text-gray-900">฿0</span>
      <span class="text-gray-400 text-sm mb-1">/เดือน</span>
    </div>
    <ul class="space-y-3 mb-6">
      <li class="flex items-center gap-2 text-sm text-gray-600">
        <span class="text-green-500 flex-none">✓</span> 20 บทเรียนแรก
      </li>
      <li class="flex items-center gap-2 text-sm text-gray-600">
        <span class="text-green-500 flex-none">✓</span> Community access
      </li>
      <li class="flex items-center gap-2 text-sm text-gray-300">
        <span class="flex-none">✗</span> โปรเจกต์ขั้นสูง
      </li>
      <li class="flex items-center gap-2 text-sm text-gray-300">
        <span class="flex-none">✗</span> Certificate
      </li>
    </ul>
    <button class="w-full border-2 border-gray-200 hover:border-indigo-400 text-gray-700 font-semibold py-2.5 rounded-xl transition-colors">
      เริ่มใช้งาน
    </button>
  </div>

  <!-- Pro (featured) -->
  <div class="bg-indigo-600 text-white rounded-2xl p-6 shadow-xl shadow-indigo-600/25 relative">
    <div class="absolute -top-3 left-1/2 -translate-x-1/2 bg-yellow-400 text-yellow-900 text-xs font-black px-3 py-1 rounded-full whitespace-nowrap">
      ⭐ ยอดนิยมที่สุด
    </div>
    <h3 class="font-bold text-xl mt-2">Pro</h3>
    <p class="text-indigo-200 text-sm mt-1 mb-5">สำหรับผู้เรียนจริงจัง</p>
    <div class="flex items-end gap-1 mb-6">
      <span class="text-4xl font-black">฿299</span>
      <span class="text-indigo-300 text-sm mb-1">/เดือน</span>
    </div>
    <ul class="space-y-3 mb-6">
      <li class="flex items-center gap-2 text-sm">
        <span class="text-yellow-300 flex-none">✓</span> บทเรียนทั้งหมด 100+
      </li>
      <li class="flex items-center gap-2 text-sm">
        <span class="text-yellow-300 flex-none">✓</span> โปรเจกต์จริง 20+
      </li>
      <li class="flex items-center gap-2 text-sm">
        <span class="text-yellow-300 flex-none">✓</span> Certificate
      </li>
      <li class="flex items-center gap-2 text-sm">
        <span class="text-yellow-300 flex-none">✓</span> Q&A กับผู้สอน
      </li>
    </ul>
    <button class="w-full bg-white text-indigo-700 hover:bg-indigo-50 font-bold py-2.5 rounded-xl transition-colors">
      สมัคร Pro
    </button>
  </div>

  <!-- Enterprise -->
  <div class="bg-white rounded-2xl border-2 border-gray-200 p-6">
    <h3 class="font-bold text-gray-900 text-lg">Enterprise</h3>
    <p class="text-gray-500 text-sm mt-1 mb-5">สำหรับทีม/บริษัท</p>
    <div class="flex items-end gap-1 mb-6">
      <span class="text-2xl font-black text-gray-900">ติดต่อเรา</span>
    </div>
    <ul class="space-y-3 mb-6">
      <li class="flex items-center gap-2 text-sm text-gray-600">
        <span class="text-green-500 flex-none">✓</span> ทุกอย่างใน Pro
      </li>
      <li class="flex items-center gap-2 text-sm text-gray-600">
        <span class="text-green-500 flex-none">✓</span> Team dashboard
      </li>
      <li class="flex items-center gap-2 text-sm text-gray-600">
        <span class="text-green-500 flex-none">✓</span> Custom learning path
      </li>
      <li class="flex items-center gap-2 text-sm text-gray-600">
        <span class="text-green-500 flex-none">✓</span> Invoice billing
      </li>
    </ul>
    <button class="w-full border-2 border-gray-200 hover:border-indigo-400 text-gray-700 font-semibold py-2.5 rounded-xl transition-colors">
      ติดต่อ Sales
    </button>
  </div>
  
</div>
```

---

## Step 186: List Items

```html
<!-- Simple list -->
<ul class="divide-y divide-gray-100">
  <li class="flex items-center gap-4 py-3 hover:bg-gray-50 px-4 rounded-xl -mx-4 transition-colors">
    <div class="size-10 rounded-xl bg-blue-100 flex items-center justify-center text-blue-600 flex-none">📚</div>
    <div class="flex-1 min-w-0">
      <p class="font-medium text-gray-900 text-sm truncate">Part 01 — Introduction</p>
      <p class="text-xs text-gray-500 mt-0.5">10 Steps • 45 นาที</p>
    </div>
    <div class="flex items-center gap-2 flex-none">
      <span class="text-xs bg-green-100 text-green-700 px-2 py-0.5 rounded-full font-medium">เสร็จแล้ว</span>
      <button class="text-gray-400 hover:text-gray-600 text-sm">→</button>
    </div>
  </li>
  
  <li class="flex items-center gap-4 py-3 hover:bg-gray-50 px-4 rounded-xl -mx-4 transition-colors">
    <div class="size-10 rounded-xl bg-indigo-100 flex items-center justify-center text-indigo-600 flex-none">🎨</div>
    <div class="flex-1 min-w-0">
      <p class="font-medium text-gray-900 text-sm truncate">Part 02 — Installation</p>
      <p class="text-xs text-gray-500 mt-0.5">10 Steps • 30 นาที</p>
    </div>
    <div class="flex items-center gap-2 flex-none">
      <div class="w-16 h-1.5 bg-gray-100 rounded-full overflow-hidden">
        <div class="h-full w-3/4 bg-indigo-500 rounded-full"></div>
      </div>
      <span class="text-xs text-indigo-600 font-bold">75%</span>
    </div>
  </li>
  
  <li class="flex items-center gap-4 py-3 hover:bg-gray-50 px-4 rounded-xl -mx-4 transition-colors cursor-pointer">
    <div class="size-10 rounded-xl bg-gray-100 flex items-center justify-center text-gray-400 flex-none">🔒</div>
    <div class="flex-1 min-w-0">
      <p class="font-medium text-gray-400 text-sm truncate">Part 03 — Typography</p>
      <p class="text-xs text-gray-400 mt-0.5">ปลดล็อคด้วยแผน Pro</p>
    </div>
    <span class="text-xs bg-yellow-100 text-yellow-700 px-2 py-0.5 rounded-full font-medium flex-none">Pro</span>
  </li>
</ul>
```

---

## Step 187: Modal Dialog

```html
<!-- Modal backdrop + dialog -->
<div class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-black/50 backdrop-blur-sm">
  <div class="w-full max-w-md bg-white rounded-2xl shadow-2xl overflow-hidden">
    
    <!-- Modal header -->
    <div class="flex items-center justify-between p-6 border-b border-gray-100">
      <div>
        <h3 class="font-bold text-gray-900 text-lg">ยืนยันการลบ</h3>
        <p class="text-gray-500 text-sm mt-0.5">ไม่สามารถยกเลิกได้</p>
      </div>
      <button class="size-8 flex items-center justify-center text-gray-400 hover:text-gray-600 hover:bg-gray-100 rounded-lg transition-colors">
        ✕
      </button>
    </div>
    
    <!-- Modal body -->
    <div class="p-6">
      <div class="flex gap-4">
        <div class="flex-none size-12 bg-red-100 rounded-2xl flex items-center justify-center text-red-600 text-xl">⚠</div>
        <div>
          <p class="text-gray-700 text-sm leading-relaxed">
            คุณกำลังจะลบ <strong>"Part 01 — Introduction"</strong> ออกจากระบบ 
            ข้อมูลทั้งหมดจะหายไปและไม่สามารถกู้คืนได้
          </p>
        </div>
      </div>
    </div>
    
    <!-- Modal footer -->
    <div class="flex gap-3 px-6 pb-6">
      <button class="flex-1 border-2 border-gray-200 hover:border-gray-300 text-gray-700 font-semibold py-2.5 rounded-xl transition-colors">
        ยกเลิก
      </button>
      <button class="flex-1 bg-red-600 hover:bg-red-700 text-white font-semibold py-2.5 rounded-xl transition-colors">
        ลบทิ้ง
      </button>
    </div>
    
  </div>
</div>
```

---

## Step 188: Tooltip

```html
<!-- Tooltip on hover (CSS only) -->
<div class="relative inline-block group">
  <button class="bg-indigo-100 text-indigo-700 size-6 rounded-full text-xs font-bold">?</button>
  <div class="
    absolute bottom-full left-1/2 -translate-x-1/2 mb-2
    bg-gray-900 text-white text-xs px-3 py-1.5 rounded-lg whitespace-nowrap
    opacity-0 pointer-events-none
    group-hover:opacity-100 group-hover:pointer-events-auto
    transition-opacity duration-200
    z-50
  ">
    Tailwind CSS v4 ออกมาแล้ว!
    <div class="absolute top-full left-1/2 -translate-x-1/2 border-4 border-transparent border-t-gray-900"></div>
  </div>
</div>

<!-- Right tooltip -->
<div class="relative inline-block group">
  <span class="text-gray-500 cursor-help underline decoration-dotted">hover me</span>
  <div class="
    absolute left-full top-1/2 -translate-y-1/2 ml-2
    bg-gray-900 text-white text-xs px-3 py-1.5 rounded-lg whitespace-nowrap
    opacity-0 pointer-events-none group-hover:opacity-100
    transition-opacity z-50
  ">
    Tooltip on the right!
  </div>
</div>
```

---

## Step 189: Empty States

```html
<!-- Empty state -->
<div class="flex flex-col items-center justify-center py-16 text-center">
  <div class="text-6xl mb-4">📭</div>
  <h3 class="font-bold text-gray-900 text-lg mb-2">ยังไม่มีบทเรียน</h3>
  <p class="text-gray-500 text-sm max-w-xs mb-6">
    เริ่มต้นสร้างหลักสูตรแรกของคุณได้เลย
    ใช้เวลาไม่ถึง 5 นาที
  </p>
  <button class="bg-indigo-600 hover:bg-indigo-700 text-white font-semibold px-6 py-2.5 rounded-xl transition-colors flex items-center gap-2">
    <span>+</span> เพิ่มบทเรียน
  </button>
</div>

<!-- Search empty state -->
<div class="flex flex-col items-center justify-center py-12 text-center">
  <div class="size-16 bg-gray-100 rounded-2xl flex items-center justify-center text-3xl mb-4">🔍</div>
  <h3 class="font-semibold text-gray-900 mb-1">ไม่พบผลการค้นหา</h3>
  <p class="text-gray-500 text-sm">ลองค้นหาด้วยคำอื่น หรือล้างตัวกรอง</p>
  <button class="mt-4 text-indigo-600 text-sm font-medium hover:text-indigo-800 transition-colors">
    ล้างการค้นหา
  </button>
</div>
```

---

## Step 190: Workshop — Course Card Grid

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Cards Workshop</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 min-h-screen p-8">

<div class="max-w-6xl mx-auto">
  <div class="flex items-center justify-between mb-8">
    <div>
      <h1 class="text-2xl font-bold text-gray-900">หลักสูตรทั้งหมด</h1>
      <p class="text-gray-500 text-sm mt-1">100+ บทเรียน • 4 ระดับ</p>
    </div>
    <div class="flex gap-2">
      <button class="flex items-center gap-2 text-sm px-4 py-2 bg-white border border-gray-200 rounded-xl hover:border-gray-300 text-gray-600 transition-colors">
        ⚙ กรอง
      </button>
      <select class="text-sm px-4 py-2 bg-white border border-gray-200 rounded-xl text-gray-600 focus:outline-none focus:border-indigo-400">
        <option>ล่าสุด</option>
        <option>ยอดนิยม</option>
        <option>คะแนนสูงสุด</option>
      </select>
    </div>
  </div>

  <!-- Filter chips -->
  <div class="flex flex-wrap gap-2 mb-6">
    <button class="px-4 py-1.5 rounded-full text-sm font-medium bg-indigo-600 text-white">ทั้งหมด</button>
    <button class="px-4 py-1.5 rounded-full text-sm font-medium border border-gray-300 text-gray-600 hover:border-indigo-400 hover:bg-indigo-50 transition-colors">Level 1</button>
    <button class="px-4 py-1.5 rounded-full text-sm font-medium border border-gray-300 text-gray-600 hover:border-indigo-400 hover:bg-indigo-50 transition-colors">Level 2</button>
    <button class="px-4 py-1.5 rounded-full text-sm font-medium border border-gray-300 text-gray-600 hover:border-indigo-400 hover:bg-indigo-50 transition-colors">Level 3</button>
    <button class="px-4 py-1.5 rounded-full text-sm font-medium border border-gray-300 text-gray-600 hover:border-indigo-400 hover:bg-indigo-50 transition-colors">Level 4</button>
  </div>

  <!-- Cards grid -->
  <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
    
    <!-- Card 1 -->
    <div class="bg-white rounded-2xl overflow-hidden shadow-sm hover:shadow-xl transition-all duration-300 group border border-gray-100">
      <div class="relative overflow-hidden h-44 bg-gradient-to-br from-blue-500 to-indigo-700">
        <div class="absolute inset-0 flex items-center justify-center text-6xl group-hover:scale-110 transition-transform duration-500">🎨</div>
        <div class="absolute top-3 left-3">
          <span class="text-xs font-bold bg-white/90 text-indigo-700 px-2.5 py-1 rounded-full">Level 1</span>
        </div>
        <div class="absolute top-3 right-3">
          <span class="text-xs bg-green-500 text-white font-bold px-2 py-0.5 rounded-full">ฟรี</span>
        </div>
      </div>
      <div class="p-5">
        <div class="text-xs text-gray-400 mb-2">Part 01 — 10 Steps</div>
        <h3 class="font-bold text-gray-900 group-hover:text-indigo-600 transition-colors">Introduction to Tailwind</h3>
        <p class="text-gray-500 text-xs mt-1.5 line-clamp-2 leading-relaxed">เรียนรู้ Tailwind CSS ตั้งแต่พื้นฐาน utility-first approach และการติดตั้ง</p>
        <div class="flex items-center justify-between mt-4 pt-4 border-t border-gray-100">
          <div class="flex items-center gap-1.5">
            <div class="size-5 rounded-full bg-indigo-600 flex items-center justify-center text-white text-[10px] font-bold">A</div>
            <span class="text-xs text-gray-500">อาจารย์สมชาย</span>
          </div>
          <div class="flex items-center gap-1 text-xs">
            <span class="text-yellow-400">★</span>
            <span class="font-medium text-gray-700">4.9</span>
            <span class="text-gray-400">(48)</span>
          </div>
        </div>
        <div class="mt-3">
          <div class="flex justify-between text-xs text-gray-500 mb-1">
            <span>Progress</span>
            <span class="text-green-600 font-bold">100%</span>
          </div>
          <div class="h-1.5 bg-gray-100 rounded-full">
            <div class="h-full w-full bg-green-500 rounded-full"></div>
          </div>
        </div>
      </div>
    </div>

    <!-- Card 2 -->
    <div class="bg-white rounded-2xl overflow-hidden shadow-sm hover:shadow-xl transition-all duration-300 group border border-gray-100">
      <div class="relative overflow-hidden h-44 bg-gradient-to-br from-green-400 to-emerald-600">
        <div class="absolute inset-0 flex items-center justify-center text-6xl group-hover:scale-110 transition-transform duration-500">📐</div>
        <div class="absolute top-3 left-3">
          <span class="text-xs font-bold bg-white/90 text-green-700 px-2.5 py-1 rounded-full">Level 1</span>
        </div>
      </div>
      <div class="p-5">
        <div class="text-xs text-gray-400 mb-2">Part 07–08 — 20 Steps</div>
        <h3 class="font-bold text-gray-900 group-hover:text-green-600 transition-colors">Flexbox & Grid Layout</h3>
        <p class="text-gray-500 text-xs mt-1.5 line-clamp-2 leading-relaxed">Layout ทุกรูปแบบด้วย Flexbox และ CSS Grid อย่างมืออาชีพ</p>
        <div class="flex items-center justify-between mt-4 pt-4 border-t border-gray-100">
          <div class="flex items-center gap-1.5">
            <div class="size-5 rounded-full bg-green-600 flex items-center justify-center text-white text-[10px] font-bold">A</div>
            <span class="text-xs text-gray-500">อาจารย์สมชาย</span>
          </div>
          <div class="flex items-center gap-1 text-xs">
            <span class="text-yellow-400">★</span>
            <span class="font-medium text-gray-700">4.8</span>
          </div>
        </div>
        <div class="mt-3">
          <div class="flex justify-between text-xs text-gray-500 mb-1">
            <span>Progress</span>
            <span class="text-indigo-600 font-bold">60%</span>
          </div>
          <div class="h-1.5 bg-gray-100 rounded-full">
            <div class="h-full w-[60%] bg-indigo-500 rounded-full"></div>
          </div>
        </div>
      </div>
    </div>

    <!-- Card 3 — Locked -->
    <div class="bg-white rounded-2xl overflow-hidden shadow-sm border border-gray-100 opacity-75">
      <div class="relative overflow-hidden h-44 bg-gradient-to-br from-purple-400 to-pink-600">
        <div class="absolute inset-0 flex items-center justify-center text-6xl">✨</div>
        <div class="absolute top-3 left-3">
          <span class="text-xs font-bold bg-white/90 text-purple-700 px-2.5 py-1 rounded-full">Level 2</span>
        </div>
        <div class="absolute top-3 right-3">
          <span class="text-xs bg-yellow-500 text-yellow-900 font-bold px-2 py-0.5 rounded-full">⭐ Pro</span>
        </div>
        <!-- Lock overlay -->
        <div class="absolute inset-0 bg-black/30 flex items-center justify-center">
          <div class="bg-white/90 rounded-full px-4 py-2 flex items-center gap-2">
            <span class="text-sm">🔒</span>
            <span class="text-xs font-bold text-gray-700">Pro เท่านั้น</span>
          </div>
        </div>
      </div>
      <div class="p-5">
        <div class="text-xs text-gray-400 mb-2">Part 21–30 — 100 Steps</div>
        <h3 class="font-bold text-gray-700">Tailwind Configuration</h3>
        <p class="text-gray-400 text-xs mt-1.5 line-clamp-2">ปรับแต่ง Tailwind ให้ตรงกับ Design System ของโปรเจกต์</p>
        <button class="mt-4 w-full bg-yellow-500 hover:bg-yellow-600 text-yellow-900 font-bold text-sm py-2.5 rounded-xl transition-colors">
          ⭐ อัพเกรด Pro
        </button>
      </div>
    </div>

  </div>
</div>

</body>
</html>
```

---

## 📝 สรุป Part 19

| Component | Pattern สำคัญ |
|-----------|--------------|
| Image Card | `overflow-hidden` + `group-hover:scale-105 transition-transform` |
| Profile | `relative inline-block` + status dot |
| Stat Card | Icon + number + trend badge |
| Pricing | Featured: `bg-indigo-600 text-white shadow-xl` |
| List Item | `divide-y divide-gray-100` + `hover:bg-gray-50` |
| Modal | `fixed inset-0 bg-black/50 flex items-center justify-center` |
| Tooltip | `group` + `group-hover:opacity-100` |
| Empty State | centered content + CTA button |

---

*Part 19 — จาก 100 Parts | Steps 181–190 จาก 1,000 Steps*
