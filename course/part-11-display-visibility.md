# Part 11: Display, Visibility, Overflow
## Steps 101–110: ควบคุมการแสดงผล Element

---

## 🎯 เป้าหมายของ Part นี้

- Display types ทั้งหมด
- Visibility vs Hidden
- Overflow handling
- Z-Index
- Pointer Events
- Cursor
- User Select

---

## Step 101: Display

```html
<!-- Display types ที่สำคัญ -->
<div class="block">block — เต็มความกว้าง</div>
<span class="inline">inline — ขนาดตาม content</span>
<span class="inline-block">inline-block — inline แต่ได้ width/height</span>
<div class="flex">flex container</div>
<div class="inline-flex">inline flex</div>
<div class="grid">grid container</div>
<div class="inline-grid">inline grid</div>
<div class="table">table</div>
<div class="table-row">table row</div>
<div class="table-cell">table cell</div>
<div class="hidden">hidden (display: none)</div>

<!-- List -->
<ul class="list-item">list-item display</ul>

<!-- Contents (ไม่สร้าง box แต่ children ยังอยู่) -->
<div class="contents">
  ตัวมันเองหายไป แต่ children ยังแสดง
</div>
```

### ตัวอย่างการใช้ Display

```html
<!-- Block vs Inline -->
<div class="space-y-4">
  
  <!-- Block: เต็มแถว -->
  <p class="block bg-blue-100 p-2">Block paragraph</p>
  <p class="block bg-blue-100 p-2">Block paragraph 2</p>
  
  <!-- Inline: ต่อกัน -->
  <span class="inline bg-green-100 px-2 py-1">Inline 1</span>
  <span class="inline bg-green-100 px-2 py-1">Inline 2</span>
  <span class="inline bg-green-100 px-2 py-1">Inline 3</span>
  
  <!-- Inline-block: ต่อกัน แต่มี width/height -->
  <span class="inline-block w-24 text-center bg-purple-100 px-2 py-2">
    Inline-block 1
  </span>
  <span class="inline-block w-24 text-center bg-purple-100 px-2 py-2">
    Inline-block 2
  </span>
  
</div>

<!-- Hidden ซ่อน element -->
<div class="hidden md:block">
  ซ่อนบน mobile, แสดงบน md+
</div>

<div class="block md:hidden">
  แสดงบน mobile, ซ่อนบน md+
</div>
```

---

## Step 102: Visibility

```html
<!-- visible vs invisible -->
<div class="visible">แสดงผล (default)</div>
<div class="invisible">ซ่อน แต่ยังจอง space อยู่</div>

<!-- ต่างจาก hidden อย่างไร? -->
<div class="hidden">hidden — ไม่แสดง ไม่จอง space</div>
<div class="invisible">invisible — ไม่แสดง แต่ยังจอง space</div>
```

### ตัวอย่างการใช้ Visibility

```html
<!-- Loading state -->
<div class="relative">
  <div id="content" class="visible">เนื้อหา</div>
  <div id="skeleton" class="invisible absolute inset-0 bg-gray-200 animate-pulse rounded">
    <!-- Skeleton loader ที่ซ่อนอยู่ -->
  </div>
</div>

<!-- Tooltip pattern -->
<div class="relative group inline-block">
  <button class="bg-blue-500 text-white px-4 py-2 rounded">Hover me</button>
  
  <!-- Tooltip — invisible by default, visible on hover -->
  <div class="invisible group-hover:visible absolute bottom-full left-1/2 -translate-x-1/2 mb-2 bg-gray-900 text-white text-xs px-3 py-2 rounded whitespace-nowrap">
    This is a tooltip!
    <div class="absolute top-full left-1/2 -translate-x-1/2 border-4 border-transparent border-t-gray-900"></div>
  </div>
</div>
```

---

## Step 103: Overflow

```html
<!-- Main Overflow -->
<div class="overflow-auto">scroll เมื่อ content เกิน</div>
<div class="overflow-hidden">ซ่อน content ที่เกิน</div>
<div class="overflow-visible">แสดง content ที่เกิน (default)</div>
<div class="overflow-scroll">scroll เสมอ (แสดง scrollbar)</div>
<div class="overflow-clip">clip content ที่เกิน</div>

<!-- Axis control -->
<div class="overflow-x-auto">horizontal scroll</div>
<div class="overflow-y-auto">vertical scroll</div>
<div class="overflow-x-hidden">ซ่อนแนวนอน</div>
<div class="overflow-y-hidden">ซ่อนแนวตั้ง</div>
<div class="overflow-x-scroll">horizontal scroll เสมอ</div>
<div class="overflow-y-scroll">vertical scroll เสมอ</div>
```

### Scroll Behavior

```html
<div class="scroll-smooth">smooth scroll</div>
<div class="scroll-auto">instant scroll</div>

<!-- Scroll Snap -->
<div class="overflow-x-auto snap-x snap-mandatory flex gap-4 w-full">
  <div class="snap-start flex-none w-full bg-blue-500 h-48 rounded-xl">Slide 1</div>
  <div class="snap-start flex-none w-full bg-green-500 h-48 rounded-xl">Slide 2</div>
  <div class="snap-start flex-none w-full bg-purple-500 h-48 rounded-xl">Slide 3</div>
</div>
```

---

## Step 104: Z-Index

```html
<!-- Z-Index scale -->
<div class="z-0">0</div>
<div class="z-10">10</div>
<div class="z-20">20</div>
<div class="z-30">30</div>
<div class="z-40">40</div>
<div class="z-50">50</div>
<div class="z-auto">auto</div>

<!-- Negative z-index -->
<div class="-z-10">-10 (อยู่ใต้ปกติ)</div>

<!-- Arbitrary -->
<div class="z-[100]">100</div>
<div class="z-[999]">999</div>
```

### Z-Index Stack ในชีวิตจริง

```html
<!-- Typical stacking order -->
<div class="relative">
  <!-- Background: z-0 (default) -->
  <div class="bg-gray-100 p-8">Background</div>
  
  <!-- Sticky Header: z-50 -->
  <header class="sticky top-0 z-50 bg-white shadow">
    Header (z-50)
  </header>
  
  <!-- Sidebar: z-40 -->
  <aside class="fixed left-0 top-0 z-40 w-64 h-screen bg-gray-900">
    Sidebar (z-40)
  </aside>
  
  <!-- Modal Overlay: z-[60] -->
  <div class="fixed inset-0 bg-black/50 z-[60]">
    Modal overlay (z-60)
  </div>
  
  <!-- Modal: z-[70] -->
  <div class="fixed top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 bg-white rounded-xl z-[70] p-6">
    Modal content (z-70)
  </div>
  
  <!-- Toast/Notification: z-[80] -->
  <div class="fixed bottom-4 right-4 bg-gray-900 text-white px-4 py-2 rounded-lg z-[80]">
    Toast (z-80)
  </div>
</div>
```

---

## Step 105: Pointer Events และ Cursor

```html
<!-- Pointer Events -->
<div class="pointer-events-none">ไม่รับ click events</div>
<div class="pointer-events-auto">รับ events ตามปกติ</div>

<!-- Cursor types -->
<div class="cursor-auto">auto</div>
<div class="cursor-default">default (arrow)</div>
<div class="cursor-pointer">pointer (hand)</div>
<div class="cursor-wait">wait (loading)</div>
<div class="cursor-text">text (I-beam)</div>
<div class="cursor-move">move</div>
<div class="cursor-help">help (?)</div>
<div class="cursor-not-allowed">not-allowed (ไม่อนุญาต)</div>
<div class="cursor-none">hidden cursor</div>
<div class="cursor-crosshair">crosshair</div>
<div class="cursor-zoom-in">zoom in</div>
<div class="cursor-zoom-out">zoom out</div>
<div class="cursor-grab">grab</div>
<div class="cursor-grabbing">grabbing</div>

<!-- Resize cursors -->
<div class="cursor-col-resize">col-resize</div>
<div class="cursor-row-resize">row-resize</div>
<div class="cursor-n-resize">n-resize</div>
<div class="cursor-e-resize">e-resize</div>
```

---

## Step 106: User Select

```html
<!-- User Select -->
<p class="select-none">ไม่สามารถ select ข้อความได้</p>
<p class="select-text">select ได้ตามปกติ</p>
<p class="select-all">click เดียว select ทั้งหมด</p>
<p class="select-auto">browser decides (default)</p>
```

### ตัวอย่างการใช้งาน

```html
<!-- Code block ที่ select-all -->
<div class="bg-gray-900 rounded-xl p-4 relative group">
  <button class="absolute top-3 right-3 bg-gray-700 text-gray-300 text-xs px-2 py-1 rounded opacity-0 group-hover:opacity-100 transition-opacity">
    Copy
  </button>
  <code class="text-green-400 text-sm font-mono select-all block">
    npm install -D tailwindcss
  </code>
</div>

<!-- Prevent accidental text selection on drag-to-scroll -->
<div class="overflow-x-auto select-none">
  <div class="flex gap-4 w-max">
    <div class="w-48 h-32 bg-blue-500 rounded-xl flex-none"></div>
    <div class="w-48 h-32 bg-green-500 rounded-xl flex-none"></div>
    <div class="w-48 h-32 bg-purple-500 rounded-xl flex-none"></div>
  </div>
</div>
```

---

## Step 107: Appearance และ Will Change

```html
<!-- Remove native browser styling -->
<input class="appearance-none border rounded px-4 py-2" type="number">
<select class="appearance-none border rounded px-4 py-2 pr-8">
<input type="checkbox" class="appearance-none size-5 border-2 border-gray-300 rounded">

<!-- Will Change (performance hint) -->
<div class="will-change-auto">auto</div>
<div class="will-change-scroll">scroll-position</div>
<div class="will-change-contents">contents</div>
<div class="will-change-transform">transform (GPU acceleration)</div>

<!-- ใช้สำหรับ animation ที่ smooth -->
<div class="will-change-transform transition-transform duration-300 hover:scale-105">
  Smooth scale animation
</div>
```

---

## Step 108: Resize

```html
<!-- Resize behavior -->
<textarea class="resize">resize ทุกทิศ (default)</textarea>
<textarea class="resize-none">ไม่ resize</textarea>
<textarea class="resize-x">resize แนวนอน</textarea>
<textarea class="resize-y">resize แนวตั้ง</textarea>
```

---

## Step 109: Scroll Margin และ Padding

```html
<!-- Scroll margin (offset เมื่อ scroll to element) -->
<div class="scroll-mt-16">margin top 64px เมื่อ scroll มา</div>
<div class="scroll-mb-4">margin bottom</div>
<div class="scroll-mx-4">margin x</div>
<div class="scroll-my-8">margin y</div>

<!-- Scroll padding (ใน scroll container) -->
<div class="scroll-pt-16">padding top สำหรับ scroll snap</div>
```

### ตัวอย่าง Sticky Header + Scroll Offset

```html
<!-- เมื่อคลิก nav link แล้ว scroll มาที่ section -->
<!-- header สูง 64px จะ overlap content -->
<!-- แก้ด้วย scroll-mt-16 -->

<header class="fixed top-0 w-full h-16 bg-white z-50">Fixed Header</header>

<nav>
  <a href="#about">เกี่ยวกับ</a>
  <a href="#services">บริการ</a>
</nav>

<section id="about" class="scroll-mt-20 py-16">
  เนื้อหา About — ไม่ถูก header บัง
</section>

<section id="services" class="scroll-mt-20 py-16">
  เนื้อหา Services
</section>
```

---

## Step 110: Workshop — Interactive UI Components

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Display & Visibility Workshop</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 p-8 space-y-8">

  <h1 class="text-3xl font-bold text-gray-900">Display & Visibility Examples</h1>

  <!-- Responsive Show/Hide -->
  <section>
    <h2 class="text-lg font-semibold text-gray-700 mb-3">Responsive Show/Hide</h2>
    <div class="bg-white rounded-xl p-6 shadow-sm">
      <div class="block md:hidden bg-blue-100 text-blue-700 p-3 rounded-lg text-sm">
        📱 คุณอยู่ใน Mobile view
      </div>
      <div class="hidden md:block lg:hidden bg-green-100 text-green-700 p-3 rounded-lg text-sm">
        💻 คุณอยู่ใน Tablet view  
      </div>
      <div class="hidden lg:block bg-purple-100 text-purple-700 p-3 rounded-lg text-sm">
        🖥️ คุณอยู่ใน Desktop view
      </div>
    </div>
  </section>

  <!-- Overflow Examples -->
  <section>
    <h2 class="text-lg font-semibold text-gray-700 mb-3">Overflow</h2>
    <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
      
      <div>
        <p class="text-xs text-gray-500 mb-2">overflow-hidden</p>
        <div class="overflow-hidden h-24 bg-white rounded-xl p-3 shadow-sm">
          <p class="text-gray-700 text-sm">
            Lorem ipsum dolor sit amet consectetur. Ornare scelerisque nec 
            platea blandit nunc vitae blandit. Porta volutpat ultrices 
            enim nec vitae vel. Nisl elementum commodo tempus pulvinar.
          </p>
        </div>
      </div>
      
      <div>
        <p class="text-xs text-gray-500 mb-2">overflow-auto</p>
        <div class="overflow-auto h-24 bg-white rounded-xl p-3 shadow-sm">
          <p class="text-gray-700 text-sm">
            Lorem ipsum dolor sit amet consectetur. Ornare scelerisque nec 
            platea blandit nunc vitae blandit. Porta volutpat ultrices 
            enim nec vitae vel. Nisl elementum commodo tempus pulvinar.
          </p>
        </div>
      </div>
      
      <div>
        <p class="text-xs text-gray-500 mb-2">overflow-x-auto (horizontal)</p>
        <div class="overflow-x-auto bg-white rounded-xl p-3 shadow-sm">
          <div class="flex gap-3 w-max">
            <div class="w-32 h-16 bg-blue-100 rounded-lg flex-none flex items-center justify-center text-sm text-blue-700">Item 1</div>
            <div class="w-32 h-16 bg-green-100 rounded-lg flex-none flex items-center justify-center text-sm text-green-700">Item 2</div>
            <div class="w-32 h-16 bg-purple-100 rounded-lg flex-none flex items-center justify-center text-sm text-purple-700">Item 3</div>
            <div class="w-32 h-16 bg-red-100 rounded-lg flex-none flex items-center justify-center text-sm text-red-700">Item 4</div>
            <div class="w-32 h-16 bg-amber-100 rounded-lg flex-none flex items-center justify-center text-sm text-amber-700">Item 5</div>
          </div>
        </div>
      </div>
      
    </div>
  </section>

  <!-- Tooltip using visibility -->
  <section>
    <h2 class="text-lg font-semibold text-gray-700 mb-3">Tooltip (Hover)</h2>
    <div class="flex gap-4 flex-wrap">
      
      <div class="relative group inline-block">
        <button class="bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm font-medium">
          Hover me (Top)
        </button>
        <div class="invisible group-hover:visible opacity-0 group-hover:opacity-100 absolute bottom-full left-1/2 -translate-x-1/2 mb-2 bg-gray-900 text-white text-xs px-3 py-2 rounded-lg whitespace-nowrap transition-all duration-150">
          This is a tooltip!
          <div class="absolute top-full left-1/2 -translate-x-1/2 border-4 border-transparent border-t-gray-900"></div>
        </div>
      </div>
      
      <div class="relative group inline-block">
        <button class="bg-purple-600 text-white px-4 py-2 rounded-xl text-sm font-medium">
          Hover me (Right)
        </button>
        <div class="invisible group-hover:visible opacity-0 group-hover:opacity-100 absolute left-full top-1/2 -translate-y-1/2 ml-2 bg-gray-900 text-white text-xs px-3 py-2 rounded-lg whitespace-nowrap transition-all duration-150">
          Right tooltip!
          <div class="absolute right-full top-1/2 -translate-y-1/2 border-4 border-transparent border-r-gray-900"></div>
        </div>
      </div>
      
    </div>
  </section>

  <!-- Cursor Examples -->
  <section>
    <h2 class="text-lg font-semibold text-gray-700 mb-3">Cursors</h2>
    <div class="flex flex-wrap gap-3">
      <div class="cursor-pointer bg-blue-100 text-blue-700 px-4 py-2 rounded-lg text-sm">cursor-pointer</div>
      <div class="cursor-not-allowed bg-red-100 text-red-700 px-4 py-2 rounded-lg text-sm">cursor-not-allowed</div>
      <div class="cursor-wait bg-yellow-100 text-yellow-700 px-4 py-2 rounded-lg text-sm">cursor-wait</div>
      <div class="cursor-text bg-green-100 text-green-700 px-4 py-2 rounded-lg text-sm">cursor-text</div>
      <div class="cursor-grab bg-purple-100 text-purple-700 px-4 py-2 rounded-lg text-sm">cursor-grab</div>
      <div class="cursor-zoom-in bg-indigo-100 text-indigo-700 px-4 py-2 rounded-lg text-sm">cursor-zoom-in</div>
    </div>
  </section>

  <!-- Z-Index Stack -->
  <section>
    <h2 class="text-lg font-semibold text-gray-700 mb-3">Z-Index Stack</h2>
    <div class="relative h-40 bg-white rounded-xl shadow-sm overflow-hidden">
      <div class="absolute inset-4 z-10 bg-blue-500/80 rounded-xl flex items-center justify-center text-white text-sm">z-10 (Blue)</div>
      <div class="absolute inset-8 z-20 bg-purple-500/80 rounded-xl flex items-center justify-center text-white text-sm">z-20 (Purple)</div>
      <div class="absolute inset-12 z-30 bg-pink-500/80 rounded-xl flex items-center justify-center text-white text-sm">z-30 (Pink) — สูงสุด</div>
    </div>
  </section>

</body>
</html>
```

---

## 📝 สรุป Part 11

| Category | Utilities |
|----------|-----------|
| Display | `block` `inline` `inline-block` `flex` `grid` `hidden` |
| Visibility | `visible` `invisible` |
| Overflow | `overflow-{auto,hidden,visible,scroll}` `overflow-x/y-*` |
| Z-Index | `z-{0,10,20,30,40,50}` `z-auto` `-z-{n}` `z-[n]` |
| Pointer Events | `pointer-events-none` `pointer-events-auto` |
| Cursor | `cursor-pointer` `cursor-not-allowed` etc |
| User Select | `select-none` `select-text` `select-all` |
| Resize | `resize` `resize-none` `resize-x` `resize-y` |
| Scroll Margin | `scroll-mt-*` `scroll-mb-*` etc |

---

*Part 11 — จาก 100 Parts | Steps 101–110 จาก 1,000 Steps*
