# Part 70: Command Palette & Keyboard Navigation

## เป้าหมาย
- Command palette (⌘K) ด้วย keyboard navigation
- Fuzzy search + shortcut hints
- Keyboard trap + focus management
- Steps 691–700

---

## Steps 691–700: Command Palette Workshop

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Command Palette - Steps 691-700</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    @keyframes paletteIn { from { opacity:0; transform:translateY(-8px) scale(0.97); } to { opacity:1; transform:translateY(0) scale(1); } }
    .palette-in { animation: paletteIn 0.18s ease forwards; }
    .cmd-item.active { background: #eef2ff; }
    .cmd-item.active .cmd-icon { background: #c7d2fe; }
  </style>
</head>
<body class="bg-gray-100 min-h-screen">

  <!-- App shell -->
  <div class="flex h-screen">
    <!-- Sidebar -->
    <aside class="w-56 bg-white border-r flex flex-col py-4">
      <div class="px-4 mb-5 flex items-center gap-2">
        <div class="w-7 h-7 bg-indigo-600 rounded-lg flex items-center justify-center text-white text-sm">⌘</div>
        <span class="font-extrabold text-gray-900">WorkOS</span>
      </div>
      <nav class="flex-1 px-2 space-y-0.5">
        <a href="#" class="flex items-center gap-2.5 px-3 py-2 text-sm rounded-xl bg-indigo-50 text-indigo-700 font-medium">🏠 หน้าหลัก</a>
        <a href="#" class="flex items-center gap-2.5 px-3 py-2 text-sm rounded-xl text-gray-600 hover:bg-gray-100">📋 งาน</a>
        <a href="#" class="flex items-center gap-2.5 px-3 py-2 text-sm rounded-xl text-gray-600 hover:bg-gray-100">👥 ทีม</a>
        <a href="#" class="flex items-center gap-2.5 px-3 py-2 text-sm rounded-xl text-gray-600 hover:bg-gray-100">📊 รายงาน</a>
        <a href="#" class="flex items-center gap-2.5 px-3 py-2 text-sm rounded-xl text-gray-600 hover:bg-gray-100">⚙️ ตั้งค่า</a>
      </nav>
      <div class="px-3 py-2 border-t mt-2">
        <div class="flex items-center gap-2.5">
          <div class="w-8 h-8 bg-indigo-200 rounded-full flex items-center justify-center text-sm font-bold text-indigo-700">ส</div>
          <div class="flex-1 min-w-0">
            <p class="text-xs font-semibold text-gray-900 truncate">สมชาย วงค์</p>
            <p class="text-xs text-gray-500">sk@test.com</p>
          </div>
        </div>
      </div>
    </aside>

    <!-- Main -->
    <main class="flex-1 flex flex-col">
      <!-- Topbar -->
      <header class="h-12 bg-white border-b flex items-center px-5 justify-between">
        <div class="flex items-center gap-3">
          <span class="text-sm font-semibold text-gray-800">หน้าหลัก</span>
        </div>
        <button onclick="openPalette()" class="flex items-center gap-2 bg-gray-100 hover:bg-gray-200 border border-gray-200 rounded-xl px-3 py-1.5 text-xs text-gray-500 transition-colors">
          <svg class="w-3 h-3" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"/></svg>
          ค้นหา...
          <kbd class="bg-white border border-gray-300 rounded px-1 py-0.5 font-mono text-[10px] text-gray-500">⌘K</kbd>
        </button>
      </header>

      <!-- Content -->
      <div class="flex-1 p-6 overflow-y-auto">
        <div class="max-w-2xl mx-auto">
          <h1 class="text-2xl font-extrabold text-gray-900 mb-2">Command Palette Demo</h1>
          <p class="text-gray-500 mb-6">กด <kbd class="bg-gray-100 border border-gray-300 rounded px-1.5 py-0.5 font-mono text-sm">⌘K</kbd> หรือ <kbd class="bg-gray-100 border border-gray-300 rounded px-1.5 py-0.5 font-mono text-sm">Ctrl+K</kbd> เพื่อเปิด Command Palette</p>

          <div class="grid grid-cols-2 gap-4 mb-6">
            <div class="bg-white border rounded-2xl p-5 hover:shadow-md transition-shadow cursor-pointer" onclick="openPalette('งาน')">
              <div class="text-2xl mb-2">📋</div>
              <h3 class="font-bold text-gray-900 mb-1">จัดการงาน</h3>
              <p class="text-sm text-gray-500">สร้าง ค้นหา หรือ assign งาน</p>
            </div>
            <div class="bg-white border rounded-2xl p-5 hover:shadow-md transition-shadow cursor-pointer" onclick="openPalette('ทีม')">
              <div class="text-2xl mb-2">👥</div>
              <h3 class="font-bold text-gray-900 mb-1">ทีม</h3>
              <p class="text-sm text-gray-500">ค้นหาสมาชิก เชิญคนใหม่</p>
            </div>
            <div class="bg-white border rounded-2xl p-5 hover:shadow-md transition-shadow cursor-pointer" onclick="openPalette('ตั้งค่า')">
              <div class="text-2xl mb-2">⚙️</div>
              <h3 class="font-bold text-gray-900 mb-1">ตั้งค่า</h3>
              <p class="text-sm text-gray-500">โปรไฟล์ การแจ้งเตือน ธีม</p>
            </div>
            <div class="bg-white border rounded-2xl p-5 hover:shadow-md transition-shadow cursor-pointer" onclick="openPalette('รายงาน')">
              <div class="text-2xl mb-2">📊</div>
              <h3 class="font-bold text-gray-900 mb-1">รายงาน</h3>
              <p class="text-sm text-gray-500">ดูสถิติและ analytics</p>
            </div>
          </div>

          <div class="bg-indigo-50 border border-indigo-200 rounded-2xl p-5">
            <h3 class="font-bold text-indigo-800 mb-3">Keyboard Shortcuts</h3>
            <div class="grid grid-cols-2 gap-y-2.5 text-sm">
              <div class="flex items-center gap-2">
                <kbd class="bg-white border border-gray-300 rounded px-1.5 py-0.5 font-mono text-xs">⌘K</kbd>
                <span class="text-gray-600">เปิด Command Palette</span>
              </div>
              <div class="flex items-center gap-2">
                <kbd class="bg-white border border-gray-300 rounded px-1.5 py-0.5 font-mono text-xs">↑↓</kbd>
                <span class="text-gray-600">เลื่อน</span>
              </div>
              <div class="flex items-center gap-2">
                <kbd class="bg-white border border-gray-300 rounded px-1.5 py-0.5 font-mono text-xs">Enter</kbd>
                <span class="text-gray-600">เลือก</span>
              </div>
              <div class="flex items-center gap-2">
                <kbd class="bg-white border border-gray-300 rounded px-1.5 py-0.5 font-mono text-xs">Esc</kbd>
                <span class="text-gray-600">ปิด</span>
              </div>
            </div>
          </div>

          <!-- Recent actions log -->
          <div id="actionLog" class="mt-5 bg-white border rounded-2xl p-5">
            <h3 class="font-semibold text-gray-900 mb-3 text-sm">ประวัติการใช้งาน</h3>
            <p class="text-xs text-gray-400 italic">ยังไม่มีการดำเนินการ — ลอง Command Palette</p>
          </div>
        </div>
      </div>
    </main>
  </div>

  <!-- Overlay -->
  <div id="overlay" class="hidden fixed inset-0 bg-black/40 z-50 backdrop-blur-sm" onclick="closePalette()"></div>

  <!-- Command Palette Modal -->
  <div id="palette" class="hidden fixed top-[15%] left-1/2 -translate-x-1/2 z-50 w-full max-w-lg">
    <div class="palette-in bg-white rounded-2xl shadow-2xl overflow-hidden border border-gray-200">
      <!-- Search input -->
      <div class="flex items-center gap-2.5 px-4 py-3 border-b">
        <svg class="w-4 h-4 text-gray-400 flex-shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"/></svg>
        <input id="paletteInput" placeholder="พิมพ์คำสั่งหรือค้นหา..." autocomplete="off" spellcheck="false"
          class="flex-1 text-sm text-gray-900 placeholder-gray-400 focus:outline-none"
          oninput="filterCommands(this.value)"
          onkeydown="handleKey(event)">
        <kbd onclick="closePalette()" class="text-xs bg-gray-100 border border-gray-200 rounded px-1.5 py-0.5 text-gray-500 cursor-pointer hover:bg-gray-200">Esc</kbd>
      </div>

      <!-- Results -->
      <div id="paletteList" class="max-h-72 overflow-y-auto py-1.5"></div>

      <!-- Footer -->
      <div class="px-4 py-2 border-t flex items-center gap-3 text-[10px] text-gray-400">
        <span class="flex items-center gap-1"><kbd class="bg-gray-100 border border-gray-200 rounded px-1 font-mono">↑↓</kbd> เลื่อน</span>
        <span class="flex items-center gap-1"><kbd class="bg-gray-100 border border-gray-200 rounded px-1 font-mono">↵</kbd> เลือก</span>
        <span class="flex items-center gap-1"><kbd class="bg-gray-100 border border-gray-200 rounded px-1 font-mono">Esc</kbd> ปิด</span>
      </div>
    </div>
  </div>

  <script>
    const commands = [
      { id: 1,  icon: '➕', label: 'สร้างงานใหม่', shortcut: 'C', category: 'งาน', keywords: 'create task งาน' },
      { id: 2,  icon: '🔍', label: 'ค้นหางาน', shortcut: 'F', category: 'งาน', keywords: 'search find' },
      { id: 3,  icon: '✅', label: 'งานที่เสร็จแล้ว', shortcut: '', category: 'งาน', keywords: 'done completed' },
      { id: 4,  icon: '👤', label: 'โปรไฟล์ของฉัน', shortcut: 'P', category: 'ตั้งค่า', keywords: 'profile settings' },
      { id: 5,  icon: '🌙', label: 'สลับ Dark Mode', shortcut: 'D', category: 'ตั้งค่า', keywords: 'dark theme' },
      { id: 6,  icon: '🔔', label: 'การแจ้งเตือน', shortcut: '', category: 'ตั้งค่า', keywords: 'notification bell' },
      { id: 7,  icon: '📊', label: 'ดู Dashboard', shortcut: '', category: 'รายงาน', keywords: 'analytics chart' },
      { id: 8,  icon: '📈', label: 'รายงาน Revenue', shortcut: '', category: 'รายงาน', keywords: 'revenue report' },
      { id: 9,  icon: '👥', label: 'เชิญสมาชิก', shortcut: 'I', category: 'ทีม', keywords: 'invite member team' },
      { id: 10, icon: '🔐', label: 'ออกจากระบบ', shortcut: 'Q', category: 'บัญชี', keywords: 'logout signout' },
      { id: 11, icon: '📋', label: 'คัดลอกลิงก์', shortcut: '', category: 'การดำเนินการ', keywords: 'copy link share' },
      { id: 12, icon: '🗑',  label: 'ลบที่เลือก', shortcut: '', category: 'การดำเนินการ', keywords: 'delete remove' },
    ];

    let activeIdx = 0;
    let filtered = [...commands];
    let recentCmds = [];

    function openPalette(prefill = '') {
      document.getElementById('overlay').classList.remove('hidden');
      document.getElementById('palette').classList.remove('hidden');
      const input = document.getElementById('paletteInput');
      input.value = prefill;
      filterCommands(prefill);
      setTimeout(() => input.focus(), 50);
    }

    function closePalette() {
      document.getElementById('overlay').classList.add('hidden');
      document.getElementById('palette').classList.add('hidden');
      document.getElementById('paletteInput').value = '';
      activeIdx = 0;
    }

    function filterCommands(query) {
      const q = query.toLowerCase().trim();
      if (!q) {
        filtered = [...commands];
      } else {
        filtered = commands.filter(c =>
          c.label.toLowerCase().includes(q) ||
          c.category.toLowerCase().includes(q) ||
          c.keywords.toLowerCase().includes(q)
        );
      }
      activeIdx = 0;
      renderList();
    }

    function renderList() {
      if (filtered.length === 0) {
        document.getElementById('paletteList').innerHTML = `
          <div class="px-4 py-6 text-center text-sm text-gray-400">
            <p class="text-2xl mb-2">🔍</p>ไม่พบคำสั่ง
          </div>`;
        return;
      }

      const byCategory = {};
      filtered.forEach(c => {
        if (!byCategory[c.category]) byCategory[c.category] = [];
        byCategory[c.category].push(c);
      });

      let html = '', globalIdx = 0;
      for (const [cat, cmds] of Object.entries(byCategory)) {
        html += `<div class="px-3 pt-2 pb-0.5"><p class="text-[10px] font-semibold text-gray-400 uppercase tracking-wider">${cat}</p></div>`;
        cmds.forEach(c => {
          const isActive = globalIdx === activeIdx;
          html += `
            <div class="cmd-item mx-1.5 px-3 py-2 rounded-xl cursor-pointer flex items-center gap-3 ${isActive ? 'active' : 'hover:bg-gray-50'} transition-colors" 
                 data-idx="${globalIdx}" onclick="executeCommand(${c.id})" onmouseenter="activeIdx=${globalIdx}; renderList();">
              <div class="cmd-icon w-7 h-7 ${isActive ? 'bg-indigo-200' : 'bg-gray-100'} rounded-lg flex items-center justify-center text-sm flex-shrink-0 transition-colors">${c.icon}</div>
              <span class="flex-1 text-sm text-gray-800">${highlightMatch(c.label)}</span>
              ${c.shortcut ? `<kbd class="text-[10px] bg-gray-100 border border-gray-200 rounded px-1.5 py-0.5 text-gray-500 font-mono">${c.shortcut}</kbd>` : ''}
            </div>`;
          globalIdx++;
        });
      }
      document.getElementById('paletteList').innerHTML = html;
      // Scroll active into view
      const activeEl = document.querySelector(`.cmd-item[data-idx="${activeIdx}"]`);
      if (activeEl) activeEl.scrollIntoView({ block: 'nearest' });
    }

    function highlightMatch(text) {
      const q = document.getElementById('paletteInput').value.toLowerCase().trim();
      if (!q) return text;
      const idx = text.toLowerCase().indexOf(q);
      if (idx === -1) return text;
      return text.slice(0, idx) + `<mark class="bg-indigo-100 text-indigo-800 rounded">${text.slice(idx, idx + q.length)}</mark>` + text.slice(idx + q.length);
    }

    function handleKey(e) {
      if (e.key === 'ArrowDown') { e.preventDefault(); activeIdx = Math.min(activeIdx + 1, filtered.length - 1); renderList(); }
      else if (e.key === 'ArrowUp') { e.preventDefault(); activeIdx = Math.max(activeIdx - 1, 0); renderList(); }
      else if (e.key === 'Enter') { if (filtered[activeIdx]) executeCommand(filtered[activeIdx].id); }
      else if (e.key === 'Escape') { closePalette(); }
    }

    function executeCommand(id) {
      const cmd = commands.find(c => c.id === id);
      if (!cmd) return;
      closePalette();

      // Log action
      recentCmds.unshift({ ...cmd, time: new Date().toLocaleTimeString('th-TH') });
      if (recentCmds.length > 5) recentCmds.pop();

      const log = document.getElementById('actionLog');
      log.innerHTML = '<h3 class="font-semibold text-gray-900 mb-3 text-sm">ประวัติการใช้งาน</h3>' +
        recentCmds.map(c => `
          <div class="flex items-center gap-2.5 py-1.5 border-b last:border-0">
            <div class="w-6 h-6 bg-indigo-100 rounded-lg flex items-center justify-center text-sm">${c.icon}</div>
            <span class="flex-1 text-sm text-gray-700">${c.label}</span>
            <span class="text-xs text-gray-400">${c.time}</span>
          </div>`).join('');
    }

    // Keyboard shortcut to open
    document.addEventListener('keydown', e => {
      if ((e.metaKey || e.ctrlKey) && e.key === 'k') {
        e.preventDefault();
        if (document.getElementById('palette').classList.contains('hidden')) openPalette();
        else closePalette();
      }
    });
  </script>
</body>
</html>
```

---

## สรุป Part 70

| Step | เนื้อหา |
|------|---------|
| 691 | Command palette trigger — ⌘K / Ctrl+K |
| 692 | Search input พร้อม fuzzy match highlight |
| 693 | Grouped results by category |
| 694 | Keyboard navigation — ↑↓ / Enter / Esc |
| 695 | Active item highlighting |
| 696 | Shortcut hints ใน command items |
| 697 | Hover interaction + focus management |
| 698 | Execute command + action log |
| 699 | Backdrop blur overlay |
| 700 | Workshop: Complete Command Palette — search/nav/shortcuts/log |

**Part ถัดไป:** Part 71 — Data Tables (Steps 701–710)
