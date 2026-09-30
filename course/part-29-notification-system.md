# Part 29: Notification & Toast System
## Steps 281–290: ระบบแจ้งเตือนระดับมืออาชีพ

---

## 🎯 เป้าหมายของ Part นี้

- Toast notifications (success/error/warning/info)
- Toast positioning (top-right/bottom-right/etc.)
- Auto-dismiss with progress bar
- Stacked toasts
- Notification center/bell
- In-page alert banners
- Badge counts

---

## Step 281: Toast Component

```html
<style>
@keyframes toastIn { from { opacity: 0; transform: translateX(100%); } to { opacity: 1; transform: translateX(0); } }
@keyframes toastOut { from { opacity: 1; transform: translateX(0); } to { opacity: 0; transform: translateX(100%); } }
.toast-enter { animation: toastIn 0.3s ease-out; }
.toast-leave { animation: toastOut 0.3s ease-in forwards; }
</style>

<!-- Toast container — fixed top-right -->
<div id="toast-container" class="fixed top-4 right-4 z-50 flex flex-col gap-3 w-80 pointer-events-none"></div>

<script>
let toastCount = 0;

const toastConfig = {
  success: { bg: 'bg-green-50 border-green-200', icon: '✅', title: 'สำเร็จ', text: 'text-green-800', bar: 'bg-green-500' },
  error:   { bg: 'bg-red-50 border-red-200',     icon: '❌', title: 'เกิดข้อผิดพลาด', text: 'text-red-800',   bar: 'bg-red-500' },
  warning: { bg: 'bg-yellow-50 border-yellow-200', icon: '⚠️', title: 'คำเตือน', text: 'text-yellow-800', bar: 'bg-yellow-500' },
  info:    { bg: 'bg-blue-50 border-blue-200',    icon: 'ℹ️', title: 'ข้อมูล', text: 'text-blue-800',   bar: 'bg-blue-500' },
};

function showToast(type = 'success', message = '', duration = 4000) {
  const id = `toast-${++toastCount}`;
  const cfg = toastConfig[type];
  
  const el = document.createElement('div');
  el.id = id;
  el.className = `pointer-events-auto relative overflow-hidden rounded-2xl border shadow-lg ${cfg.bg} toast-enter`;
  el.innerHTML = `
    <div class="flex items-start gap-3 p-4">
      <span class="text-xl flex-none mt-0.5">${cfg.icon}</span>
      <div class="flex-1 min-w-0">
        <p class="font-semibold text-sm ${cfg.text}">${cfg.title}</p>
        <p class="text-xs text-gray-500 mt-0.5">${message}</p>
      </div>
      <button onclick="dismissToast('${id}')" class="text-gray-400 hover:text-gray-700 text-sm flex-none ml-2">✕</button>
    </div>
    <div class="absolute bottom-0 left-0 h-1 ${cfg.bar} rounded-full" id="${id}-bar" style="width:100%; transition: width ${duration}ms linear;"></div>
  `;
  
  document.getElementById('toast-container').appendChild(el);
  
  // Start progress bar
  requestAnimationFrame(() => {
    requestAnimationFrame(() => {
      const bar = document.getElementById(`${id}-bar`);
      if (bar) bar.style.width = '0%';
    });
  });
  
  // Auto dismiss
  const timer = setTimeout(() => dismissToast(id), duration);
  el._timer = timer;
}

function dismissToast(id) {
  const el = document.getElementById(id);
  if (!el) return;
  clearTimeout(el._timer);
  el.classList.remove('toast-enter');
  el.classList.add('toast-leave');
  setTimeout(() => el.remove(), 300);
}
</script>

<!-- Demo buttons -->
<div class="flex flex-wrap gap-2">
  <button onclick="showToast('success','บันทึกข้อมูลสำเร็จ')" class="px-4 py-2 bg-green-600 text-white text-sm font-semibold rounded-xl hover:bg-green-700">Success</button>
  <button onclick="showToast('error','ไม่สามารถเชื่อมต่อได้')" class="px-4 py-2 bg-red-600 text-white text-sm font-semibold rounded-xl hover:bg-red-700">Error</button>
  <button onclick="showToast('warning','พื้นที่เก็บข้อมูลเหลือน้อย')" class="px-4 py-2 bg-yellow-500 text-white text-sm font-semibold rounded-xl hover:bg-yellow-600">Warning</button>
  <button onclick="showToast('info','อัพเดทใหม่พร้อมใช้งาน')" class="px-4 py-2 bg-blue-600 text-white text-sm font-semibold rounded-xl hover:bg-blue-700">Info</button>
</div>
```

---

## Step 282: Notification Bell + Dropdown

```html
<!-- Bell button with badge -->
<div class="relative inline-block">
  <button
    onclick="toggleNotifPanel()"
    id="bell-btn"
    class="relative size-10 flex items-center justify-center rounded-xl bg-white border border-gray-200 hover:bg-gray-50 transition-colors shadow-sm"
  >
    <span class="text-lg">🔔</span>
    <span class="absolute -top-0.5 -right-0.5 size-5 bg-red-500 text-white text-[10px] font-bold rounded-full flex items-center justify-center border-2 border-white">3</span>
  </button>
  
  <!-- Notification dropdown panel -->
  <div id="notif-panel" class="hidden absolute right-0 top-12 w-80 bg-white rounded-2xl shadow-xl border border-gray-200 overflow-hidden z-30">
    <!-- Header -->
    <div class="flex items-center justify-between px-4 py-3 border-b border-gray-100">
      <h4 class="font-bold text-sm text-gray-900">การแจ้งเตือน</h4>
      <button class="text-xs text-indigo-600 hover:text-indigo-800 font-medium">ทำเครื่องหมายอ่านแล้วทั้งหมด</button>
    </div>
    
    <!-- Notification list -->
    <div class="max-h-72 overflow-y-auto divide-y divide-gray-50">
      <!-- Unread -->
      <div class="flex gap-3 px-4 py-3.5 bg-indigo-50/50 hover:bg-indigo-50 transition-colors cursor-pointer">
        <div class="size-8 rounded-full bg-green-100 flex items-center justify-center text-sm flex-none">✅</div>
        <div class="flex-1 min-w-0">
          <p class="text-xs font-semibold text-gray-900">สมชาย จบ Part 25 แล้ว</p>
          <p class="text-[11px] text-gray-400 mt-0.5">2 นาทีที่แล้ว</p>
        </div>
        <div class="size-2 bg-indigo-500 rounded-full mt-2 flex-none"></div>
      </div>
      
      <div class="flex gap-3 px-4 py-3.5 bg-indigo-50/50 hover:bg-indigo-50 transition-colors cursor-pointer">
        <div class="size-8 rounded-full bg-yellow-100 flex items-center justify-center text-sm flex-none">⭐</div>
        <div class="flex-1 min-w-0">
          <p class="text-xs font-semibold text-gray-900">รีวิวใหม่ 5 ดาวสำหรับคอร์สของคุณ</p>
          <p class="text-[11px] text-gray-400 mt-0.5">15 นาทีที่แล้ว</p>
        </div>
        <div class="size-2 bg-indigo-500 rounded-full mt-2 flex-none"></div>
      </div>
      
      <!-- Read -->
      <div class="flex gap-3 px-4 py-3.5 hover:bg-gray-50 transition-colors cursor-pointer">
        <div class="size-8 rounded-full bg-blue-100 flex items-center justify-center text-sm flex-none">💳</div>
        <div class="flex-1 min-w-0">
          <p class="text-xs font-medium text-gray-700">ชำระเงินสำเร็จ ฿2,490</p>
          <p class="text-[11px] text-gray-400 mt-0.5">1 ชั่วโมงที่แล้ว</p>
        </div>
      </div>
    </div>
    
    <!-- Footer -->
    <div class="px-4 py-3 border-t border-gray-100 text-center">
      <a href="#" class="text-xs text-indigo-600 font-medium hover:text-indigo-800">ดูการแจ้งเตือนทั้งหมด</a>
    </div>
  </div>
</div>

<script>
function toggleNotifPanel() {
  document.getElementById('notif-panel').classList.toggle('hidden');
}
document.addEventListener('click', e => {
  if (!e.target.closest('#bell-btn') && !e.target.closest('#notif-panel')) {
    document.getElementById('notif-panel').classList.add('hidden');
  }
});
</script>
```

---

## Step 283: Alert Banners

```html
<!-- In-page alert banners -->

<!-- Info banner -->
<div class="flex items-start gap-3 p-4 bg-blue-50 border border-blue-200 rounded-2xl text-sm">
  <span class="text-blue-500 text-base flex-none mt-0.5">ℹ️</span>
  <div class="flex-1">
    <p class="font-semibold text-blue-800">เวอร์ชันใหม่พร้อมให้ใช้งาน</p>
    <p class="text-blue-600 text-xs mt-0.5">อัพเดท Tailwind CSS v4.0 เพื่อรับฟีเจอร์ใหม่</p>
  </div>
  <button class="text-blue-400 hover:text-blue-700 flex-none">✕</button>
</div>

<!-- Warning banner -->
<div class="flex items-start gap-3 p-4 bg-yellow-50 border border-yellow-200 rounded-2xl text-sm">
  <span class="text-yellow-500 text-base flex-none mt-0.5">⚠️</span>
  <div class="flex-1">
    <p class="font-semibold text-yellow-800">บัญชีใกล้หมดอายุ</p>
    <p class="text-yellow-600 text-xs mt-0.5">Subscription จะหมดอายุใน 7 วัน <a href="#" class="underline">ต่ออายุเลย</a></p>
  </div>
  <button class="text-yellow-400 hover:text-yellow-700 flex-none">✕</button>
</div>

<!-- Error banner -->
<div class="flex items-start gap-3 p-4 bg-red-50 border border-red-200 rounded-2xl text-sm">
  <span class="text-red-500 text-base flex-none mt-0.5">❌</span>
  <div class="flex-1">
    <p class="font-semibold text-red-800">การชำระเงินล้มเหลว</p>
    <p class="text-red-600 text-xs mt-0.5">กรุณาอัพเดทข้อมูลบัตรเครดิตของท่าน</p>
  </div>
  <button class="text-red-400 hover:text-red-700 flex-none">✕</button>
</div>

<!-- Full-width top banner -->
<div class="bg-gradient-to-r from-indigo-600 to-purple-600 text-white px-4 py-2.5 flex items-center justify-center gap-3 text-sm">
  <span>🎉</span>
  <span>ลด 50% สำหรับคอร์สใหม่ รหัส: <strong>TAIL50</strong></span>
  <a href="#" class="bg-white text-indigo-600 px-3 py-0.5 rounded-lg text-xs font-bold hover:bg-indigo-50 transition-colors">ใช้โค้ด</a>
  <button class="ml-2 text-white/60 hover:text-white">✕</button>
</div>
```

---

## Step 284–290: Workshop — Complete Notification Demo

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Notification System</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    @keyframes tIn  { from { opacity:0; transform:translateX(100%); } to { opacity:1; transform:translateX(0); } }
    @keyframes tOut { from { opacity:1; transform:translateX(0); } to { opacity:0; transform:translateX(100%); } }
    .t-in  { animation: tIn  0.3s ease-out; }
    .t-out { animation: tOut 0.3s ease-in forwards; }
  </style>
</head>
<body class="bg-gray-50 min-h-screen">

<!-- Top announcement banner -->
<div id="top-banner" class="bg-gradient-to-r from-indigo-600 to-purple-600 text-white px-4 py-2.5 flex items-center justify-center gap-3 text-sm">
  <span>🎉</span>
  <span>ลด 50% วันนี้เท่านั้น รหัส: <strong>TAIL50</strong></span>
  <a href="#" class="bg-white/20 hover:bg-white/30 px-3 py-0.5 rounded-lg text-xs font-semibold transition-colors">ใช้โค้ด</a>
  <button onclick="document.getElementById('top-banner').remove()" class="ml-4 text-white/60 hover:text-white text-lg leading-none">✕</button>
</div>

<!-- Toast container -->
<div id="tc" class="fixed top-4 right-4 z-50 flex flex-col gap-2 w-80 pointer-events-none"></div>

<!-- Page content -->
<div class="max-w-3xl mx-auto p-6">
  <h1 class="text-3xl font-black text-gray-900 mb-2">Notification System</h1>
  <p class="text-gray-500 mb-8 text-sm">ทดสอบ notifications ประเภทต่างๆ</p>

  <!-- Inline alerts -->
  <div class="space-y-3 mb-8">
    <div class="flex items-center gap-3 p-4 bg-blue-50 border border-blue-200 rounded-2xl">
      <span class="text-lg">ℹ️</span>
      <div class="flex-1"><p class="font-semibold text-blue-800 text-sm">ข้อมูลสำคัญ</p><p class="text-blue-600 text-xs">เวอร์ชัน 4.0 พร้อมใช้งานแล้ว</p></div>
      <button onclick="this.closest('.flex').remove()" class="text-blue-300 hover:text-blue-600">✕</button>
    </div>
    <div class="flex items-center gap-3 p-4 bg-yellow-50 border border-yellow-200 rounded-2xl">
      <span class="text-lg">⚠️</span>
      <div class="flex-1"><p class="font-semibold text-yellow-800 text-sm">คำเตือน</p><p class="text-yellow-600 text-xs">พื้นที่เหลือน้อยกว่า 10%</p></div>
      <button onclick="this.closest('.flex').remove()" class="text-yellow-300 hover:text-yellow-600">✕</button>
    </div>
  </div>

  <!-- Toast buttons -->
  <div class="bg-white rounded-2xl border border-gray-200 p-6 mb-6">
    <h2 class="font-bold text-gray-900 mb-4">Toast Notifications</h2>
    <div class="flex flex-wrap gap-3">
      <button onclick="toast('success','บันทึกข้อมูลสำเร็จ!')" class="px-4 py-2.5 bg-green-600 hover:bg-green-700 text-white text-sm font-semibold rounded-xl transition-colors">✅ Success</button>
      <button onclick="toast('error','เชื่อมต่อล้มเหลว')" class="px-4 py-2.5 bg-red-600 hover:bg-red-700 text-white text-sm font-semibold rounded-xl transition-colors">❌ Error</button>
      <button onclick="toast('warning','พื้นที่เก็บข้อมูลเหลือน้อย')" class="px-4 py-2.5 bg-yellow-500 hover:bg-yellow-600 text-white text-sm font-semibold rounded-xl transition-colors">⚠️ Warning</button>
      <button onclick="toast('info','อัพเดทใหม่พร้อมแล้ว')" class="px-4 py-2.5 bg-blue-600 hover:bg-blue-700 text-white text-sm font-semibold rounded-xl transition-colors">ℹ️ Info</button>
    </div>
  </div>

  <!-- Notification center -->
  <div class="bg-white rounded-2xl border border-gray-200 overflow-hidden">
    <div class="flex items-center justify-between px-5 py-4 border-b border-gray-100">
      <div class="flex items-center gap-2">
        <h2 class="font-bold text-gray-900">การแจ้งเตือน</h2>
        <span class="size-5 bg-red-500 text-white text-[10px] font-bold rounded-full flex items-center justify-center">2</span>
      </div>
      <button class="text-xs text-indigo-600 font-medium hover:text-indigo-800">ทั้งหมดอ่านแล้ว</button>
    </div>
    
    <div class="divide-y divide-gray-50">
      <div class="flex gap-3 px-5 py-4 bg-indigo-50/40 hover:bg-indigo-50 cursor-pointer transition-colors">
        <div class="size-10 rounded-full bg-green-100 flex items-center justify-center flex-none">✅</div>
        <div class="flex-1">
          <p class="text-sm font-semibold text-gray-900">จบ Part 25 แล้ว!</p>
          <p class="text-xs text-gray-400 mt-0.5">2 นาทีที่แล้ว</p>
        </div>
        <div class="size-2 bg-indigo-500 rounded-full mt-2 flex-none"></div>
      </div>
      
      <div class="flex gap-3 px-5 py-4 bg-indigo-50/40 hover:bg-indigo-50 cursor-pointer transition-colors">
        <div class="size-10 rounded-full bg-yellow-100 flex items-center justify-center flex-none">⭐</div>
        <div class="flex-1">
          <p class="text-sm font-semibold text-gray-900">รีวิว 5 ดาวใหม่</p>
          <p class="text-xs text-gray-400 mt-0.5">15 นาทีที่แล้ว</p>
        </div>
        <div class="size-2 bg-indigo-500 rounded-full mt-2 flex-none"></div>
      </div>
      
      <div class="flex gap-3 px-5 py-4 hover:bg-gray-50 cursor-pointer transition-colors">
        <div class="size-10 rounded-full bg-blue-100 flex items-center justify-center flex-none">💳</div>
        <div class="flex-1">
          <p class="text-sm font-medium text-gray-700">ชำระเงินสำเร็จ ฿2,490</p>
          <p class="text-xs text-gray-400 mt-0.5">1 ชั่วโมงที่แล้ว</p>
        </div>
      </div>
      
      <div class="flex gap-3 px-5 py-4 hover:bg-gray-50 cursor-pointer transition-colors">
        <div class="size-10 rounded-full bg-purple-100 flex items-center justify-center flex-none">🎓</div>
        <div class="flex-1">
          <p class="text-sm font-medium text-gray-700">นักเรียนใหม่ลงทะเบียน</p>
          <p class="text-xs text-gray-400 mt-0.5">เมื่อวาน</p>
        </div>
      </div>
    </div>
  </div>
</div>

<script>
let n = 0;
const cfg = {
  success: { bg:'bg-white border-l-4 border-l-green-500', icon:'✅', t:'text-gray-900', sub:'text-gray-500', bar:'bg-green-500' },
  error:   { bg:'bg-white border-l-4 border-l-red-500',   icon:'❌', t:'text-gray-900', sub:'text-gray-500', bar:'bg-red-500' },
  warning: { bg:'bg-white border-l-4 border-l-yellow-500',icon:'⚠️', t:'text-gray-900', sub:'text-gray-500', bar:'bg-yellow-500' },
  info:    { bg:'bg-white border-l-4 border-l-blue-500',  icon:'ℹ️', t:'text-gray-900', sub:'text-gray-500', bar:'bg-blue-500' },
};

function toast(type, msg, ms = 4000) {
  const id = `t${++n}`;
  const c = cfg[type];
  const el = document.createElement('div');
  el.id = id;
  el.className = `pointer-events-auto relative overflow-hidden rounded-xl border shadow-lg ${c.bg} t-in`;
  el.innerHTML = `
    <div class="flex items-start gap-3 px-4 py-3.5">
      <span class="text-base flex-none">${c.icon}</span>
      <div class="flex-1"><p class="text-sm font-semibold ${c.t}">${msg}</p></div>
      <button onclick="dismiss('${id}')" class="text-gray-300 hover:text-gray-600 text-sm flex-none">✕</button>
    </div>
    <div id="${id}b" class="absolute bottom-0 left-0 h-1 ${c.bar}" style="width:100%;transition:width ${ms}ms linear;"></div>
  `;
  document.getElementById('tc').appendChild(el);
  requestAnimationFrame(() => requestAnimationFrame(() => { const b = document.getElementById(`${id}b`); if(b) b.style.width='0'; }));
  el._t = setTimeout(() => dismiss(id), ms);
}

function dismiss(id) {
  const el = document.getElementById(id); if(!el) return;
  clearTimeout(el._t);
  el.classList.replace('t-in','t-out');
  setTimeout(() => el.remove(), 300);
}
</script>

</body>
</html>
```

---

## 📝 สรุป Part 29

| Component | เทคนิค |
|-----------|---------|
| Toast | `fixed top-4 right-4 z-50` + JS `createElement` |
| Progress bar | CSS transition width 100%→0 |
| Auto dismiss | `setTimeout` + animation class |
| Bell dropdown | relative + `hidden` toggle |
| Alert banner | `border-l-4` + dismissal |

---

*Part 29 — Notification System | Steps 281–290 จาก 1,000 Steps*
