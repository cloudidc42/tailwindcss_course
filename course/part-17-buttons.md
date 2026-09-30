# Part 17: Buttons และ UI Components
## Steps 161–170: Button Design System

---

## 🎯 เป้าหมายของ Part นี้

- Button variants (primary, secondary, outline, ghost, link)
- Button sizes
- Icon buttons
- Button groups
- Loading states
- Badge, Tag, Chip components
- Alert / Toast components

---

## Step 161: Button Variants

```html
<!-- Primary -->
<button class="bg-indigo-600 hover:bg-indigo-700 active:bg-indigo-800 active:scale-[0.98] text-white font-semibold px-5 py-2.5 rounded-xl transition-all shadow-sm hover:shadow-md">
  Primary
</button>

<!-- Secondary -->
<button class="bg-gray-100 hover:bg-gray-200 active:bg-gray-300 text-gray-900 font-semibold px-5 py-2.5 rounded-xl transition-all">
  Secondary
</button>

<!-- Outline -->
<button class="border-2 border-indigo-600 text-indigo-600 hover:bg-indigo-600 hover:text-white active:scale-[0.98] font-semibold px-5 py-2.5 rounded-xl transition-all">
  Outline
</button>

<!-- Ghost -->
<button class="text-indigo-600 hover:bg-indigo-50 active:bg-indigo-100 font-semibold px-5 py-2.5 rounded-xl transition-all">
  Ghost
</button>

<!-- Link style -->
<button class="text-indigo-600 hover:text-indigo-800 underline-offset-2 hover:underline font-medium transition-colors">
  Link Button
</button>

<!-- Danger -->
<button class="bg-red-600 hover:bg-red-700 active:bg-red-800 text-white font-semibold px-5 py-2.5 rounded-xl transition-all">
  Delete
</button>

<!-- Success -->
<button class="bg-green-600 hover:bg-green-700 text-white font-semibold px-5 py-2.5 rounded-xl transition-all">
  Confirm
</button>

<!-- Warning -->
<button class="bg-yellow-500 hover:bg-yellow-600 text-white font-semibold px-5 py-2.5 rounded-xl transition-all">
  Warning
</button>
```

---

## Step 162: Button Sizes

```html
<!-- XS -->
<button class="bg-indigo-600 text-white text-xs font-medium px-3 py-1.5 rounded-lg">XS Button</button>

<!-- SM -->
<button class="bg-indigo-600 text-white text-sm font-medium px-4 py-2 rounded-lg">SM Button</button>

<!-- MD (default) -->
<button class="bg-indigo-600 text-white text-sm font-semibold px-5 py-2.5 rounded-xl">MD Button</button>

<!-- LG -->
<button class="bg-indigo-600 text-white font-semibold px-6 py-3 rounded-xl">LG Button</button>

<!-- XL -->
<button class="bg-indigo-600 text-white text-lg font-semibold px-8 py-4 rounded-2xl">XL Button</button>

<!-- Full width -->
<button class="w-full bg-indigo-600 text-white font-semibold px-5 py-3 rounded-xl">Full Width</button>
```

---

## Step 163: Icon Buttons

```html
<!-- Icon + text -->
<button class="flex items-center gap-2 bg-indigo-600 hover:bg-indigo-700 text-white font-semibold px-5 py-2.5 rounded-xl transition-all">
  <svg class="size-4" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
    <path stroke-linecap="round" stroke-linejoin="round" d="M12 4v16m8-8H4"/>
  </svg>
  เพิ่มรายการ
</button>

<!-- Icon right -->
<button class="flex items-center gap-2 bg-indigo-600 hover:bg-indigo-700 text-white font-semibold px-5 py-2.5 rounded-xl transition-all group">
  เริ่มเรียน
  <svg class="size-4 group-hover:translate-x-0.5 transition-transform" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
    <path stroke-linecap="round" stroke-linejoin="round" d="M13 7l5 5m0 0l-5 5m5-5H6"/>
  </svg>
</button>

<!-- Icon only (square) -->
<button class="size-10 bg-gray-100 hover:bg-gray-200 rounded-xl flex items-center justify-center text-gray-600 transition-colors" aria-label="Settings">
  ⚙
</button>

<!-- Icon only (round) -->
<button class="size-10 bg-indigo-600 hover:bg-indigo-700 rounded-full flex items-center justify-center text-white transition-colors shadow-lg shadow-indigo-600/30" aria-label="Add">
  +
</button>

<!-- FAB (Floating Action Button) -->
<button class="fixed bottom-6 right-6 size-14 bg-indigo-600 hover:bg-indigo-700 active:scale-95 rounded-full flex items-center justify-center text-white text-2xl shadow-xl shadow-indigo-600/30 transition-all z-50">
  +
</button>
```

---

## Step 164: Button States

```html
<!-- Loading state -->
<button 
  disabled
  class="flex items-center gap-2 bg-indigo-600 text-white font-semibold px-5 py-2.5 rounded-xl disabled:opacity-80 cursor-wait"
>
  <svg class="size-4 animate-spin" fill="none" viewBox="0 0 24 24">
    <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"/>
    <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4z"/>
  </svg>
  กำลังโหลด...
</button>

<!-- Success state -->
<button class="flex items-center gap-2 bg-green-600 text-white font-semibold px-5 py-2.5 rounded-xl">
  <span>✓</span> บันทึกสำเร็จ!
</button>

<!-- Disabled -->
<button 
  disabled
  class="bg-indigo-600 text-white font-semibold px-5 py-2.5 rounded-xl disabled:bg-gray-300 disabled:text-gray-500 disabled:cursor-not-allowed"
>
  Disabled
</button>

<!-- With count/badge -->
<button class="relative flex items-center gap-2 bg-white border border-gray-200 hover:border-gray-300 text-gray-700 font-medium px-4 py-2 rounded-xl transition-colors">
  <span>📢</span>
  Notifications
  <span class="absolute -top-1.5 -right-1.5 size-5 bg-red-500 text-white text-xs font-bold rounded-full flex items-center justify-center">
    3
  </span>
</button>
```

---

## Step 165: Button Groups

```html
<!-- Segmented control -->
<div class="inline-flex bg-gray-100 p-1 rounded-xl gap-1">
  <button class="px-4 py-1.5 text-sm font-medium rounded-lg bg-white shadow text-gray-900">
    รายเดือน
  </button>
  <button class="px-4 py-1.5 text-sm font-medium rounded-lg text-gray-500 hover:text-gray-700">
    รายปี
  </button>
  <button class="px-4 py-1.5 text-sm font-medium rounded-lg text-gray-500 hover:text-gray-700">
    ตลอดชีพ
  </button>
</div>

<!-- Joined button group -->
<div class="inline-flex">
  <button class="px-4 py-2 text-sm font-medium bg-white border border-gray-300 rounded-l-lg hover:bg-gray-50 text-gray-700">
    Left
  </button>
  <button class="px-4 py-2 text-sm font-medium bg-white border-y border-gray-300 hover:bg-gray-50 text-gray-700">
    Center
  </button>
  <button class="px-4 py-2 text-sm font-medium bg-white border border-gray-300 rounded-r-lg hover:bg-gray-50 text-gray-700">
    Right
  </button>
</div>

<!-- Icon group -->
<div class="inline-flex gap-1">
  <button class="px-3 py-2 text-sm border border-gray-200 rounded-lg hover:bg-gray-50 hover:border-gray-300 text-gray-600 transition-all" title="Bold">
    <strong>B</strong>
  </button>
  <button class="px-3 py-2 text-sm border border-gray-200 rounded-lg hover:bg-gray-50 hover:border-gray-300 text-gray-600 transition-all" title="Italic">
    <em>I</em>
  </button>
  <button class="px-3 py-2 text-sm border border-gray-200 rounded-lg hover:bg-gray-50 hover:border-gray-300 text-gray-600 transition-all" title="Underline">
    <u>U</u>
  </button>
</div>
```

---

## Step 166: Badge และ Tag

```html
<!-- Status badges -->
<span class="inline-flex items-center gap-1 text-xs font-semibold px-2.5 py-1 rounded-full bg-green-100 text-green-700">
  <span class="size-1.5 rounded-full bg-green-500"></span>
  Active
</span>

<span class="text-xs font-semibold px-2.5 py-1 rounded-full bg-yellow-100 text-yellow-700">Pending</span>
<span class="text-xs font-semibold px-2.5 py-1 rounded-full bg-red-100 text-red-700">Cancelled</span>
<span class="text-xs font-semibold px-2.5 py-1 rounded-full bg-blue-100 text-blue-700">In Progress</span>
<span class="text-xs font-semibold px-2.5 py-1 rounded-full bg-gray-100 text-gray-600">Draft</span>

<!-- Tag with remove -->
<span class="inline-flex items-center gap-1.5 text-sm bg-indigo-100 text-indigo-700 px-3 py-1 rounded-full">
  Tailwind CSS
  <button class="hover:bg-indigo-200 rounded-full size-4 flex items-center justify-center text-xs transition-colors">✕</button>
</span>

<!-- Count badge -->
<div class="relative inline-block">
  <button class="text-gray-600 hover:text-gray-900 size-9 flex items-center justify-center rounded-lg hover:bg-gray-100">🔔</button>
  <span class="absolute -top-1 -right-1 size-4 bg-red-500 text-white text-[10px] font-bold rounded-full flex items-center justify-center">5</span>
</div>

<!-- NEW label (skewed) -->
<span class="inline-block bg-indigo-600 text-white text-xs font-bold px-2.5 py-0.5 rounded -skew-x-6">
  <span class="inline-block skew-x-6">NEW</span>
</span>

<!-- PRO badge -->
<span class="inline-flex items-center gap-1 text-xs font-bold px-2 py-0.5 rounded-md bg-gradient-to-r from-yellow-400 to-orange-400 text-white">
  ⭐ PRO
</span>
```

---

## Step 167: Alert Component

```html
<!-- Info alert -->
<div class="flex gap-3 p-4 bg-blue-50 border border-blue-200 rounded-xl text-blue-800">
  <span class="flex-none text-lg">ℹ</span>
  <div>
    <p class="font-semibold text-sm">หมายเหตุ</p>
    <p class="text-sm mt-0.5 text-blue-700">บทเรียนนี้ต้องการความรู้พื้นฐาน HTML/CSS</p>
  </div>
  <button class="flex-none ml-auto text-blue-400 hover:text-blue-600 text-sm">✕</button>
</div>

<!-- Success alert -->
<div class="flex gap-3 p-4 bg-green-50 border border-green-200 rounded-xl">
  <span class="flex-none text-green-600 text-lg">✓</span>
  <div class="flex-1">
    <p class="font-semibold text-sm text-green-800">บันทึกสำเร็จ!</p>
    <p class="text-sm text-green-700 mt-0.5">ข้อมูลของคุณถูกบันทึกเรียบร้อยแล้ว</p>
  </div>
</div>

<!-- Warning alert -->
<div class="flex gap-3 p-4 bg-yellow-50 border border-yellow-200 rounded-xl">
  <span class="flex-none text-yellow-600 text-lg">⚠</span>
  <div>
    <p class="font-semibold text-sm text-yellow-800">คำเตือน</p>
    <p class="text-sm text-yellow-700 mt-0.5">session ของคุณจะหมดอายุใน 5 นาที</p>
  </div>
</div>

<!-- Error alert -->
<div class="flex gap-3 p-4 bg-red-50 border border-red-200 rounded-xl">
  <span class="flex-none text-red-600 text-lg">✕</span>
  <div>
    <p class="font-semibold text-sm text-red-800">เกิดข้อผิดพลาด</p>
    <p class="text-sm text-red-700 mt-0.5">ไม่สามารถเชื่อมต่อกับเซิร์ฟเวอร์ได้ กรุณาลองใหม่อีกครั้ง</p>
  </div>
  <button class="flex-none ml-auto text-red-400 hover:text-red-600">✕</button>
</div>
```

---

## Step 168: Toast Notification

```html
<!-- Toast container (bottom-right) -->
<div class="fixed bottom-4 right-4 z-50 space-y-2 w-72">

  <!-- Success toast -->
  <div class="flex items-start gap-3 bg-white border border-gray-200 rounded-xl p-4 shadow-lg">
    <div class="flex-none size-8 bg-green-100 rounded-lg flex items-center justify-center text-green-600">✓</div>
    <div class="flex-1 min-w-0">
      <p class="font-semibold text-sm text-gray-900">บันทึกสำเร็จ</p>
      <p class="text-xs text-gray-500 mt-0.5">ข้อมูลถูกบันทึกเรียบร้อย</p>
    </div>
    <button class="flex-none text-gray-400 hover:text-gray-600 text-xs mt-0.5">✕</button>
  </div>

  <!-- Progress toast -->
  <div class="bg-white border border-gray-200 rounded-xl p-4 shadow-lg overflow-hidden">
    <div class="flex items-center gap-3 mb-3">
      <div class="flex-none size-8 bg-indigo-100 rounded-lg flex items-center justify-center">
        <div class="size-4 border-2 border-indigo-600 border-t-transparent rounded-full animate-spin"></div>
      </div>
      <div>
        <p class="font-semibold text-sm text-gray-900">กำลังอัพโหลด...</p>
        <p class="text-xs text-gray-500">photo.jpg (2.4 MB)</p>
      </div>
    </div>
    <div class="h-1.5 bg-gray-100 rounded-full overflow-hidden">
      <div class="h-full w-2/3 bg-indigo-500 rounded-full"></div>
    </div>
  </div>

</div>
```

---

## Step 169: Chip / Filter Tags

```html
<!-- Filter chips -->
<div class="flex flex-wrap gap-2">
  
  <button class="flex items-center gap-1.5 px-4 py-1.5 rounded-full text-sm font-medium bg-indigo-600 text-white">
    ทั้งหมด
  </button>
  
  <button class="flex items-center gap-1.5 px-4 py-1.5 rounded-full text-sm font-medium border border-gray-300 text-gray-700 hover:border-indigo-400 hover:bg-indigo-50 transition-colors">
    พื้นฐาน
  </button>
  
  <button class="flex items-center gap-1.5 px-4 py-1.5 rounded-full text-sm font-medium border border-gray-300 text-gray-700 hover:border-indigo-400 hover:bg-indigo-50 transition-colors">
    ขั้นสูง
  </button>
  
  <button class="flex items-center gap-1.5 px-4 py-1.5 rounded-full text-sm font-medium border border-indigo-400 bg-indigo-50 text-indigo-700">
    React
    <span class="text-xs bg-indigo-600 text-white rounded-full px-1.5 py-0.5">12</span>
  </button>
  
</div>

<!-- Removable chips -->
<div class="flex flex-wrap gap-2">
  <span class="inline-flex items-center gap-1.5 px-3 py-1.5 rounded-full text-sm bg-indigo-100 text-indigo-700 font-medium">
    🎨 Design
    <button class="hover:bg-indigo-200 size-4 rounded-full flex items-center justify-center text-xs transition-colors">✕</button>
  </span>
  <span class="inline-flex items-center gap-1.5 px-3 py-1.5 rounded-full text-sm bg-green-100 text-green-700 font-medium">
    ⚡ Performance
    <button class="hover:bg-green-200 size-4 rounded-full flex items-center justify-center text-xs transition-colors">✕</button>
  </span>
  <span class="inline-flex items-center gap-1.5 px-3 py-1.5 rounded-full text-sm bg-purple-100 text-purple-700 font-medium">
    🔮 Advanced
    <button class="hover:bg-purple-200 size-4 rounded-full flex items-center justify-center text-xs transition-colors">✕</button>
  </span>
</div>
```

---

## Step 170: Workshop — Component Gallery

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Component Gallery</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-8">

<div class="max-w-4xl mx-auto space-y-10">

  <h1 class="text-3xl font-bold text-gray-900">UI Component Gallery</h1>

  <!-- Buttons Section -->
  <section class="bg-white rounded-2xl p-6 shadow-sm border border-gray-100">
    <h2 class="font-bold text-gray-900 mb-4">Buttons</h2>
    <div class="flex flex-wrap gap-3">
      <button class="bg-indigo-600 hover:bg-indigo-700 active:scale-95 text-white font-semibold px-5 py-2.5 rounded-xl transition-all">Primary</button>
      <button class="bg-gray-100 hover:bg-gray-200 text-gray-900 font-semibold px-5 py-2.5 rounded-xl transition-all">Secondary</button>
      <button class="border-2 border-indigo-600 text-indigo-600 hover:bg-indigo-600 hover:text-white font-semibold px-5 py-2.5 rounded-xl transition-all">Outline</button>
      <button class="text-indigo-600 hover:bg-indigo-50 font-semibold px-5 py-2.5 rounded-xl transition-all">Ghost</button>
      <button disabled class="bg-gray-200 text-gray-400 font-semibold px-5 py-2.5 rounded-xl cursor-not-allowed">Disabled</button>
      <button class="flex items-center gap-2 bg-indigo-600 hover:bg-indigo-700 text-white font-semibold px-5 py-2.5 rounded-xl transition-all">
        <div class="size-4 border-2 border-white/30 border-t-white rounded-full animate-spin"></div>
        Loading
      </button>
      <button class="bg-red-600 hover:bg-red-700 text-white font-semibold px-5 py-2.5 rounded-xl">Danger</button>
      <button class="bg-green-600 hover:bg-green-700 text-white font-semibold px-5 py-2.5 rounded-xl">Success</button>
    </div>
    
    <div class="mt-4 flex flex-wrap gap-2 items-center">
      <button class="bg-indigo-600 text-white text-xs px-3 py-1.5 rounded-lg">XS</button>
      <button class="bg-indigo-600 text-white text-sm px-4 py-2 rounded-lg">SM</button>
      <button class="bg-indigo-600 text-white font-semibold px-5 py-2.5 rounded-xl">MD</button>
      <button class="bg-indigo-600 text-white font-semibold px-6 py-3 rounded-xl text-base">LG</button>
      <button class="bg-indigo-600 text-white font-semibold px-8 py-4 rounded-2xl text-lg">XL</button>
    </div>
  </section>

  <!-- Badges Section -->
  <section class="bg-white rounded-2xl p-6 shadow-sm border border-gray-100">
    <h2 class="font-bold text-gray-900 mb-4">Badges & Tags</h2>
    <div class="flex flex-wrap gap-3">
      <span class="text-xs font-semibold px-2.5 py-1 rounded-full bg-green-100 text-green-700">Active</span>
      <span class="text-xs font-semibold px-2.5 py-1 rounded-full bg-yellow-100 text-yellow-700">Pending</span>
      <span class="text-xs font-semibold px-2.5 py-1 rounded-full bg-red-100 text-red-700">Error</span>
      <span class="text-xs font-semibold px-2.5 py-1 rounded-full bg-blue-100 text-blue-700">Info</span>
      <span class="text-xs font-semibold px-2.5 py-1 rounded-full bg-gray-100 text-gray-600">Draft</span>
      <span class="inline-flex items-center gap-1.5 text-xs font-semibold px-2.5 py-1 rounded-full bg-green-100 text-green-700">
        <span class="size-1.5 rounded-full bg-green-500"></span> Online
      </span>
      <span class="inline-flex items-center gap-1 text-xs font-bold px-2 py-0.5 rounded-md bg-gradient-to-r from-yellow-400 to-orange-400 text-white">⭐ PRO</span>
    </div>
  </section>

  <!-- Alerts Section -->
  <section class="bg-white rounded-2xl p-6 shadow-sm border border-gray-100 space-y-3">
    <h2 class="font-bold text-gray-900 mb-4">Alerts</h2>
    <div class="flex gap-3 p-4 bg-blue-50 border border-blue-200 rounded-xl text-blue-800">
      <span class="flex-none text-lg">ℹ</span>
      <p class="text-sm">คุณมีบทเรียนใหม่ 3 บท รอการตรวจสอบ</p>
    </div>
    <div class="flex gap-3 p-4 bg-green-50 border border-green-200 rounded-xl">
      <span class="text-green-600 flex-none text-lg">✓</span>
      <p class="text-sm text-green-800">อัพโหลดไฟล์สำเร็จ! ขนาด 2.4 MB</p>
    </div>
    <div class="flex gap-3 p-4 bg-yellow-50 border border-yellow-200 rounded-xl">
      <span class="text-yellow-600 flex-none text-lg">⚠</span>
      <p class="text-sm text-yellow-800">พื้นที่จัดเก็บเหลือน้อยกว่า 10%</p>
    </div>
    <div class="flex gap-3 p-4 bg-red-50 border border-red-200 rounded-xl">
      <span class="text-red-600 flex-none text-lg">✕</span>
      <p class="text-sm text-red-800">การชำระเงินล้มเหลว กรุณาตรวจสอบข้อมูลบัตร</p>
    </div>
  </section>

</div>

</body>
</html>
```

---

## 📝 สรุป Part 17

| Component | Key Pattern |
|-----------|------------|
| Primary btn | `bg-indigo-600 hover:bg-indigo-700 active:scale-[0.98]` |
| Outline btn | `border-2 border-indigo-600 hover:bg-indigo-600 hover:text-white` |
| Ghost btn | `hover:bg-indigo-50 text-indigo-600` |
| Loading btn | `disabled` + `animate-spin` spinner |
| Badge | `text-xs px-2.5 py-1 rounded-full bg-green-100 text-green-700` |
| Alert | `flex gap-3 p-4 bg-green-50 border border-green-200 rounded-xl` |
| Toggle chip | `peer sr-only` + `peer-checked:bg-indigo-600` |

---

*Part 17 — จาก 100 Parts | Steps 161–170 จาก 1,000 Steps*
