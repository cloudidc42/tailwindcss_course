# Part 40: Project 3 — E-Commerce Store (Full Project)

## เป้าหมาย
- สร้าง E-Commerce Store ระดับ Production
- หน้า Catalog, Product Detail, Cart, Checkout, Order Confirmation
- เชื่อมทุก concept จาก Level 3 (Steps 391–400)

---

## Step 391: Store Layout & Header

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>TechShop — Store Layout</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-white text-gray-900">

  <!-- Header -->
  <header class="sticky top-0 z-40 bg-white border-b shadow-sm">
    <div class="max-w-7xl mx-auto px-4 h-16 flex items-center justify-between">

      <!-- Logo -->
      <a href="#" class="text-xl font-extrabold text-indigo-600 tracking-tight">TechShop</a>

      <!-- Search Bar -->
      <div class="hidden md:flex flex-1 max-w-md mx-8">
        <div class="relative w-full">
          <svg class="absolute left-3 top-1/2 -translate-y-1/2 w-4 h-4 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"/></svg>
          <input type="text" placeholder="ค้นหาสินค้า..." class="w-full border border-gray-300 rounded-xl pl-10 pr-4 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
        </div>
      </div>

      <!-- Actions -->
      <div class="flex items-center gap-2">
        <button class="p-2 hover:bg-gray-100 rounded-xl transition-colors relative">
          <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z"/></svg>
          <span class="absolute top-1 right-1 w-3.5 h-3.5 bg-red-500 rounded-full text-[10px] text-white flex items-center justify-center font-bold">3</span>
        </button>
        <button class="p-2 hover:bg-gray-100 rounded-xl transition-colors relative" onclick="toggleCart()">
          <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 11V7a4 4 0 00-8 0v4M5 9h14l1 12H4L5 9z"/></svg>
          <span class="absolute top-1 right-1 w-3.5 h-3.5 bg-indigo-600 rounded-full text-[10px] text-white flex items-center justify-center font-bold" id="cart-count">2</span>
        </button>
        <a href="#" class="hidden md:flex items-center gap-2 hover:bg-gray-100 rounded-xl px-3 py-2 transition-colors">
          <img src="https://i.pravatar.cc/28?img=1" class="w-7 h-7 rounded-full" alt="">
          <span class="text-sm font-medium">สมชาย</span>
        </a>
      </div>
    </div>

    <!-- Category Nav -->
    <div class="border-t bg-gray-50">
      <div class="max-w-7xl mx-auto px-4 h-10 flex items-center gap-1 overflow-x-auto scrollbar-hide text-sm">
        <a href="#" class="whitespace-nowrap px-3 py-1 rounded-lg bg-indigo-600 text-white font-medium">ทั้งหมด</a>
        <a href="#" class="whitespace-nowrap px-3 py-1 rounded-lg text-gray-600 hover:bg-gray-200 transition-colors">โทรศัพท์</a>
        <a href="#" class="whitespace-nowrap px-3 py-1 rounded-lg text-gray-600 hover:bg-gray-200 transition-colors">Laptop</a>
        <a href="#" class="whitespace-nowrap px-3 py-1 rounded-lg text-gray-600 hover:bg-gray-200 transition-colors">หูฟัง</a>
        <a href="#" class="whitespace-nowrap px-3 py-1 rounded-lg text-gray-600 hover:bg-gray-200 transition-colors">กล้อง</a>
        <a href="#" class="whitespace-nowrap px-3 py-1 rounded-lg text-gray-600 hover:bg-gray-200 transition-colors">Smart Watch</a>
        <a href="#" class="whitespace-nowrap px-3 py-1 rounded-lg text-gray-600 hover:bg-gray-200 transition-colors">Gaming</a>
        <a href="#" class="whitespace-nowrap px-3 py-1 rounded-lg text-gray-600 hover:bg-gray-200 transition-colors">อุปกรณ์เสริม</a>
      </div>
    </div>
  </header>

  <!-- Hero Banner -->
  <div class="bg-gradient-to-r from-indigo-900 via-indigo-700 to-purple-700 text-white py-12 px-4">
    <div class="max-w-7xl mx-auto flex items-center justify-between">
      <div class="max-w-md">
        <span class="bg-yellow-400 text-yellow-900 text-xs font-bold px-2.5 py-1 rounded-full uppercase tracking-wide">Flash Sale</span>
        <h1 class="text-3xl font-extrabold mt-3 leading-tight">iPhone 15 Pro Max<br>ราคาพิเศษ!</h1>
        <p class="text-indigo-200 mt-2 text-sm">ลดสูงสุด 15% ถึงวันที่ 30 มิ.ย. นี้เท่านั้น</p>
        <div class="flex gap-3 mt-5">
          <button class="bg-white text-indigo-700 font-bold px-5 py-2.5 rounded-xl text-sm hover:bg-indigo-50 transition-colors">ดูสินค้า</button>
          <button class="border border-white/40 text-white px-5 py-2.5 rounded-xl text-sm hover:bg-white/10 transition-colors">โปรทั้งหมด</button>
        </div>
      </div>
      <div class="hidden md:block text-8xl">📱</div>
    </div>
  </div>

  <!-- Cart Drawer -->
  <div id="cart-overlay" class="hidden fixed inset-0 bg-black/40 z-50" onclick="toggleCart()"></div>
  <div id="cart-drawer" class="fixed top-0 right-0 h-full w-full max-w-sm bg-white shadow-2xl z-50 transform translate-x-full transition-transform duration-300 flex flex-col">
    <div class="flex items-center justify-between p-4 border-b">
      <h2 class="font-extrabold text-lg">ตะกร้าสินค้า (2)</h2>
      <button onclick="toggleCart()" class="text-gray-400 hover:text-gray-600 p-1">✕</button>
    </div>
    <div class="flex-1 overflow-y-auto p-4 space-y-3">
      <div class="flex gap-3 p-3 border rounded-xl">
        <div class="w-16 h-16 bg-gray-100 rounded-lg flex items-center justify-center text-2xl flex-shrink-0">📱</div>
        <div class="flex-1 min-w-0">
          <p class="text-sm font-medium truncate">iPhone 15 Pro Max 256GB</p>
          <p class="text-xs text-gray-500 mt-0.5">Natural Titanium · 256GB</p>
          <div class="flex items-center justify-between mt-2">
            <div class="flex items-center gap-2 border rounded-lg">
              <button class="px-2 py-0.5 text-gray-600 hover:bg-gray-100 transition-colors text-sm">−</button>
              <span class="text-sm w-5 text-center">1</span>
              <button class="px-2 py-0.5 text-gray-600 hover:bg-gray-100 transition-colors text-sm">+</button>
            </div>
            <p class="text-sm font-bold text-indigo-600">฿45,900</p>
          </div>
        </div>
      </div>
      <div class="flex gap-3 p-3 border rounded-xl">
        <div class="w-16 h-16 bg-gray-100 rounded-lg flex items-center justify-center text-2xl flex-shrink-0">🎧</div>
        <div class="flex-1 min-w-0">
          <p class="text-sm font-medium truncate">AirPods Pro (2nd Gen)</p>
          <p class="text-xs text-gray-500 mt-0.5">สีขาว</p>
          <div class="flex items-center justify-between mt-2">
            <div class="flex items-center gap-2 border rounded-lg">
              <button class="px-2 py-0.5 text-gray-600 hover:bg-gray-100 transition-colors text-sm">−</button>
              <span class="text-sm w-5 text-center">1</span>
              <button class="px-2 py-0.5 text-gray-600 hover:bg-gray-100 transition-colors text-sm">+</button>
            </div>
            <p class="text-sm font-bold text-indigo-600">฿8,900</p>
          </div>
        </div>
      </div>
    </div>
    <div class="p-4 border-t space-y-3">
      <div class="flex justify-between text-sm text-gray-600"><span>ราคารวม</span><span>฿54,800</span></div>
      <div class="flex justify-between text-sm text-gray-600"><span>ค่าจัดส่ง</span><span class="text-green-600">ฟรี</span></div>
      <div class="flex justify-between font-extrabold text-lg border-t pt-2"><span>รวมทั้งสิ้น</span><span class="text-indigo-600">฿54,800</span></div>
      <button class="w-full bg-indigo-600 text-white py-3 rounded-xl font-semibold hover:bg-indigo-700 transition-colors">ชำระเงิน</button>
      <button onclick="toggleCart()" class="w-full text-center text-sm text-indigo-600 hover:underline">ช้อปปิ้งต่อ</button>
    </div>
  </div>

  <script>
    function toggleCart() {
      const drawer = document.getElementById('cart-drawer');
      const overlay = document.getElementById('cart-overlay');
      const open = drawer.classList.contains('translate-x-full');
      drawer.classList.toggle('translate-x-full', !open);
      overlay.classList.toggle('hidden', !open);
    }
  </script>

</body>
</html>
```

---

## Step 392: Product Catalog with Filters

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Product Catalog - Step 392</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-6">

  <div class="max-w-7xl mx-auto">
    <div class="flex gap-6">

      <!-- Filter Sidebar -->
      <aside class="hidden lg:block w-56 flex-shrink-0">
        <div class="bg-white rounded-xl border p-5 sticky top-5 space-y-5">
          <div>
            <h4 class="font-semibold text-sm mb-3">ราคา</h4>
            <div class="space-y-1 text-sm">
              <label class="flex items-center gap-2 cursor-pointer hover:text-indigo-600 transition-colors">
                <input type="checkbox" class="rounded text-indigo-600"> ต่ำกว่า ฿5,000
              </label>
              <label class="flex items-center gap-2 cursor-pointer hover:text-indigo-600 transition-colors">
                <input type="checkbox" class="rounded text-indigo-600"> ฿5,000 – ฿20,000
              </label>
              <label class="flex items-center gap-2 cursor-pointer hover:text-indigo-600 transition-colors">
                <input type="checkbox" checked class="rounded text-indigo-600"> ฿20,000 – ฿50,000
              </label>
              <label class="flex items-center gap-2 cursor-pointer hover:text-indigo-600 transition-colors">
                <input type="checkbox" class="rounded text-indigo-600"> มากกว่า ฿50,000
              </label>
            </div>
          </div>
          <hr>
          <div>
            <h4 class="font-semibold text-sm mb-3">แบรนด์</h4>
            <div class="space-y-1 text-sm">
              <label class="flex items-center gap-2 cursor-pointer"><input type="checkbox" checked class="rounded text-indigo-600"> Apple</label>
              <label class="flex items-center gap-2 cursor-pointer"><input type="checkbox" class="rounded text-indigo-600"> Samsung</label>
              <label class="flex items-center gap-2 cursor-pointer"><input type="checkbox" class="rounded text-indigo-600"> Sony</label>
              <label class="flex items-center gap-2 cursor-pointer"><input type="checkbox" class="rounded text-indigo-600"> Xiaomi</label>
              <label class="flex items-center gap-2 cursor-pointer"><input type="checkbox" class="rounded text-indigo-600"> ASUS</label>
            </div>
          </div>
          <hr>
          <div>
            <h4 class="font-semibold text-sm mb-3">คะแนน</h4>
            <div class="space-y-1 text-sm">
              <label class="flex items-center gap-2 cursor-pointer"><input type="radio" name="rating" class="text-indigo-600"> ⭐⭐⭐⭐⭐ ขึ้นไป</label>
              <label class="flex items-center gap-2 cursor-pointer"><input type="radio" name="rating" checked class="text-indigo-600"> ⭐⭐⭐⭐ ขึ้นไป</label>
              <label class="flex items-center gap-2 cursor-pointer"><input type="radio" name="rating" class="text-indigo-600"> ⭐⭐⭐ ขึ้นไป</label>
            </div>
          </div>
          <button class="w-full text-xs text-gray-500 hover:text-red-500 transition-colors">ล้างตัวกรอง</button>
        </div>
      </aside>

      <!-- Products Grid -->
      <div class="flex-1">
        <!-- Sort Bar -->
        <div class="flex items-center justify-between mb-4">
          <p class="text-sm text-gray-500">พบ <strong>48</strong> สินค้า</p>
          <div class="flex items-center gap-2">
            <select class="text-sm border border-gray-300 rounded-xl px-3 py-1.5 focus:outline-none focus:ring-2 focus:ring-indigo-500">
              <option>เรียงตาม: แนะนำ</option>
              <option>ราคา: ต่ำ→สูง</option>
              <option>ราคา: สูง→ต่ำ</option>
              <option>คะแนนสูงสุด</option>
              <option>ขายดีสุด</option>
            </select>
            <div class="flex border border-gray-300 rounded-xl overflow-hidden">
              <button class="p-2 bg-indigo-50 text-indigo-600">
                <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 20 20"><path d="M5 3a2 2 0 00-2 2v2a2 2 0 002 2h2a2 2 0 002-2V5a2 2 0 00-2-2H5zM5 11a2 2 0 00-2 2v2a2 2 0 002 2h2a2 2 0 002-2v-2a2 2 0 00-2-2H5zM11 5a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2V5zM11 13a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2v-2z"/></svg>
              </button>
              <button class="p-2 text-gray-400 hover:bg-gray-50 transition-colors">
                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 10h16M4 14h16M4 18h16"/></svg>
              </button>
            </div>
          </div>
        </div>

        <!-- Products -->
        <div class="grid grid-cols-2 md:grid-cols-3 xl:grid-cols-4 gap-4">
          <!-- Product Card -->
          <div class="bg-white rounded-xl border hover:shadow-md transition-shadow group cursor-pointer">
            <div class="relative p-4 pb-2">
              <span class="absolute top-3 left-3 bg-red-500 text-white text-[10px] font-bold px-2 py-0.5 rounded-full z-10">-15%</span>
              <button class="absolute top-3 right-3 text-gray-300 hover:text-red-500 transition-colors z-10">♥</button>
              <div class="w-full aspect-square bg-gray-50 rounded-xl flex items-center justify-center text-5xl group-hover:scale-105 transition-transform duration-200">📱</div>
            </div>
            <div class="p-3">
              <p class="text-[11px] text-indigo-600 font-semibold uppercase tracking-wide">Apple</p>
              <p class="text-sm font-semibold line-clamp-2 mt-0.5">iPhone 15 Pro Max 256GB Natural Titanium</p>
              <div class="flex items-center gap-1 mt-1.5">
                <span class="text-yellow-400 text-xs">★★★★★</span>
                <span class="text-[11px] text-gray-400">(2.4k)</span>
              </div>
              <div class="flex items-center justify-between mt-2">
                <div>
                  <p class="text-base font-extrabold text-gray-900">฿45,900</p>
                  <p class="text-[11px] text-gray-400 line-through">฿53,900</p>
                </div>
                <button class="bg-indigo-600 text-white p-2 rounded-xl hover:bg-indigo-700 transition-colors">
                  <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 11V7a4 4 0 00-8 0v4M5 9h14l1 12H4L5 9z"/></svg>
                </button>
              </div>
            </div>
          </div>

          <div class="bg-white rounded-xl border hover:shadow-md transition-shadow group cursor-pointer">
            <div class="relative p-4 pb-2">
              <span class="absolute top-3 left-3 bg-green-500 text-white text-[10px] font-bold px-2 py-0.5 rounded-full z-10">ใหม่</span>
              <button class="absolute top-3 right-3 text-red-400 hover:text-red-600 transition-colors z-10">♥</button>
              <div class="w-full aspect-square bg-gray-50 rounded-xl flex items-center justify-center text-5xl group-hover:scale-105 transition-transform duration-200">💻</div>
            </div>
            <div class="p-3">
              <p class="text-[11px] text-indigo-600 font-semibold uppercase tracking-wide">Apple</p>
              <p class="text-sm font-semibold line-clamp-2 mt-0.5">MacBook Pro 14" M3 Pro 512GB</p>
              <div class="flex items-center gap-1 mt-1.5">
                <span class="text-yellow-400 text-xs">★★★★★</span>
                <span class="text-[11px] text-gray-400">(890)</span>
              </div>
              <div class="flex items-center justify-between mt-2">
                <div>
                  <p class="text-base font-extrabold text-gray-900">฿79,900</p>
                  <p class="text-[11px] text-transparent">-</p>
                </div>
                <button class="bg-indigo-600 text-white p-2 rounded-xl hover:bg-indigo-700 transition-colors">
                  <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 11V7a4 4 0 00-8 0v4M5 9h14l1 12H4L5 9z"/></svg>
                </button>
              </div>
            </div>
          </div>

          <div class="bg-white rounded-xl border hover:shadow-md transition-shadow group cursor-pointer">
            <div class="relative p-4 pb-2">
              <button class="absolute top-3 right-3 text-gray-300 hover:text-red-500 transition-colors z-10">♥</button>
              <div class="w-full aspect-square bg-gray-50 rounded-xl flex items-center justify-center text-5xl group-hover:scale-105 transition-transform duration-200">🎧</div>
            </div>
            <div class="p-3">
              <p class="text-[11px] text-indigo-600 font-semibold uppercase tracking-wide">Apple</p>
              <p class="text-sm font-semibold line-clamp-2 mt-0.5">AirPods Pro (2nd Generation) USB-C</p>
              <div class="flex items-center gap-1 mt-1.5">
                <span class="text-yellow-400 text-xs">★★★★☆</span>
                <span class="text-[11px] text-gray-400">(3.1k)</span>
              </div>
              <div class="flex items-center justify-between mt-2">
                <div>
                  <p class="text-base font-extrabold text-gray-900">฿8,900</p>
                  <p class="text-[11px] text-gray-400 line-through">฿9,900</p>
                </div>
                <button class="bg-indigo-600 text-white p-2 rounded-xl hover:bg-indigo-700 transition-colors">
                  <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 11V7a4 4 0 00-8 0v4M5 9h14l1 12H4L5 9z"/></svg>
                </button>
              </div>
            </div>
          </div>

          <div class="bg-white rounded-xl border hover:shadow-md transition-shadow group cursor-pointer">
            <div class="relative p-4 pb-2">
              <span class="absolute top-3 left-3 bg-orange-500 text-white text-[10px] font-bold px-2 py-0.5 rounded-full z-10">Hot</span>
              <button class="absolute top-3 right-3 text-gray-300 hover:text-red-500 transition-colors z-10">♥</button>
              <div class="w-full aspect-square bg-gray-50 rounded-xl flex items-center justify-center text-5xl group-hover:scale-105 transition-transform duration-200">⌚</div>
            </div>
            <div class="p-3">
              <p class="text-[11px] text-indigo-600 font-semibold uppercase tracking-wide">Apple</p>
              <p class="text-sm font-semibold line-clamp-2 mt-0.5">Apple Watch Series 9 GPS 41mm</p>
              <div class="flex items-center gap-1 mt-1.5">
                <span class="text-yellow-400 text-xs">★★★★★</span>
                <span class="text-[11px] text-gray-400">(1.8k)</span>
              </div>
              <div class="flex items-center justify-between mt-2">
                <div>
                  <p class="text-base font-extrabold text-gray-900">฿14,900</p>
                  <p class="text-[11px] text-gray-400 line-through">฿17,900</p>
                </div>
                <button class="bg-indigo-600 text-white p-2 rounded-xl hover:bg-indigo-700 transition-colors">
                  <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 11V7a4 4 0 00-8 0v4M5 9h14l1 12H4L5 9z"/></svg>
                </button>
              </div>
            </div>
          </div>
        </div>

        <!-- Pagination -->
        <div class="flex items-center justify-center gap-1 mt-8">
          <button class="w-9 h-9 flex items-center justify-center border rounded-lg text-gray-400 hover:border-indigo-500 hover:text-indigo-600 transition-colors">‹</button>
          <button class="w-9 h-9 flex items-center justify-center border rounded-lg bg-indigo-600 text-white font-semibold text-sm">1</button>
          <button class="w-9 h-9 flex items-center justify-center border rounded-lg text-sm hover:border-indigo-500 hover:text-indigo-600 transition-colors">2</button>
          <button class="w-9 h-9 flex items-center justify-center border rounded-lg text-sm hover:border-indigo-500 hover:text-indigo-600 transition-colors">3</button>
          <span class="w-9 h-9 flex items-center justify-center text-gray-400">...</span>
          <button class="w-9 h-9 flex items-center justify-center border rounded-lg text-sm hover:border-indigo-500 hover:text-indigo-600 transition-colors">12</button>
          <button class="w-9 h-9 flex items-center justify-center border rounded-lg text-gray-400 hover:border-indigo-500 hover:text-indigo-600 transition-colors">›</button>
        </div>
      </div>
    </div>
  </div>

</body>
</html>
```

---

## Step 393: Product Detail Page

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Product Detail - Step 393</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-6">
  <div class="max-w-5xl mx-auto">
    <!-- Breadcrumb -->
    <div class="flex items-center gap-2 text-sm text-gray-500 mb-5">
      <a href="#" class="hover:text-indigo-600">หน้าแรก</a><span>›</span>
      <a href="#" class="hover:text-indigo-600">โทรศัพท์</a><span>›</span>
      <a href="#" class="hover:text-indigo-600">Apple</a><span>›</span>
      <span class="text-gray-800">iPhone 15 Pro Max</span>
    </div>

    <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
      <!-- Image Gallery -->
      <div class="space-y-3">
        <div class="bg-white border rounded-2xl p-6 flex items-center justify-center aspect-square text-8xl">
          📱
        </div>
        <div class="grid grid-cols-4 gap-2">
          <button class="bg-white border-2 border-indigo-500 rounded-xl p-2 aspect-square flex items-center justify-center text-2xl">📱</button>
          <button class="bg-white border rounded-xl p-2 aspect-square flex items-center justify-center text-2xl hover:border-indigo-300 transition-colors">📲</button>
          <button class="bg-white border rounded-xl p-2 aspect-square flex items-center justify-center text-2xl hover:border-indigo-300 transition-colors">🔋</button>
          <button class="bg-white border rounded-xl p-2 aspect-square flex items-center justify-center text-2xl hover:border-indigo-300 transition-colors">📦</button>
        </div>
      </div>

      <!-- Product Info -->
      <div>
        <div class="flex items-center gap-2 mb-2">
          <span class="bg-green-100 text-green-700 text-xs font-bold px-2.5 py-1 rounded-full">มีสินค้า</span>
          <span class="bg-red-100 text-red-700 text-xs font-bold px-2.5 py-1 rounded-full">Flash Sale</span>
        </div>
        <h1 class="text-2xl font-extrabold">iPhone 15 Pro Max</h1>
        <p class="text-gray-500 text-sm mt-1">256GB · Natural Titanium</p>

        <!-- Rating -->
        <div class="flex items-center gap-2 mt-3">
          <div class="flex text-yellow-400">★★★★★</div>
          <span class="text-sm font-semibold">4.9</span>
          <span class="text-sm text-gray-400">(2,412 รีวิว)</span>
          <span class="text-sm text-gray-400">· ขายแล้ว 15,432 ชิ้น</span>
        </div>

        <!-- Price -->
        <div class="mt-4 p-4 bg-red-50 rounded-2xl">
          <div class="flex items-baseline gap-2">
            <span class="text-3xl font-extrabold text-red-600">฿45,900</span>
            <span class="text-gray-400 line-through text-lg">฿53,900</span>
            <span class="bg-red-500 text-white text-xs font-bold px-2 py-0.5 rounded-full">-15%</span>
          </div>
          <p class="text-xs text-red-500 mt-1">ราคาพิเศษจนถึง 30 มิ.ย. เท่านั้น</p>
        </div>

        <!-- Storage -->
        <div class="mt-4">
          <p class="text-sm font-semibold mb-2">ความจุ</p>
          <div class="flex gap-2">
            <button class="border-2 border-indigo-500 bg-indigo-50 text-indigo-700 px-4 py-1.5 rounded-lg text-sm font-semibold">256GB</button>
            <button class="border border-gray-300 text-gray-700 px-4 py-1.5 rounded-lg text-sm hover:border-indigo-300 transition-colors">512GB</button>
            <button class="border border-gray-300 text-gray-700 px-4 py-1.5 rounded-lg text-sm hover:border-indigo-300 transition-colors">1TB</button>
          </div>
        </div>

        <!-- Color -->
        <div class="mt-4">
          <p class="text-sm font-semibold mb-2">สี: <span class="font-normal text-gray-500">Natural Titanium</span></p>
          <div class="flex gap-2">
            <button class="w-7 h-7 rounded-full bg-stone-400 ring-2 ring-indigo-500 ring-offset-2" title="Natural"></button>
            <button class="w-7 h-7 rounded-full bg-gray-800 hover:ring-2 hover:ring-gray-400 hover:ring-offset-2 transition-all" title="Black"></button>
            <button class="w-7 h-7 rounded-full bg-slate-300 hover:ring-2 hover:ring-slate-400 hover:ring-offset-2 transition-all" title="White"></button>
            <button class="w-7 h-7 rounded-full bg-blue-600 hover:ring-2 hover:ring-blue-500 hover:ring-offset-2 transition-all" title="Blue"></button>
          </div>
        </div>

        <!-- Quantity -->
        <div class="mt-4 flex items-center gap-4">
          <p class="text-sm font-semibold">จำนวน</p>
          <div class="flex items-center border rounded-xl">
            <button class="px-3 py-2 text-gray-600 hover:bg-gray-100 rounded-l-xl transition-colors">−</button>
            <span class="px-4 py-2 text-sm font-semibold border-x">1</span>
            <button class="px-3 py-2 text-gray-600 hover:bg-gray-100 rounded-r-xl transition-colors">+</button>
          </div>
          <p class="text-xs text-gray-400">เหลือ 28 ชิ้น</p>
        </div>

        <!-- Actions -->
        <div class="mt-5 flex gap-3">
          <button class="flex-1 bg-indigo-600 text-white py-3 rounded-xl font-semibold hover:bg-indigo-700 transition-colors flex items-center justify-center gap-2">
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 11V7a4 4 0 00-8 0v4M5 9h14l1 12H4L5 9z"/></svg>
            เพิ่มลงตะกร้า
          </button>
          <button class="border-2 border-indigo-600 text-indigo-600 px-4 py-3 rounded-xl font-semibold hover:bg-indigo-50 transition-colors">
            ♥
          </button>
        </div>

        <!-- Delivery & Warranty -->
        <div class="mt-4 space-y-2 text-sm text-gray-600">
          <div class="flex items-center gap-2"><span>🚚</span><span>ส่งฟรี ถึงบ้าน 1–2 วัน</span></div>
          <div class="flex items-center gap-2"><span>🛡️</span><span>รับประกัน 1 ปี โดย Apple Thailand</span></div>
          <div class="flex items-center gap-2"><span>↩️</span><span>คืนสินค้าภายใน 14 วัน</span></div>
        </div>
      </div>
    </div>
  </div>
</body>
</html>
```

---

## Step 394–398: Checkout Flow

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Checkout - Steps 394-398</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50">

  <!-- Progress Steps -->
  <div class="bg-white border-b py-4 px-4 sticky top-0 z-20">
    <div class="max-w-3xl mx-auto">
      <div class="flex items-center justify-between">
        <div class="flex-1">
          <div class="flex items-center">
            <div class="w-8 h-8 bg-indigo-600 text-white rounded-full flex items-center justify-center text-sm font-bold">1</div>
            <div class="flex-1 h-1 bg-indigo-600 mx-2"></div>
          </div>
          <p class="text-xs text-indigo-600 font-medium mt-1">ตะกร้า</p>
        </div>
        <div class="flex-1">
          <div class="flex items-center">
            <div class="w-8 h-8 bg-indigo-600 text-white rounded-full flex items-center justify-center text-sm font-bold">2</div>
            <div class="flex-1 h-1 bg-gray-200 mx-2"></div>
          </div>
          <p class="text-xs text-indigo-600 font-medium mt-1">ที่อยู่</p>
        </div>
        <div class="flex-1">
          <div class="flex items-center">
            <div class="w-8 h-8 bg-gray-200 text-gray-500 rounded-full flex items-center justify-center text-sm font-bold">3</div>
            <div class="flex-1 h-1 bg-gray-200 mx-2"></div>
          </div>
          <p class="text-xs text-gray-400 mt-1">ชำระเงิน</p>
        </div>
        <div>
          <div class="w-8 h-8 bg-gray-200 text-gray-500 rounded-full flex items-center justify-center text-sm font-bold">4</div>
          <p class="text-xs text-gray-400 mt-1">ยืนยัน</p>
        </div>
      </div>
    </div>
  </div>

  <div class="max-w-5xl mx-auto px-4 py-6">
    <div class="grid grid-cols-1 lg:grid-cols-[1fr_380px] gap-6">

      <!-- Left: Shipping Address -->
      <div class="space-y-5">

        <!-- Saved Addresses -->
        <div class="bg-white rounded-2xl border p-5">
          <div class="flex items-center justify-between mb-4">
            <h3 class="font-extrabold text-lg">ที่อยู่จัดส่ง</h3>
            <button class="text-sm text-indigo-600 hover:underline">+ เพิ่มที่อยู่</button>
          </div>
          <div class="space-y-3">
            <label class="flex gap-3 p-3 border-2 border-indigo-500 bg-indigo-50 rounded-xl cursor-pointer">
              <input type="radio" name="addr" checked class="mt-0.5 text-indigo-600">
              <div>
                <div class="flex items-center gap-2">
                  <p class="text-sm font-semibold">สมชาย เทคโน</p>
                  <span class="bg-indigo-100 text-indigo-700 text-[10px] font-bold px-2 py-0.5 rounded-full">ค่าเริ่มต้น</span>
                </div>
                <p class="text-sm text-gray-600 mt-0.5">123/45 ถ.สุขุมวิท 21 แขวงคลองเตยเหนือ</p>
                <p class="text-sm text-gray-600">เขตวัฒนา กทม. 10110</p>
                <p class="text-sm text-gray-600">Tel: 081-234-5678</p>
              </div>
            </label>
            <label class="flex gap-3 p-3 border rounded-xl cursor-pointer hover:border-indigo-300 transition-colors">
              <input type="radio" name="addr" class="mt-0.5 text-indigo-600">
              <div>
                <p class="text-sm font-semibold">สำนักงาน</p>
                <p class="text-sm text-gray-600 mt-0.5">456/78 อาคาร ABC ชั้น 12 ถ.พระราม 9</p>
                <p class="text-sm text-gray-600">เขตห้วยขวาง กทม. 10310</p>
                <p class="text-sm text-gray-600">Tel: 02-456-7890</p>
              </div>
            </label>
          </div>
        </div>

        <!-- Delivery Method -->
        <div class="bg-white rounded-2xl border p-5">
          <h3 class="font-extrabold text-lg mb-4">วิธีจัดส่ง</h3>
          <div class="space-y-3">
            <label class="flex items-center gap-3 p-3 border-2 border-indigo-500 bg-indigo-50 rounded-xl cursor-pointer">
              <input type="radio" name="ship" checked class="text-indigo-600">
              <div class="flex-1">
                <div class="flex items-center justify-between">
                  <p class="text-sm font-semibold">ส่งด่วน (1–2 วันทำการ)</p>
                  <p class="text-sm font-bold text-green-600">ฟรี</p>
                </div>
                <p class="text-xs text-gray-500 mt-0.5">รับของภายในวันที่ 20–21 มิ.ย.</p>
              </div>
            </label>
            <label class="flex items-center gap-3 p-3 border rounded-xl cursor-pointer hover:border-indigo-300 transition-colors">
              <input type="radio" name="ship" class="text-indigo-600">
              <div class="flex-1">
                <div class="flex items-center justify-between">
                  <p class="text-sm font-semibold">ส่งธรรมดา (3–5 วันทำการ)</p>
                  <p class="text-sm font-bold">฿0</p>
                </div>
                <p class="text-xs text-gray-500 mt-0.5">รับของภายในวันที่ 22–26 มิ.ย.</p>
              </div>
            </label>
          </div>
        </div>

        <!-- Payment Method -->
        <div class="bg-white rounded-2xl border p-5">
          <h3 class="font-extrabold text-lg mb-4">วิธีชำระเงิน</h3>
          <div class="space-y-3">
            <label class="flex items-center gap-3 p-3 border-2 border-indigo-500 bg-indigo-50 rounded-xl cursor-pointer">
              <input type="radio" name="pay" checked class="text-indigo-600">
              <div class="flex items-center gap-2">
                <div class="w-8 h-6 bg-gradient-to-r from-blue-600 to-blue-400 rounded text-white text-[10px] font-bold flex items-center justify-center">VISA</div>
                <p class="text-sm font-medium">•••• 4242</p>
              </div>
            </label>
            <label class="flex items-center gap-3 p-3 border rounded-xl cursor-pointer hover:border-indigo-300 transition-colors">
              <input type="radio" name="pay" class="text-indigo-600">
              <div class="flex items-center gap-2">
                <div class="w-8 h-6 bg-green-500 rounded text-white text-[10px] font-bold flex items-center justify-center">PP</div>
                <p class="text-sm font-medium">PromptPay / QR</p>
              </div>
            </label>
            <label class="flex items-center gap-3 p-3 border rounded-xl cursor-pointer hover:border-indigo-300 transition-colors">
              <input type="radio" name="pay" class="text-indigo-600">
              <div class="flex items-center gap-2">
                <div class="w-8 h-6 bg-gray-800 rounded text-white text-[10px] font-bold flex items-center justify-center">🍎</div>
                <p class="text-sm font-medium">Apple Pay</p>
              </div>
            </label>
          </div>
        </div>
      </div>

      <!-- Right: Order Summary -->
      <div class="space-y-4">
        <div class="bg-white rounded-2xl border p-5 sticky top-24">
          <h3 class="font-extrabold text-lg mb-4">สรุปคำสั่งซื้อ</h3>

          <div class="space-y-3 mb-4">
            <div class="flex gap-3">
              <div class="w-12 h-12 bg-gray-100 rounded-xl flex items-center justify-center text-xl flex-shrink-0">📱</div>
              <div class="flex-1 min-w-0">
                <p class="text-xs font-medium line-clamp-2">iPhone 15 Pro Max 256GB</p>
                <p class="text-xs text-gray-500 mt-0.5">1 ชิ้น</p>
              </div>
              <p class="text-sm font-bold flex-shrink-0">฿45,900</p>
            </div>
            <div class="flex gap-3">
              <div class="w-12 h-12 bg-gray-100 rounded-xl flex items-center justify-center text-xl flex-shrink-0">🎧</div>
              <div class="flex-1 min-w-0">
                <p class="text-xs font-medium line-clamp-2">AirPods Pro (2nd Gen)</p>
                <p class="text-xs text-gray-500 mt-0.5">1 ชิ้น</p>
              </div>
              <p class="text-sm font-bold flex-shrink-0">฿8,900</p>
            </div>
          </div>

          <!-- Coupon -->
          <div class="flex gap-2 mb-4">
            <input type="text" placeholder="รหัสโปรโมชัน" class="flex-1 border border-gray-300 rounded-xl px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
            <button class="bg-gray-800 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-gray-700 transition-colors">ใช้</button>
          </div>

          <div class="space-y-2 text-sm border-t pt-3">
            <div class="flex justify-between text-gray-600"><span>ราคาสินค้า</span><span>฿54,800</span></div>
            <div class="flex justify-between text-gray-600"><span>ส่วนลด (-15%)</span><span class="text-green-600">-฿9,275</span></div>
            <div class="flex justify-between text-gray-600"><span>ค่าจัดส่ง</span><span class="text-green-600">ฟรี</span></div>
            <div class="flex justify-between font-extrabold text-base border-t pt-2 mt-1">
              <span>รวมทั้งสิ้น</span>
              <span class="text-indigo-600">฿45,525</span>
            </div>
          </div>

          <button class="w-full mt-4 bg-indigo-600 text-white py-3.5 rounded-xl font-bold text-base hover:bg-indigo-700 transition-colors">
            ยืนยันคำสั่งซื้อ
          </button>
          <p class="text-center text-xs text-gray-400 mt-2">🔒 ข้อมูลของคุณปลอดภัย</p>
        </div>
      </div>
    </div>
  </div>

</body>
</html>
```

---

## Step 399–400: Order Confirmation & Full Workshop

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>E-Commerce Workshop - Steps 399-400</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 text-gray-900">

  <!-- App Shell -->
  <div id="app">

    <!-- Order Confirmation (Step 399) -->
    <div id="view-confirm" class="min-h-screen flex flex-col items-center justify-center p-6 text-center">
      <div class="bg-white rounded-2xl border shadow-sm p-8 max-w-md w-full">
        <div class="w-16 h-16 bg-green-100 rounded-full flex items-center justify-center mx-auto mb-4">
          <svg class="w-8 h-8 text-green-600" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"/></svg>
        </div>
        <h2 class="text-2xl font-extrabold mb-2">สั่งซื้อสำเร็จ! 🎉</h2>
        <p class="text-gray-500 text-sm mb-1">หมายเลขคำสั่งซื้อ</p>
        <p class="font-mono font-bold text-lg text-indigo-600">#TS-2024-06-18754</p>

        <div class="bg-gray-50 rounded-xl p-4 mt-5 text-left space-y-2 text-sm">
          <div class="flex justify-between"><span class="text-gray-500">สินค้า</span><span>2 รายการ</span></div>
          <div class="flex justify-between"><span class="text-gray-500">ยอดรวม</span><span class="font-bold">฿45,525</span></div>
          <div class="flex justify-between"><span class="text-gray-500">จัดส่งถึง</span><span>สมชาย เทคโน</span></div>
          <div class="flex justify-between"><span class="text-gray-500">คาดว่าจะได้รับ</span><span class="text-green-600 font-semibold">20–21 มิ.ย. 2024</span></div>
        </div>

        <div class="flex flex-col gap-3 mt-6">
          <button onclick="showTracking()" class="w-full bg-indigo-600 text-white py-3 rounded-xl font-semibold hover:bg-indigo-700 transition-colors">ติดตามพัสดุ</button>
          <button onclick="showCatalog()" class="w-full border border-gray-300 text-gray-700 py-3 rounded-xl font-semibold hover:bg-gray-50 transition-colors">ช้อปปิ้งต่อ</button>
        </div>
      </div>
    </div>

    <!-- Order Tracking (Step 400) -->
    <div id="view-tracking" class="hidden min-h-screen p-6">
      <div class="max-w-2xl mx-auto">
        <button onclick="document.getElementById('view-tracking').classList.add('hidden'); document.getElementById('view-confirm').classList.remove('hidden')" class="flex items-center gap-2 text-sm text-gray-500 hover:text-indigo-600 mb-6 transition-colors">
          ← กลับไปยืนยันคำสั่งซื้อ
        </button>

        <div class="bg-white rounded-2xl border p-6 mb-5">
          <div class="flex items-center justify-between mb-4">
            <div>
              <h3 class="font-extrabold text-lg">ติดตามพัสดุ</h3>
              <p class="text-sm text-gray-500 font-mono">#TS-2024-06-18754</p>
            </div>
            <span class="bg-blue-100 text-blue-700 text-sm font-bold px-3 py-1.5 rounded-full">กำลังจัดส่ง</span>
          </div>

          <!-- Tracking Timeline -->
          <div class="space-y-4 relative">
            <div class="absolute left-4 top-4 bottom-4 w-0.5 bg-gray-200"></div>

            <div class="flex gap-4 relative">
              <div class="w-8 h-8 bg-green-500 rounded-full flex items-center justify-center text-white text-xs flex-shrink-0 z-10">✓</div>
              <div>
                <p class="text-sm font-semibold">คำสั่งซื้อได้รับการยืนยัน</p>
                <p class="text-xs text-gray-500">18 มิ.ย. 2024 · 10:32 น.</p>
              </div>
            </div>
            <div class="flex gap-4 relative">
              <div class="w-8 h-8 bg-green-500 rounded-full flex items-center justify-center text-white text-xs flex-shrink-0 z-10">✓</div>
              <div>
                <p class="text-sm font-semibold">กำลังเตรียมสินค้า</p>
                <p class="text-xs text-gray-500">18 มิ.ย. 2024 · 14:15 น.</p>
              </div>
            </div>
            <div class="flex gap-4 relative">
              <div class="w-8 h-8 bg-blue-500 rounded-full flex items-center justify-center text-white text-xs flex-shrink-0 z-10 animate-pulse">📦</div>
              <div>
                <p class="text-sm font-semibold text-blue-700">อยู่ระหว่างการจัดส่ง</p>
                <p class="text-xs text-gray-500">19 มิ.ย. 2024 · 08:45 น.</p>
                <p class="text-xs text-blue-600 mt-1 font-medium">Kerry Express · KERRY6789012345</p>
              </div>
            </div>
            <div class="flex gap-4 relative opacity-40">
              <div class="w-8 h-8 bg-gray-200 rounded-full flex items-center justify-center text-gray-400 text-xs flex-shrink-0 z-10">🏠</div>
              <div>
                <p class="text-sm font-semibold text-gray-500">จัดส่งสำเร็จ</p>
                <p class="text-xs text-gray-400">คาดว่า 20–21 มิ.ย. 2024</p>
              </div>
            </div>
          </div>
        </div>

        <!-- Items in Order -->
        <div class="bg-white rounded-2xl border p-5">
          <h3 class="font-semibold mb-3">สินค้าในคำสั่งซื้อ</h3>
          <div class="space-y-3">
            <div class="flex items-center gap-3">
              <div class="w-12 h-12 bg-gray-100 rounded-xl flex items-center justify-center text-xl">📱</div>
              <div class="flex-1">
                <p class="text-sm font-medium">iPhone 15 Pro Max 256GB</p>
                <p class="text-xs text-gray-500">Natural Titanium · 1 ชิ้น</p>
              </div>
              <p class="text-sm font-bold">฿45,900</p>
            </div>
            <div class="flex items-center gap-3">
              <div class="w-12 h-12 bg-gray-100 rounded-xl flex items-center justify-center text-xl">🎧</div>
              <div class="flex-1">
                <p class="text-sm font-medium">AirPods Pro (2nd Gen)</p>
                <p class="text-xs text-gray-500">สีขาว · 1 ชิ้น</p>
              </div>
              <p class="text-sm font-bold">฿8,900</p>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Catalog View (simplified) -->
    <div id="view-catalog" class="hidden min-h-screen p-6">
      <div class="max-w-5xl mx-auto">
        <button onclick="showConfirm()" class="flex items-center gap-2 text-sm text-gray-500 hover:text-indigo-600 mb-4 transition-colors">
          ← กลับ
        </button>
        <h2 class="text-xl font-extrabold mb-5">สินค้าแนะนำ</h2>
        <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
          <div class="bg-white rounded-xl border p-4 text-center hover:shadow-md transition-shadow cursor-pointer">
            <div class="text-4xl mb-2">📱</div>
            <p class="text-sm font-semibold">iPhone 15</p>
            <p class="text-xs text-indigo-600 font-bold mt-1">฿35,900</p>
          </div>
          <div class="bg-white rounded-xl border p-4 text-center hover:shadow-md transition-shadow cursor-pointer">
            <div class="text-4xl mb-2">💻</div>
            <p class="text-sm font-semibold">MacBook Air M2</p>
            <p class="text-xs text-indigo-600 font-bold mt-1">฿45,900</p>
          </div>
          <div class="bg-white rounded-xl border p-4 text-center hover:shadow-md transition-shadow cursor-pointer">
            <div class="text-4xl mb-2">⌚</div>
            <p class="text-sm font-semibold">Apple Watch SE</p>
            <p class="text-xs text-indigo-600 font-bold mt-1">฿9,900</p>
          </div>
          <div class="bg-white rounded-xl border p-4 text-center hover:shadow-md transition-shadow cursor-pointer">
            <div class="text-4xl mb-2">📷</div>
            <p class="text-sm font-semibold">iPad Pro 11"</p>
            <p class="text-xs text-indigo-600 font-bold mt-1">฿29,900</p>
          </div>
        </div>
      </div>
    </div>

  </div>

  <script>
    function showTracking() {
      document.getElementById('view-confirm').classList.add('hidden');
      document.getElementById('view-tracking').classList.remove('hidden');
    }
    function showCatalog() {
      document.getElementById('view-confirm').classList.add('hidden');
      document.getElementById('view-catalog').classList.remove('hidden');
    }
    function showConfirm() {
      document.getElementById('view-catalog').classList.add('hidden');
      document.getElementById('view-tracking').classList.add('hidden');
      document.getElementById('view-confirm').classList.remove('hidden');
    }
  </script>

</body>
</html>
```

---

## สรุป Part 40

| Step | เนื้อหา |
|------|---------|
| 391 | Store Header: sticky nav, search, category nav, cart drawer |
| 392 | Product Catalog: filter sidebar, grid, sort, badges, pagination |
| 393 | Product Detail: gallery, variants, quantity, delivery info |
| 394–396 | Checkout: address picker, delivery method, payment method |
| 397–398 | Order Summary: coupon input, price breakdown |
| 399 | Order Confirmation: success state, summary card |
| 400 | Order Tracking: timeline UI, items in order |

**Part ถัดไป:** Part 41 — Level 4: React + Tailwind Integration (Steps 401–410)

---

## จบ Level 3 (Parts 31–40, Steps 301–400)

ขอแสดงความยินดี! คุณผ่าน Level 3 ครบทั้ง 10 Parts แล้ว:
- Parts 31–35: Core Advanced (Typography, Colors, Grid, Animation, E-Commerce)
- Part 36: Blog Layout System
- Part 37: SaaS Landing Page
- Part 38: Authentication Pages
- Part 39: Admin Settings
- Part 40: E-Commerce Project ✅

**Level 4 ถัดไป:** Framework Integration + Design Systems (Parts 41–60)
