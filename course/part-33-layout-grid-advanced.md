# Part 33: Advanced Grid & Layout Systems
## Steps 321–330: Layout ขั้นสูงสำหรับ Modern Web

---

## 🎯 เป้าหมายของ Part นี้

- CSS Grid advanced (named areas, auto-fill/fit)
- Masonry layout
- Holy Grail layout
- Bento Grid
- Subgrid
- Intrinsic sizing

---

## Step 321: Named Grid Areas

```html
<script src="https://cdn.tailwindcss.com"></script>
<style>
  /* Named grid areas layout */
  .app-layout {
    display: grid;
    grid-template-columns: 240px 1fr;
    grid-template-rows: 64px 1fr;
    grid-template-areas:
      "sidebar header"
      "sidebar content";
    height: 100vh;
  }
  .area-sidebar  { grid-area: sidebar; }
  .area-header   { grid-area: header; }
  .area-content  { grid-area: content; }
</style>

<div class="app-layout">
  <aside class="area-sidebar bg-gray-900 text-white p-4">Sidebar</aside>
  <header class="area-header bg-white border-b border-gray-200 flex items-center px-6">Header</header>
  <main class="area-content bg-gray-50 overflow-y-auto p-6">Content</main>
</div>
```

---

## Step 322: Auto-fill vs Auto-fit

```html
<!-- auto-fill: สร้าง columns ทุกครั้งแม้ว่างเปล่า -->
<div class="grid p-6" style="grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); gap: 16px;">
  <div class="bg-indigo-100 rounded-xl p-4 text-sm text-center">Item 1</div>
  <div class="bg-indigo-100 rounded-xl p-4 text-sm text-center">Item 2</div>
  <div class="bg-indigo-100 rounded-xl p-4 text-sm text-center">Item 3</div>
</div>

<!-- auto-fit: stretch items เต็มพื้นที่ -->
<div class="grid p-6" style="grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 16px;">
  <div class="bg-purple-100 rounded-xl p-4 text-sm text-center">Item 1</div>
  <div class="bg-purple-100 rounded-xl p-4 text-sm text-center">Item 2</div>
  <div class="bg-purple-100 rounded-xl p-4 text-sm text-center">Item 3</div>
</div>

<!-- Tailwind version: grid-cols-[repeat(auto-fit,minmax(200px,1fr))] -->
<div class="grid gap-4 p-6 [grid-template-columns:repeat(auto-fit,minmax(200px,1fr))]">
  <div class="bg-pink-100 rounded-xl p-4 text-sm text-center">Auto Item</div>
  <div class="bg-pink-100 rounded-xl p-4 text-sm text-center">Auto Item</div>
</div>
```

---

## Step 323: Bento Grid Layout

```html
<!-- Bento grid: Instagram/Apple-style mixed sizes -->
<div class="grid grid-cols-4 grid-rows-3 gap-3 p-6 aspect-video">
  <!-- Large hero -->
  <div class="col-span-2 row-span-2 bg-gradient-to-br from-indigo-600 to-purple-700 rounded-2xl flex items-center justify-center text-white">
    <div class="text-center p-6">
      <div class="text-4xl mb-2">🚀</div>
      <p class="font-black text-xl">Feature One</p>
      <p class="text-indigo-200 text-sm mt-1">ฟีเจอร์หลักขนาดใหญ่</p>
    </div>
  </div>
  
  <!-- Top right small -->
  <div class="col-span-1 row-span-1 bg-gradient-to-br from-pink-500 to-rose-600 rounded-2xl flex items-center justify-center text-white p-4">
    <div class="text-center"><div class="text-2xl">⚡</div><p class="text-xs font-bold mt-1">Fast</p></div>
  </div>
  
  <div class="col-span-1 row-span-1 bg-gradient-to-br from-amber-400 to-orange-500 rounded-2xl flex items-center justify-center text-white p-4">
    <div class="text-center"><div class="text-2xl">🔒</div><p class="text-xs font-bold mt-1">Secure</p></div>
  </div>
  
  <!-- Mid right wide -->
  <div class="col-span-2 row-span-1 bg-gradient-to-r from-emerald-500 to-teal-600 rounded-2xl flex items-center gap-4 px-5 text-white">
    <div class="text-3xl">📊</div>
    <div><p class="font-bold">Analytics</p><p class="text-emerald-200 text-xs">Real-time insights</p></div>
  </div>
  
  <!-- Bottom row -->
  <div class="col-span-1 row-span-1 bg-gradient-to-br from-violet-500 to-purple-600 rounded-2xl flex items-center justify-center text-white p-3">
    <div class="text-center"><div class="text-2xl">🎨</div><p class="text-xs font-bold mt-1">Design</p></div>
  </div>
  
  <div class="col-span-1 row-span-1 bg-gray-100 rounded-2xl flex items-center justify-center p-3">
    <div class="text-center"><div class="text-2xl">📱</div><p class="text-xs font-bold text-gray-700 mt-1">Mobile</p></div>
  </div>
  
  <div class="col-span-2 row-span-1 bg-gray-900 rounded-2xl flex items-center gap-4 px-5 text-white">
    <div class="flex-1"><p class="font-bold text-sm">Built with Tailwind CSS</p><p class="text-gray-400 text-xs">Utility-first CSS</p></div>
    <button class="bg-white text-gray-900 text-xs font-bold px-3 py-1.5 rounded-lg">Start →</button>
  </div>
</div>
```

---

## Step 324: Masonry Layout

```html
<!-- CSS columns masonry -->
<style>
  .masonry {
    columns: 3;
    column-gap: 1rem;
  }
  .masonry-item {
    break-inside: avoid;
    margin-bottom: 1rem;
  }
  @media (max-width: 768px) { .masonry { columns: 2; } }
  @media (max-width: 480px) { .masonry { columns: 1; } }
</style>

<div class="masonry p-6">
  <div class="masonry-item bg-indigo-100 rounded-2xl p-5">
    <div class="h-24 bg-indigo-300 rounded-xl mb-3"></div>
    <h3 class="font-bold text-sm">Card สั้น</h3>
    <p class="text-xs text-gray-500 mt-1">เนื้อหาสั้น</p>
  </div>
  <div class="masonry-item bg-pink-100 rounded-2xl p-5">
    <div class="h-40 bg-pink-300 rounded-xl mb-3"></div>
    <h3 class="font-bold text-sm">Card ยาวกว่า</h3>
    <p class="text-xs text-gray-500 mt-1 leading-relaxed">เนื้อหาที่ยาวกว่าทำให้ card สูงกว่า masonry จัดให้โดยอัตโนมัติ</p>
  </div>
  <div class="masonry-item bg-amber-100 rounded-2xl p-5">
    <div class="h-20 bg-amber-300 rounded-xl mb-3"></div>
    <h3 class="font-bold text-sm">Medium Card</h3>
    <p class="text-xs text-gray-500 mt-1">ความสูงปานกลาง</p>
  </div>
  <div class="masonry-item bg-emerald-100 rounded-2xl p-5">
    <div class="h-52 bg-emerald-300 rounded-xl mb-3"></div>
    <h3 class="font-bold text-sm">Tall Card</h3>
    <p class="text-xs text-gray-500 mt-1 leading-relaxed">Card ที่สูงมาก</p>
  </div>
  <div class="masonry-item bg-violet-100 rounded-2xl p-5">
    <h3 class="font-bold text-sm mb-2">Text Only Card</h3>
    <p class="text-xs text-gray-500 leading-relaxed">Card ที่มีแค่ข้อความ ไม่มีรูปภาพ แต่ masonry ก็จัดให้เป็นระเบียบ</p>
  </div>
  <div class="masonry-item bg-blue-100 rounded-2xl p-5">
    <div class="h-32 bg-blue-300 rounded-xl mb-3"></div>
    <h3 class="font-bold text-sm">Another Card</h3>
  </div>
</div>
```

---

## Step 325: Sidebar + Content Layout Patterns

```html
<!-- Pattern 1: Fixed sidebar + scrollable content -->
<div class="flex h-screen">
  <aside class="w-64 flex-none overflow-y-auto bg-white border-r p-4">Sidebar scrolls independently</aside>
  <main class="flex-1 overflow-y-auto p-6">Content scrolls independently</main>
</div>

<!-- Pattern 2: Sticky sidebar, natural content flow -->
<div class="max-w-6xl mx-auto flex gap-8 p-6">
  <aside class="w-64 flex-none">
    <div class="sticky top-6 space-y-2 bg-white rounded-2xl border p-4">
      <a href="#" class="block py-2 px-3 text-sm rounded-lg bg-indigo-50 text-indigo-600 font-medium">Section 1</a>
      <a href="#" class="block py-2 px-3 text-sm rounded-lg hover:bg-gray-100 text-gray-600">Section 2</a>
      <a href="#" class="block py-2 px-3 text-sm rounded-lg hover:bg-gray-100 text-gray-600">Section 3</a>
    </div>
  </aside>
  <main class="flex-1 min-w-0 space-y-8">
    <section id="s1" class="bg-white rounded-2xl border p-6 h-64">Section 1 Content</section>
    <section id="s2" class="bg-white rounded-2xl border p-6 h-64">Section 2 Content</section>
    <section id="s3" class="bg-white rounded-2xl border p-6 h-64">Section 3 Content</section>
  </main>
</div>

<!-- Pattern 3: Content + Right sidebar (article + TOC) -->
<div class="max-w-6xl mx-auto grid grid-cols-[1fr_250px] gap-8 p-6">
  <article class="prose prose-lg">
    <h1>บทความ</h1>
    <p>เนื้อหาหลัก...</p>
  </article>
  <aside>
    <div class="sticky top-6">
      <h4 class="text-xs font-bold text-gray-400 uppercase mb-3">สารบัญ</h4>
      <nav class="space-y-1 text-sm text-gray-500">
        <a href="#" class="block hover:text-indigo-600 transition-colors">บทนำ</a>
        <a href="#" class="block hover:text-indigo-600 transition-colors">เนื้อหา</a>
        <a href="#" class="block hover:text-indigo-600 transition-colors">สรุป</a>
      </nav>
    </div>
  </aside>
</div>
```

---

## Step 326–330: Workshop — Advanced Layout Showcase

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Advanced Grid Workshop</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    .masonry { columns: 3; column-gap: 1rem; }
    .masonry-item { break-inside: avoid; margin-bottom: 1rem; }
    @media(max-width:640px) { .masonry { columns: 1; } }
    @media(max-width:768px) { .masonry { columns: 2; } }
  </style>
</head>
<body class="bg-gray-50 p-6 space-y-10">

<div class="max-w-5xl mx-auto">
  <h1 class="text-3xl font-black text-gray-900 mb-10">Advanced Layout Patterns</h1>

  <!-- Bento Grid -->
  <section class="mb-12">
    <h2 class="text-xl font-bold text-gray-900 mb-5">Bento Grid</h2>
    <div class="grid grid-cols-3 sm:grid-cols-6 grid-rows-3 gap-3 h-80">
      <div class="col-span-2 row-span-2 bg-gradient-to-br from-indigo-600 to-purple-700 rounded-2xl flex items-center justify-center text-white">
        <div class="text-center p-5"><div class="text-4xl mb-2">🚀</div><p class="font-black text-lg">Tailwind</p><p class="text-indigo-200 text-xs">CSS Framework</p></div>
      </div>
      <div class="col-span-1 bg-gradient-to-br from-pink-500 to-rose-600 rounded-2xl flex items-center justify-center text-white">
        <div class="text-center p-3"><div class="text-xl">⚡</div><p class="text-xs font-bold mt-1">Fast</p></div>
      </div>
      <div class="col-span-1 bg-gradient-to-br from-amber-400 to-orange-500 rounded-2xl flex items-center justify-center text-white">
        <div class="text-center p-3"><div class="text-xl">🔒</div><p class="text-xs font-bold mt-1">Safe</p></div>
      </div>
      <div class="col-span-2 bg-gradient-to-r from-emerald-500 to-teal-600 rounded-2xl flex items-center gap-3 px-4 text-white">
        <span class="text-2xl">📊</span>
        <div><p class="font-bold text-sm">Analytics</p><p class="text-emerald-200 text-xs">Real-time</p></div>
      </div>
      <div class="col-span-2 bg-gray-100 rounded-2xl flex items-center justify-center">
        <p class="text-sm font-medium text-gray-600">Mobile First</p>
      </div>
      <div class="col-span-2 bg-gray-900 rounded-2xl flex items-center gap-3 px-4 text-white">
        <p class="font-bold text-xs flex-1">Built with ❤️ Tailwind</p>
        <span class="text-xs text-gray-400">v3.4</span>
      </div>
      <div class="col-span-2 bg-gradient-to-r from-violet-500 to-purple-600 rounded-2xl flex items-center justify-center text-white">
        <p class="font-bold text-sm">Custom Plugins</p>
      </div>
    </div>
  </section>

  <!-- Auto-fit responsive grid -->
  <section class="mb-12">
    <h2 class="text-xl font-bold text-gray-900 mb-5">Auto-fit Responsive Grid</h2>
    <div class="grid gap-4" style="grid-template-columns: repeat(auto-fit, minmax(200px, 1fr))">
      <div class="bg-white rounded-2xl border border-gray-200 p-5 text-center">
        <div class="text-3xl mb-2">🎨</div>
        <p class="font-bold text-gray-900 text-sm">Design</p>
        <p class="text-xs text-gray-400 mt-1">Beautiful UI</p>
      </div>
      <div class="bg-white rounded-2xl border border-gray-200 p-5 text-center">
        <div class="text-3xl mb-2">⚡</div>
        <p class="font-bold text-gray-900 text-sm">Performance</p>
        <p class="text-xs text-gray-400 mt-1">Blazing fast</p>
      </div>
      <div class="bg-white rounded-2xl border border-gray-200 p-5 text-center">
        <div class="text-3xl mb-2">📱</div>
        <p class="font-bold text-gray-900 text-sm">Responsive</p>
        <p class="text-xs text-gray-400 mt-1">Any device</p>
      </div>
      <div class="bg-white rounded-2xl border border-gray-200 p-5 text-center">
        <div class="text-3xl mb-2">🛠️</div>
        <p class="font-bold text-gray-900 text-sm">Customizable</p>
        <p class="text-xs text-gray-400 mt-1">Your design</p>
      </div>
    </div>
  </section>

  <!-- Masonry -->
  <section>
    <h2 class="text-xl font-bold text-gray-900 mb-5">Masonry Layout</h2>
    <div class="masonry">
      <div class="masonry-item bg-white rounded-2xl border border-gray-200 p-4">
        <div class="bg-indigo-100 rounded-xl h-32 mb-3"></div>
        <p class="font-bold text-sm text-gray-900">Photo 1</p>
        <p class="text-xs text-gray-400 mt-1">Short description</p>
      </div>
      <div class="masonry-item bg-white rounded-2xl border border-gray-200 p-4">
        <div class="bg-pink-100 rounded-xl h-52 mb-3"></div>
        <p class="font-bold text-sm text-gray-900">Photo 2 — Tall</p>
        <p class="text-xs text-gray-400 mt-1 leading-relaxed">ภาพแนวตั้งที่สูงกว่า masonry จัดอัตโนมัติ</p>
      </div>
      <div class="masonry-item bg-white rounded-2xl border border-gray-200 p-4">
        <div class="bg-amber-100 rounded-xl h-20 mb-3"></div>
        <p class="font-bold text-sm text-gray-900">Photo 3 — Short</p>
      </div>
      <div class="masonry-item bg-white rounded-2xl border border-gray-200 p-4">
        <div class="bg-emerald-100 rounded-xl h-40 mb-3"></div>
        <p class="font-bold text-sm text-gray-900">Photo 4</p>
        <p class="text-xs text-gray-400 mt-1">Medium height image</p>
      </div>
      <div class="masonry-item bg-white rounded-2xl border border-gray-200 p-4">
        <p class="font-bold text-sm text-gray-900 mb-2">Text Only</p>
        <p class="text-xs text-gray-500 leading-relaxed">Card ที่มีเฉพาะข้อความ ไม่มีรูปภาพ ก็ยังอยู่ใน masonry ได้อย่างสวยงาม</p>
      </div>
      <div class="masonry-item bg-white rounded-2xl border border-gray-200 p-4">
        <div class="bg-violet-100 rounded-xl h-28 mb-3"></div>
        <p class="font-bold text-sm text-gray-900">Photo 6</p>
      </div>
    </div>
  </section>
</div>

</body>
</html>
```

---

## 📝 สรุป Part 33

| Pattern | วิธีทำ |
|---------|--------|
| Named grid areas | `grid-template-areas` + `grid-area` |
| Auto-fill/fit | `repeat(auto-fit, minmax(...))` |
| Bento grid | `col-span` + `row-span` combinations |
| Masonry | CSS `columns` + `break-inside: avoid` |
| Sticky sidebar | `sticky top-0` + `overflow-y-auto` |

---

*Part 33 — Advanced Grid | Steps 321–330 จาก 1,000 Steps*
