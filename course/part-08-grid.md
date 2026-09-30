# Part 08: Grid Layout พื้นฐาน
## Steps 71–80: CSS Grid ที่ทรงพลังด้วย Tailwind

---

## 🎯 เป้าหมายของ Part นี้

- Grid Template Columns/Rows
- Grid Placement (Column/Row Span)
- Grid Auto
- Grid Flow
- Grid Alignment
- Responsive Grid
- Complex Grid Layouts

---

## Step 71: เปิดใช้ Grid

```html
<!-- เปิด Grid -->
<div class="grid">
  <div>Item 1</div>
  <div>Item 2</div>
</div>

<!-- Inline Grid -->
<div class="inline-grid">
  ...
</div>
```

### Grid Template Columns

```html
<!-- จำนวน column ที่แน่นอน -->
<div class="grid grid-cols-1">1 column</div>
<div class="grid grid-cols-2">2 columns เท่าๆ กัน</div>
<div class="grid grid-cols-3">3 columns</div>
<div class="grid grid-cols-4">4 columns</div>
<div class="grid grid-cols-5">5 columns</div>
<div class="grid grid-cols-6">6 columns</div>
<div class="grid grid-cols-7">7 columns</div>
<div class="grid grid-cols-8">8 columns</div>
<div class="grid grid-cols-9">9 columns</div>
<div class="grid grid-cols-10">10 columns</div>
<div class="grid grid-cols-11">11 columns</div>
<div class="grid grid-cols-12">12 columns</div>
<div class="grid grid-cols-none">ไม่กำหนด columns</div>
<div class="grid grid-cols-subgrid">subgrid</div>
```

### Grid Template Rows

```html
<div class="grid grid-rows-1">1 row</div>
<div class="grid grid-rows-2">2 rows</div>
<div class="grid grid-rows-3">3 rows</div>
<div class="grid grid-rows-4">4 rows</div>
<div class="grid grid-rows-5">5 rows</div>
<div class="grid grid-rows-6">6 rows</div>
<div class="grid grid-rows-none">ไม่กำหนด</div>
```

---

## Step 72: Column Span (colspan)

```html
<div class="grid grid-cols-12 gap-4">
  
  <!-- Span ข้าม column -->
  <div class="col-span-12">เต็มแถว</div>
  <div class="col-span-6">ครึ่งหนึ่ง</div>
  <div class="col-span-6">ครึ่งหนึ่ง</div>
  <div class="col-span-4">1/3</div>
  <div class="col-span-4">1/3</div>
  <div class="col-span-4">1/3</div>
  <div class="col-span-3">1/4</div>
  <div class="col-span-9">3/4</div>
  
  <!-- Span ค่าอื่นๆ -->
  <div class="col-span-1">1</div>
  <div class="col-span-2">2</div>
  <div class="col-span-full">เต็มทุก column</div>
  
</div>
```

### Column Start/End

```html
<div class="grid grid-cols-12 gap-4">
  
  <!-- เริ่มที่ column 3 สิ้นสุดที่ column 7 -->
  <div class="col-start-3 col-end-7">span col 3-6</div>
  
  <!-- ระบุ start/end ชัดเจน -->
  <div class="col-start-1 col-end-5">col 1-4</div>
  <div class="col-start-5 col-end-9">col 5-8</div>
  <div class="col-start-9 col-end-13">col 9-12</div>
  
  <!-- Auto placement -->
  <div class="col-start-auto col-end-auto">auto</div>
  
</div>
```

---

## Step 73: Row Span

```html
<div class="grid grid-cols-3 grid-rows-3 gap-4 h-64">
  
  <!-- Span แนวตั้ง 2 rows -->
  <div class="row-span-2 bg-blue-200">Tall item</div>
  <div class="bg-green-200">Item</div>
  <div class="bg-yellow-200">Item</div>
  <div class="bg-red-200">Item</div>
  <div class="bg-purple-200">Item</div>
  <div class="row-span-2 bg-pink-200">Tall item 2</div>
  
</div>
```

---

## Step 74: Grid Auto Flow

```html
<!-- Auto flow direction -->
<div class="grid grid-flow-row">default (rows)</div>
<div class="grid grid-flow-col">columns first</div>
<div class="grid grid-flow-dense">fill gaps (dense)</div>
<div class="grid grid-flow-row-dense">row + dense</div>
<div class="grid grid-flow-col-dense">col + dense</div>
```

### Grid Auto Columns/Rows

```html
<div class="grid grid-cols-3 auto-cols-auto">
  Column ขนาด auto
</div>

<div class="grid grid-cols-3 auto-cols-min">
  Column ขนาดน้อยสุด
</div>

<div class="grid grid-cols-3 auto-cols-max">
  Column ขนาดมากสุด
</div>

<div class="grid grid-cols-3 auto-cols-fr">
  Column 1 fraction
</div>

<div class="grid auto-rows-auto">row auto</div>
<div class="grid auto-rows-min">row min</div>
<div class="grid auto-rows-max">row max</div>
<div class="grid auto-rows-fr">row fraction</div>
```

---

## Step 75: Grid Alignment

### Justify Items (inline axis)

```html
<div class="grid grid-cols-3 justify-items-start">ชิดซ้าย</div>
<div class="grid grid-cols-3 justify-items-center">จัดกลาง</div>
<div class="grid grid-cols-3 justify-items-end">ชิดขวา</div>
<div class="grid grid-cols-3 justify-items-stretch">ยืดเต็ม (default)</div>
```

### Align Items (block axis)

```html
<div class="grid grid-cols-3 items-start">ชิดบน</div>
<div class="grid grid-cols-3 items-center">จัดกลาง</div>
<div class="grid grid-cols-3 items-end">ชิดล่าง</div>
<div class="grid grid-cols-3 items-stretch">ยืดเต็ม</div>
```

### Justify Self / Align Self

```html
<div class="grid grid-cols-3">
  <div class="justify-self-start">ชิดซ้าย</div>
  <div class="justify-self-center">กลาง</div>
  <div class="justify-self-end">ชิดขวา</div>
</div>

<div class="grid grid-rows-3 h-48">
  <div class="self-start">บน</div>
  <div class="self-center">กลาง</div>
  <div class="self-end">ล่าง</div>
</div>
```

---

## Step 76: Arbitrary Grid Templates

```html
<!-- Custom column widths -->
<div class="grid grid-cols-[200px_1fr_200px]">
  <div>Fixed 200px</div>
  <div>Flexible</div>
  <div>Fixed 200px</div>
</div>

<!-- Repeat -->
<div class="grid grid-cols-[repeat(3,minmax(200px,1fr))]">
  3 columns min 200px
</div>

<!-- Complex template -->
<div class="grid grid-cols-[1fr_2fr] grid-rows-[auto_1fr_auto] min-h-screen">
  <header class="col-span-2">Header</header>
  <aside>Sidebar</aside>
  <main>Main</main>
  <footer class="col-span-2">Footer</footer>
</div>
```

---

## Step 77: Responsive Grid

```html
<!-- 1 col → 2 col → 3 col → 4 col -->
<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4">
  <div>Card</div>
  <div>Card</div>
  <div>Card</div>
  <div>Card</div>
</div>

<!-- Auto-fit columns (responsive without breakpoints) -->
<div class="grid grid-cols-[repeat(auto-fit,minmax(250px,1fr))] gap-4">
  Items ปรับจำนวน column อัตโนมัติ
</div>

<!-- Responsive span -->
<div class="grid grid-cols-12 gap-4">
  <div class="col-span-12 md:col-span-8">Main content</div>
  <div class="col-span-12 md:col-span-4">Sidebar</div>
</div>
```

---

## Step 78: Grid Layout Patterns

### 1. Holy Grail Layout

```html
<div class="grid grid-cols-[200px_1fr_200px] grid-rows-[auto_1fr_auto] min-h-screen">
  <header class="col-span-3 bg-indigo-600 text-white p-4">Header</header>
  <aside class="bg-gray-100 p-4">Left Sidebar</aside>
  <main class="p-6">Main Content</main>
  <aside class="bg-gray-100 p-4">Right Sidebar</aside>
  <footer class="col-span-3 bg-gray-900 text-white p-4">Footer</footer>
</div>
```

### 2. Bento Grid

```html
<div class="grid grid-cols-4 grid-rows-3 gap-4 h-[600px]">
  <!-- Large feature card -->
  <div class="col-span-2 row-span-2 bg-indigo-500 rounded-2xl p-6 text-white flex flex-col justify-end">
    <h3 class="text-2xl font-bold">Feature Card ใหญ่</h3>
    <p class="text-indigo-200 mt-1">คำอธิบาย</p>
  </div>
  
  <!-- Small cards -->
  <div class="bg-purple-500 rounded-2xl p-4 text-white flex flex-col justify-center items-center">
    <span class="text-3xl mb-2">📊</span>
    <span class="font-semibold text-sm">Analytics</span>
  </div>
  
  <div class="bg-pink-500 rounded-2xl p-4 text-white flex flex-col justify-center items-center">
    <span class="text-3xl mb-2">🚀</span>
    <span class="font-semibold text-sm">Performance</span>
  </div>
  
  <div class="col-span-2 bg-blue-500 rounded-2xl p-4 text-white flex items-center gap-4">
    <span class="text-4xl">🌟</span>
    <div>
      <h4 class="font-bold">Wide Card</h4>
      <p class="text-blue-200 text-sm">สองช่อง</p>
    </div>
  </div>
  
  <!-- Bottom row -->
  <div class="bg-amber-500 rounded-2xl p-4 text-white flex items-center justify-center">
    <span class="text-2xl">💡</span>
  </div>
  
  <div class="bg-green-500 rounded-2xl p-4 text-white flex items-center justify-center">
    <span class="text-2xl">🎯</span>
  </div>
  
  <div class="col-span-2 bg-gray-900 rounded-2xl p-4 text-white flex items-center gap-4">
    <div class="size-10 bg-white/20 rounded-lg flex items-center justify-center text-xl">⚡</div>
    <div>
      <p class="font-bold text-sm">Fast & Efficient</p>
      <p class="text-gray-400 text-xs">Build faster</p>
    </div>
  </div>
</div>
```

### 3. Magazine Layout

```html
<div class="grid grid-cols-12 gap-6">
  <!-- Feature article (7/12) -->
  <div class="col-span-12 md:col-span-7 bg-white rounded-xl overflow-hidden shadow">
    <div class="aspect-video bg-gradient-to-br from-blue-400 to-purple-600"></div>
    <div class="p-6">
      <span class="text-xs text-blue-500 font-semibold uppercase">Feature</span>
      <h2 class="text-xl font-bold text-gray-900 mt-1">บทความหลัก</h2>
      <p class="text-gray-500 text-sm mt-2 line-clamp-3">รายละเอียดบทความหลัก...</p>
    </div>
  </div>
  
  <!-- Side articles (5/12) -->
  <div class="col-span-12 md:col-span-5 space-y-4">
    <div class="bg-white rounded-xl p-4 shadow flex gap-4">
      <div class="size-20 flex-none bg-gray-100 rounded-lg"></div>
      <div>
        <span class="text-xs text-green-500 font-semibold">News</span>
        <h3 class="font-bold text-gray-900 text-sm mt-1">บทความรอง 1</h3>
        <p class="text-gray-400 text-xs mt-1 line-clamp-2">เนื้อหา...</p>
      </div>
    </div>
    <div class="bg-white rounded-xl p-4 shadow flex gap-4">
      <div class="size-20 flex-none bg-gray-100 rounded-lg"></div>
      <div>
        <span class="text-xs text-purple-500 font-semibold">Tech</span>
        <h3 class="font-bold text-gray-900 text-sm mt-1">บทความรอง 2</h3>
        <p class="text-gray-400 text-xs mt-1 line-clamp-2">เนื้อหา...</p>
      </div>
    </div>
    <div class="bg-white rounded-xl p-4 shadow flex gap-4">
      <div class="size-20 flex-none bg-gray-100 rounded-lg"></div>
      <div>
        <span class="text-xs text-red-500 font-semibold">Design</span>
        <h3 class="font-bold text-gray-900 text-sm mt-1">บทความรอง 3</h3>
        <p class="text-gray-400 text-xs mt-1 line-clamp-2">เนื้อหา...</p>
      </div>
    </div>
  </div>
</div>
```

---

## Step 79: Grid vs Flexbox — เลือกใช้อะไร?

```
Grid ดีกว่าเมื่อ:
  ✅ Layout 2 มิติ (row + column พร้อมกัน)
  ✅ Card grids
  ✅ Page layouts (header/sidebar/footer)
  ✅ Photo galleries
  ✅ Dashboard widgets
  ✅ Content ต้องการ explicit placement

Flexbox ดีกว่าเมื่อ:
  ✅ Layout 1 มิติ (row หรือ column อย่างเดียว)
  ✅ Navigation bars
  ✅ Centering
  ✅ Form elements
  ✅ Chips/Tags
  ✅ Icon + Text combinations
```

---

## Step 80: Workshop — Dashboard Grid

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Dashboard Grid</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 p-6">

  <h1 class="text-2xl font-bold text-gray-900 mb-6">Dashboard</h1>

  <!-- Stats Row -->
  <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 mb-6">
    
    <div class="bg-white rounded-xl p-5 shadow-sm">
      <div class="flex items-center justify-between mb-3">
        <span class="text-gray-500 text-sm">ยอดขายวันนี้</span>
        <div class="size-8 bg-blue-100 text-blue-600 rounded-lg flex items-center justify-center text-sm">💰</div>
      </div>
      <p class="text-2xl font-bold text-gray-900">฿48,295</p>
      <p class="text-xs text-green-500 mt-1">↑ 12.5% จากเมื่อวาน</p>
    </div>
    
    <div class="bg-white rounded-xl p-5 shadow-sm">
      <div class="flex items-center justify-between mb-3">
        <span class="text-gray-500 text-sm">ออเดอร์ใหม่</span>
        <div class="size-8 bg-purple-100 text-purple-600 rounded-lg flex items-center justify-center text-sm">📦</div>
      </div>
      <p class="text-2xl font-bold text-gray-900">342</p>
      <p class="text-xs text-green-500 mt-1">↑ 8.2% จากเมื่อวาน</p>
    </div>
    
    <div class="bg-white rounded-xl p-5 shadow-sm">
      <div class="flex items-center justify-between mb-3">
        <span class="text-gray-500 text-sm">ลูกค้าใหม่</span>
        <div class="size-8 bg-green-100 text-green-600 rounded-lg flex items-center justify-center text-sm">👥</div>
      </div>
      <p class="text-2xl font-bold text-gray-900">128</p>
      <p class="text-xs text-red-500 mt-1">↓ 2.1% จากเมื่อวาน</p>
    </div>
    
    <div class="bg-white rounded-xl p-5 shadow-sm">
      <div class="flex items-center justify-between mb-3">
        <span class="text-gray-500 text-sm">อัตราแปลง</span>
        <div class="size-8 bg-amber-100 text-amber-600 rounded-lg flex items-center justify-center text-sm">📈</div>
      </div>
      <p class="text-2xl font-bold text-gray-900">3.24%</p>
      <p class="text-xs text-green-500 mt-1">↑ 0.3% จากเมื่อวาน</p>
    </div>
    
  </div>

  <!-- Main Grid: Chart + Sidebar -->
  <div class="grid grid-cols-1 lg:grid-cols-3 gap-6 mb-6">
    
    <!-- Chart (2/3) -->
    <div class="lg:col-span-2 bg-white rounded-xl p-6 shadow-sm">
      <div class="flex items-center justify-between mb-6">
        <h3 class="font-bold text-gray-900">ยอดขายรายเดือน</h3>
        <select class="text-sm text-gray-500 border border-gray-200 rounded-lg px-3 py-1.5 outline-none focus:ring-2 focus:ring-indigo-500">
          <option>2026</option>
          <option>2025</option>
        </select>
      </div>
      <!-- Chart placeholder -->
      <div class="h-48 bg-gray-50 rounded-xl flex items-center justify-center text-gray-400">
        Chart จะมาใส่ตรงนี้
      </div>
    </div>
    
    <!-- Quick Stats (1/3) -->
    <div class="bg-white rounded-xl p-6 shadow-sm">
      <h3 class="font-bold text-gray-900 mb-4">สรุปยอดขาย</h3>
      <div class="space-y-4">
        <div class="flex items-center justify-between">
          <div class="flex items-center gap-2">
            <div class="size-2 bg-blue-500 rounded-full"></div>
            <span class="text-sm text-gray-600">Online</span>
          </div>
          <span class="font-semibold text-gray-900">฿28,450</span>
        </div>
        <div class="flex items-center justify-between">
          <div class="flex items-center gap-2">
            <div class="size-2 bg-purple-500 rounded-full"></div>
            <span class="text-sm text-gray-600">Offline</span>
          </div>
          <span class="font-semibold text-gray-900">฿12,340</span>
        </div>
        <div class="flex items-center justify-between">
          <div class="flex items-center gap-2">
            <div class="size-2 bg-green-500 rounded-full"></div>
            <span class="text-sm text-gray-600">Mobile</span>
          </div>
          <span class="font-semibold text-gray-900">฿7,505</span>
        </div>
      </div>
      
      <div class="mt-6 pt-4 border-t border-gray-100">
        <div class="flex justify-between">
          <span class="text-sm font-semibold text-gray-700">รวมทั้งหมด</span>
          <span class="font-bold text-gray-900">฿48,295</span>
        </div>
      </div>
    </div>
    
  </div>

  <!-- Recent Orders Table -->
  <div class="bg-white rounded-xl shadow-sm overflow-hidden">
    <div class="p-6 border-b border-gray-100">
      <h3 class="font-bold text-gray-900">ออเดอร์ล่าสุด</h3>
    </div>
    <div class="overflow-x-auto">
      <table class="w-full">
        <thead class="bg-gray-50">
          <tr>
            <th class="text-left text-xs font-semibold text-gray-500 uppercase px-6 py-3">ออเดอร์</th>
            <th class="text-left text-xs font-semibold text-gray-500 uppercase px-6 py-3">ลูกค้า</th>
            <th class="text-left text-xs font-semibold text-gray-500 uppercase px-6 py-3">สินค้า</th>
            <th class="text-left text-xs font-semibold text-gray-500 uppercase px-6 py-3">ราคา</th>
            <th class="text-left text-xs font-semibold text-gray-500 uppercase px-6 py-3">สถานะ</th>
          </tr>
        </thead>
        <tbody class="divide-y divide-gray-100">
          <tr class="hover:bg-gray-50 transition-colors">
            <td class="px-6 py-4 text-sm font-medium text-gray-900">#12345</td>
            <td class="px-6 py-4 text-sm text-gray-600">สมชาย ใจดี</td>
            <td class="px-6 py-4 text-sm text-gray-600">Tailwind Course</td>
            <td class="px-6 py-4 text-sm font-semibold text-gray-900">฿1,290</td>
            <td class="px-6 py-4"><span class="text-xs bg-green-100 text-green-700 px-2.5 py-1 rounded-full font-medium">ชำระแล้ว</span></td>
          </tr>
          <tr class="hover:bg-gray-50 transition-colors">
            <td class="px-6 py-4 text-sm font-medium text-gray-900">#12344</td>
            <td class="px-6 py-4 text-sm text-gray-600">สมหญิง รักดี</td>
            <td class="px-6 py-4 text-sm text-gray-600">React Course</td>
            <td class="px-6 py-4 text-sm font-semibold text-gray-900">฿1,990</td>
            <td class="px-6 py-4"><span class="text-xs bg-yellow-100 text-yellow-700 px-2.5 py-1 rounded-full font-medium">รอชำระ</span></td>
          </tr>
          <tr class="hover:bg-gray-50 transition-colors">
            <td class="px-6 py-4 text-sm font-medium text-gray-900">#12343</td>
            <td class="px-6 py-4 text-sm text-gray-600">วิชัย ดีเลิศ</td>
            <td class="px-6 py-4 text-sm text-gray-600">Next.js Course</td>
            <td class="px-6 py-4 text-sm font-semibold text-gray-900">฿2,490</td>
            <td class="px-6 py-4"><span class="text-xs bg-green-100 text-green-700 px-2.5 py-1 rounded-full font-medium">ชำระแล้ว</span></td>
          </tr>
          <tr class="hover:bg-gray-50 transition-colors">
            <td class="px-6 py-4 text-sm font-medium text-gray-900">#12342</td>
            <td class="px-6 py-4 text-sm text-gray-600">มณี ศรีสุข</td>
            <td class="px-6 py-4 text-sm text-gray-600">TypeScript Course</td>
            <td class="px-6 py-4 text-sm font-semibold text-gray-900">฿1,590</td>
            <td class="px-6 py-4"><span class="text-xs bg-red-100 text-red-700 px-2.5 py-1 rounded-full font-medium">ยกเลิก</span></td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>

</body>
</html>
```

---

## 📝 สรุป Part 08

| Property | Classes |
|----------|---------|
| Grid Display | `grid` `inline-grid` |
| Template Cols | `grid-cols-{1-12}` `grid-cols-none` |
| Template Rows | `grid-rows-{1-6}` `grid-rows-none` |
| Col Span | `col-span-{1-12}` `col-span-full` |
| Row Span | `row-span-{1-6}` `row-span-full` |
| Col Start/End | `col-start-{1-13}` `col-end-{1-13}` |
| Row Start/End | `row-start-{1-7}` `row-end-{1-7}` |
| Auto Flow | `grid-flow-row` `grid-flow-col` `grid-flow-dense` |
| Gap | `gap-{n}` `gap-x-{n}` `gap-y-{n}` |
| Justify | `justify-items-*` `justify-self-*` |
| Align | `items-*` `self-*` |
| Arbitrary | `grid-cols-[...]` |

---

*Part 08 — จาก 100 Parts | Steps 71–80 จาก 1,000 Steps*
