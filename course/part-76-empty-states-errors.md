# Part 76: Empty States & Error Pages

## เป้าหมาย
- Empty state patterns — no data, no results, first use
- Error pages — 404, 500, maintenance, offline
- Skeleton loading states
- Steps 751–760

---

## Steps 751–760: Empty States & Error Pages Workshop

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Empty States & Errors - Steps 751-760</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    @keyframes float   { 0%,100%{transform:translateY(0)} 50%{transform:translateY(-10px)} }
    @keyframes shake   { 0%,100%{transform:translateX(0)} 25%{transform:translateX(-6px)} 75%{transform:translateX(6px)} }
    @keyframes fadeIn  { from{opacity:0;transform:translateY(12px)} to{opacity:1;transform:translateY(0)} }
    @keyframes shimmer { 0%{background-position:-200% 0} 100%{background-position:200% 0} }
    .float-anim  { animation: float  3s ease-in-out infinite; }
    .shake-anim  { animation: shake  0.5s ease; }
    .fade-in-up  { animation: fadeIn 0.5s ease forwards; }
    .shimmer-bg  { background:linear-gradient(90deg,#f3f4f6 25%,#e9eaec 50%,#f3f4f6 75%); background-size:200%; animation:shimmer 1.5s infinite; }
  </style>
</head>
<body class="bg-gray-100 min-h-screen p-6">

  <div class="max-w-5xl mx-auto space-y-6">
    <h1 class="text-2xl font-extrabold text-gray-900">Empty States & Error Pages</h1>

    <!-- Tab nav -->
    <div class="flex gap-2 flex-wrap">
      <button onclick="showSection('empty')" id="btn-empty" class="px-4 py-2 text-sm font-semibold bg-indigo-600 text-white rounded-xl">Empty States</button>
      <button onclick="showSection('errors')" id="btn-errors" class="px-4 py-2 text-sm font-semibold bg-white border text-gray-600 rounded-xl hover:bg-gray-50">Error Pages</button>
      <button onclick="showSection('skeleton')" id="btn-skeleton" class="px-4 py-2 text-sm font-semibold bg-white border text-gray-600 rounded-xl hover:bg-gray-50">Skeleton</button>
    </div>

    <!-- Empty States Section -->
    <div id="section-empty" class="space-y-5">

      <!-- No Data -->
      <div class="bg-white rounded-2xl border p-6">
        <p class="text-xs font-semibold text-gray-400 uppercase mb-4">Step 751 — No Data State</p>
        <div class="flex flex-col items-center py-10 fade-in-up">
          <div class="float-anim text-5xl mb-4">📂</div>
          <h3 class="font-bold text-gray-800 text-lg mb-1">ยังไม่มีข้อมูล</h3>
          <p class="text-sm text-gray-500 mb-5 text-center max-w-xs">เริ่มสร้างข้อมูลชิ้นแรกของคุณ<br>มันง่ายมากกว่าที่คิด!</p>
          <button class="bg-indigo-600 text-white px-5 py-2.5 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors flex items-center gap-1.5">
            <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4"/></svg>
            สร้างรายการแรก
          </button>
        </div>
      </div>

      <!-- No Search Results -->
      <div class="bg-white rounded-2xl border p-6">
        <p class="text-xs font-semibold text-gray-400 uppercase mb-4">Step 752 — No Search Results</p>
        <div class="mb-4">
          <div class="relative">
            <svg class="absolute left-3 top-1/2 -translate-y-1/2 w-4 h-4 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"/></svg>
            <input value="xyzqwerty123" readonly class="w-full max-w-xs pl-9 pr-4 py-2 border border-gray-300 rounded-xl text-sm bg-gray-50">
          </div>
        </div>
        <div class="flex flex-col items-center py-8">
          <div class="text-5xl mb-4">🔍</div>
          <h3 class="font-bold text-gray-800 mb-1">ไม่พบผลลัพธ์</h3>
          <p class="text-sm text-gray-500 mb-4 text-center">ลองค้นหาด้วยคำอื่น<br>หรือตรวจสอบการสะกด</p>
          <div class="flex gap-2 flex-wrap justify-center">
            <button class="text-xs font-semibold text-indigo-600 bg-indigo-50 px-3 py-1.5 rounded-lg hover:bg-indigo-100 transition-colors">ล้างการค้นหา</button>
            <button class="text-xs font-semibold text-gray-600 bg-gray-100 px-3 py-1.5 rounded-lg hover:bg-gray-200 transition-colors">ดูทั้งหมด</button>
          </div>
        </div>
      </div>

      <!-- First Use -->
      <div class="bg-white rounded-2xl border p-6">
        <p class="text-xs font-semibold text-gray-400 uppercase mb-4">Step 753 — First Use / Onboarding</p>
        <div class="flex flex-col sm:flex-row items-center gap-6 py-4">
          <div class="float-anim text-6xl">🎉</div>
          <div>
            <h3 class="font-extrabold text-gray-900 text-lg mb-2">ยินดีต้อนรับ!</h3>
            <p class="text-sm text-gray-600 mb-4">คุณยังไม่ได้สร้างโปรเจกต์ใดๆ เลย<br>เริ่มต้นด้วย 3 ขั้นตอนง่ายๆ:</p>
            <div class="space-y-2">
              <div class="flex items-center gap-2 text-sm"><span class="w-5 h-5 bg-emerald-500 text-white rounded-full flex items-center justify-center text-xs font-bold flex-shrink-0">1</span> สร้างโปรเจกต์ใหม่</div>
              <div class="flex items-center gap-2 text-sm text-gray-400"><span class="w-5 h-5 bg-gray-200 text-gray-500 rounded-full flex items-center justify-center text-xs font-bold flex-shrink-0">2</span> เชิญสมาชิกทีม</div>
              <div class="flex items-center gap-2 text-sm text-gray-400"><span class="w-5 h-5 bg-gray-200 text-gray-500 rounded-full flex items-center justify-center text-xs font-bold flex-shrink-0">3</span> เริ่มทำงาน</div>
            </div>
            <button class="mt-4 bg-indigo-600 text-white px-5 py-2.5 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">เริ่มเลย →</button>
          </div>
        </div>
      </div>

      <!-- Empty Inbox -->
      <div class="bg-white rounded-2xl border p-6">
        <p class="text-xs font-semibold text-gray-400 uppercase mb-4">Step 754 — Empty Inbox</p>
        <div class="flex flex-col items-center py-8">
          <div class="w-20 h-20 bg-blue-50 rounded-full flex items-center justify-center text-4xl mb-4 float-anim">✉️</div>
          <h3 class="font-bold text-gray-800 mb-1">กล่องจดหมายว่าง</h3>
          <p class="text-sm text-gray-500">ไม่มีข้อความใหม่ ✅</p>
          <p class="text-xs text-gray-400 mt-1">เราจะแจ้งเตือนเมื่อมีข้อความใหม่</p>
        </div>
      </div>
    </div>

    <!-- Error Pages Section -->
    <div id="section-errors" class="hidden space-y-5">

      <!-- 404 -->
      <div class="bg-white rounded-2xl border overflow-hidden">
        <p class="text-xs font-semibold text-gray-400 uppercase px-6 pt-5 mb-0">Step 755 — 404 Not Found</p>
        <div class="flex flex-col items-center px-6 py-10 text-center">
          <div class="relative mb-6">
            <p class="text-8xl font-black text-gray-100 select-none">404</p>
            <div class="absolute inset-0 flex items-center justify-center text-5xl float-anim">🛸</div>
          </div>
          <h2 class="text-2xl font-extrabold text-gray-900 mb-2">หน้านี้หายไปแล้ว</h2>
          <p class="text-gray-500 mb-6 max-w-sm">ดูเหมือน UFO จะพาหน้านี้ไปแล้ว<br>ลองกลับหน้าหลักหรือค้นหาสิ่งที่ต้องการ</p>
          <div class="flex gap-3 flex-wrap justify-center">
            <button class="bg-indigo-600 text-white px-5 py-2.5 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">← กลับหน้าหลัก</button>
            <button class="border border-gray-300 text-gray-700 px-5 py-2.5 rounded-xl text-sm font-semibold hover:bg-gray-50 transition-colors">ค้นหา</button>
          </div>
        </div>
      </div>

      <!-- 500 -->
      <div class="bg-gray-900 rounded-2xl border border-gray-700 overflow-hidden">
        <p class="text-xs font-semibold text-gray-500 uppercase px-6 pt-5 mb-0">Step 756 — 500 Server Error</p>
        <div class="flex flex-col items-center px-6 py-10 text-center">
          <div class="text-5xl mb-4 shake-anim" id="fireEmoji" onclick="document.getElementById('fireEmoji').style.animation='shake 0.5s ease'">🔥</div>
          <h2 class="text-2xl font-extrabold text-white mb-2">เซิร์ฟเวอร์มีปัญหา</h2>
          <p class="text-gray-400 mb-2">เกิดข้อผิดพลาดที่ไม่คาดคิด ทีมงานกำลังแก้ไข</p>
          <div class="bg-gray-800 rounded-xl px-4 py-2 mb-6 font-mono text-xs text-red-400">Error 500: Internal Server Error</div>
          <div class="flex gap-3 flex-wrap justify-center">
            <button class="bg-white text-gray-900 px-5 py-2.5 rounded-xl text-sm font-semibold hover:bg-gray-100 transition-colors">↺ ลองใหม่</button>
            <button class="border border-gray-600 text-gray-300 px-5 py-2.5 rounded-xl text-sm font-semibold hover:bg-gray-800 transition-colors">รายงานปัญหา</button>
          </div>
        </div>
      </div>

      <!-- Maintenance -->
      <div class="bg-gradient-to-br from-indigo-900 to-purple-900 rounded-2xl overflow-hidden">
        <p class="text-xs font-semibold text-indigo-300 uppercase px-6 pt-5 mb-0">Step 757 — Maintenance Mode</p>
        <div class="flex flex-col items-center px-6 py-10 text-center">
          <div class="text-5xl mb-4 float-anim">🔧</div>
          <h2 class="text-2xl font-extrabold text-white mb-2">กำลังปรับปรุงระบบ</h2>
          <p class="text-indigo-200 mb-4">เราจะกลับมาเร็วๆ นี้ ขอโทษในความไม่สะดวก</p>
          <div class="bg-white/10 rounded-xl px-5 py-3 mb-5">
            <p class="text-white/60 text-xs mb-1">คาดว่าจะเสร็จ</p>
            <p class="text-white font-bold text-lg" id="maintenanceTimer">02:30:00</p>
          </div>
          <div class="flex items-center gap-2 text-sm text-indigo-200">
            <input type="email" placeholder="แจ้งเตือนเมื่อกลับมา" class="bg-white/10 border border-white/20 rounded-xl px-4 py-2 text-sm text-white placeholder-white/40 focus:outline-none focus:border-white/50 w-48">
            <button class="bg-white text-indigo-900 px-4 py-2 rounded-xl text-sm font-semibold hover:bg-indigo-50 transition-colors">แจ้ง</button>
          </div>
        </div>
      </div>

      <!-- Offline -->
      <div class="bg-white rounded-2xl border">
        <p class="text-xs font-semibold text-gray-400 uppercase px-6 pt-5 mb-0">Step 758 — Offline / No Connection</p>
        <div class="flex flex-col items-center px-6 py-10 text-center">
          <div class="w-20 h-20 bg-gray-100 rounded-full flex items-center justify-center text-4xl mb-4">📡</div>
          <h2 class="text-xl font-extrabold text-gray-900 mb-2">ไม่มีการเชื่อมต่ออินเทอร์เน็ต</h2>
          <p class="text-sm text-gray-500 mb-5">ตรวจสอบ WiFi หรือข้อมูลมือถือของคุณ</p>
          <div class="flex gap-2 mb-4">
            <div class="h-1.5 w-3 bg-gray-200 rounded-full"></div>
            <div class="h-1.5 w-5 bg-gray-200 rounded-full"></div>
            <div class="h-1.5 w-8 bg-gray-200 rounded-full"></div>
            <div class="h-1.5 w-12 bg-gray-200 rounded-full"></div>
          </div>
          <button id="retryBtn" onclick="simulateRetry()" class="bg-indigo-600 text-white px-5 py-2.5 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">↺ ลองเชื่อมต่อใหม่</button>
        </div>
      </div>
    </div>

    <!-- Skeleton Section -->
    <div id="section-skeleton" class="hidden space-y-5">

      <!-- Card skeleton -->
      <div class="bg-white rounded-2xl border p-5">
        <p class="text-xs font-semibold text-gray-400 uppercase mb-4">Step 759 — Card Skeleton</p>
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
          <div class="border rounded-xl p-4 space-y-3">
            <div class="shimmer-bg h-32 rounded-xl"></div>
            <div class="shimmer-bg h-4 rounded-full w-3/4"></div>
            <div class="shimmer-bg h-3 rounded-full w-full"></div>
            <div class="shimmer-bg h-3 rounded-full w-5/6"></div>
            <div class="flex gap-2">
              <div class="shimmer-bg h-8 flex-1 rounded-xl"></div>
              <div class="shimmer-bg h-8 w-8 rounded-xl"></div>
            </div>
          </div>
          <div class="border rounded-xl p-4 space-y-3">
            <div class="shimmer-bg h-32 rounded-xl"></div>
            <div class="shimmer-bg h-4 rounded-full w-2/3"></div>
            <div class="shimmer-bg h-3 rounded-full w-full"></div>
            <div class="shimmer-bg h-3 rounded-full w-3/4"></div>
            <div class="flex gap-2">
              <div class="shimmer-bg h-8 flex-1 rounded-xl"></div>
              <div class="shimmer-bg h-8 w-8 rounded-xl"></div>
            </div>
          </div>
        </div>
      </div>

      <!-- Table skeleton -->
      <div class="bg-white rounded-2xl border p-5">
        <p class="text-xs font-semibold text-gray-400 uppercase mb-4">Step 760 — Table Skeleton</p>
        <div class="space-y-3">
          <div class="flex gap-4">
            <div class="shimmer-bg h-4 flex-1 rounded-full"></div>
            <div class="shimmer-bg h-4 flex-1 rounded-full"></div>
            <div class="shimmer-bg h-4 flex-1 rounded-full"></div>
            <div class="shimmer-bg h-4 w-20 rounded-full"></div>
          </div>
          <div class="border-b"></div>
          ${[1,2,3,4,5].map(() => `
            <div class="flex items-center gap-4 py-1">
              <div class="shimmer-bg w-8 h-8 rounded-full flex-shrink-0"></div>
              <div class="shimmer-bg h-3 flex-1 rounded-full"></div>
              <div class="shimmer-bg h-3 flex-1 rounded-full"></div>
              <div class="shimmer-bg h-3 flex-1 rounded-full"></div>
              <div class="shimmer-bg h-6 w-16 rounded-full flex-shrink-0"></div>
            </div>
          `).join('')}
        </div>
      </div>
    </div>

  </div>

  <script>
    function showSection(name) {
      ['empty','errors','skeleton'].forEach(s => {
        document.getElementById('section-' + s).classList.add('hidden');
        const btn = document.getElementById('btn-' + s);
        btn.className = `px-4 py-2 text-sm font-semibold rounded-xl ${s === name ? 'bg-indigo-600 text-white' : 'bg-white border text-gray-600 hover:bg-gray-50'}`;
      });
      document.getElementById('section-' + name).classList.remove('hidden');
    }

    function simulateRetry() {
      const btn = document.getElementById('retryBtn');
      btn.innerHTML = '<svg class="w-4 h-4 animate-spin inline mr-1" fill="none" viewBox="0 0 24 24"><circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle><path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4z"></path></svg> กำลังเชื่อมต่อ...';
      btn.disabled = true;
      setTimeout(() => {
        btn.innerHTML = '↺ ลองเชื่อมต่อใหม่';
        btn.disabled = false;
      }, 2000);
    }

    // Maintenance timer countdown
    let seconds = 9000;
    setInterval(() => {
      seconds--;
      const h = Math.floor(seconds/3600);
      const m = Math.floor((seconds%3600)/60);
      const s = seconds % 60;
      const el = document.getElementById('maintenanceTimer');
      if (el) el.textContent = `${String(h).padStart(2,'0')}:${String(m).padStart(2,'0')}:${String(s).padStart(2,'0')}`;
    }, 1000);
  </script>
</body>
</html>
```

---

## สรุป Part 76

| Step | เนื้อหา |
|------|---------|
| 751 | No data empty state — floating icon, CTA button |
| 752 | No search results — search box + clear action |
| 753 | First use onboarding — checklist steps |
| 754 | Empty inbox — reassurance copy |
| 755 | 404 page — UFO floating over large number |
| 756 | 500 server error — dark bg, fire icon, error code |
| 757 | Maintenance mode — countdown timer, email subscribe |
| 758 | Offline state — wifi signal visual, retry button |
| 759 | Card skeleton shimmer loading |
| 760 | Table skeleton shimmer loading |

**Part ถัดไป:** Part 77 — Accessibility Deep Dive (Steps 761–770)
