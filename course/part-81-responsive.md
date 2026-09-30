# Part 81: Responsive Design Patterns

## เป้าหมาย
- Breakpoint system ของ Tailwind
- Mobile-first approach
- Responsive layout patterns: sidebar, grid, navigation
- Container queries
- Steps 801–810

---

## Step 801: Breakpoint System

```
Tailwind Breakpoints (mobile-first):
  sm   640px   @media (min-width: 640px)
  md   768px   @media (min-width: 768px)
  lg  1024px   @media (min-width: 1024px)
  xl  1280px   @media (min-width: 1280px)
  2xl 1536px   @media (min-width: 1536px)
```

```html
<!-- Mobile-first: ค่า default = mobile, เพิ่ม breakpoint = ขยาย -->
<div class="
  flex flex-col      <!-- mobile: stack vertical -->
  md:flex-row        <!-- tablet+: side by side -->
  gap-4
">
  <aside class="w-full md:w-64 lg:w-72">Sidebar</aside>
  <main class="flex-1">Content</main>
</div>

<!-- Typography responsive -->
<h1 class="text-xl sm:text-2xl md:text-3xl lg:text-4xl xl:text-5xl font-extrabold">
  Responsive Heading
</h1>

<!-- Grid responsive -->
<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4">
  <!-- Cards -->
</div>
```

---

## Step 802: Responsive Navigation

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Responsive Nav</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50">
  <nav class="bg-white border-b sticky top-0 z-40">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="flex items-center justify-between h-16">
        <!-- Logo -->
        <div class="font-extrabold text-xl text-indigo-600">Brand</div>

        <!-- Desktop nav -->
        <div class="hidden md:flex items-center gap-6">
          <a href="#" class="text-sm font-medium text-gray-700 hover:text-indigo-600 transition-colors">Home</a>
          <a href="#" class="text-sm font-medium text-gray-700 hover:text-indigo-600 transition-colors">Products</a>
          <a href="#" class="text-sm font-medium text-gray-700 hover:text-indigo-600 transition-colors">About</a>
          <a href="#" class="text-sm font-medium text-gray-700 hover:text-indigo-600 transition-colors">Contact</a>
        </div>

        <!-- Desktop CTA -->
        <div class="hidden md:flex items-center gap-3">
          <button class="text-sm font-medium text-gray-600 hover:text-gray-900">Sign in</button>
          <button class="px-4 py-2 bg-indigo-600 text-white text-sm font-semibold rounded-xl hover:bg-indigo-700 transition-colors">Get started</button>
        </div>

        <!-- Mobile hamburger -->
        <button id="hamburger" onclick="toggleMenu()" class="md:hidden p-2 rounded-xl hover:bg-gray-100 transition-colors" aria-expanded="false" aria-controls="mobile-menu">
          <div id="ham-icon" class="space-y-1.5 w-5">
            <span class="block h-0.5 bg-gray-700 transition-all origin-center"></span>
            <span class="block h-0.5 bg-gray-700 transition-all"></span>
            <span class="block h-0.5 bg-gray-700 transition-all origin-center"></span>
          </div>
        </button>
      </div>
    </div>

    <!-- Mobile menu -->
    <div id="mobile-menu" class="md:hidden hidden border-t bg-white">
      <div class="px-4 py-3 space-y-1">
        <a href="#" class="block px-3 py-2 text-sm font-medium text-gray-700 rounded-xl hover:bg-gray-50">Home</a>
        <a href="#" class="block px-3 py-2 text-sm font-medium text-gray-700 rounded-xl hover:bg-gray-50">Products</a>
        <a href="#" class="block px-3 py-2 text-sm font-medium text-gray-700 rounded-xl hover:bg-gray-50">About</a>
        <a href="#" class="block px-3 py-2 text-sm font-medium text-gray-700 rounded-xl hover:bg-gray-50">Contact</a>
      </div>
      <div class="px-4 pb-4 flex flex-col gap-2">
        <button class="w-full py-2.5 border text-sm font-semibold rounded-xl hover:bg-gray-50">Sign in</button>
        <button class="w-full py-2.5 bg-indigo-600 text-white text-sm font-semibold rounded-xl">Get started</button>
      </div>
    </div>
  </nav>

  <main class="max-w-7xl mx-auto px-4 py-12">
    <h1 class="text-3xl font-extrabold text-gray-900">Page Content</h1>
    <p class="text-gray-500 mt-2">Resize the window to see mobile navigation.</p>
  </main>

  <script>
    function toggleMenu() {
      const menu = document.getElementById('mobile-menu');
      const btn  = document.getElementById('hamburger');
      const open = !menu.classList.contains('hidden');
      menu.classList.toggle('hidden');
      btn.setAttribute('aria-expanded', String(!open));
    }
  </script>
</body>
</html>
```

---

## Step 803: Responsive Sidebar Layout

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Responsive Sidebar</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    @keyframes slideIn { from{transform:translateX(-100%)} to{transform:translateX(0)} }
    .sidebar-mobile { animation: slideIn 0.25s ease; }
  </style>
</head>
<body class="bg-gray-100 h-screen flex overflow-hidden">
  <!-- Sidebar — hidden on mobile, fixed on lg -->
  <aside id="sidebar" class="
    hidden lg:flex
    w-64 flex-shrink-0 flex-col
    bg-white border-r
  ">
    <div class="h-16 flex items-center px-6 border-b font-extrabold text-indigo-600 text-xl">Brand</div>
    <nav class="flex-1 p-4 space-y-1 overflow-y-auto">
      <a href="#" class="flex items-center gap-3 px-3 py-2 rounded-xl bg-indigo-50 text-indigo-700 font-medium text-sm">🏠 Dashboard</a>
      <a href="#" class="flex items-center gap-3 px-3 py-2 rounded-xl text-gray-600 hover:bg-gray-100 text-sm">📦 Products</a>
      <a href="#" class="flex items-center gap-3 px-3 py-2 rounded-xl text-gray-600 hover:bg-gray-100 text-sm">👥 Customers</a>
      <a href="#" class="flex items-center gap-3 px-3 py-2 rounded-xl text-gray-600 hover:bg-gray-100 text-sm">📊 Analytics</a>
      <a href="#" class="flex items-center gap-3 px-3 py-2 rounded-xl text-gray-600 hover:bg-gray-100 text-sm">⚙️ Settings</a>
    </nav>
  </aside>

  <!-- Mobile drawer overlay -->
  <div id="overlay" class="hidden fixed inset-0 bg-black/50 z-30 lg:hidden" onclick="closeSidebar()"></div>
  <!-- Mobile sidebar drawer -->
  <aside id="mobile-sidebar" class="
    hidden fixed left-0 top-0 bottom-0 z-40 w-64
    bg-white border-r flex-col lg:hidden
  ">
    <div class="h-16 flex items-center justify-between px-6 border-b">
      <span class="font-extrabold text-indigo-600 text-xl">Brand</span>
      <button onclick="closeSidebar()" class="text-gray-400 hover:text-gray-600">✕</button>
    </div>
    <nav class="flex-1 p-4 space-y-1 overflow-y-auto">
      <a href="#" class="flex items-center gap-3 px-3 py-2 rounded-xl bg-indigo-50 text-indigo-700 font-medium text-sm">🏠 Dashboard</a>
      <a href="#" class="flex items-center gap-3 px-3 py-2 rounded-xl text-gray-600 hover:bg-gray-100 text-sm">📦 Products</a>
      <a href="#" class="flex items-center gap-3 px-3 py-2 rounded-xl text-gray-600 hover:bg-gray-100 text-sm">👥 Customers</a>
    </nav>
  </aside>

  <!-- Main content -->
  <div class="flex-1 flex flex-col min-w-0 overflow-hidden">
    <!-- Top bar -->
    <header class="h-16 bg-white border-b flex items-center px-4 gap-4">
      <button onclick="openSidebar()" class="lg:hidden p-2 rounded-xl hover:bg-gray-100 transition-colors">☰</button>
      <h1 class="font-bold text-gray-900">Dashboard</h1>
    </header>
    <!-- Page content -->
    <main class="flex-1 overflow-y-auto p-4 md:p-6">
      <div class="grid grid-cols-1 sm:grid-cols-2 xl:grid-cols-4 gap-4 mb-6">
        <div class="bg-white rounded-2xl border p-5"><p class="text-xs text-gray-400">Revenue</p><p class="text-2xl font-extrabold text-gray-900 mt-1">$48,295</p></div>
        <div class="bg-white rounded-2xl border p-5"><p class="text-xs text-gray-400">Orders</p><p class="text-2xl font-extrabold text-gray-900 mt-1">1,293</p></div>
        <div class="bg-white rounded-2xl border p-5"><p class="text-xs text-gray-400">Customers</p><p class="text-2xl font-extrabold text-gray-900 mt-1">5,841</p></div>
        <div class="bg-white rounded-2xl border p-5"><p class="text-xs text-gray-400">Conversion</p><p class="text-2xl font-extrabold text-gray-900 mt-1">3.24%</p></div>
      </div>
      <div class="bg-white rounded-2xl border p-5 h-48 flex items-center justify-center text-gray-300 text-lg">Chart Area</div>
    </main>
  </div>

  <script>
    function openSidebar() {
      document.getElementById('mobile-sidebar').classList.remove('hidden');
      document.getElementById('mobile-sidebar').classList.add('flex','sidebar-mobile');
      document.getElementById('overlay').classList.remove('hidden');
    }
    function closeSidebar() {
      document.getElementById('mobile-sidebar').classList.add('hidden');
      document.getElementById('mobile-sidebar').classList.remove('flex','sidebar-mobile');
      document.getElementById('overlay').classList.add('hidden');
    }
  </script>
</body>
</html>
```

---

## Step 804: Responsive Card Grid

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Responsive Grid</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 min-h-screen p-4 md:p-6 lg:p-8">
  <div class="max-w-6xl mx-auto">
    <h1 class="text-xl md:text-2xl font-extrabold text-gray-900 mb-6">Product Grid</h1>
    <div class="grid grid-cols-1 xs:grid-cols-2 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-3 md:gap-4 lg:gap-5">
      <!-- Cards generated by JS -->
      <div id="grid"></div>
    </div>
  </div>
  <script>
    const products = Array.from({length:12}, (_,i) => ({
      id: i+1,
      name: `Product ${i+1}`,
      price: (Math.random()*200+20).toFixed(2),
      tag: ['New','Sale','Hot',''][i%4],
    }));
    const grid = document.getElementById('grid');
    grid.outerHTML = `<div class="contents">${products.map(p => `
      <div class="bg-white rounded-2xl border border-gray-200 overflow-hidden hover:shadow-lg hover:-translate-y-0.5 transition-all group">
        <div class="relative">
          <div class="h-40 sm:h-48 bg-gradient-to-br from-indigo-100 to-purple-100 flex items-center justify-center text-4xl">
            🛍️
          </div>
          ${p.tag ? `<span class="absolute top-2 right-2 px-2 py-0.5 bg-indigo-600 text-white text-xs rounded-lg font-semibold">${p.tag}</span>` : ''}
        </div>
        <div class="p-3 md:p-4">
          <h3 class="font-semibold text-gray-900 text-sm group-hover:text-indigo-600 transition-colors truncate">${p.name}</h3>
          <p class="text-indigo-600 font-bold mt-1">$${p.price}</p>
          <button class="mt-3 w-full py-1.5 bg-indigo-600 text-white text-xs font-semibold rounded-xl hover:bg-indigo-700 transition-colors">Add to cart</button>
        </div>
      </div>
    `).join('')}</div>`;
  </script>
</body>
</html>
```

---

## Step 805: Responsive Table → Cards

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Responsive Table</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 min-h-screen p-4 md:p-8">
  <div class="max-w-4xl mx-auto">
    <h1 class="text-xl font-extrabold text-gray-900 mb-4">Orders</h1>

    <!-- Desktop table -->
    <div class="hidden md:block bg-white rounded-2xl border overflow-hidden">
      <table class="w-full text-sm">
        <thead class="bg-gray-50 border-b">
          <tr>
            <th class="text-left px-5 py-3 font-semibold text-gray-600">Order</th>
            <th class="text-left px-5 py-3 font-semibold text-gray-600">Customer</th>
            <th class="text-left px-5 py-3 font-semibold text-gray-600">Status</th>
            <th class="text-right px-5 py-3 font-semibold text-gray-600">Amount</th>
          </tr>
        </thead>
        <tbody class="divide-y divide-gray-100" id="table-body"></tbody>
      </table>
    </div>

    <!-- Mobile card list -->
    <div class="md:hidden space-y-3" id="card-list"></div>
  </div>

  <script>
    const orders = [
      { id:'#1001', customer:'John Doe',    status:'completed', amount:'$149.00' },
      { id:'#1002', customer:'Sarah Smith', status:'pending',   amount:'$89.50'  },
      { id:'#1003', customer:'Mike Johnson',status:'shipped',   amount:'$220.00' },
      { id:'#1004', customer:'Anna Lee',    status:'cancelled', amount:'$55.00'  },
    ];
    const colors = { completed:'bg-emerald-100 text-emerald-700', pending:'bg-yellow-100 text-yellow-700', shipped:'bg-blue-100 text-blue-700', cancelled:'bg-red-100 text-red-700' };
    document.getElementById('table-body').innerHTML = orders.map(o => `
      <tr class="hover:bg-gray-50 transition-colors">
        <td class="px-5 py-3.5 font-semibold text-gray-900">${o.id}</td>
        <td class="px-5 py-3.5 text-gray-600">${o.customer}</td>
        <td class="px-5 py-3.5"><span class="px-2.5 py-0.5 rounded-full text-xs font-semibold ${colors[o.status]}">${o.status}</span></td>
        <td class="px-5 py-3.5 text-right font-semibold text-gray-900">${o.amount}</td>
      </tr>`).join('');
    document.getElementById('card-list').innerHTML = orders.map(o => `
      <div class="bg-white rounded-2xl border p-4">
        <div class="flex justify-between items-start">
          <div>
            <p class="font-bold text-gray-900">${o.id}</p>
            <p class="text-sm text-gray-500 mt-0.5">${o.customer}</p>
          </div>
          <span class="px-2.5 py-0.5 rounded-full text-xs font-semibold ${colors[o.status]}">${o.status}</span>
        </div>
        <div class="mt-3 pt-3 border-t flex justify-between">
          <span class="text-xs text-gray-400">Amount</span>
          <span class="font-bold text-gray-900">${o.amount}</span>
        </div>
      </div>`).join('');
  </script>
</body>
</html>
```

---

## Step 806: Container Queries

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Container Queries</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    /* Container query — component adapts to its container, not viewport */
    .card-container { container-type: inline-size; }

    @container (min-width: 400px) {
      .cq-card { display: flex; gap: 1rem; }
      .cq-img  { width: 8rem; height: 8rem; flex-shrink: 0; }
      .cq-body { padding: 0; }
    }
  </style>
</head>
<body class="bg-gray-100 min-h-screen p-8">
  <div class="max-w-3xl mx-auto">
    <h1 class="text-xl font-extrabold text-gray-900 mb-6">Container Queries</h1>
    <p class="text-gray-500 text-sm mb-6">Card adapts to its container width, not the viewport.</p>
    <div class="grid gap-6">
      <!-- Narrow container -->
      <div>
        <p class="text-xs font-semibold text-gray-400 uppercase tracking-wide mb-2">Narrow (250px)</p>
        <div class="card-container w-[250px]">
          <article class="cq-card bg-white rounded-2xl border overflow-hidden">
            <div class="cq-img h-32 bg-gradient-to-br from-indigo-100 to-purple-100 flex items-center justify-center text-4xl">📸</div>
            <div class="cq-body p-4">
              <h3 class="font-semibold text-gray-900 text-sm">Article Title</h3>
              <p class="text-gray-500 text-xs mt-1">Short description here.</p>
            </div>
          </article>
        </div>
      </div>
      <!-- Wide container -->
      <div>
        <p class="text-xs font-semibold text-gray-400 uppercase tracking-wide mb-2">Wide (500px)</p>
        <div class="card-container w-[500px]">
          <article class="cq-card bg-white rounded-2xl border overflow-hidden">
            <div class="cq-img h-32 bg-gradient-to-br from-indigo-100 to-purple-100 flex items-center justify-center text-4xl">📸</div>
            <div class="cq-body p-4">
              <h3 class="font-semibold text-gray-900 text-sm">Article Title</h3>
              <p class="text-gray-500 text-xs mt-1">Now displayed side by side because container is wider.</p>
            </div>
          </article>
        </div>
      </div>
    </div>
  </div>
</body>
</html>
```

---

## Step 807: Responsive Typography

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Responsive Typography</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    /* Fluid typography — scales smoothly between breakpoints */
    .fluid-title {
      font-size: clamp(1.5rem, 5vw, 3.5rem);
      line-height: 1.15;
      font-weight: 900;
    }
    .fluid-body {
      font-size: clamp(0.9rem, 2vw, 1.125rem);
      line-height: 1.75;
    }
  </style>
</head>
<body class="bg-white min-h-screen">
  <div class="max-w-3xl mx-auto px-4 sm:px-6 lg:px-8 py-12 md:py-20">
    <!-- Fluid heading -->
    <h1 class="fluid-title text-gray-900 mb-6">
      Responsive Typography<br>
      <span class="text-indigo-600">Scales with Viewport</span>
    </h1>
    <p class="fluid-body text-gray-600 mb-8">
      เนื้อหาบทความที่ใช้ clamp() เพื่อให้ขนาด font scale อย่าง smooth
      ระหว่าง viewport ขนาดเล็กถึงขนาดใหญ่ โดยไม่ต้องใช้ breakpoint
    </p>
    <!-- Step scale (Tailwind) -->
    <div class="space-y-2 border-t pt-8">
      <p class="text-xs font-semibold text-gray-400 uppercase tracking-wide mb-4">Tailwind Step Scale</p>
      <p class="text-sm sm:text-base md:text-lg text-gray-700">text-sm → sm:text-base → md:text-lg</p>
      <p class="text-base sm:text-lg md:text-xl lg:text-2xl font-semibold text-gray-800">text-base → sm:text-lg → md:text-xl → lg:text-2xl</p>
      <p class="text-2xl sm:text-3xl md:text-4xl lg:text-5xl xl:text-6xl font-extrabold text-gray-900">Hero scale</p>
    </div>
  </div>
</body>
</html>
```

---

## Step 808: Responsive Form Layout

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Responsive Form</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 min-h-screen p-4 md:p-8 flex items-start justify-center pt-12">
  <div class="bg-white rounded-2xl border shadow-sm w-full max-w-2xl p-6 md:p-8">
    <h1 class="text-xl font-extrabold text-gray-900 mb-6">Account Settings</h1>
    <form class="space-y-5">
      <!-- Two-column on md+ -->
      <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1.5">First Name</label>
          <input type="text" class="w-full px-3 py-2 border rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500" placeholder="John">
        </div>
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1.5">Last Name</label>
          <input type="text" class="w-full px-3 py-2 border rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500" placeholder="Doe">
        </div>
      </div>
      <!-- Full width -->
      <div>
        <label class="block text-sm font-medium text-gray-700 mb-1.5">Email</label>
        <input type="email" class="w-full px-3 py-2 border rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500" placeholder="john@example.com">
      </div>
      <!-- Three-column on lg+ -->
      <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1.5">City</label>
          <input type="text" class="w-full px-3 py-2 border rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
        </div>
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1.5">State</label>
          <select class="w-full px-3 py-2 border rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 bg-white">
            <option>Bangkok</option><option>Chiang Mai</option>
          </select>
        </div>
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1.5">ZIP</label>
          <input type="text" class="w-full px-3 py-2 border rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
        </div>
      </div>
      <!-- Actions: stack on mobile, row on md -->
      <div class="flex flex-col-reverse sm:flex-row sm:justify-end gap-3 pt-2">
        <button type="button" class="py-2.5 px-5 border text-gray-700 text-sm font-semibold rounded-xl hover:bg-gray-50 transition-colors">Cancel</button>
        <button type="submit" class="py-2.5 px-5 bg-indigo-600 text-white text-sm font-semibold rounded-xl hover:bg-indigo-700 transition-colors">Save Changes</button>
      </div>
    </form>
  </div>
</body>
</html>
```

---

## Step 809: Responsive Images & Media

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Responsive Media</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-white min-h-screen">
  <div class="max-w-4xl mx-auto px-4 py-8 space-y-8">
    <!-- aspect-ratio + object-fit -->
    <div>
      <p class="text-xs font-semibold text-gray-400 uppercase tracking-wide mb-3">Aspect Ratio</p>
      <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
        <div class="aspect-video bg-indigo-100 rounded-2xl flex items-center justify-center text-sm text-indigo-500 font-medium">16:9 (aspect-video)</div>
        <div class="aspect-square bg-purple-100 rounded-2xl flex items-center justify-center text-sm text-purple-500 font-medium">1:1 (aspect-square)</div>
        <div class="aspect-[4/3] bg-rose-100 rounded-2xl flex items-center justify-center text-sm text-rose-500 font-medium">4:3</div>
      </div>
    </div>

    <!-- Hero image breakout (full bleed on mobile, rounded on md) -->
    <div>
      <p class="text-xs font-semibold text-gray-400 uppercase tracking-wide mb-3">Full-bleed → Rounded</p>
      <div class="-mx-4 md:mx-0">
        <div class="aspect-video md:rounded-2xl bg-gradient-to-br from-indigo-500 to-purple-600 flex items-center justify-center text-white text-2xl font-black">
          Hero Image
        </div>
      </div>
    </div>

    <!-- Video embed responsive -->
    <div>
      <p class="text-xs font-semibold text-gray-400 uppercase tracking-wide mb-3">Responsive Video Embed</p>
      <div class="aspect-video rounded-2xl overflow-hidden bg-gray-900 flex items-center justify-center text-gray-400 text-sm">
        iframe / video would go here
      </div>
    </div>
  </div>
</body>
</html>
```

---

## Step 810: Responsive Workshop

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Responsive Workshop</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 min-h-screen">
  <!-- Responsive nav -->
  <nav class="bg-white border-b sticky top-0 z-10">
    <div class="max-w-6xl mx-auto px-4 h-14 flex items-center justify-between">
      <span class="font-extrabold text-indigo-600">Brand</span>
      <div class="hidden sm:flex gap-5 text-sm font-medium text-gray-600">
        <a href="#" class="hover:text-indigo-600">Home</a>
        <a href="#" class="hover:text-indigo-600">Products</a>
        <a href="#" class="hover:text-indigo-600">About</a>
      </div>
      <button class="px-3 py-1.5 bg-indigo-600 text-white text-sm rounded-xl font-semibold">Sign up</button>
    </div>
  </nav>

  <!-- Hero -->
  <header class="max-w-6xl mx-auto px-4 py-12 md:py-20 text-center md:text-left">
    <div class="md:flex md:items-center md:gap-12">
      <div class="flex-1">
        <h1 class="text-3xl md:text-4xl lg:text-5xl font-extrabold text-gray-900 leading-tight mb-4">
          Build <span class="text-indigo-600">Responsive</span><br>
          Layouts Fast
        </h1>
        <p class="text-gray-500 text-base md:text-lg mb-6 max-w-lg mx-auto md:mx-0">
          Tailwind CSS mobile-first approach ทำให้ responsive design ง่ายกว่าเดิม
        </p>
        <div class="flex flex-col sm:flex-row gap-3 justify-center md:justify-start">
          <button class="px-6 py-3 bg-indigo-600 text-white rounded-xl font-semibold hover:bg-indigo-700 transition-colors">Get started</button>
          <button class="px-6 py-3 border text-gray-700 rounded-xl font-semibold hover:bg-gray-50 transition-colors">Learn more</button>
        </div>
      </div>
      <div class="hidden md:block flex-1">
        <div class="aspect-video bg-gradient-to-br from-indigo-100 to-purple-100 rounded-2xl flex items-center justify-center text-indigo-400 text-5xl">🎨</div>
      </div>
    </div>
  </header>

  <!-- Feature grid -->
  <section class="max-w-6xl mx-auto px-4 pb-16">
    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-5">
      <div class="bg-white rounded-2xl border p-5 hover:shadow-md transition-shadow">
        <div class="text-2xl mb-3">📱</div>
        <h3 class="font-bold text-gray-900 mb-1">Mobile-First</h3>
        <p class="text-gray-500 text-sm">ออกแบบสำหรับ mobile ก่อน แล้วขยายไป desktop</p>
      </div>
      <div class="bg-white rounded-2xl border p-5 hover:shadow-md transition-shadow">
        <div class="text-2xl mb-3">🎯</div>
        <h3 class="font-bold text-gray-900 mb-1">Breakpoints</h3>
        <p class="text-gray-500 text-sm">sm, md, lg, xl, 2xl — prefix กำหนด style ต่อ breakpoint</p>
      </div>
      <div class="bg-white rounded-2xl border p-5 hover:shadow-md transition-shadow">
        <div class="text-2xl mb-3">📦</div>
        <h3 class="font-bold text-gray-900 mb-1">Container Queries</h3>
        <p class="text-gray-500 text-sm">Component ปรับตาม container ไม่ใช่แค่ viewport</p>
      </div>
    </div>
  </section>
</body>
</html>
```

---

## สรุป Part 81

| Step | เนื้อหา |
|------|---------|
| 801 | Breakpoint system + mobile-first syntax |
| 802 | Responsive navigation + hamburger menu |
| 803 | Responsive sidebar layout + mobile drawer |
| 804 | Responsive card grid |
| 805 | Table → cards ตาม breakpoint |
| 806 | Container queries |
| 807 | Responsive typography + fluid clamp() |
| 808 | Responsive form layout |
| 809 | Responsive images + aspect-ratio |
| 810 | Workshop: Complete responsive page |

**Part ถัดไป:** Part 82 — Advanced Tailwind Plugins (Steps 811–820)
