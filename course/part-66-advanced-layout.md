# Part 66: Advanced Layout Patterns

## เป้าหมาย
- CSS Grid advanced patterns ด้วย Tailwind
- Subgrid, auto-fill, auto-fit
- Holy Grail layout, Magazine layout
- Container queries (Tailwind v3.3+)
- Steps 651–660

---

## Step 651: Advanced Grid Patterns

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Advanced Grid - Step 651</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      theme: {
        extend: {
          gridTemplateColumns: {
            'sidebar': '240px 1fr',
            'sidebar-right': '1fr 240px',
            'three-col': '1fr 2fr 1fr',
            'auto-fill': 'repeat(auto-fill, minmax(200px, 1fr))',
            'auto-fit':  'repeat(auto-fit, minmax(200px, 1fr))',
          },
          gridTemplateRows: {
            'layout': 'auto 1fr auto',
          },
        },
      },
    }
  </script>
</head>
<body class="bg-gray-100 p-8">
  <div class="space-y-10 max-w-5xl mx-auto">

    <!-- Auto-fill vs Auto-fit -->
    <section>
      <h2 class="text-sm font-semibold text-indigo-600 uppercase tracking-widest mb-3">auto-fill vs auto-fit</h2>

      <p class="text-xs text-gray-500 mb-2">auto-fill — keeps empty grid tracks</p>
      <div class="grid gap-3 mb-6" style="grid-template-columns: repeat(auto-fill, minmax(140px, 1fr))">
        <div v-for="i in 3" :key="i" class="bg-indigo-100 rounded-xl p-4 text-sm font-semibold text-indigo-700 text-center">Item</div>
        <script>
          [1,2,3].forEach(i => document.currentScript.insertAdjacentHTML('beforebegin',
            `<div class="bg-indigo-100 rounded-xl p-4 text-sm font-semibold text-indigo-700 text-center">Item ${i}</div>`))
        </script>
      </div>

      <p class="text-xs text-gray-500 mb-2">auto-fit — stretches items to fill space</p>
      <div class="grid gap-3" style="grid-template-columns: repeat(auto-fit, minmax(140px, 1fr))">
        <script>
          [1,2,3].forEach(i => document.currentScript.insertAdjacentHTML('beforebegin',
            `<div class="bg-purple-100 rounded-xl p-4 text-sm font-semibold text-purple-700 text-center">Item ${i}</div>`))
        </script>
      </div>
    </section>

    <!-- Dense packing -->
    <section>
      <h2 class="text-sm font-semibold text-indigo-600 uppercase tracking-widest mb-3">Dense Grid (Masonry-like)</h2>
      <div class="grid gap-3" style="grid-template-columns: repeat(4, 1fr); grid-auto-rows: 60px; grid-auto-flow: dense">
        <div class="bg-indigo-500 rounded-xl col-span-2 row-span-2 flex items-center justify-center text-white font-bold">2×2</div>
        <div class="bg-blue-400 rounded-xl flex items-center justify-center text-white font-bold">1×1</div>
        <div class="bg-purple-400 rounded-xl row-span-2 flex items-center justify-center text-white font-bold">1×2</div>
        <div class="bg-pink-400 rounded-xl flex items-center justify-center text-white font-bold">1×1</div>
        <div class="bg-orange-400 rounded-xl col-span-2 flex items-center justify-center text-white font-bold">2×1</div>
        <div class="bg-green-400 rounded-xl flex items-center justify-center text-white font-bold">1×1</div>
        <div class="bg-teal-400 rounded-xl flex items-center justify-center text-white font-bold">1×1</div>
        <div class="bg-yellow-400 rounded-xl col-span-2 flex items-center justify-center text-white font-bold">2×1</div>
      </div>
    </section>

    <!-- Magazine layout -->
    <section>
      <h2 class="text-sm font-semibold text-indigo-600 uppercase tracking-widest mb-3">Magazine Layout</h2>
      <div class="grid gap-4" style="grid-template-columns: 2fr 1fr 1fr; grid-template-rows: auto auto">
        <!-- Featured -->
        <div class="row-span-2 bg-indigo-600 rounded-2xl p-6 flex flex-col justify-end text-white">
          <span class="text-xs font-semibold bg-white/20 px-2 py-1 rounded-full w-fit mb-3">Featured</span>
          <h3 class="text-xl font-extrabold mb-2">บทความหลักขนาดใหญ่</h3>
          <p class="text-sm opacity-80">เนื้อหาสำคัญที่ต้องการพื้นที่แสดงผลมากกว่าปกติ</p>
        </div>
        <!-- Secondary 1 -->
        <div class="bg-white rounded-2xl p-4 shadow-sm border">
          <span class="text-xs text-indigo-600 font-semibold">Technology</span>
          <h4 class="font-bold text-sm text-gray-900 mt-1">บทความรอง 1</h4>
          <p class="text-xs text-gray-500 mt-1">คำอธิบายสั้นๆ</p>
        </div>
        <!-- Secondary 2 -->
        <div class="bg-white rounded-2xl p-4 shadow-sm border">
          <span class="text-xs text-purple-600 font-semibold">Design</span>
          <h4 class="font-bold text-sm text-gray-900 mt-1">บทความรอง 2</h4>
          <p class="text-xs text-gray-500 mt-1">คำอธิบายสั้นๆ</p>
        </div>
        <!-- Secondary 3 -->
        <div class="bg-white rounded-2xl p-4 shadow-sm border">
          <span class="text-xs text-green-600 font-semibold">Business</span>
          <h4 class="font-bold text-sm text-gray-900 mt-1">บทความรอง 3</h4>
          <p class="text-xs text-gray-500 mt-1">คำอธิบายสั้นๆ</p>
        </div>
        <!-- Secondary 4 -->
        <div class="bg-white rounded-2xl p-4 shadow-sm border">
          <span class="text-xs text-orange-600 font-semibold">Culture</span>
          <h4 class="font-bold text-sm text-gray-900 mt-1">บทความรอง 4</h4>
          <p class="text-xs text-gray-500 mt-1">คำอธิบายสั้นๆ</p>
        </div>
      </div>
    </section>

  </div>
</body>
</html>
```

---

## Steps 652–660: Container Queries + Holy Grail + Workshop

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Layout Workshop - Steps 652-660</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    /* Container Queries */
    .card-container {
      container-type: inline-size;
      container-name: card;
    }

    @container card (min-width: 400px) {
      .card-inner {
        flex-direction: row;
        gap: 1rem;
      }
      .card-img {
        width: 8rem;
        height: 8rem;
        flex-shrink: 0;
      }
    }

    @container card (min-width: 600px) {
      .card-inner { gap: 1.5rem; }
      .card-img { width: 12rem; height: 10rem; }
    }

    /* Holy Grail with CSS Grid */
    .holy-grail {
      display: grid;
      grid-template: "header header header" auto
                     "nav    main   aside"  1fr
                     "footer footer footer" auto
                     / 180px 1fr 180px;
      min-height: 100vh;
    }
    .hg-header  { grid-area: header; }
    .hg-nav     { grid-area: nav; }
    .hg-main    { grid-area: main; }
    .hg-aside   { grid-area: aside; }
    .hg-footer  { grid-area: footer; }

    @media (max-width: 768px) {
      .holy-grail {
        grid-template: "header" auto
                       "nav"    auto
                       "main"   1fr
                       "aside"  auto
                       "footer" auto
                       / 1fr;
      }
    }

    /* Sidebar + Content + Aside — 3 col layout */
    .three-panel {
      display: grid;
      grid-template-columns: 200px 1fr 240px;
      gap: 0;
      height: 100%;
    }
    @media (max-width: 1024px) {
      .three-panel { grid-template-columns: 180px 1fr; }
      .three-panel .panel-aside { display: none; }
    }
    @media (max-width: 640px) {
      .three-panel { grid-template-columns: 1fr; }
      .three-panel .panel-nav { display: none; }
    }

    /* Sticky sidebar */
    .sticky-sidebar {
      position: sticky;
      top: 0;
      height: 100vh;
      overflow-y: auto;
    }
  </style>
</head>
<body class="bg-gray-100">

  <!-- Demo tabs -->
  <div class="bg-white border-b px-6 py-3 flex items-center gap-2">
    <span class="text-sm font-semibold text-gray-700 mr-2">Demos:</span>
    <button onclick="showDemo('container')" class="demo-btn px-3 py-1.5 bg-indigo-600 text-white text-xs font-semibold rounded-lg">Container Queries</button>
    <button onclick="showDemo('holy')" class="demo-btn px-3 py-1.5 bg-white border text-gray-600 text-xs font-semibold rounded-lg hover:bg-gray-50">Holy Grail</button>
    <button onclick="showDemo('three')" class="demo-btn px-3 py-1.5 bg-white border text-gray-600 text-xs font-semibold rounded-lg hover:bg-gray-50">3-Panel</button>
    <button onclick="showDemo('subgrid')" class="demo-btn px-3 py-1.5 bg-white border text-gray-600 text-xs font-semibold rounded-lg hover:bg-gray-50">Subgrid</button>
  </div>

  <!-- Container Queries Demo -->
  <div id="demo-container" class="p-6 space-y-4">
    <h2 class="text-sm font-semibold text-indigo-600 uppercase tracking-widest">Container Queries</h2>
    <p class="text-xs text-gray-500 mb-4">ลาก resize handle ของ container เพื่อดู card ปรับ layout ตามขนาด container (ไม่ใช่ viewport)</p>

    <!-- Resizable container -->
    <div style="resize: horizontal; overflow: hidden; border: 2px dashed #c7d2fe; border-radius: 1rem; padding: 1rem; min-width: 200px; max-width: 100%">
      <p class="text-xs text-indigo-400 mb-3">← drag to resize container →</p>

      <div class="card-container">
        <div class="card-inner flex flex-col gap-3 bg-white rounded-2xl p-4 shadow-sm border">
          <div class="card-img w-full h-32 bg-gradient-to-br from-indigo-400 to-purple-500 rounded-xl flex items-center justify-center text-white text-3xl font-bold">🖼</div>
          <div class="flex-1">
            <h3 class="font-bold text-gray-900">Container Query Card</h3>
            <p class="text-sm text-gray-500 mt-1">Card นี้จะเปลี่ยน layout จาก vertical เป็น horizontal เมื่อ container กว้างพอ ไม่ใช่เมื่อ viewport กว้าง</p>
            <div class="flex gap-2 mt-3">
              <span class="text-xs bg-indigo-100 text-indigo-700 px-2 py-1 rounded-full font-semibold">Design</span>
              <span class="text-xs bg-purple-100 text-purple-700 px-2 py-1 rounded-full font-semibold">Layout</span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Multiple cards in different width containers -->
    <div class="grid grid-cols-1 gap-4">
      <div style="max-width: 280px" class="card-container">
        <div class="card-inner flex flex-col gap-3 bg-white rounded-2xl p-4 shadow-sm border">
          <div class="card-img w-full h-24 bg-gradient-to-br from-blue-400 to-cyan-500 rounded-xl flex items-center justify-center text-white text-2xl">📱</div>
          <div><h3 class="font-bold text-sm text-gray-900">Narrow (280px)</h3><p class="text-xs text-gray-500">Vertical layout</p></div>
        </div>
      </div>
      <div style="max-width: 500px" class="card-container">
        <div class="card-inner flex flex-col gap-3 bg-white rounded-2xl p-4 shadow-sm border">
          <div class="card-img w-full h-24 bg-gradient-to-br from-green-400 to-emerald-500 rounded-xl flex items-center justify-center text-white text-2xl">🖥</div>
          <div><h3 class="font-bold text-sm text-gray-900">Medium (500px)</h3><p class="text-xs text-gray-500">Switches to horizontal</p></div>
        </div>
      </div>
    </div>
  </div>

  <!-- Holy Grail Demo -->
  <div id="demo-holy" class="hidden h-screen">
    <div class="holy-grail h-full">
      <header class="hg-header bg-indigo-700 text-white px-6 py-3 flex items-center font-bold">Header</header>
      <nav class="hg-nav bg-indigo-900 text-white p-4 text-sm">
        <p class="font-semibold mb-3 text-indigo-300">Navigation</p>
        <ul class="space-y-2">
          <li><a class="text-white hover:text-indigo-200">Dashboard</a></li>
          <li><a class="text-white hover:text-indigo-200">Users</a></li>
          <li><a class="text-white hover:text-indigo-200">Settings</a></li>
        </ul>
      </nav>
      <main class="hg-main bg-gray-50 p-6 overflow-y-auto">
        <h1 class="text-xl font-extrabold text-gray-900 mb-4">Holy Grail Layout</h1>
        <p class="text-gray-600 text-sm">Header + Left Nav + Main Content + Right Aside + Footer ด้วย CSS Grid Area</p>
        <div class="mt-4 grid grid-cols-2 gap-3">
          <div class="bg-white rounded-xl p-4 border"><p class="text-sm font-semibold">Card 1</p></div>
          <div class="bg-white rounded-xl p-4 border"><p class="text-sm font-semibold">Card 2</p></div>
          <div class="bg-white rounded-xl p-4 border"><p class="text-sm font-semibold">Card 3</p></div>
          <div class="bg-white rounded-xl p-4 border"><p class="text-sm font-semibold">Card 4</p></div>
        </div>
      </main>
      <aside class="hg-aside bg-white border-l p-4 text-sm overflow-y-auto">
        <p class="font-semibold mb-3 text-gray-700">Aside</p>
        <div class="space-y-3">
          <div class="bg-gray-50 rounded-xl p-3 text-xs text-gray-600">Widget 1</div>
          <div class="bg-gray-50 rounded-xl p-3 text-xs text-gray-600">Widget 2</div>
        </div>
      </aside>
      <footer class="hg-footer bg-gray-800 text-gray-300 px-6 py-3 text-sm flex items-center">Footer — &copy; 2025</footer>
    </div>
  </div>

  <!-- Three Panel Demo -->
  <div id="demo-three" class="hidden h-screen">
    <div class="three-panel h-full">
      <div class="panel-nav sticky-sidebar bg-white border-r p-4">
        <p class="font-bold text-sm text-gray-900 mb-3">Navigation</p>
        <ul class="space-y-1 text-sm text-gray-600">
          <li class="bg-indigo-50 text-indigo-700 font-semibold px-3 py-2 rounded-xl">Dashboard</li>
          <li class="px-3 py-2 hover:bg-gray-50 rounded-xl cursor-pointer">Users</li>
          <li class="px-3 py-2 hover:bg-gray-50 rounded-xl cursor-pointer">Analytics</li>
          <li class="px-3 py-2 hover:bg-gray-50 rounded-xl cursor-pointer">Settings</li>
        </ul>
      </div>
      <div class="overflow-y-auto p-6 bg-gray-50">
        <h1 class="text-xl font-extrabold text-gray-900 mb-4">3-Panel Layout</h1>
        <p class="text-sm text-gray-600 mb-4">Nav sidebar + Main content + Details sidebar — responsive: hides aside on &lt;1024px, hides nav on &lt;640px</p>
        <div class="space-y-3">
          <div class="bg-white rounded-2xl border p-5">Main content area</div>
          <div class="bg-white rounded-2xl border p-5">More content</div>
          <div class="bg-white rounded-2xl border p-5">Even more content</div>
        </div>
      </div>
      <div class="panel-aside sticky-sidebar bg-white border-l p-4">
        <p class="font-bold text-sm text-gray-900 mb-3">Details</p>
        <div class="space-y-3 text-xs text-gray-500">
          <div class="bg-gray-50 rounded-xl p-3">Info panel 1</div>
          <div class="bg-gray-50 rounded-xl p-3">Info panel 2</div>
        </div>
      </div>
    </div>
  </div>

  <!-- Subgrid Demo -->
  <div id="demo-subgrid" class="hidden p-6">
    <h2 class="text-sm font-semibold text-indigo-600 uppercase tracking-widest mb-4">CSS Subgrid</h2>
    <p class="text-xs text-gray-500 mb-4">Subgrid ให้ child elements align กับ parent grid tracks</p>
    <div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 1rem">
      <div style="display: grid; grid-row: span 3; grid-template-rows: subgrid" class="bg-white rounded-2xl border overflow-hidden">
        <div class="bg-indigo-500 h-32"></div>
        <div class="p-4">
          <h3 class="font-bold text-gray-900">Card A</h3>
          <p class="text-sm text-gray-500 mt-1">ยาวกว่า — Lorem ipsum dolor sit amet consectetur adipiscing elit sed do</p>
        </div>
        <div class="p-4 border-t">
          <button class="text-xs font-semibold text-indigo-600">Read more →</button>
        </div>
      </div>
      <div style="display: grid; grid-row: span 3; grid-template-rows: subgrid" class="bg-white rounded-2xl border overflow-hidden">
        <div class="bg-purple-500 h-32"></div>
        <div class="p-4">
          <h3 class="font-bold text-gray-900">Card B</h3>
          <p class="text-sm text-gray-500 mt-1">สั้นกว่า</p>
        </div>
        <div class="p-4 border-t">
          <button class="text-xs font-semibold text-purple-600">Read more →</button>
        </div>
      </div>
      <div style="display: grid; grid-row: span 3; grid-template-rows: subgrid" class="bg-white rounded-2xl border overflow-hidden">
        <div class="bg-pink-500 h-32"></div>
        <div class="p-4">
          <h3 class="font-bold text-gray-900">Card C</h3>
          <p class="text-sm text-gray-500 mt-1">ความยาวปานกลาง — เนื้อหาพอสมควร</p>
        </div>
        <div class="p-4 border-t">
          <button class="text-xs font-semibold text-pink-600">Read more →</button>
        </div>
      </div>
    </div>
    <p class="text-xs text-indigo-600 mt-4">✓ ปุ่ม "Read more" align กันทุก card โดยไม่ต้องกำหนด fixed height</p>
  </div>

  <script>
    function showDemo(name) {
      ['container','holy','three','subgrid'].forEach(d => {
        document.getElementById('demo-' + d).classList.toggle('hidden', d !== name);
      });
      document.querySelectorAll('.demo-btn').forEach((btn, i) => {
        const names = ['container','holy','three','subgrid'];
        const active = names[i] === name;
        btn.className = `demo-btn px-3 py-1.5 text-xs font-semibold rounded-lg ${active ? 'bg-indigo-600 text-white' : 'bg-white border text-gray-600 hover:bg-gray-50'}`;
      });
    }
  </script>
</body>
</html>
```

---

## สรุป Part 66

| Step | เนื้อหา |
|------|---------|
| 651 | auto-fill vs auto-fit, dense grid |
| 652 | Magazine layout ด้วย Grid Template Areas |
| 653 | Container Queries — @container + container-type |
| 654 | Card ที่ responsive ต่อ container ไม่ใช่ viewport |
| 655 | Holy Grail layout ด้วย grid-template areas |
| 656 | 3-Panel layout responsive |
| 657 | Sticky sidebar patterns |
| 658 | CSS Subgrid — align cards โดยไม่ต้องใช้ fixed height |
| 659 | Tailwind custom gridTemplateColumns + gridTemplateRows |
| 660 | Workshop: Layout playground — Container/Holy Grail/3-Panel/Subgrid |

**Part ถัดไป:** Part 67 — E-commerce Components (Steps 661–670)
