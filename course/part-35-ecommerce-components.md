# Part 35: E-Commerce Components
## Steps 341–350: Component สำหรับร้านค้าออนไลน์

---

## 🎯 เป้าหมายของ Part นี้

- Product card (multiple styles)
- Shopping cart sidebar
- Product gallery (image zoom)
- Filter sidebar
- Breadcrumbs e-commerce
- Review & Rating
- Checkout summary

---

## Step 341: Product Card Variants

```html
<!-- Basic product card -->
<div class="bg-white rounded-2xl border border-gray-200 overflow-hidden hover:shadow-xl hover:-translate-y-1 transition-all duration-300 group w-72">
  <!-- Image -->
  <div class="relative overflow-hidden aspect-square bg-gray-50">
    <div class="w-full h-full bg-gradient-to-br from-indigo-100 to-purple-100 group-hover:scale-105 transition-transform duration-500 flex items-center justify-center text-6xl">
      👟
    </div>
    <!-- Badges -->
    <div class="absolute top-3 left-3 flex gap-2">
      <span class="bg-red-500 text-white text-[11px] font-bold px-2.5 py-1 rounded-full">-30%</span>
      <span class="bg-gray-900 text-white text-[11px] font-bold px-2.5 py-1 rounded-full">ใหม่</span>
    </div>
    <!-- Quick actions -->
    <div class="absolute top-3 right-3 flex flex-col gap-2 opacity-0 group-hover:opacity-100 transition-opacity duration-200">
      <button class="size-9 bg-white rounded-xl shadow-md flex items-center justify-center hover:bg-indigo-50 transition-colors text-sm">♡</button>
      <button class="size-9 bg-white rounded-xl shadow-md flex items-center justify-center hover:bg-indigo-50 transition-colors text-sm">🔍</button>
    </div>
    <!-- Quick add (bottom) -->
    <div class="absolute bottom-0 left-0 right-0 translate-y-full group-hover:translate-y-0 transition-transform duration-300">
      <button class="w-full py-3 bg-gray-900 text-white text-sm font-bold hover:bg-indigo-600 transition-colors">
        เพิ่มในตะกร้า
      </button>
    </div>
  </div>
  
  <!-- Info -->
  <div class="p-4">
    <p class="text-xs text-gray-400 mb-1">Nike</p>
    <h3 class="font-bold text-gray-900 text-sm">Air Max 270 React</h3>
    <!-- Stars -->
    <div class="flex items-center gap-1.5 mt-1">
      <div class="flex text-yellow-400 text-xs">★★★★★</div>
      <span class="text-xs text-gray-400">(128)</span>
    </div>
    <!-- Price -->
    <div class="flex items-center gap-2 mt-2">
      <span class="text-lg font-black text-gray-900">฿3,290</span>
      <span class="text-sm text-gray-400 line-through">฿4,700</span>
    </div>
    <!-- Color swatches -->
    <div class="flex gap-2 mt-3">
      <button class="size-5 rounded-full bg-gray-900 ring-2 ring-offset-1 ring-gray-900"></button>
      <button class="size-5 rounded-full bg-indigo-600 ring-offset-1 hover:ring-2 hover:ring-indigo-400"></button>
      <button class="size-5 rounded-full bg-red-500 ring-offset-1 hover:ring-2 hover:ring-red-400"></button>
      <button class="size-5 rounded-full bg-amber-400 ring-offset-1 hover:ring-2 hover:ring-amber-400"></button>
    </div>
  </div>
</div>
```

---

## Step 342: Cart Sidebar

```html
<!-- Cart drawer (slide from right) -->
<div id="cart-sidebar" class="fixed inset-y-0 right-0 z-50 w-full sm:w-96 bg-white shadow-2xl flex flex-col translate-x-full transition-transform duration-300">
  <!-- Header -->
  <div class="flex items-center justify-between p-5 border-b border-gray-100">
    <div class="flex items-center gap-2">
      <h3 class="font-bold text-lg text-gray-900">ตะกร้าสินค้า</h3>
      <span class="size-5 bg-indigo-600 text-white text-[11px] font-bold rounded-full flex items-center justify-center">3</span>
    </div>
    <button onclick="closeCart()" class="size-9 flex items-center justify-center rounded-xl hover:bg-gray-100 text-gray-400 hover:text-gray-700">✕</button>
  </div>
  
  <!-- Items -->
  <div class="flex-1 overflow-y-auto p-5 space-y-4">
    <!-- Cart Item -->
    <div class="flex gap-3">
      <div class="size-20 rounded-xl bg-gray-100 flex items-center justify-center text-3xl flex-none">👟</div>
      <div class="flex-1 min-w-0">
        <p class="font-semibold text-sm text-gray-900 truncate">Nike Air Max 270</p>
        <p class="text-xs text-gray-400 mt-0.5">Size: 42 • Black</p>
        <!-- Quantity -->
        <div class="flex items-center gap-2 mt-2">
          <button class="size-6 rounded-lg border border-gray-200 flex items-center justify-center text-gray-500 hover:bg-gray-100 text-xs">−</button>
          <span class="text-sm font-medium w-6 text-center">1</span>
          <button class="size-6 rounded-lg border border-gray-200 flex items-center justify-center text-gray-500 hover:bg-gray-100 text-xs">+</button>
        </div>
      </div>
      <div class="text-right flex flex-col justify-between">
        <button class="text-gray-300 hover:text-red-500 transition-colors text-sm">✕</button>
        <p class="font-bold text-gray-900 text-sm">฿3,290</p>
      </div>
    </div>
    
    <div class="flex gap-3">
      <div class="size-20 rounded-xl bg-gray-100 flex items-center justify-center text-3xl flex-none">👕</div>
      <div class="flex-1 min-w-0">
        <p class="font-semibold text-sm text-gray-900 truncate">Uniqlo Ultra Light Down</p>
        <p class="text-xs text-gray-400 mt-0.5">Size: L • Navy</p>
        <div class="flex items-center gap-2 mt-2">
          <button class="size-6 rounded-lg border border-gray-200 flex items-center justify-center text-gray-500 hover:bg-gray-100 text-xs">−</button>
          <span class="text-sm font-medium w-6 text-center">2</span>
          <button class="size-6 rounded-lg border border-gray-200 flex items-center justify-center text-gray-500 hover:bg-gray-100 text-xs">+</button>
        </div>
      </div>
      <div class="text-right flex flex-col justify-between">
        <button class="text-gray-300 hover:text-red-500 text-sm">✕</button>
        <p class="font-bold text-gray-900 text-sm">฿2,990</p>
      </div>
    </div>
  </div>
  
  <!-- Footer / Summary -->
  <div class="border-t border-gray-100 p-5 space-y-3">
    <!-- Promo code -->
    <div class="flex gap-2">
      <input placeholder="โปรโมโค้ด" class="flex-1 px-3 py-2 text-sm border border-gray-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-indigo-200">
      <button class="px-4 py-2 bg-gray-900 text-white text-sm font-semibold rounded-xl hover:bg-gray-700 transition-colors">ใช้</button>
    </div>
    <!-- Order summary -->
    <div class="space-y-1.5 text-sm">
      <div class="flex justify-between text-gray-500"><span>ยอดรวม</span><span>฿9,270</span></div>
      <div class="flex justify-between text-green-600"><span>ส่วนลด</span><span>-฿500</span></div>
      <div class="flex justify-between text-gray-500"><span>ค่าจัดส่ง</span><span>ฟรี</span></div>
      <div class="flex justify-between font-bold text-gray-900 text-base pt-2 border-t border-gray-100">
        <span>ยอดชำระ</span><span>฿8,770</span>
      </div>
    </div>
    <button class="w-full py-3.5 bg-indigo-600 text-white font-bold rounded-2xl hover:bg-indigo-700 transition-colors shadow-lg shadow-indigo-500/30">
      ดำเนินการสั่งซื้อ →
    </button>
    <p class="text-center text-xs text-gray-400">🔒 ชำระเงินปลอดภัย 100%</p>
  </div>
</div>

<script>
function openCart() {
  document.getElementById('cart-sidebar').style.transform = 'translateX(0)';
  document.body.style.overflow = 'hidden';
}
function closeCart() {
  document.getElementById('cart-sidebar').style.transform = 'translateX(100%)';
  document.body.style.overflow = '';
}
</script>
```

---

## Step 343: Star Rating Component

```html
<!-- Star rating (interactive) -->
<div id="rating-container" class="flex gap-1">
  <button onclick="setRating(1)" class="star text-3xl text-gray-200 hover:text-yellow-400 transition-colors cursor-pointer" data-star="1">★</button>
  <button onclick="setRating(2)" class="star text-3xl text-gray-200 hover:text-yellow-400 transition-colors cursor-pointer" data-star="2">★</button>
  <button onclick="setRating(3)" class="star text-3xl text-gray-200 hover:text-yellow-400 transition-colors cursor-pointer" data-star="3">★</button>
  <button onclick="setRating(4)" class="star text-3xl text-gray-200 hover:text-yellow-400 transition-colors cursor-pointer" data-star="4">★</button>
  <button onclick="setRating(5)" class="star text-3xl text-gray-200 hover:text-yellow-400 transition-colors cursor-pointer" data-star="5">★</button>
</div>
<p id="rating-text" class="text-sm text-gray-500 mt-2">คลิกเพื่อให้คะแนน</p>

<script>
let currentRating = 0;
const labels = ['', 'แย่', 'พอใช้ได้', 'ดี', 'ดีมาก', 'ยอดเยี่ยม!'];

function setRating(n) {
  currentRating = n;
  document.querySelectorAll('.star').forEach((s, i) => {
    s.classList.toggle('text-yellow-400', i < n);
    s.classList.toggle('text-gray-200', i >= n);
  });
  document.getElementById('rating-text').textContent = `${n}/5 — ${labels[n]}`;
}

// Hover preview
document.querySelectorAll('.star').forEach((s, i) => {
  s.addEventListener('mouseenter', () => {
    document.querySelectorAll('.star').forEach((st, j) => {
      st.classList.toggle('text-yellow-300', j <= i);
      st.classList.toggle('text-gray-200', j > i);
    });
  });
  s.addEventListener('mouseleave', () => {
    setRating(currentRating);
  });
});
</script>

<!-- Display-only star rating -->
<div class="flex items-center gap-2">
  <div class="flex text-yellow-400 text-sm">
    <!-- 4.5 stars: 4 full + 1 half -->
    <span>★</span><span>★</span><span>★</span><span>★</span>
    <span class="relative">
      <span class="text-gray-200">★</span>
      <span class="absolute inset-0 overflow-hidden w-1/2 text-yellow-400">★</span>
    </span>
  </div>
  <span class="text-sm font-semibold text-gray-900">4.5</span>
  <span class="text-sm text-gray-400">(1,284 รีวิว)</span>
</div>
```

---

## Step 344: Product Filter Sidebar

```html
<!-- Filter sidebar with sections -->
<aside class="w-64 bg-white rounded-2xl border border-gray-200 p-5 space-y-6">
  <!-- Header -->
  <div class="flex items-center justify-between">
    <h3 class="font-bold text-gray-900">ตัวกรอง</h3>
    <button class="text-xs text-indigo-600 font-medium hover:text-indigo-800">ล้างทั้งหมด</button>
  </div>
  
  <!-- Category -->
  <div>
    <h4 class="text-xs font-bold text-gray-400 uppercase tracking-wider mb-3">หมวดหมู่</h4>
    <div class="space-y-2">
      <label class="flex items-center gap-3 cursor-pointer">
        <input type="checkbox" checked class="rounded border-gray-300 text-indigo-600">
        <span class="text-sm text-gray-700">รองเท้า</span>
        <span class="ml-auto text-xs text-gray-400 bg-gray-100 px-1.5 py-0.5 rounded-full">48</span>
      </label>
      <label class="flex items-center gap-3 cursor-pointer">
        <input type="checkbox" class="rounded border-gray-300 text-indigo-600">
        <span class="text-sm text-gray-700">เสื้อผ้า</span>
        <span class="ml-auto text-xs text-gray-400 bg-gray-100 px-1.5 py-0.5 rounded-full">124</span>
      </label>
      <label class="flex items-center gap-3 cursor-pointer">
        <input type="checkbox" class="rounded border-gray-300 text-indigo-600">
        <span class="text-sm text-gray-700">กระเป๋า</span>
        <span class="ml-auto text-xs text-gray-400 bg-gray-100 px-1.5 py-0.5 rounded-full">32</span>
      </label>
    </div>
  </div>
  
  <!-- Price range -->
  <div>
    <h4 class="text-xs font-bold text-gray-400 uppercase tracking-wider mb-3">ช่วงราคา</h4>
    <div class="space-y-3">
      <input type="range" min="0" max="10000" value="5000" oninput="document.getElementById('price-val').textContent = `฿${this.value.toLocaleString()}`" class="w-full accent-indigo-600">
      <div class="flex justify-between text-xs text-gray-500">
        <span>฿0</span>
        <span id="price-val" class="font-semibold text-indigo-600">฿5,000</span>
        <span>฿10,000</span>
      </div>
    </div>
  </div>
  
  <!-- Color filter -->
  <div>
    <h4 class="text-xs font-bold text-gray-400 uppercase tracking-wider mb-3">สี</h4>
    <div class="flex flex-wrap gap-2">
      <button class="size-8 rounded-full bg-black ring-2 ring-offset-1 ring-black" title="Black"></button>
      <button class="size-8 rounded-full bg-white border border-gray-300 ring-offset-1 hover:ring-2 hover:ring-gray-300" title="White"></button>
      <button class="size-8 rounded-full bg-red-500 ring-offset-1 hover:ring-2 hover:ring-red-400" title="Red"></button>
      <button class="size-8 rounded-full bg-blue-600 ring-offset-1 hover:ring-2 hover:ring-blue-400" title="Blue"></button>
      <button class="size-8 rounded-full bg-green-600 ring-offset-1 hover:ring-2 hover:ring-green-400" title="Green"></button>
      <button class="size-8 rounded-full bg-yellow-400 ring-offset-1 hover:ring-2 hover:ring-yellow-400" title="Yellow"></button>
    </div>
  </div>
  
  <!-- Rating filter -->
  <div>
    <h4 class="text-xs font-bold text-gray-400 uppercase tracking-wider mb-3">คะแนน</h4>
    <div class="space-y-1.5">
      <label class="flex items-center gap-2 cursor-pointer"><input type="radio" name="rating" class="text-indigo-600"><span class="text-yellow-400 text-sm">★★★★★</span><span class="text-xs text-gray-400 ml-1">(5.0)</span></label>
      <label class="flex items-center gap-2 cursor-pointer"><input type="radio" name="rating" class="text-indigo-600"><span class="text-yellow-400 text-sm">★★★★</span><span class="text-yellow-200 text-sm">★</span><span class="text-xs text-gray-400 ml-1">(4+)</span></label>
      <label class="flex items-center gap-2 cursor-pointer"><input type="radio" name="rating" class="text-indigo-600"><span class="text-yellow-400 text-sm">★★★</span><span class="text-yellow-200 text-sm">★★</span><span class="text-xs text-gray-400 ml-1">(3+)</span></label>
    </div>
  </div>
  
  <button class="w-full py-2.5 bg-indigo-600 text-white text-sm font-semibold rounded-xl hover:bg-indigo-700 transition-colors">
    ดูผลลัพธ์
  </button>
</aside>
```

---

## Step 345–350: Workshop — E-Commerce Product Page

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Product Page Workshop</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50">

<!-- Cart overlay -->
<div id="cart-bg" onclick="closeCart()" class="hidden fixed inset-0 bg-black/40 z-40 backdrop-blur-sm"></div>
<div id="cart" class="fixed inset-y-0 right-0 z-50 w-full sm:w-96 bg-white shadow-2xl flex flex-col translate-x-full transition-transform duration-300">
  <div class="flex items-center justify-between p-5 border-b border-gray-100">
    <h3 class="font-bold text-gray-900">ตะกร้าสินค้า <span class="ml-1 text-xs bg-indigo-600 text-white px-1.5 py-0.5 rounded-full" id="cart-count">0</span></h3>
    <button onclick="closeCart()" class="text-gray-400 hover:text-gray-700 text-lg">✕</button>
  </div>
  <div id="cart-items" class="flex-1 overflow-y-auto p-5">
    <p class="text-center text-gray-400 text-sm py-12">ตะกร้าว่างเปล่า</p>
  </div>
  <div class="p-5 border-t border-gray-100">
    <div class="flex justify-between font-bold text-gray-900 mb-4"><span>ยอดรวม</span><span id="cart-total">฿0</span></div>
    <button class="w-full py-3.5 bg-indigo-600 text-white font-bold rounded-2xl hover:bg-indigo-700 transition-colors">ดำเนินการสั่งซื้อ →</button>
  </div>
</div>

<!-- Header -->
<header class="bg-white border-b border-gray-200 sticky top-0 z-30">
  <div class="max-w-6xl mx-auto px-4 h-16 flex items-center justify-between">
    <span class="font-black text-gray-900 text-lg">ShopTW</span>
    <button onclick="openCart()" class="relative size-10 flex items-center justify-center rounded-xl hover:bg-gray-100 text-gray-600 transition-colors">
      🛒
      <span id="badge" class="hidden absolute -top-0.5 -right-0.5 size-4 bg-red-500 text-white text-[9px] font-bold rounded-full flex items-center justify-center border border-white" id="cart-badge">0</span>
    </button>
  </div>
</header>

<!-- Breadcrumb -->
<div class="max-w-6xl mx-auto px-4 py-3">
  <nav class="flex items-center gap-2 text-xs text-gray-400">
    <a href="#" class="hover:text-gray-700">หน้าแรก</a>
    <span>›</span>
    <a href="#" class="hover:text-gray-700">รองเท้า</a>
    <span>›</span>
    <span class="text-gray-900">Nike Air Max 270</span>
  </nav>
</div>

<!-- Product detail -->
<div class="max-w-6xl mx-auto px-4 pb-12">
  <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
    
    <!-- Images -->
    <div class="space-y-3">
      <div class="aspect-square bg-gray-100 rounded-2xl flex items-center justify-center text-8xl overflow-hidden">
        <span id="main-img">👟</span>
      </div>
      <div class="grid grid-cols-4 gap-2">
        <button onclick="setImg('👟')" class="aspect-square bg-gray-100 rounded-xl flex items-center justify-center text-2xl hover:ring-2 hover:ring-indigo-400 transition-all ring-2 ring-indigo-400">👟</button>
        <button onclick="setImg('🥾')" class="aspect-square bg-gray-100 rounded-xl flex items-center justify-center text-2xl hover:ring-2 hover:ring-indigo-400 transition-all">🥾</button>
        <button onclick="setImg('👞')" class="aspect-square bg-gray-100 rounded-xl flex items-center justify-center text-2xl hover:ring-2 hover:ring-indigo-400 transition-all">👞</button>
        <button onclick="setImg('🩴')" class="aspect-square bg-gray-100 rounded-xl flex items-center justify-center text-2xl hover:ring-2 hover:ring-indigo-400 transition-all">🩴</button>
      </div>
    </div>
    
    <!-- Product info -->
    <div class="space-y-5">
      <div>
        <p class="text-sm text-indigo-600 font-semibold">Nike</p>
        <h1 class="text-3xl font-black text-gray-900 mt-1">Air Max 270 React</h1>
        <div class="flex items-center gap-2 mt-2">
          <div class="flex text-yellow-400">★★★★★</div>
          <span class="text-sm text-gray-400">(128 รีวิว)</span>
        </div>
      </div>
      
      <div class="flex items-baseline gap-3">
        <span class="text-3xl font-black text-gray-900">฿3,290</span>
        <span class="text-lg text-gray-400 line-through">฿4,700</span>
        <span class="bg-red-100 text-red-600 text-sm font-bold px-2.5 py-1 rounded-full">-30%</span>
      </div>
      
      <!-- Color -->
      <div>
        <p class="text-sm font-semibold text-gray-700 mb-2">สี: <span id="selected-color" class="text-gray-500">Black</span></p>
        <div class="flex gap-3">
          <button onclick="selectColor(this,'Black')" data-color-active class="size-8 rounded-full bg-gray-900 ring-2 ring-offset-2 ring-gray-900"></button>
          <button onclick="selectColor(this,'Indigo')" class="size-8 rounded-full bg-indigo-600 ring-offset-2 hover:ring-2 hover:ring-indigo-400 transition-all"></button>
          <button onclick="selectColor(this,'Red')" class="size-8 rounded-full bg-red-500 ring-offset-2 hover:ring-2 hover:ring-red-400 transition-all"></button>
        </div>
      </div>
      
      <!-- Size -->
      <div>
        <p class="text-sm font-semibold text-gray-700 mb-2">ไซส์</p>
        <div class="flex flex-wrap gap-2" id="size-grid">
          <button onclick="selectSize(this)" class="size-btn w-12 h-12 rounded-xl border border-gray-200 text-sm font-medium text-gray-700 hover:border-indigo-400 hover:text-indigo-600 transition-all">39</button>
          <button onclick="selectSize(this)" class="size-btn w-12 h-12 rounded-xl border border-gray-200 text-sm font-medium text-gray-700 hover:border-indigo-400 hover:text-indigo-600 transition-all">40</button>
          <button onclick="selectSize(this)" class="size-btn w-12 h-12 rounded-xl border-2 border-indigo-600 bg-indigo-50 text-sm font-bold text-indigo-600">41</button>
          <button onclick="selectSize(this)" class="size-btn w-12 h-12 rounded-xl border border-gray-200 text-sm font-medium text-gray-700 hover:border-indigo-400 hover:text-indigo-600 transition-all">42</button>
          <button onclick="selectSize(this)" class="size-btn w-12 h-12 rounded-xl border border-gray-200 text-sm font-medium text-gray-700 hover:border-indigo-400 hover:text-indigo-600 transition-all">43</button>
          <button class="size-btn w-12 h-12 rounded-xl border border-gray-100 text-sm font-medium text-gray-300 cursor-not-allowed" disabled>44</button>
        </div>
      </div>
      
      <!-- Add to cart -->
      <div class="flex gap-3">
        <div class="flex items-center border border-gray-200 rounded-xl overflow-hidden">
          <button class="px-4 py-3 hover:bg-gray-100 transition-colors text-gray-500" onclick="changeQty(-1)">−</button>
          <span id="qty" class="px-4 text-sm font-medium w-10 text-center">1</span>
          <button class="px-4 py-3 hover:bg-gray-100 transition-colors text-gray-500" onclick="changeQty(1)">+</button>
        </div>
        <button onclick="addToCart()" class="flex-1 py-3 bg-indigo-600 text-white font-bold rounded-xl hover:bg-indigo-700 active:scale-95 transition-all shadow-lg shadow-indigo-500/30">
          🛒 เพิ่มในตะกร้า
        </button>
        <button class="size-12 border border-gray-200 rounded-xl flex items-center justify-center text-gray-400 hover:text-red-500 hover:border-red-200 transition-all text-lg">♡</button>
      </div>
      
      <!-- Shipping info -->
      <div class="bg-gray-50 rounded-2xl p-4 space-y-2 text-sm">
        <div class="flex items-center gap-2 text-gray-600"><span>🚚</span> จัดส่งฟรีเมื่อซื้อครบ ฿1,000</div>
        <div class="flex items-center gap-2 text-gray-600"><span>🔄</span> คืนสินค้าได้ภายใน 30 วัน</div>
        <div class="flex items-center gap-2 text-gray-600"><span>🔒</span> ชำระเงินปลอดภัย 100%</div>
      </div>
    </div>
  </div>
</div>

<script>
let qty = 1, cartItems = [], selectedSize = '41';

function setImg(e) { document.getElementById('main-img').textContent = e; }

function selectColor(btn, name) {
  document.querySelectorAll('[data-color-active]').forEach(b => {
    delete b.dataset.colorActive;
    b.className = b.className.replace(/ring-2\s+ring-offset-2\s+ring-\w+-\d+/g,'') + ' ring-offset-2 hover:ring-2';
  });
  btn.dataset.colorActive = '';
  document.getElementById('selected-color').textContent = name;
}

function selectSize(btn) {
  document.querySelectorAll('.size-btn').forEach(b => {
    if (!b.disabled) b.className = 'size-btn w-12 h-12 rounded-xl border border-gray-200 text-sm font-medium text-gray-700 hover:border-indigo-400 hover:text-indigo-600 transition-all';
  });
  btn.className = 'size-btn w-12 h-12 rounded-xl border-2 border-indigo-600 bg-indigo-50 text-sm font-bold text-indigo-600';
  selectedSize = btn.textContent.trim();
}

function changeQty(d) {
  qty = Math.max(1, qty + d);
  document.getElementById('qty').textContent = qty;
}

function addToCart() {
  const item = { name: 'Nike Air Max 270', price: 3290, qty, emoji: '👟', size: selectedSize };
  cartItems.push(item);
  renderCart();
  openCart();
}

function renderCart() {
  const total = cartItems.reduce((s, i) => s + i.price * i.qty, 0);
  document.getElementById('cart-count').textContent = cartItems.length;
  document.getElementById('cart-total').textContent = `฿${total.toLocaleString()}`;
  const badge = document.getElementById('badge');
  badge.textContent = cartItems.length;
  badge.classList.toggle('hidden', cartItems.length === 0);
  document.getElementById('cart-items').innerHTML = cartItems.length === 0
    ? '<p class="text-center text-gray-400 text-sm py-12">ตะกร้าว่างเปล่า</p>'
    : cartItems.map((item,i) => `
        <div class="flex gap-3 mb-4">
          <div class="size-16 rounded-xl bg-gray-100 flex items-center justify-center text-2xl flex-none">${item.emoji}</div>
          <div class="flex-1"><p class="font-semibold text-sm text-gray-900">${item.name}</p><p class="text-xs text-gray-400">Size: ${item.size}</p></div>
          <div class="text-right"><p class="font-bold text-sm text-gray-900">฿${(item.price * item.qty).toLocaleString()}</p></div>
        </div>
      `).join('');
}

function openCart() { document.getElementById('cart').style.transform='translateX(0)'; document.getElementById('cart-bg').classList.remove('hidden'); document.body.style.overflow='hidden'; }
function closeCart() { document.getElementById('cart').style.transform='translateX(100%)'; document.getElementById('cart-bg').classList.add('hidden'); document.body.style.overflow=''; }
</script>

</body>
</html>
```

---

## 📝 สรุป Part 35

| Component | เทคนิค |
|-----------|---------|
| Product card hover | `group` + `group-hover:` |
| Cart sidebar | `translate-x-full` → `translateX(0)` |
| Star rating | dynamic class + hover preview |
| Price display | `line-through` + discount badge |
| Color swatches | `ring-2 ring-offset-2` |
| Size picker | active state with border/bg |

---

*Part 35 — E-Commerce | Steps 341–350 จาก 1,000 Steps*
