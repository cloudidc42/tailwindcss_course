# Part 45: Design System Components

## เป้าหมาย
- สร้าง Component Library / Design System ด้วย Tailwind
- Tokens, Buttons, Forms, Cards, Badges, Alerts, Modals
- Storybook-style showcase
- Steps 441–450

---

## Step 441: Design Tokens & Color Palette

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Design Tokens - Step 441</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-8">

  <div class="max-w-4xl mx-auto space-y-8">
    <h1 class="text-2xl font-extrabold">Design System · TechShop</h1>

    <!-- Color Palette -->
    <section>
      <h2 class="text-lg font-bold mb-4">Color Palette</h2>
      <div class="space-y-4">

        <!-- Primary -->
        <div>
          <p class="text-sm font-semibold text-gray-600 mb-2">Primary (Indigo)</p>
          <div class="grid grid-cols-10 gap-1">
            <div><div class="h-10 rounded-lg bg-indigo-50"></div><p class="text-[10px] text-center mt-1 text-gray-400">50</p></div>
            <div><div class="h-10 rounded-lg bg-indigo-100"></div><p class="text-[10px] text-center mt-1 text-gray-400">100</p></div>
            <div><div class="h-10 rounded-lg bg-indigo-200"></div><p class="text-[10px] text-center mt-1 text-gray-400">200</p></div>
            <div><div class="h-10 rounded-lg bg-indigo-300"></div><p class="text-[10px] text-center mt-1 text-gray-400">300</p></div>
            <div><div class="h-10 rounded-lg bg-indigo-400"></div><p class="text-[10px] text-center mt-1 text-gray-400">400</p></div>
            <div><div class="h-10 rounded-lg bg-indigo-500"></div><p class="text-[10px] text-center mt-1 text-gray-400">500</p></div>
            <div><div class="h-10 rounded-lg bg-indigo-600 ring-2 ring-offset-1 ring-indigo-600"></div><p class="text-[10px] text-center mt-1 font-bold text-indigo-600">600✓</p></div>
            <div><div class="h-10 rounded-lg bg-indigo-700"></div><p class="text-[10px] text-center mt-1 text-gray-400">700</p></div>
            <div><div class="h-10 rounded-lg bg-indigo-800"></div><p class="text-[10px] text-center mt-1 text-gray-400">800</p></div>
            <div><div class="h-10 rounded-lg bg-indigo-900"></div><p class="text-[10px] text-center mt-1 text-gray-400">900</p></div>
          </div>
        </div>

        <!-- Semantic Colors -->
        <div>
          <p class="text-sm font-semibold text-gray-600 mb-2">Semantic Colors</p>
          <div class="grid grid-cols-2 md:grid-cols-4 gap-3">
            <div class="p-4 bg-green-500 rounded-xl text-white text-center text-sm font-semibold">Success #22c55e</div>
            <div class="p-4 bg-red-500 rounded-xl text-white text-center text-sm font-semibold">Danger #ef4444</div>
            <div class="p-4 bg-yellow-500 rounded-xl text-white text-center text-sm font-semibold">Warning #eab308</div>
            <div class="p-4 bg-blue-500 rounded-xl text-white text-center text-sm font-semibold">Info #3b82f6</div>
          </div>
        </div>

        <!-- Neutrals -->
        <div>
          <p class="text-sm font-semibold text-gray-600 mb-2">Neutrals</p>
          <div class="grid grid-cols-10 gap-1">
            <div><div class="h-10 rounded-lg bg-gray-50 border"></div><p class="text-[10px] text-center mt-1 text-gray-400">50</p></div>
            <div><div class="h-10 rounded-lg bg-gray-100"></div><p class="text-[10px] text-center mt-1 text-gray-400">100</p></div>
            <div><div class="h-10 rounded-lg bg-gray-200"></div><p class="text-[10px] text-center mt-1 text-gray-400">200</p></div>
            <div><div class="h-10 rounded-lg bg-gray-300"></div><p class="text-[10px] text-center mt-1 text-gray-400">300</p></div>
            <div><div class="h-10 rounded-lg bg-gray-400"></div><p class="text-[10px] text-center mt-1 text-gray-400">400</p></div>
            <div><div class="h-10 rounded-lg bg-gray-500"></div><p class="text-[10px] text-center mt-1 text-gray-400">500</p></div>
            <div><div class="h-10 rounded-lg bg-gray-600"></div><p class="text-[10px] text-center mt-1 text-gray-400">600</p></div>
            <div><div class="h-10 rounded-lg bg-gray-700"></div><p class="text-[10px] text-center mt-1 text-gray-400">700</p></div>
            <div><div class="h-10 rounded-lg bg-gray-800"></div><p class="text-[10px] text-center mt-1 text-gray-400">800</p></div>
            <div><div class="h-10 rounded-lg bg-gray-900"></div><p class="text-[10px] text-center mt-1 text-gray-400">900</p></div>
          </div>
        </div>
      </div>
    </section>

    <!-- Typography -->
    <section>
      <h2 class="text-lg font-bold mb-4">Typography Scale</h2>
      <div class="bg-white border rounded-2xl p-6 space-y-3">
        <div class="flex items-baseline gap-4"><code class="text-xs text-gray-400 w-20 flex-shrink-0">text-xs</code><p class="text-xs">ขนาด 12px · สำหรับ caption และ label เล็ก</p></div>
        <div class="flex items-baseline gap-4"><code class="text-xs text-gray-400 w-20 flex-shrink-0">text-sm</code><p class="text-sm">ขนาด 14px · สำหรับ body text ทั่วไป</p></div>
        <div class="flex items-baseline gap-4"><code class="text-xs text-gray-400 w-20 flex-shrink-0">text-base</code><p class="text-base">ขนาด 16px · สำหรับ paragraph หลัก</p></div>
        <div class="flex items-baseline gap-4"><code class="text-xs text-gray-400 w-20 flex-shrink-0">text-lg</code><p class="text-lg font-semibold">ขนาด 18px · Section heading</p></div>
        <div class="flex items-baseline gap-4"><code class="text-xs text-gray-400 w-20 flex-shrink-0">text-xl</code><p class="text-xl font-bold">ขนาด 20px · Page title</p></div>
        <div class="flex items-baseline gap-4"><code class="text-xs text-gray-400 w-20 flex-shrink-0">text-2xl</code><p class="text-2xl font-extrabold">ขนาด 24px · Hero heading</p></div>
        <div class="flex items-baseline gap-4"><code class="text-xs text-gray-400 w-20 flex-shrink-0">text-4xl</code><p class="text-4xl font-extrabold">ขนาด 36px · Display</p></div>
      </div>
    </section>

    <!-- Spacing -->
    <section>
      <h2 class="text-lg font-bold mb-4">Spacing Scale</h2>
      <div class="bg-white border rounded-2xl p-6">
        <div class="flex items-end gap-2">
          <div class="bg-indigo-100 w-1 h-1" title="1 = 4px"></div>
          <div class="bg-indigo-200 w-2 h-2" title="2 = 8px"></div>
          <div class="bg-indigo-300 w-3 h-3" title="3 = 12px"></div>
          <div class="bg-indigo-400 w-4 h-4" title="4 = 16px"></div>
          <div class="bg-indigo-500 w-5 h-5" title="5 = 20px"></div>
          <div class="bg-indigo-600 w-6 h-6" title="6 = 24px"></div>
          <div class="bg-indigo-700 w-8 h-8" title="8 = 32px"></div>
          <div class="bg-indigo-800 w-10 h-10" title="10 = 40px"></div>
          <div class="bg-indigo-900 w-12 h-12" title="12 = 48px"></div>
        </div>
        <div class="flex items-end gap-2 mt-1 text-[10px] text-gray-400">
          <span class="w-1 text-center">4</span>
          <span class="w-2 text-center">8</span>
          <span class="w-3 text-center">12</span>
          <span class="w-4 text-center">16</span>
          <span class="w-5 text-center">20</span>
          <span class="w-6 text-center">24</span>
          <span class="w-8 text-center">32</span>
          <span class="w-10 text-center">40</span>
          <span class="w-12 text-center">48</span>
        </div>
      </div>
    </section>

  </div>
</body>
</html>
```

---

## Step 442: Button System

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Buttons - Step 442</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-8">
  <div class="max-w-4xl mx-auto space-y-8">
    <h2 class="text-xl font-extrabold">Button Components</h2>

    <!-- Variants -->
    <section class="bg-white rounded-2xl border p-6">
      <h3 class="text-sm font-bold text-gray-500 uppercase tracking-wide mb-4">Variants</h3>
      <div class="flex flex-wrap gap-3">
        <button class="bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-indigo-700 active:bg-indigo-800 transition-colors">Primary</button>
        <button class="bg-white border-2 border-indigo-600 text-indigo-600 px-4 py-2 rounded-xl text-sm font-semibold hover:bg-indigo-50 transition-colors">Outline</button>
        <button class="bg-gray-100 text-gray-700 px-4 py-2 rounded-xl text-sm font-semibold hover:bg-gray-200 transition-colors">Ghost</button>
        <button class="text-indigo-600 px-4 py-2 rounded-xl text-sm font-semibold hover:bg-indigo-50 transition-colors underline-offset-2 hover:underline">Link</button>
        <button class="bg-red-600 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-red-700 transition-colors">Danger</button>
        <button class="bg-green-600 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-green-700 transition-colors">Success</button>
      </div>
    </section>

    <!-- Sizes -->
    <section class="bg-white rounded-2xl border p-6">
      <h3 class="text-sm font-bold text-gray-500 uppercase tracking-wide mb-4">Sizes</h3>
      <div class="flex flex-wrap items-center gap-3">
        <button class="bg-indigo-600 text-white px-2.5 py-1 rounded-lg text-xs font-semibold hover:bg-indigo-700 transition-colors">XS</button>
        <button class="bg-indigo-600 text-white px-3 py-1.5 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">Small</button>
        <button class="bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">Medium</button>
        <button class="bg-indigo-600 text-white px-5 py-2.5 rounded-xl text-base font-semibold hover:bg-indigo-700 transition-colors">Large</button>
        <button class="bg-indigo-600 text-white px-6 py-3 rounded-xl text-lg font-bold hover:bg-indigo-700 transition-colors">XL</button>
      </div>
    </section>

    <!-- With Icons -->
    <section class="bg-white rounded-2xl border p-6">
      <h3 class="text-sm font-bold text-gray-500 uppercase tracking-wide mb-4">With Icons</h3>
      <div class="flex flex-wrap gap-3">
        <button class="bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors flex items-center gap-2">
          <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4"/></svg>
          เพิ่มใหม่
        </button>
        <button class="border border-gray-300 text-gray-700 px-4 py-2 rounded-xl text-sm font-semibold hover:bg-gray-50 transition-colors flex items-center gap-2">
          <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4"/></svg>
          ดาวน์โหลด
        </button>
        <button class="bg-red-600 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-red-700 transition-colors flex items-center gap-2">
          <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"/></svg>
          ลบ
        </button>
        <!-- Icon-only -->
        <button class="w-9 h-9 bg-indigo-600 text-white rounded-xl flex items-center justify-center hover:bg-indigo-700 transition-colors" title="Settings">⚙️</button>
        <button class="w-9 h-9 border border-gray-300 text-gray-600 rounded-xl flex items-center justify-center hover:bg-gray-50 transition-colors" title="Copy">📋</button>
      </div>
    </section>

    <!-- States -->
    <section class="bg-white rounded-2xl border p-6">
      <h3 class="text-sm font-bold text-gray-500 uppercase tracking-wide mb-4">States</h3>
      <div class="flex flex-wrap gap-3">
        <button class="bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm font-semibold">Normal</button>
        <button class="bg-indigo-700 text-white px-4 py-2 rounded-xl text-sm font-semibold ring-2 ring-indigo-500 ring-offset-2">Focused</button>
        <button class="bg-indigo-400 text-white/80 px-4 py-2 rounded-xl text-sm font-semibold cursor-not-allowed" disabled>Disabled</button>
        <button class="bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm font-semibold flex items-center gap-2 cursor-wait">
          <svg class="w-4 h-4 animate-spin" fill="none" viewBox="0 0 24 24"><circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle><path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4z"></path></svg>
          Loading...
        </button>
        <button class="bg-green-600 text-white px-4 py-2 rounded-xl text-sm font-semibold flex items-center gap-2">
          ✓ Success
        </button>
      </div>
    </section>

    <!-- Button Group -->
    <section class="bg-white rounded-2xl border p-6">
      <h3 class="text-sm font-bold text-gray-500 uppercase tracking-wide mb-4">Button Groups</h3>
      <div class="flex">
        <button class="bg-indigo-600 text-white px-4 py-2 rounded-l-xl text-sm font-semibold border-r border-indigo-700 hover:bg-indigo-700 transition-colors">รายวัน</button>
        <button class="bg-indigo-600 text-white px-4 py-2 text-sm font-semibold border-r border-indigo-700 hover:bg-indigo-700 transition-colors opacity-70">รายสัปดาห์</button>
        <button class="bg-indigo-600 text-white px-4 py-2 rounded-r-xl text-sm font-semibold hover:bg-indigo-700 transition-colors opacity-70">รายเดือน</button>
      </div>
    </section>

  </div>
</body>
</html>
```

---

## Step 443: Form Components

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Form Components - Step 443</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-8">
  <div class="max-w-2xl mx-auto space-y-6">
    <h2 class="text-xl font-extrabold">Form Components</h2>

    <div class="bg-white rounded-2xl border p-6 space-y-5">

      <!-- Text Input States -->
      <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1.5">Normal</label>
          <input type="text" placeholder="พิมพ์ข้อความ..." class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
        </div>
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1.5">Focused</label>
          <input type="text" value="สมชาย เทคโน" class="w-full border-2 border-indigo-500 rounded-xl px-4 py-2.5 text-sm outline-none ring-2 ring-indigo-200">
        </div>
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1.5">Error</label>
          <input type="email" value="invalid-email" class="w-full border-2 border-red-500 rounded-xl px-4 py-2.5 text-sm focus:outline-none">
          <p class="text-xs text-red-500 mt-1">รูปแบบ email ไม่ถูกต้อง</p>
        </div>
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1.5">Disabled</label>
          <input type="text" value="ค่าคงที่" disabled class="w-full border border-gray-200 bg-gray-50 rounded-xl px-4 py-2.5 text-sm text-gray-400 cursor-not-allowed">
        </div>
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1.5">With Icon Left</label>
          <div class="relative">
            <svg class="absolute left-3 top-1/2 -translate-y-1/2 w-4 h-4 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"/></svg>
            <input type="text" placeholder="ค้นหา..." class="w-full border border-gray-300 rounded-xl pl-9 pr-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
          </div>
        </div>
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1.5">With Addon</label>
          <div class="flex">
            <span class="bg-gray-100 border border-r-0 border-gray-300 rounded-l-xl px-3 flex items-center text-sm text-gray-500">https://</span>
            <input type="text" placeholder="yoursite.com" class="flex-1 border border-gray-300 rounded-r-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
          </div>
        </div>
      </div>

      <!-- Select -->
      <div>
        <label class="block text-sm font-medium text-gray-700 mb-1.5">Select</label>
        <div class="relative">
          <select class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 appearance-none">
            <option>เลือกจังหวัด...</option>
            <option>กรุงเทพมหานคร</option>
            <option>เชียงใหม่</option>
            <option>ภูเก็ต</option>
          </select>
          <svg class="absolute right-3 top-1/2 -translate-y-1/2 w-4 h-4 text-gray-400 pointer-events-none" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"/></svg>
        </div>
      </div>

      <!-- Checkbox & Radio -->
      <div class="grid grid-cols-2 gap-6">
        <div>
          <p class="text-sm font-medium text-gray-700 mb-2">Checkbox</p>
          <div class="space-y-2">
            <label class="flex items-center gap-2 cursor-pointer"><input type="checkbox" checked class="w-4 h-4 rounded text-indigo-600 focus:ring-indigo-500"><span class="text-sm">ตัวเลือก 1</span></label>
            <label class="flex items-center gap-2 cursor-pointer"><input type="checkbox" class="w-4 h-4 rounded text-indigo-600 focus:ring-indigo-500"><span class="text-sm">ตัวเลือก 2</span></label>
            <label class="flex items-center gap-2 cursor-pointer opacity-50"><input type="checkbox" disabled class="w-4 h-4 rounded"><span class="text-sm">Disabled</span></label>
          </div>
        </div>
        <div>
          <p class="text-sm font-medium text-gray-700 mb-2">Radio</p>
          <div class="space-y-2">
            <label class="flex items-center gap-2 cursor-pointer"><input type="radio" name="r" checked class="w-4 h-4 text-indigo-600 focus:ring-indigo-500"><span class="text-sm">ตัวเลือก A</span></label>
            <label class="flex items-center gap-2 cursor-pointer"><input type="radio" name="r" class="w-4 h-4 text-indigo-600 focus:ring-indigo-500"><span class="text-sm">ตัวเลือก B</span></label>
            <label class="flex items-center gap-2 cursor-pointer"><input type="radio" name="r" class="w-4 h-4 text-indigo-600 focus:ring-indigo-500"><span class="text-sm">ตัวเลือก C</span></label>
          </div>
        </div>
      </div>

      <!-- Toggle Switch -->
      <div>
        <p class="text-sm font-medium text-gray-700 mb-2">Toggle Switches</p>
        <div class="flex flex-wrap gap-6">
          <label class="flex items-center gap-3 cursor-pointer">
            <button class="relative bg-indigo-600 rounded-full w-11 h-6 transition-colors" onclick="this.classList.toggle('bg-indigo-600'); this.classList.toggle('bg-gray-300'); this.querySelector('span').classList.toggle('translate-x-5')">
              <span class="block w-5 h-5 bg-white rounded-full absolute top-0.5 left-0.5 translate-x-5 transition-transform shadow-sm"></span>
            </button>
            <span class="text-sm">เปิดใช้งาน</span>
          </label>
          <label class="flex items-center gap-3 cursor-pointer">
            <button class="relative bg-gray-300 rounded-full w-11 h-6 transition-colors" onclick="this.classList.toggle('bg-indigo-600'); this.classList.toggle('bg-gray-300'); this.querySelector('span').classList.toggle('translate-x-5')">
              <span class="block w-5 h-5 bg-white rounded-full absolute top-0.5 left-0.5 transition-transform shadow-sm"></span>
            </button>
            <span class="text-sm">ปิด</span>
          </label>
        </div>
      </div>

      <!-- Range Slider -->
      <div>
        <label class="block text-sm font-medium text-gray-700 mb-2">Range Slider</label>
        <input type="range" min="0" max="100" value="65" class="w-full accent-indigo-600 h-1.5">
        <div class="flex justify-between text-xs text-gray-400 mt-1"><span>0</span><span>65%</span><span>100</span></div>
      </div>

    </div>
  </div>
</body>
</html>
```

---

## Step 444–448: Badges, Alerts, Cards, Modals, Tooltips

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>UI Components - Steps 444-448</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-8">
  <div class="max-w-4xl mx-auto space-y-8">

    <!-- Badges (Step 444) -->
    <section class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4">Badges</h3>
      <div class="flex flex-wrap gap-2">
        <span class="bg-indigo-100 text-indigo-700 text-xs font-semibold px-2.5 py-1 rounded-full">Default</span>
        <span class="bg-green-100 text-green-700 text-xs font-semibold px-2.5 py-1 rounded-full">Success</span>
        <span class="bg-red-100 text-red-700 text-xs font-semibold px-2.5 py-1 rounded-full">Danger</span>
        <span class="bg-yellow-100 text-yellow-700 text-xs font-semibold px-2.5 py-1 rounded-full">Warning</span>
        <span class="bg-blue-100 text-blue-700 text-xs font-semibold px-2.5 py-1 rounded-full">Info</span>
        <span class="bg-gray-100 text-gray-700 text-xs font-semibold px-2.5 py-1 rounded-full">Neutral</span>
        <span class="bg-indigo-600 text-white text-xs font-semibold px-2.5 py-1 rounded-full">Solid</span>
        <span class="border border-indigo-500 text-indigo-600 text-xs font-semibold px-2.5 py-1 rounded-full">Outline</span>
        <!-- Dot badge -->
        <span class="flex items-center gap-1.5 bg-green-100 text-green-700 text-xs font-semibold px-2.5 py-1 rounded-full">
          <span class="w-1.5 h-1.5 bg-green-500 rounded-full"></span>Online
        </span>
        <!-- Number badge -->
        <div class="relative inline-block">
          <button class="bg-gray-100 text-gray-700 px-3 py-1.5 rounded-xl text-sm font-medium">การแจ้งเตือน</button>
          <span class="absolute -top-1.5 -right-1.5 w-5 h-5 bg-red-500 text-white text-[10px] font-bold rounded-full flex items-center justify-center">9+</span>
        </div>
      </div>
    </section>

    <!-- Alerts (Step 445) -->
    <section class="bg-white rounded-2xl border p-5 space-y-3">
      <h3 class="font-bold mb-4">Alert Components</h3>
      <div class="bg-blue-50 border border-blue-200 rounded-xl p-4 flex gap-3">
        <span class="text-blue-500 flex-shrink-0 mt-0.5">ℹ️</span>
        <div><p class="text-sm font-semibold text-blue-800">ข้อมูลเพิ่มเติม</p><p class="text-sm text-blue-700 mt-0.5">คุณสามารถเปลี่ยนการตั้งค่าได้ใน Profile Settings</p></div>
        <button class="ml-auto text-blue-400 hover:text-blue-600 transition-colors text-lg">✕</button>
      </div>
      <div class="bg-green-50 border border-green-200 rounded-xl p-4 flex gap-3">
        <span class="text-green-500 flex-shrink-0 mt-0.5">✅</span>
        <div><p class="text-sm font-semibold text-green-800">บันทึกสำเร็จ</p><p class="text-sm text-green-700 mt-0.5">ข้อมูลของคุณถูกบันทึกเรียบร้อยแล้ว</p></div>
        <button class="ml-auto text-green-400 hover:text-green-600 transition-colors text-lg">✕</button>
      </div>
      <div class="bg-yellow-50 border border-yellow-200 rounded-xl p-4 flex gap-3">
        <span class="text-yellow-500 flex-shrink-0 mt-0.5">⚠️</span>
        <div><p class="text-sm font-semibold text-yellow-800">คำเตือน</p><p class="text-sm text-yellow-700 mt-0.5">Subscription ของคุณจะหมดอายุใน 7 วัน</p></div>
        <button class="ml-auto text-yellow-400 hover:text-yellow-600 transition-colors text-lg">✕</button>
      </div>
      <div class="bg-red-50 border border-red-200 rounded-xl p-4 flex gap-3">
        <span class="text-red-500 flex-shrink-0 mt-0.5">❌</span>
        <div><p class="text-sm font-semibold text-red-800">เกิดข้อผิดพลาด</p><p class="text-sm text-red-700 mt-0.5">ไม่สามารถบันทึกได้ กรุณาลองใหม่อีกครั้ง</p></div>
        <button class="ml-auto text-red-400 hover:text-red-600 transition-colors text-lg">✕</button>
      </div>
    </section>

    <!-- Cards (Step 446) -->
    <section class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4">Card Variants</h3>
      <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
        <!-- Basic card -->
        <div class="border rounded-xl p-4 hover:shadow-md transition-shadow cursor-pointer">
          <div class="w-8 h-8 bg-indigo-100 rounded-lg flex items-center justify-center text-indigo-600 mb-3">📊</div>
          <h4 class="font-semibold text-sm">Basic Card</h4>
          <p class="text-xs text-gray-500 mt-1">Card ธรรมดาที่ใช้บ่อยที่สุด มี border และ hover shadow</p>
        </div>
        <!-- Elevated -->
        <div class="shadow-lg rounded-xl p-4 bg-white cursor-pointer hover:shadow-xl transition-shadow">
          <div class="w-8 h-8 bg-green-100 rounded-lg flex items-center justify-center text-green-600 mb-3">💡</div>
          <h4 class="font-semibold text-sm">Elevated</h4>
          <p class="text-xs text-gray-500 mt-1">ใช้ shadow แทน border สร้างความลึก</p>
        </div>
        <!-- Gradient -->
        <div class="bg-gradient-to-br from-indigo-500 to-purple-600 rounded-xl p-4 text-white cursor-pointer hover:shadow-lg transition-shadow">
          <div class="w-8 h-8 bg-white/20 rounded-lg flex items-center justify-center mb-3">✨</div>
          <h4 class="font-semibold text-sm">Gradient</h4>
          <p class="text-xs text-white/80 mt-1">Gradient card สำหรับ highlight feature</p>
        </div>
      </div>
    </section>

    <!-- Tooltip (Step 447) -->
    <section class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4">Tooltips</h3>
      <div class="flex flex-wrap gap-6 items-center">
        <div class="relative group">
          <button class="bg-indigo-100 text-indigo-700 px-3 py-1.5 rounded-xl text-sm">Hover me</button>
          <div class="absolute bottom-full left-1/2 -translate-x-1/2 mb-2 bg-gray-900 text-white text-xs px-2.5 py-1.5 rounded-lg whitespace-nowrap opacity-0 group-hover:opacity-100 transition-opacity pointer-events-none">
            Tooltip text!
            <div class="absolute top-full left-1/2 -translate-x-1/2 border-4 border-transparent border-t-gray-900"></div>
          </div>
        </div>
        <div class="relative group">
          <button class="bg-gray-100 text-gray-700 px-3 py-1.5 rounded-xl text-sm">Right tooltip</button>
          <div class="absolute left-full top-1/2 -translate-y-1/2 ml-2 bg-gray-900 text-white text-xs px-2.5 py-1.5 rounded-lg whitespace-nowrap opacity-0 group-hover:opacity-100 transition-opacity pointer-events-none">
            Right side!
            <div class="absolute top-1/2 right-full -translate-y-1/2 border-4 border-transparent border-r-gray-900"></div>
          </div>
        </div>
      </div>
    </section>

    <!-- Modal (Step 448) -->
    <section class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4">Modal Dialog</h3>
      <button onclick="document.getElementById('demo-modal').classList.remove('hidden')"
        class="bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">
        เปิด Modal
      </button>
    </section>

  </div>

  <!-- Modal -->
  <div id="demo-modal" class="hidden fixed inset-0 bg-black/50 z-50 flex items-center justify-center p-4">
    <div class="bg-white rounded-2xl shadow-2xl w-full max-w-md overflow-hidden animate-in">
      <!-- Modal Header -->
      <div class="flex items-center justify-between p-5 border-b">
        <h3 class="font-extrabold text-lg">ยืนยันการดำเนินการ</h3>
        <button onclick="document.getElementById('demo-modal').classList.add('hidden')" class="text-gray-400 hover:text-gray-600 transition-colors text-xl">✕</button>
      </div>
      <!-- Modal Body -->
      <div class="p-5">
        <p class="text-gray-600 text-sm leading-relaxed">คุณแน่ใจหรือไม่ว่าต้องการดำเนินการนี้? การกระทำนี้ไม่สามารถยกเลิกได้</p>
        <div class="mt-4 bg-yellow-50 border border-yellow-200 rounded-xl p-3">
          <p class="text-xs text-yellow-800">⚠️ ข้อมูลที่เกี่ยวข้องทั้งหมดจะถูกลบออกด้วย</p>
        </div>
      </div>
      <!-- Modal Footer -->
      <div class="flex gap-3 p-5 border-t bg-gray-50">
        <button onclick="document.getElementById('demo-modal').classList.add('hidden')"
          class="flex-1 border border-gray-300 text-gray-700 py-2.5 rounded-xl text-sm font-semibold hover:bg-gray-100 transition-colors">
          ยกเลิก
        </button>
        <button onclick="document.getElementById('demo-modal').classList.add('hidden')"
          class="flex-1 bg-red-600 text-white py-2.5 rounded-xl text-sm font-semibold hover:bg-red-700 transition-colors">
          ยืนยัน
        </button>
      </div>
    </div>
  </div>

</body>
</html>
```

---

## Steps 449–450: Component Showcase Workshop

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Design System Showcase - Steps 449-450</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 text-gray-900">

  <!-- Storybook-style Header -->
  <header class="bg-white border-b px-6 h-14 flex items-center justify-between sticky top-0 z-20">
    <div class="flex items-center gap-3">
      <div class="w-7 h-7 bg-indigo-600 rounded-lg flex items-center justify-center text-white font-bold text-sm">DS</div>
      <h1 class="font-extrabold">Design System</h1>
      <span class="text-xs bg-indigo-100 text-indigo-700 font-semibold px-2 py-0.5 rounded-full">v2.0</span>
    </div>
    <div class="flex items-center gap-3">
      <!-- Dark mode toggle -->
      <button onclick="toggleDark()" class="w-8 h-8 border rounded-xl flex items-center justify-center text-gray-500 hover:bg-gray-50 transition-colors" id="dark-toggle">🌙</button>
      <button class="bg-indigo-600 text-white text-xs px-3 py-1.5 rounded-lg hover:bg-indigo-700 transition-colors">Copy Token</button>
    </div>
  </header>

  <div class="flex">
    <!-- Component Nav -->
    <aside class="w-48 bg-white border-r p-3 sticky top-14 h-[calc(100vh-56px)] overflow-y-auto flex-shrink-0">
      <p class="text-[10px] font-bold text-gray-400 uppercase tracking-wider px-2 mb-1.5">Foundation</p>
      <nav class="space-y-0.5 mb-3">
        <button onclick="showComp('colors')" class="comp-btn w-full text-left px-2 py-1.5 rounded-lg text-xs text-gray-600 hover:bg-gray-50 bg-indigo-50 text-indigo-700 font-medium transition-colors">🎨 Colors</button>
        <button onclick="showComp('type')" class="comp-btn w-full text-left px-2 py-1.5 rounded-lg text-xs text-gray-600 hover:bg-gray-50 transition-colors">📝 Typography</button>
        <button onclick="showComp('icons')" class="comp-btn w-full text-left px-2 py-1.5 rounded-lg text-xs text-gray-600 hover:bg-gray-50 transition-colors">✨ Icons</button>
      </nav>
      <p class="text-[10px] font-bold text-gray-400 uppercase tracking-wider px-2 mb-1.5">Components</p>
      <nav class="space-y-0.5">
        <button onclick="showComp('buttons')" class="comp-btn w-full text-left px-2 py-1.5 rounded-lg text-xs text-gray-600 hover:bg-gray-50 transition-colors">🔘 Buttons</button>
        <button onclick="showComp('forms')" class="comp-btn w-full text-left px-2 py-1.5 rounded-lg text-xs text-gray-600 hover:bg-gray-50 transition-colors">📋 Forms</button>
        <button onclick="showComp('badges')" class="comp-btn w-full text-left px-2 py-1.5 rounded-lg text-xs text-gray-600 hover:bg-gray-50 transition-colors">🏷️ Badges</button>
        <button onclick="showComp('alerts')" class="comp-btn w-full text-left px-2 py-1.5 rounded-lg text-xs text-gray-600 hover:bg-gray-50 transition-colors">🔔 Alerts</button>
        <button onclick="showComp('cards')" class="comp-btn w-full text-left px-2 py-1.5 rounded-lg text-xs text-gray-600 hover:bg-gray-50 transition-colors">🃏 Cards</button>
        <button onclick="showComp('modals')" class="comp-btn w-full text-left px-2 py-1.5 rounded-lg text-xs text-gray-600 hover:bg-gray-50 transition-colors">🪟 Modals</button>
      </nav>
    </aside>

    <!-- Component Display -->
    <main class="flex-1 p-6 overflow-y-auto">
      <div id="comp-display"></div>
    </main>
  </div>

  <script>
    const components = {
      colors: `
        <h2 class="text-xl font-extrabold mb-5">Colors</h2>
        <div class="space-y-4">
          <div class="bg-white rounded-2xl border p-5">
            <p class="text-sm font-semibold mb-3">Brand Palette</p>
            <div class="grid grid-cols-5 gap-2">
              ${['bg-indigo-100','bg-indigo-300','bg-indigo-500','bg-indigo-700','bg-indigo-900'].map((c,i) => `<div><div class="h-12 rounded-xl ${c}"></div><p class="text-[10px] text-center mt-1 text-gray-500">${[100,300,500,700,900][i]}</p></div>`).join('')}
            </div>
          </div>
          <div class="bg-white rounded-2xl border p-5">
            <p class="text-sm font-semibold mb-3">Semantic</p>
            <div class="grid grid-cols-4 gap-2">
              <div class="p-3 bg-green-500 rounded-xl text-white text-center text-xs font-bold">Success</div>
              <div class="p-3 bg-red-500 rounded-xl text-white text-center text-xs font-bold">Danger</div>
              <div class="p-3 bg-yellow-500 rounded-xl text-white text-center text-xs font-bold">Warning</div>
              <div class="p-3 bg-blue-500 rounded-xl text-white text-center text-xs font-bold">Info</div>
            </div>
          </div>
        </div>
      `,
      type: `
        <h2 class="text-xl font-extrabold mb-5">Typography</h2>
        <div class="bg-white rounded-2xl border p-6 space-y-4">
          <div class="flex items-baseline gap-4"><code class="text-xs text-gray-400 w-20">text-xs</code><p class="text-xs">Caption · 12px</p></div>
          <div class="flex items-baseline gap-4"><code class="text-xs text-gray-400 w-20">text-sm</code><p class="text-sm">Body · 14px</p></div>
          <div class="flex items-baseline gap-4"><code class="text-xs text-gray-400 w-20">text-base</code><p class="text-base">Paragraph · 16px</p></div>
          <div class="flex items-baseline gap-4"><code class="text-xs text-gray-400 w-20">text-lg</code><p class="text-lg font-semibold">Heading · 18px</p></div>
          <div class="flex items-baseline gap-4"><code class="text-xs text-gray-400 w-20">text-2xl</code><p class="text-2xl font-extrabold">Title · 24px</p></div>
        </div>
      `,
      icons: `
        <h2 class="text-xl font-extrabold mb-5">Icons</h2>
        <div class="bg-white rounded-2xl border p-5 grid grid-cols-8 gap-3 text-center text-2xl">
          ${['🏠','📊','⚙️','👤','🔔','📁','💬','🔍','✏️','🗑️','📤','📥','🔒','🔑','💡','❤️'].map(e => `<div class="p-2 hover:bg-gray-50 rounded-xl cursor-pointer transition-colors">${e}</div>`).join('')}
        </div>
      `,
      buttons: `
        <h2 class="text-xl font-extrabold mb-5">Buttons</h2>
        <div class="bg-white rounded-2xl border p-5 space-y-4">
          <div class="flex flex-wrap gap-3">
            <button class="bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">Primary</button>
            <button class="border-2 border-indigo-600 text-indigo-600 px-4 py-2 rounded-xl text-sm font-semibold hover:bg-indigo-50 transition-colors">Outline</button>
            <button class="bg-gray-100 text-gray-700 px-4 py-2 rounded-xl text-sm font-semibold hover:bg-gray-200 transition-colors">Ghost</button>
            <button class="bg-red-600 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-red-700 transition-colors">Danger</button>
          </div>
        </div>
      `,
      forms: `
        <h2 class="text-xl font-extrabold mb-5">Forms</h2>
        <div class="bg-white rounded-2xl border p-5 space-y-4 max-w-md">
          <div><label class="text-sm font-medium block mb-1.5">Text Input</label><input type="text" placeholder="พิมพ์ที่นี่..." class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500"></div>
          <div><label class="text-sm font-medium block mb-1.5">Select</label><select class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500"><option>เลือก...</option><option>ตัวเลือก 1</option></select></div>
          <div><label class="text-sm font-medium block mb-1.5">Textarea</label><textarea rows="3" class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 resize-none" placeholder="รายละเอียด..."></textarea></div>
        </div>
      `,
      badges: `
        <h2 class="text-xl font-extrabold mb-5">Badges</h2>
        <div class="bg-white rounded-2xl border p-5">
          <div class="flex flex-wrap gap-2">
            <span class="bg-indigo-100 text-indigo-700 text-xs font-semibold px-2.5 py-1 rounded-full">Default</span>
            <span class="bg-green-100 text-green-700 text-xs font-semibold px-2.5 py-1 rounded-full">Success</span>
            <span class="bg-red-100 text-red-700 text-xs font-semibold px-2.5 py-1 rounded-full">Danger</span>
            <span class="bg-yellow-100 text-yellow-700 text-xs font-semibold px-2.5 py-1 rounded-full">Warning</span>
            <span class="flex items-center gap-1.5 bg-green-100 text-green-700 text-xs font-semibold px-2.5 py-1 rounded-full"><span class="w-1.5 h-1.5 bg-green-500 rounded-full"></span>Online</span>
          </div>
        </div>
      `,
      alerts: `
        <h2 class="text-xl font-extrabold mb-5">Alerts</h2>
        <div class="space-y-3">
          <div class="bg-blue-50 border border-blue-200 rounded-xl p-4 flex gap-3"><span>ℹ️</span><p class="text-sm text-blue-800">Alert ประเภท Info</p></div>
          <div class="bg-green-50 border border-green-200 rounded-xl p-4 flex gap-3"><span>✅</span><p class="text-sm text-green-800">Alert ประเภท Success</p></div>
          <div class="bg-yellow-50 border border-yellow-200 rounded-xl p-4 flex gap-3"><span>⚠️</span><p class="text-sm text-yellow-800">Alert ประเภท Warning</p></div>
          <div class="bg-red-50 border border-red-200 rounded-xl p-4 flex gap-3"><span>❌</span><p class="text-sm text-red-800">Alert ประเภท Error</p></div>
        </div>
      `,
      cards: `
        <h2 class="text-xl font-extrabold mb-5">Cards</h2>
        <div class="grid grid-cols-3 gap-4">
          <div class="border rounded-xl p-4 hover:shadow-md transition-shadow cursor-pointer bg-white"><div class="text-2xl mb-2">📊</div><h4 class="font-semibold text-sm">Basic Card</h4><p class="text-xs text-gray-500 mt-1">border + hover shadow</p></div>
          <div class="shadow-lg rounded-xl p-4 bg-white cursor-pointer hover:shadow-xl transition-shadow"><div class="text-2xl mb-2">💡</div><h4 class="font-semibold text-sm">Elevated</h4><p class="text-xs text-gray-500 mt-1">shadow-only card</p></div>
          <div class="bg-gradient-to-br from-indigo-500 to-purple-600 rounded-xl p-4 text-white cursor-pointer"><div class="text-2xl mb-2">✨</div><h4 class="font-semibold text-sm">Gradient</h4><p class="text-xs text-white/80 mt-1">highlight card</p></div>
        </div>
      `,
      modals: `
        <h2 class="text-xl font-extrabold mb-5">Modals</h2>
        <div class="bg-white rounded-2xl border p-5">
          <button onclick="document.getElementById('showcase-modal').classList.remove('hidden')" class="bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">เปิด Modal ตัวอย่าง</button>
        </div>
        <div id="showcase-modal" class="hidden fixed inset-0 bg-black/50 z-50 flex items-center justify-center p-4">
          <div class="bg-white rounded-2xl shadow-2xl w-full max-w-sm p-6">
            <h3 class="font-extrabold text-lg mb-3">Modal Title</h3>
            <p class="text-sm text-gray-600 mb-5">นี่คือตัวอย่าง Modal Dialog ที่ใช้ใน Design System</p>
            <div class="flex gap-3">
              <button onclick="document.getElementById('showcase-modal').classList.add('hidden')" class="flex-1 border py-2.5 rounded-xl text-sm">ยกเลิก</button>
              <button onclick="document.getElementById('showcase-modal').classList.add('hidden')" class="flex-1 bg-indigo-600 text-white py-2.5 rounded-xl text-sm font-semibold">ยืนยัน</button>
            </div>
          </div>
        </div>
      `,
    };

    function showComp(name) {
      document.getElementById('comp-display').innerHTML = components[name] || '<p>เลือก component จากเมนูด้านซ้าย</p>';
      document.querySelectorAll('.comp-btn').forEach(b => b.className = 'comp-btn w-full text-left px-2 py-1.5 rounded-lg text-xs text-gray-600 hover:bg-gray-50 transition-colors');
      event.currentTarget.className = 'comp-btn w-full text-left px-2 py-1.5 rounded-lg text-xs bg-indigo-50 text-indigo-700 font-medium transition-colors';
    }

    function toggleDark() {
      document.documentElement.classList.toggle('dark');
      document.body.classList.toggle('bg-gray-900');
      document.body.classList.toggle('text-white');
      document.getElementById('dark-toggle').textContent = document.body.classList.contains('bg-gray-900') ? '☀️' : '🌙';
    }

    // Show colors by default
    showComp('colors');
  </script>

</body>
</html>
```

---

## สรุป Part 45

| Step | เนื้อหา |
|------|---------|
| 441 | Design Tokens: Color palette, Typography scale, Spacing scale |
| 442 | Button System: variants, sizes, icons, states, groups |
| 443 | Form Components: inputs, select, checkbox, radio, toggle, range |
| 444 | Badges: variants, dot badge, number badge |
| 445 | Alert Components: info, success, warning, error |
| 446 | Card Variants: basic, elevated, gradient |
| 447 | Tooltips: top, right, CSS-only hover |
| 448 | Modal Dialog: header/body/footer pattern |
| 449–450 | Workshop: Storybook-style Component Showcase |

**Part ถัดไป:** Part 46 — Advanced Animations & Micro-interactions (Steps 451–460)
