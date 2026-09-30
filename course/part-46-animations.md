# Part 46: Advanced Animations & Micro-interactions

## เป้าหมาย
- ใช้ Tailwind animations อย่างมืออาชีพ
- Transitions, hover effects, scroll animations
- Loading states, skeleton screens
- Micro-interactions ที่ทำให้ UX ดีขึ้น
- Steps 451–460

---

## Step 451: Built-in Tailwind Animations

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Built-in Animations - Step 451</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-8">
  <div class="max-w-3xl mx-auto space-y-8">
    <h2 class="text-xl font-extrabold">Tailwind Built-in Animations</h2>

    <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
      <!-- spin -->
      <div class="bg-white rounded-2xl border p-5 flex flex-col items-center gap-3">
        <div class="w-12 h-12 border-4 border-indigo-200 border-t-indigo-600 rounded-full animate-spin"></div>
        <code class="text-xs text-gray-500">animate-spin</code>
      </div>
      <!-- bounce -->
      <div class="bg-white rounded-2xl border p-5 flex flex-col items-center gap-3">
        <div class="w-12 h-12 bg-indigo-600 rounded-full animate-bounce"></div>
        <code class="text-xs text-gray-500">animate-bounce</code>
      </div>
      <!-- pulse -->
      <div class="bg-white rounded-2xl border p-5 flex flex-col items-center gap-3">
        <div class="w-12 h-12 bg-indigo-300 rounded-xl animate-pulse"></div>
        <code class="text-xs text-gray-500">animate-pulse</code>
      </div>
      <!-- ping -->
      <div class="bg-white rounded-2xl border p-5 flex flex-col items-center gap-3">
        <div class="relative w-12 h-12 flex items-center justify-center">
          <span class="absolute w-full h-full bg-indigo-400 rounded-full animate-ping opacity-75"></span>
          <span class="w-8 h-8 bg-indigo-600 rounded-full block"></span>
        </div>
        <code class="text-xs text-gray-500">animate-ping</code>
      </div>
    </div>

    <!-- Transition durations -->
    <div class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold text-sm mb-3">Transition Durations</h3>
      <div class="flex flex-wrap gap-3">
        <button class="bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm hover:bg-indigo-800 transition-none">none</button>
        <button class="bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm hover:bg-indigo-800 transition-all duration-75">75ms</button>
        <button class="bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm hover:bg-indigo-800 transition-all duration-150">150ms</button>
        <button class="bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm hover:bg-indigo-800 transition-all duration-300">300ms</button>
        <button class="bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm hover:bg-indigo-800 transition-all duration-500">500ms</button>
        <button class="bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm hover:bg-indigo-800 transition-all duration-700">700ms</button>
      </div>
    </div>

    <!-- Easing functions -->
    <div class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold text-sm mb-3">Easing Functions</h3>
      <div class="flex flex-col gap-3">
        <div class="flex items-center gap-3">
          <span class="text-xs text-gray-500 w-24">ease-linear</span>
          <div class="flex-1 h-10 bg-gray-100 rounded-xl overflow-hidden relative group">
            <div class="absolute inset-y-0 left-0 w-0 group-hover:w-full bg-indigo-500 transition-all duration-1000 ease-linear rounded-xl"></div>
          </div>
        </div>
        <div class="flex items-center gap-3">
          <span class="text-xs text-gray-500 w-24">ease-in</span>
          <div class="flex-1 h-10 bg-gray-100 rounded-xl overflow-hidden relative group">
            <div class="absolute inset-y-0 left-0 w-0 group-hover:w-full bg-green-500 transition-all duration-1000 ease-in rounded-xl"></div>
          </div>
        </div>
        <div class="flex items-center gap-3">
          <span class="text-xs text-gray-500 w-24">ease-out</span>
          <div class="flex-1 h-10 bg-gray-100 rounded-xl overflow-hidden relative group">
            <div class="absolute inset-y-0 left-0 w-0 group-hover:w-full bg-yellow-500 transition-all duration-1000 ease-out rounded-xl"></div>
          </div>
        </div>
        <div class="flex items-center gap-3">
          <span class="text-xs text-gray-500 w-24">ease-in-out</span>
          <div class="flex-1 h-10 bg-gray-100 rounded-xl overflow-hidden relative group">
            <div class="absolute inset-y-0 left-0 w-0 group-hover:w-full bg-red-500 transition-all duration-1000 ease-in-out rounded-xl"></div>
          </div>
        </div>
      </div>
      <p class="text-xs text-gray-400 mt-3">Hover แต่ละแถวเพื่อดู easing ที่ต่างกัน</p>
    </div>

  </div>
</body>
</html>
```

---

## Step 452: Hover Micro-interactions

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Hover Interactions - Step 452</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 p-8">
  <div class="max-w-4xl mx-auto space-y-6">
    <h2 class="text-xl font-extrabold">Hover Micro-interactions</h2>
    <div class="grid grid-cols-2 md:grid-cols-3 gap-4">

      <!-- Scale on hover -->
      <div class="bg-white rounded-2xl border p-5 cursor-pointer hover:scale-105 transition-transform duration-200">
        <div class="text-2xl mb-2">📈</div>
        <h4 class="font-bold text-sm">Scale Up</h4>
        <p class="text-xs text-gray-500 mt-1">hover:scale-105</p>
      </div>

      <!-- Lift (shadow) on hover -->
      <div class="bg-white rounded-2xl border p-5 cursor-pointer hover:-translate-y-2 hover:shadow-xl transition-all duration-200">
        <div class="text-2xl mb-2">🚀</div>
        <h4 class="font-bold text-sm">Lift Effect</h4>
        <p class="text-xs text-gray-500 mt-1">hover:-translate-y-2</p>
      </div>

      <!-- Color change -->
      <div class="bg-gray-800 rounded-2xl p-5 cursor-pointer hover:bg-indigo-600 transition-colors duration-300">
        <div class="text-2xl mb-2">🎨</div>
        <h4 class="font-bold text-sm text-white">Color Shift</h4>
        <p class="text-xs text-gray-400 mt-1">hover:bg-indigo-600</p>
      </div>

      <!-- Reveal text -->
      <div class="bg-white rounded-2xl border overflow-hidden cursor-pointer group relative">
        <div class="p-5">
          <div class="text-2xl mb-2">👁️</div>
          <h4 class="font-bold text-sm">Reveal Info</h4>
        </div>
        <div class="absolute inset-x-0 bottom-0 bg-indigo-600 text-white p-4 text-xs translate-y-full group-hover:translate-y-0 transition-transform duration-300">
          ✨ ข้อมูลเพิ่มเติมเมื่อ hover
        </div>
      </div>

      <!-- Rotate icon -->
      <div class="bg-white rounded-2xl border p-5 cursor-pointer group">
        <div class="text-2xl mb-2 group-hover:rotate-12 transition-transform duration-300 inline-block">⚙️</div>
        <h4 class="font-bold text-sm">Rotate Icon</h4>
        <p class="text-xs text-gray-500 mt-1">group-hover:rotate-12</p>
      </div>

      <!-- Underline slide -->
      <div class="bg-white rounded-2xl border p-5 cursor-pointer group">
        <h4 class="font-bold text-sm relative inline-block">
          Underline Slide
          <span class="absolute bottom-0 left-0 w-0 h-0.5 bg-indigo-600 group-hover:w-full transition-all duration-300"></span>
        </h4>
        <p class="text-xs text-gray-500 mt-2">pseudo element slide</p>
      </div>

      <!-- Border grow -->
      <div class="bg-white rounded-2xl border-2 border-transparent hover:border-indigo-500 p-5 cursor-pointer transition-all duration-200">
        <div class="text-2xl mb-2">🔲</div>
        <h4 class="font-bold text-sm">Border Appear</h4>
        <p class="text-xs text-gray-500 mt-1">hover:border-indigo-500</p>
      </div>

      <!-- Button ripple-like -->
      <div class="bg-white rounded-2xl border p-5">
        <button class="relative overflow-hidden bg-indigo-600 text-white px-5 py-2.5 rounded-xl text-sm font-semibold w-full
          before:absolute before:inset-0 before:bg-white/10 before:translate-x-[-100%] hover:before:translate-x-[100%] before:transition-transform before:duration-500">
          Shimmer Button
        </button>
        <p class="text-xs text-gray-500 mt-2">hover shimmer effect</p>
      </div>

      <!-- Gradient shift -->
      <div class="rounded-2xl p-5 cursor-pointer bg-gradient-to-r from-indigo-500 to-purple-500 hover:from-purple-500 hover:to-pink-500 transition-all duration-500">
        <h4 class="font-bold text-sm text-white">Gradient Shift</h4>
        <p class="text-xs text-white/80 mt-1">hover changes gradient</p>
      </div>

    </div>
  </div>
</body>
</html>
```

---

## Step 453: Skeleton Loading Screens

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Skeleton Screens - Step 453</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-8">
  <div class="max-w-3xl mx-auto space-y-8">
    <h2 class="text-xl font-extrabold">Skeleton Loading Screens</h2>

    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">

      <!-- Profile card skeleton -->
      <div class="bg-white rounded-2xl border p-5">
        <h3 class="text-sm text-gray-500 mb-4">Profile Card Skeleton</h3>
        <div class="flex items-center gap-4">
          <div class="w-14 h-14 bg-gray-200 rounded-full animate-pulse flex-shrink-0"></div>
          <div class="flex-1 space-y-2">
            <div class="h-4 bg-gray-200 rounded-full animate-pulse w-3/4"></div>
            <div class="h-3 bg-gray-200 rounded-full animate-pulse w-1/2"></div>
          </div>
        </div>
        <div class="mt-4 space-y-2">
          <div class="h-3 bg-gray-200 rounded-full animate-pulse"></div>
          <div class="h-3 bg-gray-200 rounded-full animate-pulse w-5/6"></div>
          <div class="h-3 bg-gray-200 rounded-full animate-pulse w-4/6"></div>
        </div>
        <div class="flex gap-3 mt-5">
          <div class="flex-1 h-8 bg-gray-200 rounded-xl animate-pulse"></div>
          <div class="flex-1 h-8 bg-gray-200 rounded-xl animate-pulse"></div>
        </div>
      </div>

      <!-- Product card skeleton -->
      <div class="bg-white rounded-2xl border overflow-hidden">
        <div class="h-48 bg-gray-200 animate-pulse"></div>
        <div class="p-4 space-y-3">
          <div class="h-4 bg-gray-200 rounded-full animate-pulse w-3/4"></div>
          <div class="h-3 bg-gray-200 rounded-full animate-pulse w-1/2"></div>
          <div class="flex items-center justify-between mt-2">
            <div class="h-5 bg-gray-200 rounded-full animate-pulse w-20"></div>
            <div class="h-8 bg-gray-200 rounded-xl animate-pulse w-24"></div>
          </div>
        </div>
      </div>

      <!-- Table skeleton -->
      <div class="bg-white rounded-2xl border overflow-hidden">
        <h3 class="text-sm text-gray-500 p-4">Table Skeleton</h3>
        <div class="border-t">
          <div class="grid grid-cols-4 gap-4 p-3 bg-gray-50">
            <div class="h-3 bg-gray-200 rounded-full animate-pulse"></div>
            <div class="h-3 bg-gray-200 rounded-full animate-pulse"></div>
            <div class="h-3 bg-gray-200 rounded-full animate-pulse"></div>
            <div class="h-3 bg-gray-200 rounded-full animate-pulse"></div>
          </div>
          {[1,2,3,4].map(() => ``)}
          <div class="grid grid-cols-4 gap-4 p-3 border-t">
            <div class="h-3 bg-gray-200 rounded-full animate-pulse"></div>
            <div class="h-3 bg-gray-200 rounded-full animate-pulse w-3/4"></div>
            <div class="h-3 bg-gray-200 rounded-full animate-pulse w-1/2"></div>
            <div class="h-5 bg-gray-200 rounded-full animate-pulse w-16"></div>
          </div>
          <div class="grid grid-cols-4 gap-4 p-3 border-t">
            <div class="h-3 bg-gray-200 rounded-full animate-pulse"></div>
            <div class="h-3 bg-gray-200 rounded-full animate-pulse w-2/3"></div>
            <div class="h-3 bg-gray-200 rounded-full animate-pulse w-3/4"></div>
            <div class="h-5 bg-gray-200 rounded-full animate-pulse w-16"></div>
          </div>
          <div class="grid grid-cols-4 gap-4 p-3 border-t">
            <div class="h-3 bg-gray-200 rounded-full animate-pulse"></div>
            <div class="h-3 bg-gray-200 rounded-full animate-pulse w-4/5"></div>
            <div class="h-3 bg-gray-200 rounded-full animate-pulse w-1/2"></div>
            <div class="h-5 bg-gray-200 rounded-full animate-pulse w-16"></div>
          </div>
        </div>
      </div>

      <!-- Feed skeleton -->
      <div class="bg-white rounded-2xl border p-4 space-y-4">
        <h3 class="text-sm text-gray-500">Feed Skeleton</h3>
        <div class="flex gap-3">
          <div class="w-8 h-8 bg-gray-200 rounded-full animate-pulse flex-shrink-0"></div>
          <div class="flex-1 space-y-2">
            <div class="h-3 bg-gray-200 rounded-full animate-pulse w-1/3"></div>
            <div class="h-3 bg-gray-200 rounded-full animate-pulse"></div>
            <div class="h-3 bg-gray-200 rounded-full animate-pulse w-4/5"></div>
            <div class="h-20 bg-gray-200 rounded-xl animate-pulse mt-2"></div>
          </div>
        </div>
        <div class="flex gap-3">
          <div class="w-8 h-8 bg-gray-200 rounded-full animate-pulse flex-shrink-0"></div>
          <div class="flex-1 space-y-2">
            <div class="h-3 bg-gray-200 rounded-full animate-pulse w-1/4"></div>
            <div class="h-3 bg-gray-200 rounded-full animate-pulse w-5/6"></div>
            <div class="h-3 bg-gray-200 rounded-full animate-pulse w-3/4"></div>
          </div>
        </div>
      </div>

    </div>

    <!-- Toggle skeleton/content -->
    <div class="bg-white rounded-2xl border p-5">
      <div class="flex items-center justify-between mb-4">
        <h3 class="font-bold">สลับ Skeleton / Content</h3>
        <button onclick="toggleLoad()" class="bg-indigo-600 text-white px-3 py-1.5 rounded-xl text-sm hover:bg-indigo-700 transition-colors">Toggle Load</button>
      </div>

      <div id="skeleton-view">
        <div class="flex items-center gap-4">
          <div class="w-14 h-14 bg-gray-200 rounded-full animate-pulse"></div>
          <div class="flex-1 space-y-2">
            <div class="h-4 bg-gray-200 rounded-full animate-pulse w-1/3"></div>
            <div class="h-3 bg-gray-200 rounded-full animate-pulse w-1/4"></div>
          </div>
        </div>
      </div>

      <div id="content-view" class="hidden">
        <div class="flex items-center gap-4">
          <div class="w-14 h-14 bg-indigo-100 rounded-full flex items-center justify-center text-2xl">👤</div>
          <div>
            <p class="font-bold">สมชาย เทคโน</p>
            <p class="text-sm text-gray-500">Software Engineer</p>
          </div>
        </div>
      </div>
    </div>

  </div>

  <script>
    function toggleLoad() {
      const sk = document.getElementById('skeleton-view');
      const ct = document.getElementById('content-view');
      sk.classList.toggle('hidden');
      ct.classList.toggle('hidden');
    }
  </script>
</body>
</html>
```

---

## Steps 454–459: Page Transitions, Loading Bars, Custom Animations

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Advanced Animations - Steps 454-459</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    @keyframes fadeIn { from { opacity: 0; transform: translateY(16px); } to { opacity: 1; transform: translateY(0); } }
    @keyframes slideRight { from { transform: translateX(-100%); } to { transform: translateX(0); } }
    @keyframes countUp { from { opacity: 0; transform: scale(0.5); } to { opacity: 1; transform: scale(1); } }
    @keyframes shimmer { 0% { background-position: -200% 0; } 100% { background-position: 200% 0; } }
    @keyframes ripple { 0% { transform: scale(0); opacity: 0.6; } 100% { transform: scale(4); opacity: 0; } }
    
    .fade-in { animation: fadeIn 0.4s ease-out both; }
    .fade-in-1 { animation-delay: 0.1s; }
    .fade-in-2 { animation-delay: 0.2s; }
    .fade-in-3 { animation-delay: 0.3s; }
    .fade-in-4 { animation-delay: 0.4s; }
    
    .shimmer-bg {
      background: linear-gradient(90deg, #e5e7eb 25%, #f3f4f6 50%, #e5e7eb 75%);
      background-size: 200% 100%;
      animation: shimmer 1.5s infinite;
    }
    .ripple-btn { position: relative; overflow: hidden; }
    .ripple-btn::after {
      content: '';
      position: absolute;
      border-radius: 50%;
      background: rgba(255,255,255,0.3);
      width: 100px; height: 100px;
      margin-left: -50px; margin-top: -50px;
      animation: ripple 0.6s;
      top: var(--y, 50%); left: var(--x, 50%);
      animation-play-state: paused;
    }
    .ripple-btn.rippling::after { animation-play-state: running; }
  </style>
</head>
<body class="bg-gray-50 p-8">
  <div class="max-w-4xl mx-auto space-y-8">

    <!-- Staggered list animation (Step 454) -->
    <section class="bg-white rounded-2xl border p-5">
      <div class="flex items-center justify-between mb-4">
        <h3 class="font-bold">Staggered Animations</h3>
        <button onclick="resetAnim()" class="text-sm bg-indigo-100 text-indigo-700 px-3 py-1.5 rounded-xl hover:bg-indigo-200 transition-colors">Reset</button>
      </div>
      <div id="anim-list" class="space-y-2">
        <div class="fade-in fade-in-1 opacity-0 flex items-center gap-3 bg-gray-50 p-3 rounded-xl">
          <div class="w-8 h-8 bg-indigo-100 rounded-lg flex items-center justify-center text-sm">1</div>
          <p class="text-sm font-medium">รายการที่ 1</p>
        </div>
        <div class="fade-in fade-in-2 opacity-0 flex items-center gap-3 bg-gray-50 p-3 rounded-xl">
          <div class="w-8 h-8 bg-green-100 rounded-lg flex items-center justify-center text-sm">2</div>
          <p class="text-sm font-medium">รายการที่ 2</p>
        </div>
        <div class="fade-in fade-in-3 opacity-0 flex items-center gap-3 bg-gray-50 p-3 rounded-xl">
          <div class="w-8 h-8 bg-yellow-100 rounded-lg flex items-center justify-center text-sm">3</div>
          <p class="text-sm font-medium">รายการที่ 3</p>
        </div>
        <div class="fade-in fade-in-4 opacity-0 flex items-center gap-3 bg-gray-50 p-3 rounded-xl">
          <div class="w-8 h-8 bg-red-100 rounded-lg flex items-center justify-center text-sm">4</div>
          <p class="text-sm font-medium">รายการที่ 4</p>
        </div>
      </div>
    </section>

    <!-- Progress bar / Loading (Step 455) -->
    <section class="bg-white rounded-2xl border p-5 space-y-4">
      <h3 class="font-bold">Loading Progress</h3>

      <!-- Top loading bar -->
      <div>
        <div class="flex items-center justify-between text-xs text-gray-500 mb-1">
          <span>กำลังโหลด...</span>
          <span id="prog-label">0%</span>
        </div>
        <div class="h-2 bg-gray-200 rounded-full overflow-hidden">
          <div id="progress-bar" class="h-full bg-indigo-600 rounded-full transition-all duration-200 w-0"></div>
        </div>
      </div>

      <div class="flex gap-3">
        <button onclick="startProgress()" class="bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">Start</button>
        <button onclick="resetProgress()" class="border border-gray-300 px-4 py-2 rounded-xl text-sm hover:bg-gray-50 transition-colors">Reset</button>
      </div>

      <!-- Shimmer progress -->
      <div class="h-3 rounded-full shimmer-bg"></div>
      <div class="h-3 rounded-full shimmer-bg w-3/4"></div>
      <div class="h-3 rounded-full shimmer-bg w-1/2"></div>
      <p class="text-xs text-gray-400">Shimmer loading effect</p>
    </section>

    <!-- Ripple Button (Step 456) -->
    <section class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4">Ripple Effect Button</h3>
      <div class="flex gap-3">
        <button class="ripple-btn bg-indigo-600 text-white px-6 py-3 rounded-xl text-sm font-semibold" onclick="ripple(this, event)">Click me!</button>
        <button class="ripple-btn bg-green-600 text-white px-6 py-3 rounded-xl text-sm font-semibold" onclick="ripple(this, event)">Ripple</button>
      </div>
    </section>

    <!-- Counter Animation (Step 457) -->
    <section class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4">Count-Up Animation</h3>
      <div class="grid grid-cols-4 gap-4 text-center">
        <div>
          <p class="text-3xl font-extrabold text-indigo-600" id="c1">0</p>
          <p class="text-xs text-gray-500 mt-1">ผู้ใช้งาน</p>
        </div>
        <div>
          <p class="text-3xl font-extrabold text-green-600" id="c2">0</p>
          <p class="text-xs text-gray-500 mt-1">คำสั่งซื้อ</p>
        </div>
        <div>
          <p class="text-3xl font-extrabold text-purple-600" id="c3">0</p>
          <p class="text-xs text-gray-500 mt-1">สินค้า</p>
        </div>
        <div>
          <p class="text-3xl font-extrabold text-red-600" id="c4">0</p>
          <p class="text-xs text-gray-500 mt-1">รีวิว</p>
        </div>
      </div>
      <button onclick="startCounters()" class="mt-4 bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">Start Counting</button>
    </section>

    <!-- Toast Notifications (Step 458) -->
    <section class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4">Toast Notifications</h3>
      <div class="flex flex-wrap gap-3">
        <button onclick="showToast('success', '✅ บันทึกเรียบร้อยแล้ว')" class="bg-green-600 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-green-700 transition-colors">Success</button>
        <button onclick="showToast('error', '❌ เกิดข้อผิดพลาด')" class="bg-red-600 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-red-700 transition-colors">Error</button>
        <button onclick="showToast('info', 'ℹ️ อัปเดตใหม่พร้อมให้ดาวน์โหลด')" class="bg-blue-600 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-blue-700 transition-colors">Info</button>
        <button onclick="showToast('warning', '⚠️ ที่เก็บข้อมูลเหลือน้อย')" class="bg-yellow-500 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-yellow-600 transition-colors">Warning</button>
      </div>
    </section>

    <!-- Collapse / Accordion (Step 459) -->
    <section class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4">Accordion</h3>
      <div class="space-y-2" id="accordion">
        <div class="border rounded-xl overflow-hidden">
          <button onclick="toggleAccordion(this)" class="w-full flex items-center justify-between px-4 py-3 text-left text-sm font-semibold hover:bg-gray-50 transition-colors">
            <span>Tailwind CSS คืออะไร?</span>
            <svg class="w-4 h-4 text-gray-400 transition-transform duration-200" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"/></svg>
          </button>
          <div class="acc-content hidden px-4 pb-4 text-sm text-gray-600">Tailwind CSS เป็น utility-first CSS framework ที่ให้คุณสร้าง UI โดยใช้ class ขนาดเล็กที่รวมกันได้</div>
        </div>
        <div class="border rounded-xl overflow-hidden">
          <button onclick="toggleAccordion(this)" class="w-full flex items-center justify-between px-4 py-3 text-left text-sm font-semibold hover:bg-gray-50 transition-colors">
            <span>ทำไมต้องใช้ CDN?</span>
            <svg class="w-4 h-4 text-gray-400 transition-transform duration-200" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"/></svg>
          </button>
          <div class="acc-content hidden px-4 pb-4 text-sm text-gray-600">CDN ช่วยให้ใช้ Tailwind ได้โดยไม่ต้อง build process เหมาะกับการทดสอบและ prototyping</div>
        </div>
        <div class="border rounded-xl overflow-hidden">
          <button onclick="toggleAccordion(this)" class="w-full flex items-center justify-between px-4 py-3 text-left text-sm font-semibold hover:bg-gray-50 transition-colors">
            <span>Responsive Design ทำยังไง?</span>
            <svg class="w-4 h-4 text-gray-400 transition-transform duration-200" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"/></svg>
          </button>
          <div class="acc-content hidden px-4 pb-4 text-sm text-gray-600">ใช้ prefix sm:, md:, lg:, xl: หน้า class เช่น md:flex-row, lg:grid-cols-3 เป็นต้น</div>
        </div>
      </div>
    </section>

  </div>

  <!-- Toast Container -->
  <div id="toast-container" class="fixed bottom-6 right-6 z-50 space-y-2"></div>

  <script>
    // Staggered animation reset
    function resetAnim() {
      const list = document.getElementById('anim-list');
      list.innerHTML = list.innerHTML;
    }

    // Progress bar
    let progInterval;
    let progVal = 0;
    function startProgress() {
      clearInterval(progInterval);
      progVal = 0;
      progInterval = setInterval(() => {
        progVal = Math.min(100, progVal + Math.random() * 8 + 2);
        document.getElementById('progress-bar').style.width = progVal + '%';
        document.getElementById('prog-label').textContent = Math.floor(progVal) + '%';
        if (progVal >= 100) clearInterval(progInterval);
      }, 150);
    }
    function resetProgress() {
      clearInterval(progInterval);
      progVal = 0;
      document.getElementById('progress-bar').style.width = '0%';
      document.getElementById('prog-label').textContent = '0%';
    }

    // Ripple
    function ripple(btn, e) {
      const rect = btn.getBoundingClientRect();
      btn.style.setProperty('--x', (e.clientX - rect.left) + 'px');
      btn.style.setProperty('--y', (e.clientY - rect.top) + 'px');
      btn.classList.remove('rippling');
      void btn.offsetWidth;
      btn.classList.add('rippling');
      setTimeout(() => btn.classList.remove('rippling'), 700);
    }

    // Count-up
    function animateCount(el, target, dur) {
      let start = 0;
      const step = target / (dur / 16);
      const t = setInterval(() => {
        start = Math.min(target, start + step);
        el.textContent = Math.floor(start).toLocaleString();
        if (start >= target) clearInterval(t);
      }, 16);
    }
    function startCounters() {
      animateCount(document.getElementById('c1'), 12847, 1500);
      animateCount(document.getElementById('c2'), 5923, 1200);
      animateCount(document.getElementById('c3'), 842, 1000);
      animateCount(document.getElementById('c4'), 3156, 1300);
    }

    // Toast
    const toastColors = {
      success: 'bg-green-600',
      error: 'bg-red-600',
      info: 'bg-blue-600',
      warning: 'bg-yellow-500',
    };
    function showToast(type, msg) {
      const c = document.getElementById('toast-container');
      const t = document.createElement('div');
      t.className = `${toastColors[type]} text-white px-4 py-3 rounded-xl text-sm font-medium shadow-lg max-w-xs
        transform translate-y-4 opacity-0 transition-all duration-300`;
      t.textContent = msg;
      c.appendChild(t);
      requestAnimationFrame(() => {
        requestAnimationFrame(() => {
          t.classList.remove('translate-y-4', 'opacity-0');
        });
      });
      setTimeout(() => {
        t.classList.add('translate-y-4', 'opacity-0');
        setTimeout(() => t.remove(), 300);
      }, 3000);
    }

    // Accordion
    function toggleAccordion(btn) {
      const content = btn.nextElementSibling;
      const icon = btn.querySelector('svg');
      const isOpen = !content.classList.contains('hidden');
      content.classList.toggle('hidden');
      icon.classList.toggle('rotate-180', !isOpen);
    }
  </script>
</body>
</html>
```

---

## Step 460: Workshop — Animated UI Component Pack

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Animation Workshop - Step 460</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    @keyframes fadeUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
    @keyframes slideIn { from { opacity: 0; transform: translateX(-20px); } to { opacity: 1; transform: translateX(0); } }
    @keyframes scaleIn { from { opacity: 0; transform: scale(0.8); } to { opacity: 1; transform: scale(1); } }
    @keyframes shimmer { 0% { background-position: -200% 0; } 100% { background-position: 200% 0; } }

    .anim-fade-up { animation: fadeUp 0.5s ease-out both; }
    .anim-slide-in { animation: slideIn 0.4s ease-out both; }
    .anim-scale-in { animation: scaleIn 0.3s ease-out both; }

    .shimmer {
      background: linear-gradient(90deg, #e5e7eb 25%, #f9fafb 50%, #e5e7eb 75%);
      background-size: 200% 100%;
      animation: shimmer 1.5s infinite;
    }
  </style>
</head>
<body class="bg-gray-100">

  <!-- Animated top bar (page progress) -->
  <div id="top-bar" class="fixed top-0 left-0 h-1 bg-indigo-600 z-50 transition-all duration-100" style="width:0%"></div>

  <!-- Header -->
  <header class="bg-white border-b px-6 h-14 flex items-center justify-between sticky top-0 z-10 mt-1">
    <h1 class="font-extrabold text-indigo-600">AnimationKit</h1>
    <div class="flex items-center gap-3">
      <button onclick="showToast('info','📦 Package copied!')" class="text-gray-500 hover:text-gray-700 transition-colors text-sm">Copy CDN</button>
      <button class="bg-indigo-600 text-white px-3 py-1.5 text-sm rounded-xl hover:bg-indigo-700 transition-colors">Get Started</button>
    </div>
  </header>

  <!-- Main -->
  <main class="max-w-5xl mx-auto p-6 space-y-8">

    <!-- Hero KPIs -->
    <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
      <div class="anim-fade-up bg-white rounded-2xl border p-4 text-center" style="animation-delay:0s">
        <p class="text-3xl font-extrabold text-indigo-600" id="kpi1">0</p>
        <p class="text-xs text-gray-500 mt-1">Components</p>
      </div>
      <div class="anim-fade-up bg-white rounded-2xl border p-4 text-center" style="animation-delay:0.1s">
        <p class="text-3xl font-extrabold text-green-600" id="kpi2">0</p>
        <p class="text-xs text-gray-500 mt-1">Animations</p>
      </div>
      <div class="anim-fade-up bg-white rounded-2xl border p-4 text-center" style="animation-delay:0.2s">
        <p class="text-3xl font-extrabold text-purple-600" id="kpi3">0</p>
        <p class="text-xs text-gray-500 mt-1">Lines of CSS</p>
      </div>
      <div class="anim-fade-up bg-white rounded-2xl border p-4 text-center" style="animation-delay:0.3s">
        <p class="text-3xl font-extrabold text-orange-600" id="kpi4">0</p>
        <p class="text-xs text-gray-500 mt-1">Downloads</p>
      </div>
    </div>

    <!-- Interactive demo grid -->
    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">

      <!-- Loading states demo -->
      <div class="bg-white rounded-2xl border p-5">
        <h3 class="font-bold mb-4">Loading States</h3>
        <div id="loading-demo">
          <div class="space-y-3">
            <div class="shimmer h-4 rounded-full w-3/4"></div>
            <div class="shimmer h-4 rounded-full"></div>
            <div class="shimmer h-4 rounded-full w-5/6"></div>
            <div class="shimmer h-20 rounded-xl mt-4"></div>
          </div>
        </div>
        <button onclick="toggleLoading()" class="mt-4 w-full bg-gray-100 text-gray-700 py-2 rounded-xl text-sm font-medium hover:bg-gray-200 transition-colors">
          Load Content
        </button>
      </div>

      <!-- Toast demo -->
      <div class="bg-white rounded-2xl border p-5">
        <h3 class="font-bold mb-4">Notification System</h3>
        <div class="grid grid-cols-2 gap-2">
          <button onclick="showToast('success','✅ บันทึกสำเร็จ')" class="bg-green-100 text-green-700 py-2.5 rounded-xl text-xs font-semibold hover:bg-green-200 transition-colors">Success</button>
          <button onclick="showToast('error','❌ เกิดข้อผิดพลาด')" class="bg-red-100 text-red-700 py-2.5 rounded-xl text-xs font-semibold hover:bg-red-200 transition-colors">Error</button>
          <button onclick="showToast('info','ℹ️ ข้อมูลใหม่มาแล้ว')" class="bg-blue-100 text-blue-700 py-2.5 rounded-xl text-xs font-semibold hover:bg-blue-200 transition-colors">Info</button>
          <button onclick="showToast('warning','⚠️ คำเตือน')" class="bg-yellow-100 text-yellow-700 py-2.5 rounded-xl text-xs font-semibold hover:bg-yellow-200 transition-colors">Warning</button>
        </div>
      </div>

      <!-- Counters -->
      <div class="bg-white rounded-2xl border p-5">
        <h3 class="font-bold mb-4">Animated Counters</h3>
        <div class="grid grid-cols-3 gap-3 text-center">
          <div>
            <p class="text-2xl font-extrabold text-indigo-600" id="ac1">0</p>
            <p class="text-xs text-gray-400">Users</p>
          </div>
          <div>
            <p class="text-2xl font-extrabold text-green-600" id="ac2">0</p>
            <p class="text-xs text-gray-400">Sales</p>
          </div>
          <div>
            <p class="text-2xl font-extrabold text-red-600" id="ac3">0</p>
            <p class="text-xs text-gray-400">Reviews</p>
          </div>
        </div>
        <button onclick="startAC()" class="mt-4 w-full bg-indigo-600 text-white py-2 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">Animate!</button>
      </div>

      <!-- Progress bars -->
      <div class="bg-white rounded-2xl border p-5">
        <h3 class="font-bold mb-4">Progress Animations</h3>
        <div class="space-y-3">
          <div>
            <div class="flex justify-between text-xs mb-1"><span class="text-gray-500">HTML/CSS</span><span class="font-semibold" id="pct1">0%</span></div>
            <div class="h-2 bg-gray-200 rounded-full overflow-hidden"><div id="pb1" class="h-full bg-indigo-500 rounded-full transition-all duration-1000 w-0"></div></div>
          </div>
          <div>
            <div class="flex justify-between text-xs mb-1"><span class="text-gray-500">JavaScript</span><span class="font-semibold" id="pct2">0%</span></div>
            <div class="h-2 bg-gray-200 rounded-full overflow-hidden"><div id="pb2" class="h-full bg-green-500 rounded-full transition-all duration-1000 w-0"></div></div>
          </div>
          <div>
            <div class="flex justify-between text-xs mb-1"><span class="text-gray-500">Tailwind CSS</span><span class="font-semibold" id="pct3">0%</span></div>
            <div class="h-2 bg-gray-200 rounded-full overflow-hidden"><div id="pb3" class="h-full bg-purple-500 rounded-full transition-all duration-1000 w-0"></div></div>
          </div>
        </div>
        <button onclick="animateBars()" class="mt-4 w-full bg-gray-100 text-gray-700 py-2 rounded-xl text-sm font-medium hover:bg-gray-200 transition-colors">Animate Bars</button>
      </div>

    </div>

  </main>

  <!-- Toast Container -->
  <div id="toast-container" class="fixed bottom-6 right-6 z-50 space-y-2"></div>

  <script>
    // Page progress bar
    window.addEventListener('scroll', () => {
      const scrollPct = (window.scrollY / (document.body.scrollHeight - window.innerHeight)) * 100;
      document.getElementById('top-bar').style.width = scrollPct + '%';
    });

    // Count-up KPIs on load
    function animateCount(id, target, dur) {
      const el = document.getElementById(id);
      let val = 0;
      const step = target / (dur / 16);
      const t = setInterval(() => {
        val = Math.min(target, val + step);
        el.textContent = Math.floor(val);
        if (val >= target) { el.textContent = target; clearInterval(t); }
      }, 16);
    }
    setTimeout(() => {
      animateCount('kpi1', 48, 800);
      animateCount('kpi2', 12, 600);
      animateCount('kpi3', 350, 1000);
      animateCount('kpi4', 9280, 1500);
    }, 400);

    // Loading toggle
    let loaded = false;
    function toggleLoading() {
      const d = document.getElementById('loading-demo');
      if (!loaded) {
        d.innerHTML = `
          <div class="anim-scale-in">
            <div class="flex items-center gap-3 mb-3">
              <div class="w-10 h-10 bg-indigo-100 rounded-xl flex items-center justify-center text-indigo-600">📊</div>
              <div><p class="font-bold text-sm">Dashboard Ready</p><p class="text-xs text-gray-500">โหลดข้อมูลเสร็จแล้ว</p></div>
            </div>
            <div class="text-3xl font-extrabold text-indigo-600">฿ 48,250</div>
            <p class="text-xs text-gray-400 mt-1">ยอดขายวันนี้</p>
          </div>
        `;
        event.target.textContent = 'Show Skeleton';
      } else {
        d.innerHTML = `
          <div class="space-y-3">
            <div class="shimmer h-4 rounded-full w-3/4"></div>
            <div class="shimmer h-4 rounded-full"></div>
            <div class="shimmer h-4 rounded-full w-5/6"></div>
            <div class="shimmer h-20 rounded-xl mt-4"></div>
          </div>
        `;
        event.target.textContent = 'Load Content';
      }
      loaded = !loaded;
    }

    // Toast
    const toastColors = { success: 'bg-green-600', error: 'bg-red-600', info: 'bg-blue-600', warning: 'bg-yellow-500' };
    function showToast(type, msg) {
      const c = document.getElementById('toast-container');
      const t = document.createElement('div');
      t.className = `${toastColors[type]} text-white px-4 py-3 rounded-xl text-sm font-medium shadow-lg max-w-xs transform translate-y-4 opacity-0 transition-all duration-300`;
      t.textContent = msg;
      c.appendChild(t);
      requestAnimationFrame(() => requestAnimationFrame(() => t.classList.remove('translate-y-4', 'opacity-0')));
      setTimeout(() => { t.classList.add('translate-y-4', 'opacity-0'); setTimeout(() => t.remove(), 300); }, 3000);
    }

    // Animated counters
    function startAC() {
      animateCount('ac1', 12540, 1200);
      animateCount('ac2', 8430, 1000);
      animateCount('ac3', 4290, 900);
    }

    // Progress bars
    function animateBars() {
      const data = [{ id: 'pb1', pct: 92, lbl: 'pct1' }, { id: 'pb2', pct: 78, lbl: 'pct2' }, { id: 'pb3', pct: 88, lbl: 'pct3' }];
      data.forEach(({ id, pct, lbl }) => {
        document.getElementById(id).style.width = pct + '%';
        document.getElementById(lbl).textContent = pct + '%';
      });
    }
  </script>

</body>
</html>
```

---

## สรุป Part 46

| Step | เนื้อหา |
|------|---------|
| 451 | Built-in animations: spin, bounce, pulse, ping; transitions & easing |
| 452 | Hover micro-interactions: scale, lift, color, reveal, rotate, underline |
| 453 | Skeleton loading screens: profile, product, table, feed |
| 454 | Staggered list animations with CSS animation-delay |
| 455 | Loading progress bars + shimmer effect |
| 456 | Ripple button effect with CSS custom properties |
| 457 | Count-up animation with setInterval |
| 458 | Toast notification system with transition |
| 459 | Accordion with rotate icon |
| 460 | Workshop: AnimationKit full interactive showcase |

**Part ถัดไป:** Part 47 — Responsive Design Mastery (Steps 461–470)
