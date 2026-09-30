# Part 47: Responsive Design Mastery

## เป้าหมาย
- เข้าใจ breakpoints ทุก level อย่างลึกซึ้ง
- Mobile-first approach
- Complex responsive layouts: grid, sidebar, navigation
- Container queries pattern
- Steps 461–470

---

## Step 461: Breakpoint System Deep Dive

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Breakpoints - Step 461</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-4">
  <div class="max-w-5xl mx-auto space-y-6">
    <h2 class="text-xl font-extrabold">Tailwind Breakpoints</h2>

    <!-- Breakpoint indicator -->
    <div class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-3">Current Breakpoint</h3>
      <div class="flex gap-2 flex-wrap">
        <span class="bg-red-100 text-red-700 text-xs font-bold px-3 py-1.5 rounded-full sm:hidden">xs / default (< 640px)</span>
        <span class="hidden sm:block md:hidden bg-orange-100 text-orange-700 text-xs font-bold px-3 py-1.5 rounded-full">sm (640px+)</span>
        <span class="hidden md:block lg:hidden bg-yellow-100 text-yellow-700 text-xs font-bold px-3 py-1.5 rounded-full">md (768px+)</span>
        <span class="hidden lg:block xl:hidden bg-green-100 text-green-700 text-xs font-bold px-3 py-1.5 rounded-full">lg (1024px+)</span>
        <span class="hidden xl:block 2xl:hidden bg-blue-100 text-blue-700 text-xs font-bold px-3 py-1.5 rounded-full">xl (1280px+)</span>
        <span class="hidden 2xl:block bg-purple-100 text-purple-700 text-xs font-bold px-3 py-1.5 rounded-full">2xl (1536px+)</span>
      </div>
    </div>

    <!-- Breakpoint table -->
    <div class="bg-white rounded-2xl border overflow-hidden">
      <table class="w-full text-sm">
        <thead class="bg-gray-50 text-left">
          <tr>
            <th class="px-4 py-3 font-semibold text-xs text-gray-500">Prefix</th>
            <th class="px-4 py-3 font-semibold text-xs text-gray-500">Min-width</th>
            <th class="px-4 py-3 font-semibold text-xs text-gray-500">Device</th>
            <th class="px-4 py-3 font-semibold text-xs text-gray-500">Example</th>
          </tr>
        </thead>
        <tbody class="divide-y text-sm">
          <tr><td class="px-4 py-3"><code class="bg-gray-100 px-1.5 py-0.5 rounded text-xs">none</code></td><td class="px-4 py-3 text-gray-600">0px</td><td class="px-4 py-3 text-gray-600">มือถือ</td><td class="px-4 py-3 text-gray-400">flex-col</td></tr>
          <tr class="bg-gray-50/50"><td class="px-4 py-3"><code class="bg-orange-100 text-orange-700 px-1.5 py-0.5 rounded text-xs">sm:</code></td><td class="px-4 py-3 text-gray-600">640px</td><td class="px-4 py-3 text-gray-600">มือถือใหญ่</td><td class="px-4 py-3 text-gray-400">sm:flex-row</td></tr>
          <tr><td class="px-4 py-3"><code class="bg-yellow-100 text-yellow-700 px-1.5 py-0.5 rounded text-xs">md:</code></td><td class="px-4 py-3 text-gray-600">768px</td><td class="px-4 py-3 text-gray-600">Tablet</td><td class="px-4 py-3 text-gray-400">md:grid-cols-2</td></tr>
          <tr class="bg-gray-50/50"><td class="px-4 py-3"><code class="bg-green-100 text-green-700 px-1.5 py-0.5 rounded text-xs">lg:</code></td><td class="px-4 py-3 text-gray-600">1024px</td><td class="px-4 py-3 text-gray-600">Laptop</td><td class="px-4 py-3 text-gray-400">lg:grid-cols-3</td></tr>
          <tr><td class="px-4 py-3"><code class="bg-blue-100 text-blue-700 px-1.5 py-0.5 rounded text-xs">xl:</code></td><td class="px-4 py-3 text-gray-600">1280px</td><td class="px-4 py-3 text-gray-600">Desktop</td><td class="px-4 py-3 text-gray-400">xl:grid-cols-4</td></tr>
          <tr class="bg-gray-50/50"><td class="px-4 py-3"><code class="bg-purple-100 text-purple-700 px-1.5 py-0.5 rounded text-xs">2xl:</code></td><td class="px-4 py-3 text-gray-600">1536px</td><td class="px-4 py-3 text-gray-600">Wide screen</td><td class="px-4 py-3 text-gray-400">2xl:max-w-8xl</td></tr>
        </tbody>
      </table>
    </div>

    <!-- Responsive grid demo -->
    <div class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-3">Responsive Grid</h3>
      <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-3">
        <div class="bg-indigo-100 rounded-xl p-4 text-center text-sm font-medium text-indigo-700">Item 1</div>
        <div class="bg-indigo-100 rounded-xl p-4 text-center text-sm font-medium text-indigo-700">Item 2</div>
        <div class="bg-indigo-100 rounded-xl p-4 text-center text-sm font-medium text-indigo-700">Item 3</div>
        <div class="bg-indigo-100 rounded-xl p-4 text-center text-sm font-medium text-indigo-700">Item 4</div>
      </div>
      <p class="text-xs text-gray-400 mt-3">
        1 col → sm:2 → md:3 → lg:4
      </p>
    </div>

  </div>
</body>
</html>
```

---

## Steps 462–465: Responsive Navigation Patterns

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Responsive Nav - Steps 462-465</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50">

  <!-- Mobile hamburger nav (Step 462) -->
  <nav class="bg-white border-b px-4 sm:px-6">
    <div class="max-w-5xl mx-auto flex items-center justify-between h-14">
      <div class="flex items-center gap-2">
        <div class="w-7 h-7 bg-indigo-600 rounded-lg"></div>
        <span class="font-extrabold">TechBrand</span>
      </div>

      <!-- Desktop nav -->
      <div class="hidden md:flex items-center gap-6 text-sm text-gray-600">
        <a href="#" class="hover:text-indigo-600 transition-colors">หน้าแรก</a>
        <a href="#" class="hover:text-indigo-600 transition-colors">บริการ</a>
        <a href="#" class="hover:text-indigo-600 transition-colors">ราคา</a>
        <a href="#" class="hover:text-indigo-600 transition-colors">บทความ</a>
        <a href="#" class="hover:text-indigo-600 transition-colors">ติดต่อ</a>
      </div>

      <div class="flex items-center gap-2">
        <!-- Desktop CTA -->
        <button class="hidden md:block bg-indigo-600 text-white px-4 py-1.5 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">เริ่มใช้งาน</button>
        <!-- Mobile hamburger -->
        <button class="md:hidden w-9 h-9 flex flex-col items-center justify-center gap-1.5" onclick="toggleMobileNav()">
          <span class="w-5 h-0.5 bg-gray-700 rounded-full transition-transform" id="ham1"></span>
          <span class="w-5 h-0.5 bg-gray-700 rounded-full transition-opacity" id="ham2"></span>
          <span class="w-5 h-0.5 bg-gray-700 rounded-full transition-transform" id="ham3"></span>
        </button>
      </div>
    </div>

    <!-- Mobile menu -->
    <div id="mobile-nav" class="hidden md:hidden border-t pb-4 pt-2 space-y-1">
      <a href="#" class="block px-4 py-2.5 text-sm text-gray-700 hover:bg-gray-50 rounded-xl">หน้าแรก</a>
      <a href="#" class="block px-4 py-2.5 text-sm text-gray-700 hover:bg-gray-50 rounded-xl">บริการ</a>
      <a href="#" class="block px-4 py-2.5 text-sm text-gray-700 hover:bg-gray-50 rounded-xl">ราคา</a>
      <a href="#" class="block px-4 py-2.5 text-sm text-gray-700 hover:bg-gray-50 rounded-xl">บทความ</a>
      <a href="#" class="block px-4 py-2.5 text-sm text-gray-700 hover:bg-gray-50 rounded-xl">ติดต่อ</a>
      <div class="pt-2 px-4">
        <button class="w-full bg-indigo-600 text-white py-2.5 rounded-xl text-sm font-semibold">เริ่มใช้งาน</button>
      </div>
    </div>
  </nav>

  <main class="max-w-5xl mx-auto p-4 space-y-8 mt-4">

    <!-- Responsive sidebar layout (Step 463) -->
    <section>
      <h3 class="font-bold mb-3">Responsive Sidebar Layout</h3>
      <div class="flex flex-col lg:flex-row gap-6 bg-white rounded-2xl border overflow-hidden">
        <!-- Sidebar: hidden on mobile, fixed on desktop -->
        <aside class="lg:w-48 lg:flex-shrink-0 bg-gray-50 border-b lg:border-b-0 lg:border-r p-3">
          <nav class="flex lg:flex-col gap-1 overflow-x-auto lg:overflow-visible pb-1 lg:pb-0">
            <a href="#" class="flex-shrink-0 lg:flex-shrink px-3 py-2 text-xs font-medium rounded-xl bg-indigo-50 text-indigo-700">📊 ภาพรวม</a>
            <a href="#" class="flex-shrink-0 lg:flex-shrink px-3 py-2 text-xs text-gray-600 hover:bg-gray-100 rounded-xl transition-colors">👤 โปรไฟล์</a>
            <a href="#" class="flex-shrink-0 lg:flex-shrink px-3 py-2 text-xs text-gray-600 hover:bg-gray-100 rounded-xl transition-colors">⚙️ ตั้งค่า</a>
            <a href="#" class="flex-shrink-0 lg:flex-shrink px-3 py-2 text-xs text-gray-600 hover:bg-gray-100 rounded-xl transition-colors">💳 การชำระเงิน</a>
          </nav>
        </aside>
        <div class="flex-1 p-5">
          <h4 class="font-bold mb-2">เนื้อหาหลัก</h4>
          <p class="text-sm text-gray-600">Sidebar อยู่บนสุดบนมือถือ (horizontal scroll) และอยู่ด้านซ้ายบน desktop</p>
        </div>
      </div>
    </section>

    <!-- Responsive cards with different layouts (Step 464) -->
    <section>
      <h3 class="font-bold mb-3">Responsive Card Grid</h3>
      <!-- Feature grid: 1 col mobile, 2 tablet, 3 desktop -->
      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
        <div class="bg-white border rounded-2xl p-5 flex sm:flex-col gap-4">
          <div class="w-12 h-12 bg-indigo-100 rounded-xl flex items-center justify-center text-xl flex-shrink-0">🎨</div>
          <div>
            <h4 class="font-bold text-sm">ออกแบบสวย</h4>
            <p class="text-xs text-gray-500 mt-1">UI components พร้อมใช้งาน</p>
          </div>
        </div>
        <div class="bg-white border rounded-2xl p-5 flex sm:flex-col gap-4">
          <div class="w-12 h-12 bg-green-100 rounded-xl flex items-center justify-center text-xl flex-shrink-0">⚡</div>
          <div>
            <h4 class="font-bold text-sm">เร็วมาก</h4>
            <p class="text-xs text-gray-500 mt-1">Optimized performance</p>
          </div>
        </div>
        <div class="bg-white border rounded-2xl p-5 flex sm:flex-col gap-4 sm:col-span-2 lg:col-span-1">
          <div class="w-12 h-12 bg-purple-100 rounded-xl flex items-center justify-center text-xl flex-shrink-0">📱</div>
          <div>
            <h4 class="font-bold text-sm">Responsive</h4>
            <p class="text-xs text-gray-500 mt-1">ทุกขนาดหน้าจอ</p>
          </div>
        </div>
      </div>
    </section>

    <!-- Responsive table (Step 465) -->
    <section>
      <h3 class="font-bold mb-3">Responsive Table</h3>
      <!-- Desktop table / Mobile cards -->
      <div class="hidden md:block bg-white rounded-2xl border overflow-hidden">
        <table class="w-full text-sm">
          <thead class="bg-gray-50 text-left">
            <tr>
              <th class="px-4 py-3 font-semibold text-xs text-gray-500">ชื่อ</th>
              <th class="px-4 py-3 font-semibold text-xs text-gray-500">อีเมล</th>
              <th class="px-4 py-3 font-semibold text-xs text-gray-500">สถานะ</th>
              <th class="px-4 py-3 font-semibold text-xs text-gray-500">ยอดซื้อ</th>
            </tr>
          </thead>
          <tbody class="divide-y">
            <tr><td class="px-4 py-3">สมชาย ใจดี</td><td class="px-4 py-3 text-gray-500">somchai@example.com</td><td class="px-4 py-3"><span class="bg-green-100 text-green-700 text-xs px-2 py-0.5 rounded-full">Active</span></td><td class="px-4 py-3 font-semibold">฿12,400</td></tr>
            <tr class="bg-gray-50/50"><td class="px-4 py-3">วิชัย เก่งมาก</td><td class="px-4 py-3 text-gray-500">wichai@example.com</td><td class="px-4 py-3"><span class="bg-yellow-100 text-yellow-700 text-xs px-2 py-0.5 rounded-full">Trial</span></td><td class="px-4 py-3 font-semibold">฿0</td></tr>
          </tbody>
        </table>
      </div>
      <!-- Mobile cards version -->
      <div class="md:hidden space-y-3">
        <div class="bg-white border rounded-2xl p-4">
          <div class="flex items-center justify-between mb-2">
            <p class="font-bold text-sm">สมชาย ใจดี</p>
            <span class="bg-green-100 text-green-700 text-xs px-2 py-0.5 rounded-full">Active</span>
          </div>
          <p class="text-xs text-gray-500">somchai@example.com</p>
          <p class="text-sm font-bold text-indigo-600 mt-2">฿12,400</p>
        </div>
        <div class="bg-white border rounded-2xl p-4">
          <div class="flex items-center justify-between mb-2">
            <p class="font-bold text-sm">วิชัย เก่งมาก</p>
            <span class="bg-yellow-100 text-yellow-700 text-xs px-2 py-0.5 rounded-full">Trial</span>
          </div>
          <p class="text-xs text-gray-500">wichai@example.com</p>
          <p class="text-sm font-bold text-indigo-600 mt-2">฿0</p>
        </div>
      </div>
    </section>

  </main>

  <script>
    let navOpen = false;
    function toggleMobileNav() {
      navOpen = !navOpen;
      document.getElementById('mobile-nav').classList.toggle('hidden', !navOpen);
      document.getElementById('ham1').style.transform = navOpen ? 'translateY(8px) rotate(45deg)' : '';
      document.getElementById('ham2').style.opacity = navOpen ? '0' : '1';
      document.getElementById('ham3').style.transform = navOpen ? 'translateY(-8px) rotate(-45deg)' : '';
    }
  </script>

</body>
</html>
```

---

## Steps 466–470: Advanced Responsive Patterns Workshop

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Responsive Workshop - Steps 466-470</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50">

  <!-- Fixed mobile bottom nav (Step 466) -->
  <nav class="md:hidden fixed bottom-0 inset-x-0 bg-white border-t z-20 flex">
    <button class="flex-1 flex flex-col items-center justify-center py-2 text-indigo-600">
      <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20"><path d="M10.707 2.293a1 1 0 00-1.414 0l-7 7a1 1 0 001.414 1.414L4 10.414V17a1 1 0 001 1h2a1 1 0 001-1v-2a1 1 0 011-1h2a1 1 0 011 1v2a1 1 0 001 1h2a1 1 0 001-1v-6.586l.293.293a1 1 0 001.414-1.414l-7-7z"/></svg>
      <span class="text-[10px] mt-0.5">หน้าแรก</span>
    </button>
    <button class="flex-1 flex flex-col items-center justify-center py-2 text-gray-400">
      <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"/></svg>
      <span class="text-[10px] mt-0.5">ค้นหา</span>
    </button>
    <button class="flex-1 flex flex-col items-center justify-center py-2 text-gray-400 relative">
      <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5.4 5M7 13l-2.293 2.293c-.63.63-.184 1.707.707 1.707H17m0 0a2 2 0 100 4 2 2 0 000-4zm-8 2a2 2 0 11-4 0 2 2 0 014 0z"/></svg>
      <span class="absolute top-1.5 right-4 w-4 h-4 bg-red-500 text-white text-[8px] font-bold rounded-full flex items-center justify-center">3</span>
      <span class="text-[10px] mt-0.5">ตะกร้า</span>
    </button>
    <button class="flex-1 flex flex-col items-center justify-center py-2 text-gray-400">
      <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"/></svg>
      <span class="text-[10px] mt-0.5">โปรไฟล์</span>
    </button>
  </nav>

  <!-- Main content with bottom nav safe area -->
  <div class="max-w-5xl mx-auto p-4 pb-24 md:pb-4 space-y-8">

    <!-- Responsive hero (Step 467) -->
    <section class="bg-gradient-to-br from-indigo-600 to-purple-700 rounded-2xl p-6 md:p-10 text-white">
      <div class="flex flex-col md:flex-row items-center gap-6">
        <div class="flex-1 text-center md:text-left">
          <h1 class="text-2xl md:text-4xl font-extrabold leading-tight">Responsive Design<br class="hidden md:block"> ทำได้ไม่ยาก</h1>
          <p class="text-white/80 mt-3 text-sm md:text-base">สร้าง UI ที่ดูดีทุกขนาดหน้าจอด้วย Tailwind CSS</p>
          <div class="flex gap-3 mt-5 justify-center md:justify-start">
            <button class="bg-white text-indigo-700 px-5 py-2.5 rounded-xl text-sm font-bold hover:bg-white/90 transition-colors">เริ่มเลย</button>
            <button class="border border-white/40 text-white px-5 py-2.5 rounded-xl text-sm font-semibold hover:bg-white/10 transition-colors">ดูตัวอย่าง</button>
          </div>
        </div>
        <div class="w-full md:w-auto flex-shrink-0">
          <div class="grid grid-cols-2 gap-2 max-w-xs mx-auto md:max-w-none">
            <div class="bg-white/10 rounded-xl p-4 text-center">
              <p class="text-2xl font-extrabold">48</p>
              <p class="text-xs text-white/70">Parts</p>
            </div>
            <div class="bg-white/10 rounded-xl p-4 text-center">
              <p class="text-2xl font-extrabold">470+</p>
              <p class="text-xs text-white/70">Steps</p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Responsive image + text pattern (Step 468) -->
    <section class="bg-white rounded-2xl border overflow-hidden">
      <!-- Normal: image top, text bottom -->
      <!-- lg+: image left, text right -->
      <div class="flex flex-col lg:flex-row">
        <div class="lg:w-2/5 h-48 lg:h-auto bg-gradient-to-br from-indigo-400 to-purple-500 flex items-center justify-center text-white">
          <div class="text-center">
            <div class="text-5xl mb-2">📱</div>
            <p class="text-sm font-semibold">Image Area</p>
          </div>
        </div>
        <div class="flex-1 p-6 lg:p-8">
          <span class="text-xs font-bold text-indigo-600 uppercase tracking-wide">Feature Highlight</span>
          <h3 class="text-lg font-extrabold mt-1">Mobile-First Design</h3>
          <p class="text-gray-600 text-sm mt-2 leading-relaxed">เริ่มออกแบบจากมือถือก่อนแล้วขยายขึ้นไป desktop เพื่อให้ทุก breakpoint ดูดีที่สุด</p>
          <button class="mt-4 bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">อ่านเพิ่มเติม</button>
        </div>
      </div>
    </section>

    <!-- Masonry-style responsive layout (Step 469) -->
    <section>
      <h3 class="font-bold mb-3">Masonry Grid</h3>
      <div class="columns-1 sm:columns-2 lg:columns-3 gap-4">
        <div class="break-inside-avoid mb-4 bg-white border rounded-2xl p-4">
          <div class="h-24 bg-indigo-100 rounded-xl mb-3"></div>
          <p class="font-bold text-sm">Card สั้น</p>
        </div>
        <div class="break-inside-avoid mb-4 bg-white border rounded-2xl p-4">
          <div class="h-40 bg-green-100 rounded-xl mb-3"></div>
          <p class="font-bold text-sm">Card ปานกลาง</p>
          <p class="text-xs text-gray-500 mt-1">มีเนื้อหาเพิ่มเติมนิดหน่อย</p>
        </div>
        <div class="break-inside-avoid mb-4 bg-white border rounded-2xl p-4">
          <div class="h-32 bg-purple-100 rounded-xl mb-3"></div>
          <p class="font-bold text-sm">Card กลาง</p>
        </div>
        <div class="break-inside-avoid mb-4 bg-white border rounded-2xl p-4">
          <div class="h-56 bg-yellow-100 rounded-xl mb-3"></div>
          <p class="font-bold text-sm">Card สูง</p>
          <p class="text-xs text-gray-500 mt-1">มีรูปใหญ่</p>
        </div>
        <div class="break-inside-avoid mb-4 bg-white border rounded-2xl p-4">
          <div class="h-20 bg-red-100 rounded-xl mb-3"></div>
          <p class="font-bold text-sm">Card เล็ก</p>
        </div>
        <div class="break-inside-avoid mb-4 bg-white border rounded-2xl p-4">
          <div class="h-36 bg-blue-100 rounded-xl mb-3"></div>
          <p class="font-bold text-sm">Card สูงปานกลาง</p>
        </div>
      </div>
    </section>

    <!-- Full responsive app (Step 470 Workshop) -->
    <section class="bg-white rounded-2xl border overflow-hidden">
      <div class="p-4 border-b bg-gray-50">
        <h3 class="font-bold">Workshop: Complete Responsive App Shell</h3>
      </div>

      <div class="flex h-96">
        <!-- Desktop sidebar -->
        <aside class="hidden lg:flex w-44 flex-col bg-gray-900 text-white p-3 flex-shrink-0">
          <p class="text-[10px] font-bold text-gray-400 uppercase px-2 mb-2">Menu</p>
          <nav class="space-y-0.5">
            <a href="#" class="flex items-center gap-2 px-2 py-1.5 rounded-lg bg-indigo-600 text-xs font-medium">🏠 หน้าแรก</a>
            <a href="#" class="flex items-center gap-2 px-2 py-1.5 rounded-lg hover:bg-gray-700 text-xs text-gray-300 transition-colors">📊 Analytics</a>
            <a href="#" class="flex items-center gap-2 px-2 py-1.5 rounded-lg hover:bg-gray-700 text-xs text-gray-300 transition-colors">👤 Users</a>
            <a href="#" class="flex items-center gap-2 px-2 py-1.5 rounded-lg hover:bg-gray-700 text-xs text-gray-300 transition-colors">⚙️ Settings</a>
          </nav>
        </aside>

        <!-- Content -->
        <div class="flex-1 flex flex-col overflow-hidden">
          <!-- Top bar -->
          <div class="bg-white border-b px-4 h-11 flex items-center justify-between flex-shrink-0">
            <div class="flex items-center gap-2">
              <button class="lg:hidden w-7 h-7 flex items-center justify-center text-gray-500">☰</button>
              <span class="text-sm font-semibold">Dashboard</span>
            </div>
            <div class="flex items-center gap-2">
              <div class="w-7 h-7 bg-indigo-100 rounded-full flex items-center justify-center text-xs">👤</div>
            </div>
          </div>

          <!-- Scrollable content -->
          <div class="flex-1 overflow-y-auto p-4 bg-gray-50">
            <div class="grid grid-cols-2 lg:grid-cols-4 gap-3">
              <div class="bg-white rounded-xl border p-3 text-center">
                <p class="text-xl font-extrabold text-indigo-600">1.2K</p>
                <p class="text-xs text-gray-500">Users</p>
              </div>
              <div class="bg-white rounded-xl border p-3 text-center">
                <p class="text-xl font-extrabold text-green-600">฿84K</p>
                <p class="text-xs text-gray-500">Revenue</p>
              </div>
              <div class="bg-white rounded-xl border p-3 text-center hidden lg:block">
                <p class="text-xl font-extrabold text-purple-600">98%</p>
                <p class="text-xs text-gray-500">Uptime</p>
              </div>
              <div class="bg-white rounded-xl border p-3 text-center hidden lg:block">
                <p class="text-xl font-extrabold text-orange-600">4.8</p>
                <p class="text-xs text-gray-500">Rating</p>
              </div>
            </div>

            <div class="mt-4 grid grid-cols-1 lg:grid-cols-3 gap-3">
              <div class="lg:col-span-2 bg-white rounded-xl border p-4">
                <p class="font-bold text-sm mb-3">Activity Chart</p>
                <div class="h-24 bg-gradient-to-r from-indigo-50 to-purple-50 rounded-lg flex items-end gap-1 p-2">
                  <div class="flex-1 bg-indigo-300 rounded-sm" style="height:60%"></div>
                  <div class="flex-1 bg-indigo-400 rounded-sm" style="height:80%"></div>
                  <div class="flex-1 bg-indigo-500 rounded-sm" style="height:50%"></div>
                  <div class="flex-1 bg-indigo-600 rounded-sm" style="height:90%"></div>
                  <div class="flex-1 bg-indigo-500 rounded-sm" style="height:70%"></div>
                  <div class="flex-1 bg-indigo-400 rounded-sm" style="height:40%"></div>
                  <div class="flex-1 bg-indigo-600 rounded-sm" style="height:100%"></div>
                </div>
              </div>
              <div class="bg-white rounded-xl border p-4">
                <p class="font-bold text-sm mb-3">Top Users</p>
                <div class="space-y-2">
                  <div class="flex items-center gap-2 text-xs">
                    <div class="w-6 h-6 bg-indigo-100 rounded-full flex items-center justify-center">A</div>
                    <span class="flex-1">Alice</span>
                    <span class="text-gray-400">920</span>
                  </div>
                  <div class="flex items-center gap-2 text-xs">
                    <div class="w-6 h-6 bg-green-100 rounded-full flex items-center justify-center">B</div>
                    <span class="flex-1">Bob</span>
                    <span class="text-gray-400">840</span>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

  </div>
</body>
</html>
```

---

## สรุป Part 47

| Step | เนื้อหา |
|------|---------|
| 461 | Breakpoint system: sm/md/lg/xl/2xl, breakpoint indicator, responsive grid |
| 462 | Hamburger nav with animated bars, mobile menu toggle |
| 463 | Responsive sidebar: horizontal scroll mobile → fixed sidebar desktop |
| 464 | Responsive cards: stack mobile → grid desktop, col-span tricks |
| 465 | Responsive table → mobile card fallback |
| 466 | Mobile bottom tab navigation (fixed) |
| 467 | Responsive hero: column mobile → row desktop |
| 468 | Responsive image + text: top/bottom → left/right |
| 469 | Masonry grid with columns-1/2/3 |
| 470 | Workshop: Complete responsive app shell with sidebar |

**Part ถัดไป:** Part 48 — Dark Mode Implementation (Steps 471–480)
