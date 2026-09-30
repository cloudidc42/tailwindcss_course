# Part 67: E-Commerce Components

## เป้าหมาย
- Product card, gallery, quick-view
- Shopping cart sidebar
- Checkout form + order summary
- Product filter + sort
- Steps 661–670

---

## Steps 661–670: Complete E-Commerce Workshop

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>E-Commerce - Steps 661-670</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50">

  <!-- Cart sidebar overlay -->
  <div id="cartOverlay" class="hidden fixed inset-0 bg-black/50 z-50" onclick="closeCart()"></div>
  <aside id="cartSidebar" class="fixed right-0 top-0 h-full w-96 bg-white shadow-2xl z-50 flex flex-col translate-x-full transition-transform duration-300">
    <div class="px-5 py-4 border-b flex items-center justify-between">
      <h2 class="font-extrabold text-gray-900">Cart (<span id="cartCount">0</span>)</h2>
      <button onclick="closeCart()" class="w-8 h-8 flex items-center justify-center rounded-xl hover:bg-gray-100 transition-colors text-gray-500">✕</button>
    </div>
    <div id="cartItems" class="flex-1 overflow-y-auto p-5 space-y-4"></div>
    <div class="p-5 border-t bg-gray-50">
      <div class="flex justify-between text-sm font-semibold mb-3">
        <span>Subtotal</span>
        <span id="cartTotal" class="text-indigo-600">฿0</span>
      </div>
      <button class="w-full bg-indigo-600 text-white py-3 rounded-xl font-semibold hover:bg-indigo-700 transition-colors">ดำเนินการชำระเงิน</button>
      <button onclick="closeCart()" class="w-full mt-2 text-sm text-gray-500 hover:text-gray-700 transition-colors">ช้อปต่อ</button>
    </div>
  </aside>

  <!-- Topbar -->
  <header class="bg-white border-b sticky top-0 z-40">
    <div class="max-w-6xl mx-auto px-6 h-14 flex items-center justify-between">
      <h1 class="font-extrabold text-indigo-600 text-lg">ShopTH</h1>
      <div class="flex items-center gap-3">
        <div class="relative hidden sm:block">
          <svg class="absolute left-3 top-1/2 -translate-y-1/2 w-3.5 h-3.5 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"/></svg>
          <input id="searchInput" placeholder="ค้นหาสินค้า..." oninput="filterProducts()" class="bg-gray-100 border-0 rounded-xl pl-8 pr-4 py-1.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-400 w-48">
        </div>
        <button onclick="openCart()" class="relative w-9 h-9 flex items-center justify-center rounded-xl hover:bg-gray-100 transition-colors">
          🛒
          <span id="cartBadge" class="hidden absolute top-0.5 right-0.5 w-4 h-4 bg-red-500 text-white text-[9px] font-bold rounded-full flex items-center justify-center">0</span>
        </button>
      </div>
    </div>
  </header>

  <div class="max-w-6xl mx-auto px-6 py-8">

    <!-- Category nav -->
    <div class="flex gap-2 mb-6 overflow-x-auto pb-2 scrollbar-hide">
      <button onclick="setCategory('all')" id="cat-all" class="cat-btn flex-shrink-0 px-4 py-2 bg-indigo-600 text-white text-sm font-semibold rounded-xl">ทั้งหมด</button>
      <button onclick="setCategory('electronics')" id="cat-electronics" class="cat-btn flex-shrink-0 px-4 py-2 bg-white border text-gray-600 text-sm font-semibold rounded-xl hover:bg-gray-50">📱 อิเล็กทรอนิกส์</button>
      <button onclick="setCategory('fashion')" id="cat-fashion" class="cat-btn flex-shrink-0 px-4 py-2 bg-white border text-gray-600 text-sm font-semibold rounded-xl hover:bg-gray-50">👗 แฟชั่น</button>
      <button onclick="setCategory('home')" id="cat-home" class="cat-btn flex-shrink-0 px-4 py-2 bg-white border text-gray-600 text-sm font-semibold rounded-xl hover:bg-gray-50">🏠 บ้านและสวน</button>
      <button onclick="setCategory('sports')" id="cat-sports" class="cat-btn flex-shrink-0 px-4 py-2 bg-white border text-gray-600 text-sm font-semibold rounded-xl hover:bg-gray-50">⚽ กีฬา</button>
    </div>

    <!-- Filter + Sort bar -->
    <div class="flex items-center justify-between mb-6">
      <div class="flex items-center gap-3">
        <span id="productCount" class="text-sm text-gray-500">แสดง 8 สินค้า</span>
        <div class="flex items-center gap-1.5">
          <span class="text-xs text-gray-400">ราคา:</span>
          <button onclick="setPriceRange('all')" id="pr-all" class="text-xs px-2.5 py-1 bg-indigo-600 text-white rounded-lg font-semibold">ทั้งหมด</button>
          <button onclick="setPriceRange('low')" id="pr-low" class="text-xs px-2.5 py-1 bg-white border text-gray-500 rounded-lg hover:bg-gray-50">< ฿1000</button>
          <button onclick="setPriceRange('mid')" id="pr-mid" class="text-xs px-2.5 py-1 bg-white border text-gray-500 rounded-lg hover:bg-gray-50">฿1000–5000</button>
          <button onclick="setPriceRange('high')" id="pr-high" class="text-xs px-2.5 py-1 bg-white border text-gray-500 rounded-lg hover:bg-gray-50">> ฿5000</button>
        </div>
      </div>
      <select id="sortSelect" onchange="sortProducts()" class="text-sm border border-gray-300 rounded-xl px-3 py-1.5 focus:outline-none focus:ring-2 focus:ring-indigo-400">
        <option value="default">เรียงตาม: แนะนำ</option>
        <option value="price-asc">ราคา: น้อยไปมาก</option>
        <option value="price-desc">ราคา: มากไปน้อย</option>
        <option value="rating">คะแนน</option>
        <option value="name">ชื่อ</option>
      </select>
    </div>

    <!-- Product Grid -->
    <div id="productGrid" class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-5"></div>

    <!-- Empty state -->
    <div id="emptyState" class="hidden text-center py-16 text-gray-400">
      <p class="text-4xl mb-3">🔍</p>
      <p class="font-semibold text-gray-500">ไม่พบสินค้าที่ค้นหา</p>
      <button onclick="resetFilters()" class="mt-3 text-sm text-indigo-600 hover:underline">ล้างตัวกรอง</button>
    </div>

  </div>

  <!-- Quick View Modal -->
  <div id="quickViewOverlay" class="hidden fixed inset-0 bg-black/50 z-50 flex items-center justify-center p-4" onclick="closeQuickView()">
    <div id="quickViewModal" class="bg-white rounded-2xl shadow-2xl w-full max-w-2xl max-h-[90vh] overflow-y-auto" onclick="event.stopPropagation()">
      <div class="p-6">
        <div class="flex justify-between mb-4">
          <h3 id="qvTitle" class="font-extrabold text-xl text-gray-900"></h3>
          <button onclick="closeQuickView()" class="text-gray-400 hover:text-gray-600">✕</button>
        </div>
        <div class="flex flex-col sm:flex-row gap-5">
          <div id="qvImg" class="w-full sm:w-48 h-48 rounded-2xl flex items-center justify-center text-6xl flex-shrink-0"></div>
          <div class="flex-1">
            <div class="flex items-center gap-2 mb-2">
              <span id="qvRating" class="text-yellow-400 text-sm"></span>
              <span id="qvRatingNum" class="text-xs text-gray-500"></span>
            </div>
            <p id="qvDesc" class="text-sm text-gray-600 mb-3"></p>
            <div class="flex items-center gap-3 mb-4">
              <span id="qvPrice" class="text-2xl font-extrabold text-indigo-700"></span>
              <span id="qvOrigPrice" class="text-sm text-gray-400 line-through"></span>
              <span id="qvDiscount" class="text-xs font-semibold bg-red-100 text-red-600 px-2 py-0.5 rounded-full"></span>
            </div>
            <div class="flex gap-3">
              <button id="qvAddCart" class="flex-1 bg-indigo-600 text-white py-2.5 rounded-xl font-semibold hover:bg-indigo-700 transition-colors">เพิ่มลงตะกร้า</button>
              <button class="w-10 h-10 border border-gray-300 rounded-xl flex items-center justify-center hover:bg-gray-50 transition-colors text-gray-500">♡</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <script>
    const products = [
      { id:1, name:'iPhone 15 Pro', price:42900, origPrice:44900, category:'electronics', rating:4.8, reviews:234, emoji:'📱', desc:'Apple iPhone 15 Pro พร้อม chip A17 Pro', badge:'ใหม่' },
      { id:2, name:'Sony WH-1000XM5', price:11990, origPrice:14900, category:'electronics', rating:4.7, reviews:189, emoji:'🎧', desc:'หูฟัง Noise Canceling ที่ดีที่สุด', badge:'ลด 20%' },
      { id:3, name:'MacBook Pro M3', price:59900, origPrice:64900, category:'electronics', rating:4.9, reviews:312, emoji:'💻', desc:'MacBook Pro 14" M3 Pro chip', badge:'Hot' },
      { id:4, name:'เสื้อโปโล', price:590, origPrice:790, category:'fashion', rating:4.3, reviews:56, emoji:'👕', desc:'เสื้อโปโลคุณภาพดี 100% cotton', badge:'' },
      { id:5, name:'กระเป๋าหนัง', price:2490, origPrice:3200, category:'fashion', rating:4.5, reviews:98, emoji:'👜', desc:'กระเป๋าหนังแท้ handmade', badge:'ลด 22%' },
      { id:6, name:'โซฟา 3 ที่นั่ง', price:8900, origPrice:12000, category:'home', rating:4.4, reviews:45, emoji:'🛋', desc:'โซฟาผ้า Scandinavian design', badge:'ลด 26%' },
      { id:7, name:'ชุดวิ่ง Nike', price:1890, origPrice:2200, category:'sports', rating:4.6, reviews:123, emoji:'🏃', desc:'ชุดวิ่งระบายอากาศดี ผ้า Dri-FIT', badge:'' },
      { id:8, name:'จักรยานไฟฟ้า', price:29900, origPrice:35000, category:'sports', rating:4.7, reviews:67, emoji:'🚲', desc:'จักรยานไฟฟ้า ระยะ 60km/ชาร์จ', badge:'ใหม่' },
    ];

    let cart = [];
    let activeCategory = 'all';
    let activePriceRange = 'all';

    function filterProducts() {
      const search = (document.getElementById('searchInput')?.value || '').toLowerCase();
      const sortVal = document.getElementById('sortSelect').value;

      let filtered = products.filter(p => {
        const catOk = activeCategory === 'all' || p.category === activeCategory;
        const priceOk = activePriceRange === 'all' ||
          (activePriceRange === 'low' && p.price < 1000) ||
          (activePriceRange === 'mid' && p.price >= 1000 && p.price <= 5000) ||
          (activePriceRange === 'high' && p.price > 5000);
        const searchOk = !search || p.name.toLowerCase().includes(search) || p.desc.toLowerCase().includes(search);
        return catOk && priceOk && searchOk;
      });

      if (sortVal === 'price-asc') filtered.sort((a,b) => a.price - b.price);
      else if (sortVal === 'price-desc') filtered.sort((a,b) => b.price - a.price);
      else if (sortVal === 'rating') filtered.sort((a,b) => b.rating - a.rating);
      else if (sortVal === 'name') filtered.sort((a,b) => a.name.localeCompare(b.name));

      renderProducts(filtered);
    }

    function renderProducts(list) {
      const grid = document.getElementById('productGrid');
      const empty = document.getElementById('emptyState');
      document.getElementById('productCount').textContent = `แสดง ${list.length} สินค้า`;

      if (list.length === 0) { grid.innerHTML = ''; empty.classList.remove('hidden'); return; }
      empty.classList.add('hidden');

      grid.innerHTML = list.map(p => {
        const discount = Math.round((1 - p.price / p.origPrice) * 100);
        return `
          <div class="bg-white rounded-2xl border overflow-hidden hover:shadow-lg transition-shadow group">
            <div class="relative">
              <div class="h-44 flex items-center justify-center text-5xl bg-gradient-to-br from-gray-100 to-gray-200 group-hover:from-indigo-50 group-hover:to-purple-50 transition-colors">${p.emoji}</div>
              ${p.badge ? `<span class="absolute top-2 left-2 text-xs font-bold px-2 py-0.5 rounded-full ${p.badge === 'ใหม่' ? 'bg-indigo-600 text-white' : p.badge === 'Hot' ? 'bg-red-500 text-white' : 'bg-yellow-400 text-yellow-900'}">${p.badge}</span>` : ''}
              <button onclick='openQuickView(${JSON.stringify(p)})' class="absolute bottom-2 right-2 bg-white border text-xs font-semibold px-2.5 py-1 rounded-lg opacity-0 group-hover:opacity-100 transition-opacity shadow-sm hover:bg-gray-50">ดูเร็ว</button>
            </div>
            <div class="p-3">
              <p class="text-xs text-gray-400 mb-0.5">${p.category}</p>
              <h3 class="font-semibold text-gray-900 text-sm mb-1 truncate">${p.name}</h3>
              <div class="flex items-center gap-1 mb-2">
                <span class="text-yellow-400 text-xs">★</span>
                <span class="text-xs text-gray-500">${p.rating} (${p.reviews})</span>
              </div>
              <div class="flex items-end justify-between">
                <div>
                  <span class="font-extrabold text-indigo-700 text-sm">฿${p.price.toLocaleString()}</span>
                  <span class="text-xs text-gray-400 line-through ml-1">฿${p.origPrice.toLocaleString()}</span>
                </div>
                <button onclick='addToCart(${JSON.stringify(p)})' class="bg-indigo-600 text-white px-2.5 py-1.5 rounded-xl text-xs font-semibold hover:bg-indigo-700 transition-colors">+ ตะกร้า</button>
              </div>
            </div>
          </div>
        `;
      }).join('');
    }

    function addToCart(product) {
      const existing = cart.find(i => i.id === product.id);
      if (existing) existing.qty++;
      else cart.push({ ...product, qty: 1 });
      updateCart();

      // Toast feedback
      const toast = document.createElement('div');
      toast.className = 'fixed bottom-4 left-1/2 -translate-x-1/2 z-[100] bg-indigo-700 text-white px-4 py-2.5 rounded-xl text-sm font-semibold shadow-lg transition-opacity duration-300';
      toast.textContent = `✅ เพิ่ม ${product.name} ลงตะกร้าแล้ว`;
      document.body.appendChild(toast);
      setTimeout(() => { toast.style.opacity = '0'; setTimeout(() => toast.remove(), 300); }, 2000);
    }

    function updateCart() {
      const count = cart.reduce((s, i) => s + i.qty, 0);
      const total = cart.reduce((s, i) => s + i.price * i.qty, 0);
      document.getElementById('cartCount').textContent = count;
      document.getElementById('cartTotal').textContent = '฿' + total.toLocaleString();
      const badge = document.getElementById('cartBadge');
      if (count > 0) { badge.classList.remove('hidden'); badge.textContent = count; }
      else badge.classList.add('hidden');

      document.getElementById('cartItems').innerHTML = cart.length === 0
        ? '<p class="text-center text-gray-400 text-sm py-8">ตะกร้าว่าง</p>'
        : cart.map(item => `
          <div class="flex items-center gap-3">
            <div class="w-12 h-12 bg-gray-100 rounded-xl flex items-center justify-center text-2xl flex-shrink-0">${item.emoji}</div>
            <div class="flex-1">
              <p class="text-sm font-semibold text-gray-900">${item.name}</p>
              <p class="text-xs text-indigo-700 font-bold">฿${item.price.toLocaleString()} × ${item.qty}</p>
            </div>
            <div class="flex items-center gap-1">
              <button onclick="changeQty(${item.id}, -1)" class="w-6 h-6 bg-gray-100 rounded-lg text-gray-600 hover:bg-gray-200 transition-colors text-sm">−</button>
              <span class="text-xs font-semibold w-5 text-center">${item.qty}</span>
              <button onclick="changeQty(${item.id}, 1)" class="w-6 h-6 bg-gray-100 rounded-lg text-gray-600 hover:bg-gray-200 transition-colors text-sm">+</button>
            </div>
          </div>
        `).join('');
    }

    function changeQty(id, delta) {
      const item = cart.find(i => i.id === id);
      if (!item) return;
      item.qty += delta;
      if (item.qty <= 0) cart = cart.filter(i => i.id !== id);
      updateCart();
    }

    function openCart() {
      document.getElementById('cartOverlay').classList.remove('hidden');
      document.getElementById('cartSidebar').style.transform = 'translateX(0)';
    }
    function closeCart() {
      document.getElementById('cartOverlay').classList.add('hidden');
      document.getElementById('cartSidebar').style.transform = 'translateX(100%)';
    }

    function openQuickView(p) {
      document.getElementById('qvTitle').textContent = p.name;
      document.getElementById('qvImg').textContent = p.emoji;
      document.getElementById('qvDesc').textContent = p.desc;
      document.getElementById('qvPrice').textContent = '฿' + p.price.toLocaleString();
      document.getElementById('qvOrigPrice').textContent = '฿' + p.origPrice.toLocaleString();
      const disc = Math.round((1 - p.price/p.origPrice)*100);
      document.getElementById('qvDiscount').textContent = `-${disc}%`;
      document.getElementById('qvRating').textContent = '★'.repeat(Math.floor(p.rating));
      document.getElementById('qvRatingNum').textContent = `${p.rating} (${p.reviews} reviews)`;
      document.getElementById('qvAddCart').onclick = () => { addToCart(p); closeQuickView(); };
      document.getElementById('quickViewOverlay').classList.remove('hidden');
    }
    function closeQuickView() { document.getElementById('quickViewOverlay').classList.add('hidden'); }

    function setCategory(cat) {
      activeCategory = cat;
      document.querySelectorAll('.cat-btn').forEach(b => {
        const isActive = b.id === `cat-${cat}`;
        b.className = `cat-btn flex-shrink-0 px-4 py-2 text-sm font-semibold rounded-xl ${isActive ? 'bg-indigo-600 text-white' : 'bg-white border text-gray-600 hover:bg-gray-50'}`;
      });
      filterProducts();
    }

    function setPriceRange(range) {
      activePriceRange = range;
      ['all','low','mid','high'].forEach(r => {
        const el = document.getElementById('pr-' + r);
        const isActive = r === range;
        el.className = `text-xs px-2.5 py-1 rounded-lg font-semibold ${isActive ? 'bg-indigo-600 text-white' : 'bg-white border text-gray-500 hover:bg-gray-50'}`;
      });
      filterProducts();
    }

    function sortProducts() { filterProducts(); }
    function resetFilters() {
      setCategory('all'); setPriceRange('all');
      document.getElementById('searchInput') && (document.getElementById('searchInput').value = '');
      filterProducts();
    }

    // Initial render
    filterProducts();
  </script>
</body>
</html>
```

---

## สรุป Part 67

| Step | เนื้อหา |
|------|---------|
| 661 | Product card — image, badge, rating, price, add to cart |
| 662 | Cart sidebar — slide-in animation, item list, total |
| 663 | Cart badge on header icon |
| 664 | Category filter bar |
| 665 | Price range filter |
| 666 | Sort dropdown |
| 667 | Search filter |
| 668 | Quick view modal |
| 669 | Toast notification เมื่อเพิ่มสินค้า |
| 670 | Workshop: Complete E-commerce shop — products/cart/quick-view/filter/sort |

**Part ถัดไป:** Part 68 — Dashboard Analytics (Steps 671–680)
