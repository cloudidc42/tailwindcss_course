# Part 44: File Manager & Media Gallery

## เป้าหมาย
- สร้าง File Manager ระดับ Cloud Storage
- Grid/List view, Breadcrumb, Context Menu
- Media Gallery: masonry, lightbox, upload
- Steps 431–440

---

## Step 431: File Manager Layout

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>File Manager - Step 431</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 h-screen flex overflow-hidden text-gray-900">

  <!-- Sidebar -->
  <aside class="w-52 bg-white border-r flex flex-col flex-shrink-0 p-3">
    <div class="mb-4">
      <button class="w-full bg-indigo-600 text-white rounded-xl py-2.5 text-sm font-semibold hover:bg-indigo-700 transition-colors flex items-center justify-center gap-2">
        <span>+</span> อัพโหลด
      </button>
    </div>

    <!-- Navigation -->
    <nav class="space-y-0.5 mb-4">
      <a href="#" class="flex items-center gap-2.5 px-3 py-2 rounded-xl bg-indigo-50 text-indigo-700 text-sm font-medium">
        <span>🏠</span> ทั้งหมด
      </a>
      <a href="#" class="flex items-center gap-2.5 px-3 py-2 rounded-xl text-gray-600 hover:bg-gray-50 text-sm transition-colors">
        <span>⭐</span> ที่ติดดาว
      </a>
      <a href="#" class="flex items-center gap-2.5 px-3 py-2 rounded-xl text-gray-600 hover:bg-gray-50 text-sm transition-colors">
        <span>🕐</span> ล่าสุด
      </a>
      <a href="#" class="flex items-center gap-2.5 px-3 py-2 rounded-xl text-gray-600 hover:bg-gray-50 text-sm transition-colors">
        <span>👥</span> แชร์กับฉัน
      </a>
      <a href="#" class="flex items-center gap-2.5 px-3 py-2 rounded-xl text-gray-600 hover:bg-gray-50 text-sm transition-colors">
        <span>🗑</span> ถังขยะ
      </a>
    </nav>

    <hr class="mb-3">

    <!-- Folders -->
    <p class="text-[11px] font-bold text-gray-400 uppercase tracking-wider px-2 mb-1.5">โฟลเดอร์</p>
    <div class="space-y-0.5">
      <button class="w-full flex items-center gap-2 px-2 py-1.5 rounded-lg text-sm text-left text-gray-600 hover:bg-gray-50 transition-colors">
        <span>📁</span> Projects
        <span class="ml-auto text-[11px] text-gray-400">12</span>
      </button>
      <button class="w-full flex items-center gap-2 px-2 py-1.5 rounded-lg text-sm text-left text-gray-600 hover:bg-gray-50 transition-colors">
        <span>📁</span> Designs
        <span class="ml-auto text-[11px] text-gray-400">8</span>
      </button>
      <button class="w-full flex items-center gap-2 px-2 py-1.5 rounded-lg text-sm text-left text-gray-600 hover:bg-gray-50 transition-colors">
        <span>📁</span> Documents
        <span class="ml-auto text-[11px] text-gray-400">24</span>
      </button>
    </div>

    <!-- Storage bar -->
    <div class="mt-auto pt-4 border-t">
      <div class="flex justify-between text-[11px] text-gray-500 mb-1.5">
        <span>Storage</span><span>7.2 / 15 GB</span>
      </div>
      <div class="h-1.5 bg-gray-100 rounded-full">
        <div class="h-1.5 bg-indigo-500 rounded-full" style="width:48%"></div>
      </div>
      <button class="mt-2 w-full text-center text-xs text-indigo-600 hover:underline">อัพเกรดพื้นที่</button>
    </div>
  </aside>

  <!-- Main Content -->
  <div class="flex-1 flex flex-col min-w-0 overflow-hidden">

    <!-- Toolbar -->
    <div class="bg-white border-b px-5 h-14 flex items-center gap-3 flex-shrink-0">
      <!-- Breadcrumb -->
      <div class="flex items-center gap-1 text-sm flex-1 min-w-0">
        <button class="text-gray-500 hover:text-indigo-600 transition-colors">My Drive</button>
        <span class="text-gray-300">›</span>
        <button class="text-gray-500 hover:text-indigo-600 transition-colors">Projects</button>
        <span class="text-gray-300">›</span>
        <span class="font-semibold text-gray-800 truncate">TechShop 2024</span>
      </div>

      <div class="flex items-center gap-2">
        <div class="relative">
          <svg class="absolute left-2.5 top-1/2 -translate-y-1/2 w-3.5 h-3.5 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"/></svg>
          <input type="text" placeholder="ค้นหาไฟล์..." class="border border-gray-200 rounded-xl pl-8 pr-3 py-1.5 text-xs focus:outline-none focus:ring-2 focus:ring-indigo-500 w-40">
        </div>
        <!-- View toggle -->
        <div class="flex border rounded-xl overflow-hidden">
          <button id="grid-btn" onclick="setView('grid')" class="p-1.5 bg-indigo-50 text-indigo-600 hover:bg-indigo-100 transition-colors">
            <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 20 20"><path d="M5 3a2 2 0 00-2 2v2a2 2 0 002 2h2a2 2 0 002-2V5a2 2 0 00-2-2H5zM5 11a2 2 0 00-2 2v2a2 2 0 002 2h2a2 2 0 002-2v-2a2 2 0 00-2-2H5zM11 5a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2V5zM11 13a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2v-2z"/></svg>
          </button>
          <button id="list-btn" onclick="setView('list')" class="p-1.5 text-gray-400 hover:bg-gray-50 transition-colors">
            <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 10h16M4 14h16M4 18h16"/></svg>
          </button>
        </div>
        <button class="text-xs text-indigo-600 border border-indigo-200 px-2.5 py-1.5 rounded-xl hover:bg-indigo-50 transition-colors">+ โฟลเดอร์ใหม่</button>
      </div>
    </div>

    <!-- Files Grid -->
    <div class="flex-1 overflow-y-auto p-5">
      <p class="text-xs font-bold text-gray-400 uppercase tracking-wide mb-3">โฟลเดอร์</p>
      <div id="grid-view" class="grid grid-cols-3 sm:grid-cols-4 md:grid-cols-6 gap-3 mb-6">
        <!-- Folder items -->
        <div ondblclick="openFolder(this)" class="cursor-pointer group text-center p-3 rounded-xl hover:bg-indigo-50 transition-colors">
          <div class="text-4xl mb-1.5 group-hover:scale-110 transition-transform">📁</div>
          <p class="text-[11px] font-medium truncate">Wireframes</p>
          <p class="text-[10px] text-gray-400 mt-0.5">8 items</p>
        </div>
        <div ondblclick="openFolder(this)" class="cursor-pointer group text-center p-3 rounded-xl hover:bg-indigo-50 transition-colors">
          <div class="text-4xl mb-1.5 group-hover:scale-110 transition-transform">📁</div>
          <p class="text-[11px] font-medium truncate">Assets</p>
          <p class="text-[10px] text-gray-400 mt-0.5">42 items</p>
        </div>
      </div>

      <p class="text-xs font-bold text-gray-400 uppercase tracking-wide mb-3">ไฟล์</p>
      <div id="file-grid" class="grid grid-cols-3 sm:grid-cols-4 md:grid-cols-6 gap-3">
        <div class="cursor-pointer group text-center p-3 rounded-xl hover:bg-blue-50 transition-colors relative" oncontextmenu="showCtxMenu(event)">
          <div class="text-4xl mb-1.5 group-hover:scale-110 transition-transform">📄</div>
          <p class="text-[11px] font-medium truncate">spec_v3.pdf</p>
          <p class="text-[10px] text-gray-400 mt-0.5">1.8 MB</p>
        </div>
        <div class="cursor-pointer group text-center p-3 rounded-xl hover:bg-blue-50 transition-colors relative" oncontextmenu="showCtxMenu(event)">
          <div class="text-4xl mb-1.5 group-hover:scale-110 transition-transform">🖼️</div>
          <p class="text-[11px] font-medium truncate">banner.png</p>
          <p class="text-[10px] text-gray-400 mt-0.5">2.4 MB</p>
        </div>
        <div class="cursor-pointer group text-center p-3 rounded-xl hover:bg-blue-50 transition-colors relative" oncontextmenu="showCtxMenu(event)">
          <div class="text-4xl mb-1.5 group-hover:scale-110 transition-transform">📊</div>
          <p class="text-[11px] font-medium truncate">analytics.xlsx</p>
          <p class="text-[10px] text-gray-400 mt-0.5">450 KB</p>
        </div>
        <div class="cursor-pointer group text-center p-3 rounded-xl hover:bg-blue-50 transition-colors relative" oncontextmenu="showCtxMenu(event)">
          <div class="text-4xl mb-1.5 group-hover:scale-110 transition-transform">🎬</div>
          <p class="text-[11px] font-medium truncate">demo.mp4</p>
          <p class="text-[10px] text-gray-400 mt-0.5">84 MB</p>
        </div>
        <div class="cursor-pointer group text-center p-3 rounded-xl hover:bg-blue-50 transition-colors ring-2 ring-indigo-500 bg-indigo-50 relative" oncontextmenu="showCtxMenu(event)">
          <div class="absolute top-1 right-1 w-4 h-4 bg-indigo-600 rounded-full flex items-center justify-center text-white text-[10px]">✓</div>
          <div class="text-4xl mb-1.5">📝</div>
          <p class="text-[11px] font-medium truncate text-indigo-700">notes.md</p>
          <p class="text-[10px] text-gray-400 mt-0.5">12 KB</p>
        </div>
      </div>

      <!-- List View (hidden by default) -->
      <div id="list-view" class="hidden">
        <table class="w-full text-sm">
          <thead><tr class="border-b text-xs text-gray-400"><th class="pb-2 text-left font-semibold">ชื่อ</th><th class="pb-2 text-right font-semibold hidden md:table-cell">ขนาด</th><th class="pb-2 text-right font-semibold hidden md:table-cell">แก้ไขล่าสุด</th></tr></thead>
          <tbody class="divide-y">
            <tr class="hover:bg-gray-50 cursor-pointer transition-colors"><td class="py-2.5"><div class="flex items-center gap-2">📄 <span>spec_v3.pdf</span></div></td><td class="py-2.5 text-right text-gray-500 hidden md:table-cell">1.8 MB</td><td class="py-2.5 text-right text-gray-500 hidden md:table-cell">วันนี้</td></tr>
            <tr class="hover:bg-gray-50 cursor-pointer transition-colors"><td class="py-2.5"><div class="flex items-center gap-2">🖼️ <span>banner.png</span></div></td><td class="py-2.5 text-right text-gray-500 hidden md:table-cell">2.4 MB</td><td class="py-2.5 text-right text-gray-500 hidden md:table-cell">เมื่อวาน</td></tr>
            <tr class="hover:bg-gray-50 cursor-pointer transition-colors"><td class="py-2.5"><div class="flex items-center gap-2">📊 <span>analytics.xlsx</span></div></td><td class="py-2.5 text-right text-gray-500 hidden md:table-cell">450 KB</td><td class="py-2.5 text-right text-gray-500 hidden md:table-cell">3 วันที่แล้ว</td></tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>

  <!-- Context Menu -->
  <div id="ctx-menu" class="hidden fixed bg-white border shadow-xl rounded-xl py-1.5 z-50 w-44">
    <button class="w-full text-left px-4 py-2 text-sm hover:bg-gray-50 transition-colors flex items-center gap-2">📖 เปิด</button>
    <button class="w-full text-left px-4 py-2 text-sm hover:bg-gray-50 transition-colors flex items-center gap-2">🔗 คัดลอก link</button>
    <button class="w-full text-left px-4 py-2 text-sm hover:bg-gray-50 transition-colors flex items-center gap-2">⭐ ติดดาว</button>
    <button class="w-full text-left px-4 py-2 text-sm hover:bg-gray-50 transition-colors flex items-center gap-2">👥 แชร์</button>
    <button class="w-full text-left px-4 py-2 text-sm hover:bg-gray-50 transition-colors flex items-center gap-2">✏️ เปลี่ยนชื่อ</button>
    <button class="w-full text-left px-4 py-2 text-sm hover:bg-gray-50 transition-colors flex items-center gap-2">📥 ดาวน์โหลด</button>
    <hr class="my-1">
    <button class="w-full text-left px-4 py-2 text-sm text-red-500 hover:bg-red-50 transition-colors flex items-center gap-2">🗑 ลบ</button>
  </div>

  <script>
    document.addEventListener('click', () => document.getElementById('ctx-menu').classList.add('hidden'));

    function showCtxMenu(e) {
      e.preventDefault();
      const menu = document.getElementById('ctx-menu');
      menu.style.top = e.clientY + 'px';
      menu.style.left = e.clientX + 'px';
      menu.classList.remove('hidden');
    }

    function setView(v) {
      document.getElementById('grid-view').classList.toggle('hidden', v === 'list');
      document.getElementById('file-grid').classList.toggle('hidden', v === 'list');
      document.getElementById('list-view').classList.toggle('hidden', v !== 'list');
      document.getElementById('grid-btn').className = `p-1.5 ${v==='grid'?'bg-indigo-50 text-indigo-600':'text-gray-400 hover:bg-gray-50'} transition-colors`;
      document.getElementById('list-btn').className = `p-1.5 ${v==='list'?'bg-indigo-50 text-indigo-600':'text-gray-400 hover:bg-gray-50'} transition-colors`;
    }

    function openFolder(el) {
      const name = el.querySelector('p').textContent;
      document.querySelector('.truncate.font-semibold').textContent = name;
    }
  </script>
</body>
</html>
```

---

## Steps 432–439: Upload, Lightbox, Media Gallery

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Media Gallery & Upload - Steps 432-439</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-6">
  <div class="max-w-5xl mx-auto space-y-6">

    <!-- Upload Drop Zone (Step 432) -->
    <div class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4">Upload Files</h3>
      <div id="drop-zone"
        class="border-2 border-dashed border-gray-200 rounded-2xl p-10 text-center transition-all duration-200 cursor-pointer hover:border-indigo-400 hover:bg-indigo-50"
        ondragover="dragOver(event)" ondragleave="dragLeave(event)" ondrop="handleDrop(event)" onclick="document.getElementById('file-input').click()">
        <div class="text-5xl mb-3">📤</div>
        <p class="font-semibold text-gray-700 mb-1">ลากไฟล์มาวาง หรือ คลิกเพื่อเลือก</p>
        <p class="text-sm text-gray-400">PNG, JPG, PDF, MP4 · สูงสุด 100MB ต่อไฟล์</p>
        <input type="file" id="file-input" class="hidden" multiple onchange="handleFiles(this.files)">
      </div>
      <!-- Upload progress list -->
      <div id="upload-list" class="mt-4 space-y-2"></div>
    </div>

    <!-- Media Gallery with Masonry (Step 433-435) -->
    <div class="bg-white rounded-2xl border p-5">
      <div class="flex items-center justify-between mb-4">
        <h3 class="font-bold">Media Gallery</h3>
        <div class="flex gap-2 text-sm">
          <button class="bg-indigo-100 text-indigo-700 px-3 py-1 rounded-lg font-medium text-xs">ทั้งหมด</button>
          <button class="text-gray-500 px-3 py-1 rounded-lg hover:bg-gray-100 text-xs transition-colors">รูปภาพ</button>
          <button class="text-gray-500 px-3 py-1 rounded-lg hover:bg-gray-100 text-xs transition-colors">วิดีโอ</button>
        </div>
      </div>

      <!-- Masonry Grid -->
      <div class="columns-2 md:columns-3 lg:columns-4 gap-3 space-y-3">
        <div class="break-inside-avoid rounded-xl overflow-hidden cursor-pointer group relative"
          onclick="openLightbox(0)">
          <div class="bg-gradient-to-br from-blue-400 to-indigo-600 h-32 flex items-center justify-center text-5xl group-hover:scale-105 transition-transform duration-300">🏔️</div>
          <div class="absolute inset-0 bg-black/0 group-hover:bg-black/20 transition-all duration-200 flex items-center justify-center opacity-0 group-hover:opacity-100">
            <span class="text-white text-2xl">🔍</span>
          </div>
        </div>
        <div class="break-inside-avoid rounded-xl overflow-hidden cursor-pointer group relative"
          onclick="openLightbox(1)">
          <div class="bg-gradient-to-br from-pink-400 to-rose-600 h-48 flex items-center justify-center text-5xl group-hover:scale-105 transition-transform duration-300">🌸</div>
          <div class="absolute inset-0 bg-black/0 group-hover:bg-black/20 transition-all duration-200 flex items-center justify-center opacity-0 group-hover:opacity-100">
            <span class="text-white text-2xl">🔍</span>
          </div>
        </div>
        <div class="break-inside-avoid rounded-xl overflow-hidden cursor-pointer group relative"
          onclick="openLightbox(2)">
          <div class="bg-gradient-to-br from-green-400 to-emerald-600 h-40 flex items-center justify-center text-5xl group-hover:scale-105 transition-transform duration-300">🌿</div>
          <div class="absolute inset-0 bg-black/0 group-hover:bg-black/20 transition-all duration-200 flex items-center justify-center opacity-0 group-hover:opacity-100">
            <span class="text-white text-2xl">🔍</span>
          </div>
        </div>
        <div class="break-inside-avoid rounded-xl overflow-hidden cursor-pointer group relative"
          onclick="openLightbox(3)">
          <div class="bg-gradient-to-br from-yellow-400 to-orange-600 h-36 flex items-center justify-center text-5xl group-hover:scale-105 transition-transform duration-300">☀️</div>
          <div class="absolute inset-0 bg-black/0 group-hover:bg-black/20 transition-all duration-200 flex items-center justify-center opacity-0 group-hover:opacity-100">
            <span class="text-white text-2xl">🔍</span>
          </div>
        </div>
        <div class="break-inside-avoid rounded-xl overflow-hidden cursor-pointer group relative"
          onclick="openLightbox(4)">
          <div class="bg-gradient-to-br from-purple-400 to-violet-600 h-52 flex items-center justify-center text-5xl group-hover:scale-105 transition-transform duration-300">🌌</div>
          <div class="absolute inset-0 bg-black/0 group-hover:bg-black/20 transition-all duration-200 flex items-center justify-center opacity-0 group-hover:opacity-100">
            <span class="text-white text-2xl">🔍</span>
          </div>
        </div>
        <div class="break-inside-avoid rounded-xl overflow-hidden cursor-pointer group relative"
          onclick="openLightbox(5)">
          <div class="bg-gradient-to-br from-cyan-400 to-teal-600 h-44 flex items-center justify-center text-5xl group-hover:scale-105 transition-transform duration-300">🌊</div>
          <div class="absolute inset-0 bg-black/0 group-hover:bg-black/20 transition-all duration-200 flex items-center justify-center opacity-0 group-hover:opacity-100">
            <span class="text-white text-2xl">🔍</span>
          </div>
        </div>
      </div>
    </div>

  </div>

  <!-- Lightbox Modal -->
  <div id="lightbox" class="hidden fixed inset-0 bg-black/90 z-50 flex items-center justify-center p-4" onclick="closeLightbox()">
    <button class="absolute top-4 right-4 text-white text-2xl hover:text-gray-300 transition-colors z-10" onclick="closeLightbox()">✕</button>
    <button class="absolute left-4 top-1/2 -translate-y-1/2 text-white text-4xl hover:text-gray-300 transition-colors z-10" onclick="navLightbox(-1, event)">‹</button>
    <button class="absolute right-4 top-1/2 -translate-y-1/2 text-white text-4xl hover:text-gray-300 transition-colors z-10" onclick="navLightbox(1, event)">›</button>

    <div id="lightbox-content" class="max-w-2xl max-h-[80vh] flex items-center justify-center" onclick="event.stopPropagation()">
      <!-- Content filled by JS -->
    </div>

    <!-- Caption -->
    <div class="absolute bottom-4 left-0 right-0 text-center text-white text-sm" id="lightbox-caption"></div>
  </div>

  <script>
    const media = [
      {emoji:'🏔️', from:'from-blue-400', to:'to-indigo-600', label:'Mountain Scene.jpg'},
      {emoji:'🌸', from:'from-pink-400', to:'to-rose-600', label:'Cherry Blossom.jpg'},
      {emoji:'🌿', from:'from-green-400', to:'to-emerald-600', label:'Nature Shot.jpg'},
      {emoji:'☀️', from:'from-yellow-400', to:'to-orange-600', label:'Sunset.jpg'},
      {emoji:'🌌', from:'from-purple-400', to:'to-violet-600', label:'Galaxy.jpg'},
      {emoji:'🌊', from:'from-cyan-400', to:'to-teal-600', label:'Ocean Wave.jpg'},
    ];
    let currentMedia = 0;

    function openLightbox(idx) {
      currentMedia = idx;
      updateLightbox();
      document.getElementById('lightbox').classList.remove('hidden');
    }
    function closeLightbox() {
      document.getElementById('lightbox').classList.add('hidden');
    }
    function navLightbox(dir, e) {
      e.stopPropagation();
      currentMedia = (currentMedia + dir + media.length) % media.length;
      updateLightbox();
    }
    function updateLightbox() {
      const m = media[currentMedia];
      document.getElementById('lightbox-content').innerHTML = `
        <div class="bg-gradient-to-br ${m.from} ${m.to} w-80 h-64 rounded-2xl flex items-center justify-center text-8xl shadow-2xl">${m.emoji}</div>
      `;
      document.getElementById('lightbox-caption').textContent = `${m.label} · ${currentMedia+1} / ${media.length}`;
    }

    // Upload zone
    function dragOver(e) {
      e.preventDefault();
      document.getElementById('drop-zone').classList.add('border-indigo-500', 'bg-indigo-50');
    }
    function dragLeave(e) {
      document.getElementById('drop-zone').classList.remove('border-indigo-500', 'bg-indigo-50');
    }
    function handleDrop(e) {
      e.preventDefault();
      dragLeave(e);
      handleFiles(e.dataTransfer.files);
    }
    function handleFiles(files) {
      const list = document.getElementById('upload-list');
      [...files].forEach(f => {
        const item = document.createElement('div');
        item.className = 'flex items-center gap-3 bg-gray-50 rounded-xl p-3 border';
        item.innerHTML = `
          <span class="text-xl">📄</span>
          <div class="flex-1 min-w-0">
            <div class="flex justify-between text-xs mb-1">
              <span class="font-medium truncate">${f.name}</span>
              <span class="text-gray-400 ml-2 flex-shrink-0">${(f.size/1024).toFixed(0)} KB</span>
            </div>
            <div class="h-1.5 bg-gray-200 rounded-full overflow-hidden">
              <div class="h-1.5 bg-indigo-500 rounded-full animate-upload-bar" style="width:0%" data-target="100"></div>
            </div>
          </div>
          <span class="text-green-500 text-sm hidden done-icon">✓</span>
        `;
        list.appendChild(item);
        const bar = item.querySelector('[data-target]');
        const done = item.querySelector('.done-icon');
        let w = 0;
        const iv = setInterval(() => {
          w = Math.min(w + Math.random() * 20, 100);
          bar.style.width = w + '%';
          if (w >= 100) {
            clearInterval(iv);
            bar.classList.remove('bg-indigo-500');
            bar.classList.add('bg-green-500');
            done.classList.remove('hidden');
          }
        }, 200);
      });
    }
  </script>
</body>
</html>
```

---

## Step 440: Workshop — Full File Manager + Gallery

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>File Manager Workshop - Step 440</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 h-screen flex overflow-hidden text-gray-900 text-sm">

  <!-- Sidebar -->
  <aside class="w-52 bg-white border-r p-3 flex flex-col flex-shrink-0">
    <h2 class="font-extrabold text-base mb-4 px-2">MyDrive</h2>
    <nav class="space-y-0.5 mb-5">
      <button onclick="setSection('files')" class="nav-btn w-full text-left flex items-center gap-2 px-3 py-2 rounded-xl text-xs bg-indigo-50 text-indigo-700 font-medium transition-colors">📁 ไฟล์ทั้งหมด</button>
      <button onclick="setSection('gallery')" class="nav-btn w-full text-left flex items-center gap-2 px-3 py-2 rounded-xl text-xs text-gray-600 hover:bg-gray-50 transition-colors">🖼️ Gallery</button>
      <button onclick="setSection('upload')" class="nav-btn w-full text-left flex items-center gap-2 px-3 py-2 rounded-xl text-xs text-gray-600 hover:bg-gray-50 transition-colors">📤 Upload</button>
    </nav>
    <div class="mt-auto border-t pt-3">
      <div class="flex justify-between text-[11px] text-gray-500 mb-1.5">
        <span>Storage</span><span id="storage-text">7.2 / 15 GB</span>
      </div>
      <div class="h-1.5 bg-gray-100 rounded-full"><div class="h-1.5 bg-indigo-500 rounded-full" style="width:48%"></div></div>
    </div>
  </aside>

  <!-- Main -->
  <main class="flex-1 flex flex-col min-w-0 overflow-hidden bg-gray-50">

    <!-- Files Section -->
    <div id="sec-files">
      <div class="bg-white border-b px-5 h-12 flex items-center gap-3 flex-shrink-0">
        <h3 class="font-bold">ไฟล์ทั้งหมด</h3>
        <div class="ml-auto flex items-center gap-2">
          <input type="text" placeholder="ค้นหา..." oninput="filterFiles(this.value)" class="border rounded-xl px-3 py-1.5 text-xs focus:outline-none focus:ring-2 focus:ring-indigo-500">
          <button onclick="setView('grid')" class="p-1.5 bg-indigo-50 text-indigo-600 rounded-lg">⊞</button>
          <button onclick="setView('list')" class="p-1.5 text-gray-400 hover:bg-gray-100 rounded-lg transition-colors">☰</button>
        </div>
      </div>
      <div class="p-5 overflow-y-auto flex-1" style="height: calc(100vh - 3rem)">
        <div id="file-container" class="grid grid-cols-4 md:grid-cols-6 gap-3"></div>
      </div>
    </div>

    <!-- Gallery Section -->
    <div id="sec-gallery" class="hidden p-5 overflow-y-auto">
      <h3 class="font-bold mb-4">Photo Gallery</h3>
      <div class="columns-2 md:columns-4 gap-3 space-y-3" id="gallery-masonry"></div>
    </div>

    <!-- Upload Section -->
    <div id="sec-upload" class="hidden p-5">
      <h3 class="font-bold mb-4">Upload</h3>
      <div class="border-2 border-dashed border-gray-300 rounded-2xl p-10 text-center bg-white hover:border-indigo-400 hover:bg-indigo-50 transition-all cursor-pointer" onclick="document.getElementById('ws-file-input').click()">
        <div class="text-4xl mb-2">📤</div>
        <p class="font-semibold text-gray-700">คลิกเพื่อเลือกไฟล์</p>
        <p class="text-xs text-gray-400 mt-1">ไฟล์ทุกประเภท · สูงสุด 100MB</p>
        <input type="file" id="ws-file-input" class="hidden" multiple onchange="handleWsFiles(this.files)">
      </div>
      <div id="ws-upload-list" class="mt-4 space-y-2"></div>
    </div>

  </main>

  <!-- Lightbox -->
  <div id="lb" class="hidden fixed inset-0 bg-black/90 z-50 flex items-center justify-center" onclick="document.getElementById('lb').classList.add('hidden')">
    <div id="lb-inner" class="text-8xl"></div>
    <button class="absolute top-4 right-4 text-white text-xl">✕</button>
  </div>

  <script>
    const files = [
      {name:'proposal.pdf', icon:'📄', size:'1.8 MB', type:'doc'},
      {name:'banner.png', icon:'🖼️', size:'2.4 MB', type:'img'},
      {name:'analytics.xlsx', icon:'📊', size:'450 KB', type:'doc'},
      {name:'demo.mp4', icon:'🎬', size:'84 MB', type:'vid'},
      {name:'notes.md', icon:'📝', size:'12 KB', type:'doc'},
      {name:'logo.svg', icon:'🎨', size:'28 KB', type:'img'},
      {name:'backup.zip', icon:'🗜️', size:'124 MB', type:'doc'},
      {name:'photo1.jpg', icon:'📷', size:'3.1 MB', type:'img'},
    ];
    const galleryItems = ['🏔️','🌸','🌿','☀️','🌌','🌊','🦋','🌺'];
    const galColors = ['from-blue-400 to-indigo-600','from-pink-400 to-rose-600','from-green-400 to-emerald-600','from-yellow-400 to-orange-600','from-purple-400 to-violet-600','from-cyan-400 to-teal-600','from-amber-400 to-yellow-600','from-red-400 to-pink-600'];
    const galHeights = [32, 48, 40, 36, 52, 44, 36, 48];

    let viewMode = 'grid';

    function renderFiles(data) {
      const c = document.getElementById('file-container');
      if (viewMode === 'grid') {
        c.className = 'grid grid-cols-4 md:grid-cols-6 gap-3';
        c.innerHTML = data.map(f => `
          <div class="bg-white rounded-xl p-3 text-center cursor-pointer hover:shadow-md transition-shadow border border-transparent hover:border-indigo-200" oncontextmenu="event.preventDefault()">
            <div class="text-3xl mb-1.5">${f.icon}</div>
            <p class="text-[11px] font-medium truncate">${f.name}</p>
            <p class="text-[10px] text-gray-400">${f.size}</p>
          </div>
        `).join('');
      } else {
        c.className = '';
        c.innerHTML = `<table class="w-full text-xs"><thead><tr class="border-b text-gray-400"><th class="pb-2 text-left">ชื่อ</th><th class="pb-2 text-right">ขนาด</th></tr></thead><tbody class="divide-y">${data.map(f => `<tr class="hover:bg-white cursor-pointer transition-colors"><td class="py-2">${f.icon} ${f.name}</td><td class="py-2 text-right text-gray-500">${f.size}</td></tr>`).join('')}</tbody></table>`;
      }
    }

    function filterFiles(q) {
      renderFiles(files.filter(f => f.name.toLowerCase().includes(q.toLowerCase())));
    }

    function setView(v) {
      viewMode = v;
      renderFiles(files);
    }

    function renderGallery() {
      const g = document.getElementById('gallery-masonry');
      g.innerHTML = galleryItems.map((e, i) => `
        <div class="break-inside-avoid mb-3 rounded-xl overflow-hidden cursor-pointer group relative" onclick="openLb('${e}')">
          <div class="bg-gradient-to-br ${galColors[i%galColors.length]} h-${galHeights[i]} flex items-center justify-center text-4xl group-hover:scale-105 transition-transform duration-300">${e}</div>
          <div class="absolute inset-0 bg-black/0 group-hover:bg-black/20 transition-all flex items-center justify-center opacity-0 group-hover:opacity-100"><span class="text-white text-2xl">🔍</span></div>
        </div>
      `).join('');
    }

    function openLb(emoji) {
      document.getElementById('lb-inner').textContent = emoji;
      document.getElementById('lb').classList.remove('hidden');
    }

    function setSection(s) {
      ['files','gallery','upload'].forEach(id => document.getElementById('sec-' + id).classList.add('hidden'));
      document.getElementById('sec-' + s).classList.remove('hidden');
      document.querySelectorAll('.nav-btn').forEach(b => b.className = 'nav-btn w-full text-left flex items-center gap-2 px-3 py-2 rounded-xl text-xs text-gray-600 hover:bg-gray-50 transition-colors');
      event.currentTarget.className = 'nav-btn w-full text-left flex items-center gap-2 px-3 py-2 rounded-xl text-xs bg-indigo-50 text-indigo-700 font-medium transition-colors';
    }

    function handleWsFiles(files) {
      const list = document.getElementById('ws-upload-list');
      [...files].forEach(f => {
        const div = document.createElement('div');
        div.className = 'flex items-center gap-3 bg-white border rounded-xl p-3';
        div.innerHTML = `<span>📄</span><div class="flex-1"><div class="flex justify-between text-xs mb-1"><span class="truncate font-medium">${f.name}</span><span class="text-gray-400">${(f.size/1024).toFixed(0)}KB</span></div><div class="h-1.5 bg-gray-100 rounded-full"><div class="h-1.5 bg-indigo-500 rounded-full transition-all duration-200" style="width:0%" id="bar-${f.name.replace(/\W/g,'_')}"></div></div></div>`;
        list.appendChild(div);
        let w = 0;
        const id = f.name.replace(/\W/g,'_');
        const iv = setInterval(() => {
          w = Math.min(w + Math.random() * 25, 100);
          const bar = document.getElementById('bar-' + id);
          if (bar) { bar.style.width = w + '%'; if (w >= 100) { bar.classList.replace('bg-indigo-500','bg-green-500'); clearInterval(iv); } }
        }, 250);
      });
    }

    renderFiles(files);
    renderGallery();
  </script>
</body>
</html>
```

---

## สรุป Part 44

| Step | เนื้อหา |
|------|---------|
| 431 | File Manager: sidebar, breadcrumb, grid/list toggle, context menu |
| 432 | Upload drop zone: drag-and-drop, progress bars |
| 433–435 | Masonry media gallery with hover overlay |
| 436–438 | Lightbox: open, close, navigate with keyboard-like navigation |
| 439 | File filtering, multi-select, storage meter |
| 440 | Workshop: Full File Manager + Gallery + Upload app |

**Part ถัดไป:** Part 45 — Design System Components (Steps 441–450)
