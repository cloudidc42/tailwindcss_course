# Part 07: Flexbox Layout พื้นฐาน
## Steps 61–70: Master Flexbox ด้วย Tailwind

---

## 🎯 เป้าหมายของ Part นี้

- Flex Container Properties
- Flex Item Properties
- Alignment (Main Axis & Cross Axis)
- Flex Wrap
- Order
- Flex Grow/Shrink/Basis
- Practical Flexbox Patterns

---

## Step 61: เปิดใช้ Flexbox

```html
<!-- เปิด flex mode -->
<div class="flex">
  <div>Item 1</div>
  <div>Item 2</div>
  <div>Item 3</div>
</div>

<!-- Inline flex -->
<div class="inline-flex">
  <span>Inline 1</span>
  <span>Inline 2</span>
</div>
```

### Flex Direction

```html
<!-- Row (default): left to right -->
<div class="flex flex-row">
  <div>1</div><div>2</div><div>3</div>
</div>

<!-- Row Reverse: right to left -->
<div class="flex flex-row-reverse">
  <div>1</div><div>2</div><div>3</div>
</div>

<!-- Column: top to bottom -->
<div class="flex flex-col">
  <div>1</div><div>2</div><div>3</div>
</div>

<!-- Column Reverse: bottom to top -->
<div class="flex flex-col-reverse">
  <div>1</div><div>2</div><div>3</div>
</div>
```

---

## Step 62: Flex Wrap

```html
<!-- Nowrap (default): ไม่ขึ้นบรรทัดใหม่ -->
<div class="flex flex-nowrap">
  items อยู่บรรทัดเดียวกัน อาจล้น
</div>

<!-- Wrap: ขึ้นบรรทัดใหม่เมื่อเต็ม -->
<div class="flex flex-wrap gap-2">
  <div class="w-32 h-8 bg-blue-500">Item</div>
  <div class="w-32 h-8 bg-blue-500">Item</div>
  <div class="w-32 h-8 bg-blue-500">Item</div>
  <div class="w-32 h-8 bg-blue-500">Item</div>
  <div class="w-32 h-8 bg-blue-500">Item</div>
</div>

<!-- Wrap Reverse -->
<div class="flex flex-wrap-reverse">
  ขึ้นบรรทัดใหม่ในทิศทางตรงข้าม
</div>
```

---

## Step 63: Justify Content (Main Axis)

```html
<!-- justify-content ควบคุมแนวนอน (ถ้า flex-row) -->
<div class="flex justify-start">    <!-- ชิดซ้าย (default) --></div>
<div class="flex justify-center">   <!-- จัดกลาง --></div>
<div class="flex justify-end">      <!-- ชิดขวา --></div>
<div class="flex justify-between">  <!-- กระจาย (ขอบสองด้านติดขอบ) --></div>
<div class="flex justify-around">   <!-- กระจาย (มี space รอบๆ) --></div>
<div class="flex justify-evenly">   <!-- กระจายเท่าๆ กัน --></div>
<div class="flex justify-stretch">  <!-- ยืดให้เต็ม --></div>
```

### ตัวอย่างการใช้งาน

```html
<!-- Navbar: Logo ซ้าย, Nav กลาง, Button ขวา -->
<nav class="flex items-center justify-between px-6 py-4 bg-white">
  <div class="font-bold text-xl">Logo</div>
  <div class="flex gap-6">
    <a href="#">Home</a>
    <a href="#">About</a>
    <a href="#">Contact</a>
  </div>
  <button class="bg-blue-500 text-white px-4 py-2 rounded">Login</button>
</nav>

<!-- Centered content -->
<div class="flex justify-center items-center h-screen">
  <div class="text-center">ตรงกลาง!</div>
</div>

<!-- Space between cards -->
<div class="flex justify-between gap-4">
  <div class="bg-white p-4 rounded">Card 1</div>
  <div class="bg-white p-4 rounded">Card 2</div>
  <div class="bg-white p-4 rounded">Card 3</div>
</div>
```

---

## Step 64: Align Items (Cross Axis)

```html
<!-- align-items ควบคุมแนวตั้ง (ถ้า flex-row) -->
<div class="flex items-start">    <!-- ชิดบน --></div>
<div class="flex items-center">   <!-- จัดกลาง --></div>
<div class="flex items-end">      <!-- ชิดล่าง --></div>
<div class="flex items-stretch">  <!-- ยืดให้เท่ากัน (default) --></div>
<div class="flex items-baseline"> <!-- เรียงตาม baseline --></div>
```

### Align Content (เมื่อ wrap)

```html
<!-- สำหรับ multiline flex (ต้อง flex-wrap) -->
<div class="flex flex-wrap content-start">    <!-- แถวชิดบน --></div>
<div class="flex flex-wrap content-center">   <!-- แถวจัดกลาง --></div>
<div class="flex flex-wrap content-end">      <!-- แถวชิดล่าง --></div>
<div class="flex flex-wrap content-between">  <!-- แถวกระจาย --></div>
<div class="flex flex-wrap content-around">   <!-- แถวมี space รอบ --></div>
<div class="flex flex-wrap content-evenly">   <!-- แถวเท่าๆ --></div>
<div class="flex flex-wrap content-stretch">  <!-- ยืดให้เต็ม --></div>
```

---

## Step 65: Align Self (Individual Item)

```html
<div class="flex items-center h-32">
  <div class="self-auto">auto (เหมือน parent)</div>
  <div class="self-start">ชิดบน</div>
  <div class="self-center">จัดกลาง</div>
  <div class="self-end">ชิดล่าง</div>
  <div class="self-stretch">ยืดเต็ม</div>
  <div class="self-baseline">baseline</div>
</div>
```

---

## Step 66: Flex Grow, Shrink, Basis

```html
<!-- Flex Grow: ขยายเพื่อเติมพื้นที่ว่าง -->
<div class="flex">
  <div class="flex-none w-24">Fixed 96px</div>
  <div class="flex-1">ขยายเต็มที่เหลือ (grow: 1)</div>
</div>

<div class="flex">
  <div class="flex-1">grow: 1</div>
  <div class="flex-2">grow: 2 (ใหญ่กว่า 2x)</div>
  <!-- ใช้ arbitrary: flex-[2] -->
  <div class="flex-[2]">grow: 2</div>
</div>

<!-- Flex Shrink: หดเมื่อพื้นที่ไม่พอ -->
<div class="shrink">shrink: 1 (default)</div>
<div class="shrink-0">shrink: 0 (ไม่หด!)</div>

<!-- Flex Basis: ขนาดเริ่มต้น -->
<div class="basis-0">0px</div>
<div class="basis-auto">auto</div>
<div class="basis-full">100%</div>
<div class="basis-1/2">50%</div>
<div class="basis-1/3">33.33%</div>
<div class="basis-64">256px</div>

<!-- Flex Shorthand -->
<div class="flex-none">flex: none (ไม่ grow ไม่ shrink)</div>
<div class="flex-1">flex: 1 1 0% (grow + shrink + basis 0)</div>
<div class="flex-auto">flex: 1 1 auto</div>
<div class="flex-initial">flex: 0 1 auto</div>
```

### Pattern: Equal Width Columns

```html
<!-- 3 columns ที่มีความกว้างเท่ากัน -->
<div class="flex gap-4">
  <div class="flex-1 bg-blue-100 p-4 rounded">Column 1</div>
  <div class="flex-1 bg-green-100 p-4 rounded">Column 2</div>
  <div class="flex-1 bg-purple-100 p-4 rounded">Column 3</div>
</div>

<!-- Sidebar + Main content -->
<div class="flex gap-6">
  <div class="flex-none w-64 bg-gray-100 p-4 rounded">Sidebar (fixed)</div>
  <div class="flex-1 bg-white p-4 rounded">Main content (flexible)</div>
</div>
```

---

## Step 67: Order

```html
<!-- เปลี่ยนลำดับโดยไม่เปลี่ยน DOM -->
<div class="flex">
  <div class="order-3">ใน DOM เป็น 1, แต่แสดงเป็น 3</div>
  <div class="order-1">ใน DOM เป็น 2, แต่แสดงเป็น 1</div>
  <div class="order-2">ใน DOM เป็น 3, แต่แสดงเป็น 2</div>
</div>

<!-- Responsive order -->
<div class="flex flex-col md:flex-row">
  <div class="order-2 md:order-1">ซ้ายบน desktop, ล่างบน mobile</div>
  <div class="order-1 md:order-2">ขวาบน desktop, บนบน mobile</div>
</div>

<!-- Special orders -->
<div class="order-first">-9999 (แรกสุด)</div>
<div class="order-last">9999 (หลังสุด)</div>
<div class="order-none">0 (default)</div>
```

---

## Step 68: Flexbox Patterns ที่ใช้บ่อย

### Pattern 1: Centering Everything

```html
<!-- วิธีที่ง่ายที่สุดในการจัดกลาง -->
<div class="flex items-center justify-center h-screen">
  <div>Perfectly centered! 🎯</div>
</div>
```

### Pattern 2: Sticky Footer

```html
<div class="flex flex-col min-h-screen">
  <header class="bg-white shadow p-4">Header</header>
  
  <main class="flex-1 p-6">
    Main content ที่ขยายเติมพื้นที่ที่เหลือ
  </main>
  
  <footer class="bg-gray-800 text-white p-4">
    Footer ติดล่างเสมอ
  </footer>
</div>
```

### Pattern 3: Card with Footer Push Down

```html
<div class="flex flex-col bg-white rounded-xl shadow h-64">
  <div class="p-4">
    <h3 class="font-bold">Card Title</h3>
    <p class="text-gray-600 text-sm">เนื้อหาการ์ด</p>
  </div>
  
  <!-- Push footer to bottom -->
  <div class="flex-1"></div>
  
  <div class="p-4 border-t border-gray-100">
    <button class="text-blue-500 text-sm">อ่านเพิ่มเติม</button>
  </div>
</div>
```

### Pattern 4: Icon + Text Row

```html
<div class="flex items-center gap-3">
  <div class="size-10 bg-blue-100 text-blue-600 rounded-xl flex items-center justify-center text-lg flex-none">
    📧
  </div>
  <div>
    <p class="font-medium text-gray-900">อีเมล</p>
    <p class="text-gray-500 text-sm">example@email.com</p>
  </div>
</div>
```

### Pattern 5: Horizontal Scrolling

```html
<div class="flex gap-4 overflow-x-auto pb-2 -mb-2">
  <!-- แต่ละ item ไม่หด -->
  <div class="flex-none w-64 bg-white rounded-xl p-4 shadow">Card 1</div>
  <div class="flex-none w-64 bg-white rounded-xl p-4 shadow">Card 2</div>
  <div class="flex-none w-64 bg-white rounded-xl p-4 shadow">Card 3</div>
  <div class="flex-none w-64 bg-white rounded-xl p-4 shadow">Card 4</div>
  <div class="flex-none w-64 bg-white rounded-xl p-4 shadow">Card 5</div>
</div>
```

---

## Step 69: Responsive Flexbox

```html
<!-- Stack ในมือถือ, Row ในจอใหญ่ -->
<div class="flex flex-col md:flex-row gap-6">
  <div class="md:w-1/3 bg-white p-6 rounded-xl">Sidebar</div>
  <div class="md:flex-1 bg-white p-6 rounded-xl">Main Content</div>
</div>

<!-- Responsive justify -->
<div class="flex flex-col items-center md:flex-row md:justify-between">
  <h2>หัวข้อ</h2>
  <button>ปุ่ม</button>
</div>

<!-- Responsive wrap -->
<div class="flex flex-wrap gap-4">
  <div class="w-full sm:w-1/2 lg:w-1/3">Item ที่ responsive</div>
  <div class="w-full sm:w-1/2 lg:w-1/3">Item ที่ responsive</div>
  <div class="w-full sm:w-1/2 lg:w-1/3">Item ที่ responsive</div>
</div>
```

---

## Step 70: Workshop — Full Flexbox Page

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Flexbox Workshop</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100">

  <!-- Full Page Layout with Sticky Footer -->
  <div class="flex flex-col min-h-screen">
    
    <!-- Header -->
    <header class="bg-white border-b border-gray-200">
      <div class="max-w-7xl mx-auto px-4 h-16 flex items-center justify-between">
        <div class="flex items-center gap-3">
          <div class="size-8 bg-indigo-600 rounded-lg flex items-center justify-center text-white text-sm font-bold">T</div>
          <span class="font-bold text-gray-900">TailwindCourse</span>
        </div>
        
        <nav class="hidden md:flex items-center gap-6">
          <a href="#" class="text-sm text-gray-600 hover:text-gray-900">หน้าแรก</a>
          <a href="#" class="text-sm text-gray-600 hover:text-gray-900">หลักสูตร</a>
          <a href="#" class="text-sm text-gray-600 hover:text-gray-900">เกี่ยวกับ</a>
        </nav>
        
        <div class="flex items-center gap-3">
          <button class="text-sm text-gray-600 hover:text-gray-900">เข้าสู่ระบบ</button>
          <button class="bg-indigo-600 text-white text-sm px-4 py-2 rounded-lg hover:bg-indigo-700 transition-colors">
            สมัครสมาชิก
          </button>
        </div>
      </div>
    </header>
    
    <!-- Main Content -->
    <main class="flex-1 max-w-7xl mx-auto px-4 py-8 w-full">
      
      <!-- Hero with Flex -->
      <div class="flex flex-col md:flex-row items-center gap-12 mb-16 py-8">
        <div class="flex-1 text-center md:text-left">
          <h1 class="text-4xl font-black text-gray-900 mb-4 leading-tight">
            เรียน Tailwind CSS<br>อย่างเป็นระบบ
          </h1>
          <p class="text-gray-500 text-lg mb-6">
            จาก Step 1 ถึง 1000 ครบทุกเนื้อหา
          </p>
          <div class="flex flex-col sm:flex-row gap-3 justify-center md:justify-start">
            <button class="bg-indigo-600 text-white px-6 py-3 rounded-xl font-semibold hover:bg-indigo-700">
              เริ่มเรียน
            </button>
            <button class="border-2 border-gray-300 text-gray-700 px-6 py-3 rounded-xl font-semibold hover:border-indigo-300">
              ดูหลักสูตร
            </button>
          </div>
        </div>
        <div class="flex-none">
          <div class="size-64 bg-gradient-to-br from-indigo-400 to-purple-600 rounded-3xl flex items-center justify-center text-6xl shadow-2xl shadow-indigo-500/25">
            🎨
          </div>
        </div>
      </div>
      
      <!-- Features Grid -->
      <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-12">
        
        <div class="bg-white rounded-xl p-6 shadow-sm flex flex-col">
          <div class="size-12 bg-blue-100 text-blue-600 rounded-xl flex items-center justify-center text-xl mb-4 flex-none">📚</div>
          <h3 class="font-bold text-gray-900 mb-2">100+ บทเรียน</h3>
          <p class="text-gray-500 text-sm flex-1">เนื้อหาครบตั้งแต่พื้นฐานถึงขั้นสูง</p>
          <a href="#" class="text-indigo-500 text-sm mt-4 hover:text-indigo-700">ดูหลักสูตร →</a>
        </div>
        
        <div class="bg-white rounded-xl p-6 shadow-sm flex flex-col">
          <div class="size-12 bg-green-100 text-green-600 rounded-xl flex items-center justify-center text-xl mb-4 flex-none">⚡</div>
          <h3 class="font-bold text-gray-900 mb-2">Code ใช้ได้จริง</h3>
          <p class="text-gray-500 text-sm flex-1">ตัวอย่างทุกอย่าง copy ไปใช้ได้เลย</p>
          <a href="#" class="text-indigo-500 text-sm mt-4 hover:text-indigo-700">ดูตัวอย่าง →</a>
        </div>
        
        <div class="bg-white rounded-xl p-6 shadow-sm flex flex-col">
          <div class="size-12 bg-purple-100 text-purple-600 rounded-xl flex items-center justify-center text-xl mb-4 flex-none">🏆</div>
          <h3 class="font-bold text-gray-900 mb-2">โปรเจกต์จริง</h3>
          <p class="text-gray-500 text-sm flex-1">สร้างโปรเจกต์จริงทุก level</p>
          <a href="#" class="text-indigo-500 text-sm mt-4 hover:text-indigo-700">ดูโปรเจกต์ →</a>
        </div>
        
      </div>
      
      <!-- Horizontal Scroll Cards -->
      <h2 class="text-2xl font-bold text-gray-900 mb-4">บทเรียนล่าสุด</h2>
      <div class="flex gap-4 overflow-x-auto pb-4 -mx-4 px-4">
        
        <div class="flex-none w-72 bg-white rounded-xl shadow-sm overflow-hidden hover:shadow-md transition-shadow">
          <div class="h-36 bg-gradient-to-br from-blue-400 to-indigo-600 flex items-center justify-center text-4xl">
            🎨
          </div>
          <div class="p-4">
            <span class="text-xs text-indigo-500 font-semibold uppercase tracking-wide">Part 01</span>
            <h3 class="font-bold text-gray-900 mt-1">แนะนำ Tailwind CSS</h3>
            <p class="text-gray-500 text-sm mt-1 line-clamp-2">เรียนรู้ Utility-First CSS ตั้งแต่ต้น</p>
          </div>
        </div>
        
        <div class="flex-none w-72 bg-white rounded-xl shadow-sm overflow-hidden hover:shadow-md transition-shadow">
          <div class="h-36 bg-gradient-to-br from-green-400 to-emerald-600 flex items-center justify-center text-4xl">
            ⚙️
          </div>
          <div class="p-4">
            <span class="text-xs text-green-500 font-semibold uppercase tracking-wide">Part 02</span>
            <h3 class="font-bold text-gray-900 mt-1">การติดตั้ง Tailwind</h3>
            <p class="text-gray-500 text-sm mt-1 line-clamp-2">Setup โปรเจกต์ให้พร้อมใช้งาน</p>
          </div>
        </div>
        
        <div class="flex-none w-72 bg-white rounded-xl shadow-sm overflow-hidden hover:shadow-md transition-shadow">
          <div class="h-36 bg-gradient-to-br from-purple-400 to-pink-600 flex items-center justify-center text-4xl">
            📝
          </div>
          <div class="p-4">
            <span class="text-xs text-purple-500 font-semibold uppercase tracking-wide">Part 03</span>
            <h3 class="font-bold text-gray-900 mt-1">Typography</h3>
            <p class="text-gray-500 text-sm mt-1 line-clamp-2">Font, Size, Weight และทุกอย่าง</p>
          </div>
        </div>
        
        <div class="flex-none w-72 bg-white rounded-xl shadow-sm overflow-hidden hover:shadow-md transition-shadow">
          <div class="h-36 bg-gradient-to-br from-amber-400 to-orange-600 flex items-center justify-center text-4xl">
            🎨
          </div>
          <div class="p-4">
            <span class="text-xs text-amber-500 font-semibold uppercase tracking-wide">Part 04</span>
            <h3 class="font-bold text-gray-900 mt-1">Colors</h3>
            <p class="text-gray-500 text-sm mt-1 line-clamp-2">Color System และ Gradients</p>
          </div>
        </div>
        
      </div>
      
    </main>
    
    <!-- Footer -->
    <footer class="bg-gray-900 mt-16">
      <div class="max-w-7xl mx-auto px-4 py-8">
        <div class="flex flex-col md:flex-row items-center justify-between gap-4">
          <div class="flex items-center gap-3">
            <div class="size-8 bg-indigo-600 rounded-lg flex items-center justify-center text-white text-sm font-bold">T</div>
            <span class="text-white font-bold">TailwindCourse</span>
          </div>
          <p class="text-gray-400 text-sm">© 2026 All rights reserved</p>
          <div class="flex gap-4">
            <a href="#" class="text-gray-400 hover:text-white text-sm">Privacy</a>
            <a href="#" class="text-gray-400 hover:text-white text-sm">Terms</a>
            <a href="#" class="text-gray-400 hover:text-white text-sm">Contact</a>
          </div>
        </div>
      </div>
    </footer>
    
  </div>

</body>
</html>
```

---

## 📝 สรุป Part 07

| Property | Classes |
|----------|---------|
| Display | `flex` `inline-flex` |
| Direction | `flex-row` `flex-col` `flex-row-reverse` `flex-col-reverse` |
| Wrap | `flex-wrap` `flex-nowrap` `flex-wrap-reverse` |
| Justify | `justify-start/center/end/between/around/evenly` |
| Align Items | `items-start/center/end/stretch/baseline` |
| Align Self | `self-start/center/end/stretch` |
| Align Content | `content-start/center/end/between/around/evenly` |
| Grow | `flex-1` `flex-none` `flex-auto` |
| Shrink | `shrink` `shrink-0` |
| Basis | `basis-{n}` `basis-{fraction}` |
| Order | `order-{n}` `order-first` `order-last` |

---

*Part 07 — จาก 100 Parts | Steps 61–70 จาก 1,000 Steps*
