# Part 72: Notification Center

## เป้าหมาย
- Notification bell + dropdown
- Toast stack (top-right)
- Notification types — info/success/warning/error
- Mark read / clear all
- Steps 711–720

---

## Steps 711–720: Notification Center Workshop

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Notification Center - Steps 711-720</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    @keyframes toastIn  { from { opacity:0; transform:translateX(100%); } to { opacity:1; transform:translateX(0); } }
    @keyframes toastOut { from { opacity:1; transform:translateX(0); } to { opacity:0; transform:translateX(110%); } }
    .toast-in  { animation: toastIn  0.3s ease forwards; }
    .toast-out { animation: toastOut 0.3s ease forwards; }
    @keyframes bellRing { 0%,100%{rotate:0} 25%{rotate:12deg} 75%{rotate:-12deg} }
    .bell-ring { animation: bellRing 0.5s ease; }
    @keyframes dropIn { from { opacity:0; transform:translateY(-8px) scale(0.97); } to { opacity:1; transform:translateY(0) scale(1); } }
    .drop-in { animation: dropIn 0.2s ease forwards; }
  </style>
</head>
<body class="bg-gray-100 min-h-screen">

  <!-- Toast Container (top-right) -->
  <div id="toastStack" class="fixed top-4 right-4 z-[100] flex flex-col gap-2 w-80"></div>

  <!-- App -->
  <div class="max-w-4xl mx-auto p-6">
    <!-- Header with notification bell -->
    <div class="flex items-center justify-between mb-8 bg-white rounded-2xl px-5 py-3 border">
      <h1 class="font-extrabold text-gray-900">Notification Center</h1>
      <div class="flex items-center gap-3">
        <!-- Notification Bell -->
        <div class="relative">
          <button id="bellBtn" onclick="toggleNotifPanel()" class="w-10 h-10 flex items-center justify-center rounded-xl hover:bg-gray-100 transition-colors relative">
            <span id="bellIcon" class="text-xl">🔔</span>
            <span id="notifBadge" class="absolute top-1 right-1 w-4 h-4 bg-red-500 text-white text-[9px] font-bold rounded-full flex items-center justify-center">0</span>
          </button>

          <!-- Notification Dropdown -->
          <div id="notifPanel" class="hidden drop-in absolute right-0 top-12 w-80 bg-white rounded-2xl shadow-2xl border z-50 overflow-hidden">
            <div class="flex items-center justify-between px-4 py-3 border-b">
              <div class="flex items-center gap-2">
                <h3 class="font-bold text-gray-900 text-sm">การแจ้งเตือน</h3>
                <span id="unreadBadge" class="text-xs font-bold bg-indigo-600 text-white px-1.5 py-0.5 rounded-full">0</span>
              </div>
              <button onclick="markAllRead()" class="text-xs text-indigo-600 hover:underline">อ่านทั้งหมด</button>
            </div>
            <div id="notifList" class="max-h-80 overflow-y-auto"></div>
            <div class="px-4 py-2.5 border-t flex justify-between">
              <button onclick="clearAll()" class="text-xs text-red-500 hover:underline">ลบทั้งหมด</button>
              <button onclick="viewAll()" class="text-xs text-indigo-600 hover:underline">ดูทั้งหมด →</button>
            </div>
          </div>
        </div>

        <div class="w-8 h-8 bg-indigo-200 rounded-full flex items-center justify-center text-sm font-bold text-indigo-700">ส</div>
      </div>
    </div>

    <!-- Demo Controls -->
    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 mb-6">
      <button onclick="addNotif('success')" class="flex items-center gap-2.5 bg-white border rounded-2xl px-4 py-3 hover:shadow-md transition-shadow text-left">
        <div class="w-9 h-9 bg-emerald-100 rounded-xl flex items-center justify-center text-lg">✅</div>
        <div>
          <p class="text-sm font-semibold text-gray-900">Success Toast</p>
          <p class="text-xs text-gray-400">แจ้งสำเร็จ</p>
        </div>
      </button>
      <button onclick="addNotif('error')" class="flex items-center gap-2.5 bg-white border rounded-2xl px-4 py-3 hover:shadow-md transition-shadow text-left">
        <div class="w-9 h-9 bg-red-100 rounded-xl flex items-center justify-center text-lg">❌</div>
        <div>
          <p class="text-sm font-semibold text-gray-900">Error Toast</p>
          <p class="text-xs text-gray-400">แจ้งข้อผิดพลาด</p>
        </div>
      </button>
      <button onclick="addNotif('warning')" class="flex items-center gap-2.5 bg-white border rounded-2xl px-4 py-3 hover:shadow-md transition-shadow text-left">
        <div class="w-9 h-9 bg-yellow-100 rounded-xl flex items-center justify-center text-lg">⚠️</div>
        <div>
          <p class="text-sm font-semibold text-gray-900">Warning Toast</p>
          <p class="text-xs text-gray-400">แจ้งเตือน</p>
        </div>
      </button>
      <button onclick="addNotif('info')" class="flex items-center gap-2.5 bg-white border rounded-2xl px-4 py-3 hover:shadow-md transition-shadow text-left">
        <div class="w-9 h-9 bg-blue-100 rounded-xl flex items-center justify-center text-lg">ℹ️</div>
        <div>
          <p class="text-sm font-semibold text-gray-900">Info Toast</p>
          <p class="text-xs text-gray-400">แจ้งข้อมูล</p>
        </div>
      </button>
    </div>

    <!-- Type demos -->
    <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 mb-6">
      <button onclick="triggerPush()" class="bg-white border rounded-2xl px-4 py-3 hover:shadow-md transition-shadow text-left flex items-center gap-3">
        <div class="w-10 h-10 bg-purple-100 rounded-xl flex items-center justify-center text-xl">📱</div>
        <div>
          <p class="text-sm font-bold text-gray-900">Push Notification</p>
          <p class="text-xs text-gray-400">แจ้งเตือนแบบ persistent + action buttons</p>
        </div>
      </button>
      <button onclick="triggerUpdate()" class="bg-white border rounded-2xl px-4 py-3 hover:shadow-md transition-shadow text-left flex items-center gap-3">
        <div class="w-10 h-10 bg-indigo-100 rounded-xl flex items-center justify-center text-xl">🔄</div>
        <div>
          <p class="text-sm font-bold text-gray-900">Loading → Success</p>
          <p class="text-xs text-gray-400">Update toast ในตัวเดิม</p>
        </div>
      </button>
    </div>

    <!-- Notification log -->
    <div class="bg-white rounded-2xl border p-5">
      <div class="flex items-center justify-between mb-4">
        <h2 class="font-bold text-gray-900">All Notifications</h2>
        <div class="flex gap-2">
          <button onclick="filterNotifs('all')" id="filter-all" class="text-xs font-semibold px-3 py-1.5 bg-indigo-600 text-white rounded-xl">ทั้งหมด</button>
          <button onclick="filterNotifs('unread')" id="filter-unread" class="text-xs font-semibold px-3 py-1.5 bg-white border text-gray-500 rounded-xl hover:bg-gray-50">ยังไม่อ่าน</button>
        </div>
      </div>
      <div id="allNotifList" class="space-y-2"></div>
      <div id="emptyNotif" class="hidden text-center py-8 text-gray-400">
        <p class="text-3xl mb-2">🔔</p>
        <p class="text-sm">ไม่มีการแจ้งเตือน</p>
      </div>
    </div>
  </div>

  <script>
    let notifications = [];
    let notifFilter = 'all';
    let panelOpen = false;

    const configs = {
      success: { icon:'✅', title:'สำเร็จ!', msg:'ดำเนินการเรียบร้อยแล้ว', bg:'bg-white', border:'border-emerald-300', iconBg:'bg-emerald-100', prog:'bg-emerald-500' },
      error:   { icon:'❌', title:'เกิดข้อผิดพลาด', msg:'ไม่สามารถดำเนินการได้ กรุณาลองใหม่', bg:'bg-white', border:'border-red-300', iconBg:'bg-red-100', prog:'bg-red-500' },
      warning: { icon:'⚠️', title:'คำเตือน', msg:'มีสิ่งที่ต้องตรวจสอบ', bg:'bg-white', border:'border-yellow-300', iconBg:'bg-yellow-100', prog:'bg-yellow-400' },
      info:    { icon:'ℹ️', title:'ข้อมูล', msg:'มีการอัปเดตใหม่สำหรับคุณ', bg:'bg-white', border:'border-blue-300', iconBg:'bg-blue-100', prog:'bg-blue-500' },
    };

    function addNotif(type) {
      const c = configs[type];
      const id = Date.now();
      const notif = { id, type, icon: c.icon, title: c.title, msg: c.msg, time: 'เมื่อกี้', read: false };
      notifications.unshift(notif);

      showToast(id, c, c.msg);
      ringBell();
      renderPanel();
      renderAllList();
    }

    function showToast(id, c, msg, actions) {
      const el = document.createElement('div');
      el.id = `toast-${id}`;
      el.className = `toast-in flex items-start gap-3 ${c.bg} rounded-2xl shadow-lg border-l-4 ${c.border} px-4 py-3 relative`;
      el.innerHTML = `
        <div class="w-8 h-8 ${c.iconBg} rounded-xl flex items-center justify-center text-lg flex-shrink-0">${c.icon}</div>
        <div class="flex-1 min-w-0">
          <p class="text-sm font-semibold text-gray-900">${c.title}</p>
          <p id="toast-msg-${id}" class="text-xs text-gray-500 mt-0.5">${msg}</p>
          ${actions ? `<div class="flex gap-2 mt-2">${actions}</div>` : ''}
          <div class="h-0.5 ${c.prog} mt-2 rounded-full w-full" id="prog-${id}" style="transition:width 4s linear; width:100%"></div>
        </div>
        <button onclick="removeToast(${id})" class="text-gray-300 hover:text-gray-500 transition-colors flex-shrink-0 mt-0.5">✕</button>
      `;
      document.getElementById('toastStack').prepend(el);
      setTimeout(() => { const p = document.getElementById(`prog-${id}`); if(p) p.style.width = '0'; }, 50);
      setTimeout(() => removeToast(id), 4500);
    }

    function removeToast(id) {
      const el = document.getElementById(`toast-${id}`);
      if (!el) return;
      el.className = el.className.replace('toast-in', 'toast-out');
      setTimeout(() => el.remove(), 300);
    }

    function triggerPush() {
      const id = Date.now();
      const el = document.createElement('div');
      el.id = `toast-${id}`;
      el.className = 'toast-in bg-gray-900 text-white rounded-2xl shadow-xl px-4 py-3 flex items-start gap-3';
      el.innerHTML = `
        <div class="w-8 h-8 bg-purple-500 rounded-xl flex items-center justify-center text-lg flex-shrink-0">📱</div>
        <div class="flex-1 min-w-0">
          <p class="text-sm font-semibold">นภา มีส่งงานใหม่</p>
          <p class="text-xs text-gray-300 mt-0.5">ส่งรายงาน Q3 ให้คุณแล้ว</p>
          <div class="flex gap-2 mt-2.5">
            <button onclick="removeToast(${id})" class="text-xs font-semibold bg-white text-gray-900 px-3 py-1 rounded-lg hover:bg-gray-100 transition-colors">ดู</button>
            <button onclick="removeToast(${id})" class="text-xs font-semibold bg-white/10 text-white px-3 py-1 rounded-lg hover:bg-white/20 transition-colors">ปิด</button>
          </div>
        </div>
        <button onclick="removeToast(${id})" class="text-gray-400 hover:text-gray-200 transition-colors flex-shrink-0">✕</button>
      `;
      document.getElementById('toastStack').prepend(el);
      setTimeout(() => removeToast(id), 8000);

      // Also add to notification list
      notifications.unshift({ id, type: 'info', icon: '📱', title: 'นภา มีส่งงานใหม่', msg: 'ส่งรายงาน Q3 ให้คุณแล้ว', time: 'เมื่อกี้', read: false });
      ringBell(); renderPanel(); renderAllList();
    }

    function triggerUpdate() {
      const id = Date.now();
      const loadCfg = { icon:'⏳', title:'กำลังบันทึก...', bg:'bg-white', border:'border-indigo-300', iconBg:'bg-indigo-100', prog:'bg-indigo-500' };
      showToast(id, loadCfg, 'กรุณารอสักครู่...');
      setTimeout(() => {
        removeToast(id);
        const succCfg = configs['success'];
        showToast(id + 1, succCfg, 'บันทึกข้อมูลสำเร็จแล้ว!');
      }, 2000);
    }

    function ringBell() {
      const bell = document.getElementById('bellIcon');
      bell.className = 'text-xl bell-ring';
      setTimeout(() => bell.className = 'text-xl', 600);
      const badge = document.getElementById('notifBadge');
      const unread = notifications.filter(n => !n.read).length;
      badge.textContent = unread;
      badge.className = unread > 0 ? 'absolute top-1 right-1 w-4 h-4 bg-red-500 text-white text-[9px] font-bold rounded-full flex items-center justify-center' : 'hidden';
    }

    function toggleNotifPanel() {
      panelOpen = !panelOpen;
      const panel = document.getElementById('notifPanel');
      if (panelOpen) { panel.classList.remove('hidden'); }
      else panel.classList.add('hidden');
    }

    function renderPanel() {
      const unread = notifications.filter(n => !n.read);
      document.getElementById('unreadBadge').textContent = unread.length;
      const recent = notifications.slice(0, 6);
      if (recent.length === 0) {
        document.getElementById('notifList').innerHTML = '<p class="text-center text-xs text-gray-400 py-6">ไม่มีการแจ้งเตือน</p>';
        return;
      }
      document.getElementById('notifList').innerHTML = recent.map(n => `
        <div class="flex items-start gap-2.5 px-4 py-3 border-b last:border-0 cursor-pointer hover:bg-gray-50 transition-colors ${n.read ? 'opacity-60' : ''}" onclick="readNotif(${n.id})">
          <div class="w-7 h-7 bg-gray-100 rounded-full flex items-center justify-center text-sm flex-shrink-0">${n.icon}</div>
          <div class="flex-1 min-w-0">
            <p class="text-xs font-semibold text-gray-900">${n.title}</p>
            <p class="text-xs text-gray-500 truncate">${n.msg}</p>
            <p class="text-[10px] text-gray-400 mt-0.5">${n.time}</p>
          </div>
          ${!n.read ? '<span class="w-2 h-2 bg-indigo-500 rounded-full flex-shrink-0 mt-1"></span>' : ''}
        </div>
      `).join('');
    }

    function renderAllList() {
      const toShow = notifFilter === 'unread' ? notifications.filter(n => !n.read) : notifications;
      const empty = document.getElementById('emptyNotif');
      const list = document.getElementById('allNotifList');
      if (toShow.length === 0) { list.innerHTML = ''; empty.classList.remove('hidden'); return; }
      empty.classList.add('hidden');
      list.innerHTML = toShow.map(n => `
        <div class="flex items-start gap-3 p-3 rounded-xl ${n.read ? 'opacity-60' : 'bg-indigo-50'} hover:bg-gray-100 transition-colors cursor-pointer" onclick="readNotif(${n.id})">
          <div class="w-8 h-8 bg-white rounded-xl flex items-center justify-center text-lg flex-shrink-0 shadow-sm">${n.icon}</div>
          <div class="flex-1">
            <div class="flex justify-between items-start">
              <p class="text-sm font-semibold text-gray-900">${n.title}</p>
              <span class="text-xs text-gray-400 ml-2">${n.time}</span>
            </div>
            <p class="text-xs text-gray-500">${n.msg}</p>
          </div>
          ${!n.read ? '<span class="w-2 h-2 bg-indigo-500 rounded-full flex-shrink-0 mt-2"></span>' : ''}
        </div>
      `).join('');
    }

    function readNotif(id) {
      const n = notifications.find(n => n.id === id);
      if (n) n.read = true;
      ringBell(); renderPanel(); renderAllList();
    }

    function markAllRead() {
      notifications.forEach(n => n.read = true);
      ringBell(); renderPanel(); renderAllList();
    }

    function clearAll() {
      notifications = [];
      ringBell(); renderPanel(); renderAllList();
      toggleNotifPanel();
    }

    function filterNotifs(f) {
      notifFilter = f;
      ['all','unread'].forEach(k => {
        document.getElementById('filter-' + k).className = `text-xs font-semibold px-3 py-1.5 rounded-xl ${k===f ? 'bg-indigo-600 text-white' : 'bg-white border text-gray-500 hover:bg-gray-50'}`;
      });
      renderAllList();
    }

    function viewAll() { toggleNotifPanel(); }

    // Close panel when clicking outside
    document.addEventListener('click', e => {
      if (panelOpen && !document.getElementById('bellBtn').contains(e.target) && !document.getElementById('notifPanel').contains(e.target)) {
        panelOpen = false;
        document.getElementById('notifPanel').classList.add('hidden');
      }
    });

    // Seed some notifications
    setTimeout(() => {
      ['success','info','warning'].forEach((t,i) => setTimeout(() => addNotif(t), i * 200));
    }, 500);
  </script>
</body>
</html>
```

---

## สรุป Part 72

| Step | เนื้อหา |
|------|---------|
| 711 | Toast container — fixed top-right, stack |
| 712 | Toast types — success/error/warning/info |
| 713 | Auto-dismiss + progress bar |
| 714 | Notification bell + badge count |
| 715 | Dropdown panel — recent 6 notifications |
| 716 | Unread dot indicator + mark as read |
| 717 | Mark all read + clear all |
| 718 | Loading → success toast update |
| 719 | Push notification style (dark bg + action buttons) |
| 720 | Workshop: Complete Notification Center — bell/toast/panel/filter |

**Part ถัดไป:** Part 73 — Drag & Drop UI (Steps 721–730)
