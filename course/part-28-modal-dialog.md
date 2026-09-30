# Part 28: Modal & Dialog System
## Steps 271–280: Modal ทุกประเภทที่ใช้จริง

---

## 🎯 เป้าหมายของ Part นี้

- Basic modal with backdrop
- Slide-in drawer (side modal)
- Confirm dialog
- Alert dialog
- Full-screen modal
- Nested modal
- Animation (scale-in, slide-up)

---

## Step 271: Basic Modal

```html
<!-- Trigger button -->
<button onclick="openModal('basic-modal')" class="px-5 py-2.5 bg-indigo-600 text-white text-sm font-semibold rounded-xl hover:bg-indigo-700">
  เปิด Modal
</button>

<!-- Modal backdrop + container -->
<div id="basic-modal" class="hidden fixed inset-0 z-50 flex items-center justify-center p-4">
  <!-- Backdrop -->
  <div onclick="closeModal('basic-modal')" class="absolute inset-0 bg-black/40 backdrop-blur-sm"></div>
  
  <!-- Modal panel -->
  <div class="relative bg-white rounded-2xl shadow-2xl w-full max-w-md">
    <!-- Header -->
    <div class="flex items-center justify-between p-5 border-b border-gray-100">
      <h3 class="font-bold text-gray-900 text-lg">ชื่อ Modal</h3>
      <button onclick="closeModal('basic-modal')" class="size-8 flex items-center justify-center rounded-xl hover:bg-gray-100 text-gray-400 hover:text-gray-700 transition-colors text-lg">✕</button>
    </div>
    
    <!-- Body -->
    <div class="p-5">
      <p class="text-gray-500 text-sm leading-relaxed">
        เนื้อหา modal ที่นี่ สามารถใส่ฟอร์ม ข้อมูล หรืออะไรก็ได้
      </p>
    </div>
    
    <!-- Footer -->
    <div class="flex items-center justify-end gap-3 p-5 border-t border-gray-100">
      <button onclick="closeModal('basic-modal')" class="px-5 py-2.5 border border-gray-200 text-gray-600 text-sm font-medium rounded-xl hover:bg-gray-50 transition-colors">ยกเลิก</button>
      <button class="px-5 py-2.5 bg-indigo-600 text-white text-sm font-semibold rounded-xl hover:bg-indigo-700 transition-colors">ยืนยัน</button>
    </div>
  </div>
</div>

<script>
function openModal(id) {
  document.getElementById(id).classList.remove('hidden');
  document.body.style.overflow = 'hidden';
}
function closeModal(id) {
  document.getElementById(id).classList.add('hidden');
  document.body.style.overflow = '';
}
// Close on Escape key
document.addEventListener('keydown', e => {
  if (e.key === 'Escape') document.querySelectorAll('[id$="-modal"]:not(.hidden)').forEach(m => closeModal(m.id));
});
</script>
```

---

## Step 272: Animated Modal (Scale + Fade)

```html
<style>
@keyframes modalIn { from { opacity: 0; transform: scale(0.95) translateY(10px); } to { opacity: 1; transform: scale(1) translateY(0); } }
@keyframes backdropIn { from { opacity: 0; } to { opacity: 1; } }
.modal-enter { animation: modalIn 0.2s ease-out forwards; }
.backdrop-enter { animation: backdropIn 0.2s ease-out forwards; }
</style>

<div id="animated-modal" class="hidden fixed inset-0 z-50 flex items-center justify-center p-4">
  <div onclick="closeModal('animated-modal')" class="absolute inset-0 bg-black/40 backdrop-enter"></div>
  <div class="relative bg-white rounded-2xl shadow-2xl w-full max-w-md modal-enter">
    <div class="p-6">
      <div class="size-14 rounded-2xl bg-indigo-100 flex items-center justify-center text-2xl mb-4">🎉</div>
      <h3 class="font-black text-xl text-gray-900 mb-2">ยืนดีด้วย!</h3>
      <p class="text-gray-500 text-sm leading-relaxed mb-6">คุณสมัครสมาชิกสำเร็จแล้ว พร้อมเริ่มเรียนได้เลย</p>
      <div class="flex gap-3">
        <button onclick="closeModal('animated-modal')" class="flex-1 py-2.5 border border-gray-200 text-gray-600 text-sm font-medium rounded-xl hover:bg-gray-50">ดูทีหลัง</button>
        <button class="flex-1 py-2.5 bg-indigo-600 text-white text-sm font-semibold rounded-xl hover:bg-indigo-700">เริ่มเรียน →</button>
      </div>
    </div>
  </div>
</div>
```

---

## Step 273: Confirm Dialog

```html
<!-- Confirm Delete Dialog -->
<button onclick="openConfirm()" class="px-4 py-2 bg-red-600 text-white text-sm font-semibold rounded-xl hover:bg-red-700">
  ลบบัญชี
</button>

<div id="confirm-modal" class="hidden fixed inset-0 z-50 flex items-center justify-center p-4">
  <div onclick="document.getElementById('confirm-modal').classList.add('hidden')" class="absolute inset-0 bg-black/50 backdrop-blur-sm"></div>
  <div class="relative bg-white rounded-2xl shadow-2xl w-full max-w-sm p-6 text-center">
    <div class="size-16 rounded-full bg-red-100 flex items-center justify-center text-3xl mx-auto mb-4">⚠️</div>
    <h3 class="font-black text-lg text-gray-900 mb-2">ยืนยันการลบบัญชี?</h3>
    <p class="text-gray-500 text-sm mb-6">
      การลบบัญชีไม่สามารถกู้คืนได้ ข้อมูลทั้งหมดจะหายถาวร
    </p>
    <div class="flex gap-3">
      <button onclick="document.getElementById('confirm-modal').classList.add('hidden')" class="flex-1 py-3 border border-gray-200 text-gray-600 font-semibold rounded-xl hover:bg-gray-50 transition-colors text-sm">
        ยกเลิก
      </button>
      <button onclick="confirmDelete()" class="flex-1 py-3 bg-red-600 hover:bg-red-700 text-white font-semibold rounded-xl transition-colors text-sm">
        ลบถาวร
      </button>
    </div>
  </div>
</div>

<script>
function openConfirm() { document.getElementById('confirm-modal').classList.remove('hidden'); }
function confirmDelete() {
  document.getElementById('confirm-modal').classList.add('hidden');
  alert('ลบบัญชีสำเร็จ');
}
</script>
```

---

## Step 274: Drawer (Side Panel)

```html
<!-- Right Drawer -->
<button onclick="openDrawer('right-drawer')" class="px-5 py-2.5 bg-gray-900 text-white text-sm font-semibold rounded-xl">
  เปิด Drawer
</button>

<div id="right-drawer" class="hidden fixed inset-0 z-50 flex justify-end">
  <!-- Backdrop -->
  <div onclick="closeDrawer('right-drawer')" class="absolute inset-0 bg-black/40 backdrop-blur-sm"></div>
  
  <!-- Panel -->
  <div class="relative bg-white w-full max-w-sm h-full flex flex-col shadow-2xl translate-x-full transition-transform duration-300" id="drawer-panel">
    <div class="flex items-center justify-between p-5 border-b border-gray-100">
      <h3 class="font-bold text-gray-900">การตั้งค่า</h3>
      <button onclick="closeDrawer('right-drawer')" class="size-8 flex items-center justify-center rounded-xl hover:bg-gray-100 text-gray-400 transition-colors">✕</button>
    </div>
    
    <div class="flex-1 overflow-y-auto p-5 space-y-5">
      <div>
        <h4 class="text-xs font-bold text-gray-400 uppercase tracking-wider mb-3">การแสดงผล</h4>
        <div class="space-y-3">
          <label class="flex items-center justify-between">
            <span class="text-sm text-gray-700">Dark Mode</span>
            <div class="relative cursor-pointer">
              <input type="checkbox" class="sr-only peer">
              <div class="w-11 h-6 bg-gray-200 peer-checked:bg-indigo-600 rounded-full transition-colors"></div>
              <div class="absolute top-0.5 left-0.5 w-5 h-5 bg-white rounded-full shadow transition-transform peer-checked:translate-x-5"></div>
            </div>
          </label>
          <label class="flex items-center justify-between">
            <span class="text-sm text-gray-700">Compact Mode</span>
            <div class="relative cursor-pointer">
              <input type="checkbox" class="sr-only peer" checked>
              <div class="w-11 h-6 bg-gray-200 peer-checked:bg-indigo-600 rounded-full transition-colors"></div>
              <div class="absolute top-0.5 left-0.5 w-5 h-5 bg-white rounded-full shadow transition-transform peer-checked:translate-x-5"></div>
            </div>
          </label>
        </div>
      </div>
      
      <div>
        <h4 class="text-xs font-bold text-gray-400 uppercase tracking-wider mb-3">ภาษา</h4>
        <select class="w-full px-4 py-2.5 text-sm border border-gray-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-indigo-200">
          <option>ภาษาไทย</option>
          <option>English</option>
        </select>
      </div>
    </div>
    
    <div class="p-5 border-t border-gray-100">
      <button class="w-full py-3 bg-indigo-600 text-white text-sm font-semibold rounded-xl hover:bg-indigo-700 transition-colors">บันทึก</button>
    </div>
  </div>
</div>

<script>
function openDrawer(id) {
  document.getElementById(id).classList.remove('hidden');
  document.body.style.overflow = 'hidden';
  requestAnimationFrame(() => {
    document.getElementById('drawer-panel').style.transform = 'translateX(0)';
  });
}
function closeDrawer(id) {
  document.getElementById('drawer-panel').style.transform = 'translateX(100%)';
  setTimeout(() => {
    document.getElementById(id).classList.add('hidden');
    document.body.style.overflow = '';
  }, 300);
}
</script>
```

---

## Step 275–280: Workshop — Full Modal System

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Modal System Workshop</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    @keyframes mIn { from { opacity:0; transform: scale(0.94) translateY(8px); } to { opacity:1; transform: scale(1) translateY(0); } }
    @keyframes bIn { from { opacity:0; } to { opacity:1; } }
    .m-in { animation: mIn 0.2s ease-out; }
    .b-in { animation: bIn 0.2s ease-out; }
  </style>
</head>
<body class="bg-gray-50 min-h-screen p-8">

<div class="max-w-3xl mx-auto">
  <h1 class="text-3xl font-black text-gray-900 mb-2">Modal System</h1>
  <p class="text-gray-500 mb-8">คลิกปุ่มเพื่อเปิด modal แต่ละประเภท</p>

  <div class="grid grid-cols-2 sm:grid-cols-3 gap-4">
    <button onclick="showModal('info')" class="p-4 bg-white border border-gray-200 rounded-2xl text-left hover:border-indigo-300 hover:shadow-md transition-all">
      <span class="text-2xl mb-2 block">ℹ️</span>
      <p class="font-semibold text-sm text-gray-900">Info Modal</p>
      <p class="text-xs text-gray-400 mt-0.5">แสดงข้อมูล</p>
    </button>
    <button onclick="showModal('confirm')" class="p-4 bg-white border border-gray-200 rounded-2xl text-left hover:border-red-300 hover:shadow-md transition-all">
      <span class="text-2xl mb-2 block">⚠️</span>
      <p class="font-semibold text-sm text-gray-900">Confirm</p>
      <p class="text-xs text-gray-400 mt-0.5">ยืนยันการลบ</p>
    </button>
    <button onclick="showModal('form')" class="p-4 bg-white border border-gray-200 rounded-2xl text-left hover:border-indigo-300 hover:shadow-md transition-all">
      <span class="text-2xl mb-2 block">📝</span>
      <p class="font-semibold text-sm text-gray-900">Form Modal</p>
      <p class="text-xs text-gray-400 mt-0.5">ฟอร์มใน modal</p>
    </button>
    <button onclick="showModal('success')" class="p-4 bg-white border border-gray-200 rounded-2xl text-left hover:border-green-300 hover:shadow-md transition-all">
      <span class="text-2xl mb-2 block">🎉</span>
      <p class="font-semibold text-sm text-gray-900">Success</p>
      <p class="text-xs text-gray-400 mt-0.5">แจ้งสำเร็จ</p>
    </button>
    <button onclick="openDrawer()" class="p-4 bg-white border border-gray-200 rounded-2xl text-left hover:border-gray-400 hover:shadow-md transition-all">
      <span class="text-2xl mb-2 block">📌</span>
      <p class="font-semibold text-sm text-gray-900">Drawer</p>
      <p class="text-xs text-gray-400 mt-0.5">Side panel</p>
    </button>
    <button onclick="showModal('image')" class="p-4 bg-white border border-gray-200 rounded-2xl text-left hover:border-purple-300 hover:shadow-md transition-all">
      <span class="text-2xl mb-2 block">🖼️</span>
      <p class="font-semibold text-sm text-gray-900">Lightbox</p>
      <p class="text-xs text-gray-400 mt-0.5">ดูภาพขยาย</p>
    </button>
  </div>
</div>

<!-- Modal container -->
<div id="modal-overlay" class="hidden fixed inset-0 z-50 flex items-center justify-center p-4 b-in">
  <div onclick="closeAll()" class="absolute inset-0 bg-black/50 backdrop-blur-sm"></div>
  <div id="modal-content" class="relative bg-white rounded-2xl shadow-2xl w-full max-w-md m-in"></div>
</div>

<!-- Drawer -->
<div id="drawer-overlay" class="hidden fixed inset-0 z-50 flex justify-end">
  <div onclick="closeDrawer()" class="absolute inset-0 bg-black/40 backdrop-blur-sm"></div>
  <div id="drawer-panel" class="relative bg-white w-80 h-full flex flex-col shadow-2xl" style="transform:translateX(100%); transition:transform 0.3s">
    <div class="p-5 border-b border-gray-100 flex items-center justify-between"><h3 class="font-bold">Settings</h3><button onclick="closeDrawer()" class="text-gray-400 hover:text-gray-700">✕</button></div>
    <div class="flex-1 overflow-y-auto p-5"><p class="text-sm text-gray-500">Drawer content here...</p></div>
  </div>
</div>

<script>
const modals = {
  info: `
    <div class="p-6">
      <div class="size-12 bg-blue-100 rounded-2xl flex items-center justify-center text-2xl mb-4">ℹ️</div>
      <h3 class="font-black text-lg text-gray-900 mb-2">ข้อมูลสำคัญ</h3>
      <p class="text-gray-500 text-sm leading-relaxed mb-6">นี่คือ info modal สำหรับแสดงข้อมูลให้ผู้ใช้ทราบ</p>
      <button onclick="closeAll()" class="w-full py-3 bg-blue-600 text-white font-semibold rounded-xl hover:bg-blue-700 text-sm">รับทราบ</button>
    </div>`,
  
  confirm: `
    <div class="p-6 text-center">
      <div class="size-16 bg-red-100 rounded-full flex items-center justify-center text-3xl mx-auto mb-4">🗑️</div>
      <h3 class="font-black text-lg text-gray-900 mb-2">ลบรายการนี้?</h3>
      <p class="text-gray-500 text-sm mb-6">ไม่สามารถย้อนกลับได้ ข้อมูลจะหายถาวร</p>
      <div class="flex gap-3">
        <button onclick="closeAll()" class="flex-1 py-3 border border-gray-200 text-gray-600 font-semibold rounded-xl hover:bg-gray-50 text-sm">ยกเลิก</button>
        <button onclick="alert('ลบแล้ว');closeAll()" class="flex-1 py-3 bg-red-600 text-white font-semibold rounded-xl hover:bg-red-700 text-sm">ลบเลย</button>
      </div>
    </div>`,
  
  form: `
    <div>
      <div class="flex items-center justify-between p-5 border-b border-gray-100">
        <h3 class="font-bold text-gray-900">แก้ไขโปรไฟล์</h3>
        <button onclick="closeAll()" class="text-gray-400 hover:text-gray-700">✕</button>
      </div>
      <div class="p-5 space-y-4">
        <div><label class="block text-xs font-semibold text-gray-700 mb-1.5">ชื่อ</label><input class="w-full px-3 py-2.5 text-sm rounded-xl border border-gray-200 focus:outline-none focus:ring-2 focus:ring-indigo-200" value="สมชาย ใจดี"></div>
        <div><label class="block text-xs font-semibold text-gray-700 mb-1.5">อีเมล</label><input class="w-full px-3 py-2.5 text-sm rounded-xl border border-gray-200 focus:outline-none focus:ring-2 focus:ring-indigo-200" value="somchai@co.th"></div>
      </div>
      <div class="flex gap-3 p-5 border-t border-gray-100">
        <button onclick="closeAll()" class="flex-1 py-2.5 border border-gray-200 text-gray-600 text-sm font-medium rounded-xl hover:bg-gray-50">ยกเลิก</button>
        <button class="flex-1 py-2.5 bg-indigo-600 text-white text-sm font-semibold rounded-xl hover:bg-indigo-700">บันทึก</button>
      </div>
    </div>`,
  
  success: `
    <div class="p-6 text-center">
      <div class="size-20 bg-green-100 rounded-full flex items-center justify-center text-4xl mx-auto mb-5">✅</div>
      <h3 class="font-black text-xl text-gray-900 mb-2">สำเร็จ!</h3>
      <p class="text-gray-500 text-sm mb-6">บันทึกข้อมูลเรียบร้อยแล้ว</p>
      <button onclick="closeAll()" class="w-full py-3 bg-green-600 text-white font-semibold rounded-xl hover:bg-green-700 text-sm">ปิด</button>
    </div>`,
  
  image: `
    <div>
      <div class="flex items-center justify-between px-5 py-4 border-b border-gray-100">
        <span class="font-medium text-sm text-gray-700">Lightbox</span>
        <button onclick="closeAll()" class="text-gray-400 hover:text-gray-700">✕</button>
      </div>
      <div class="bg-gray-900 aspect-video flex items-center justify-center text-gray-400 text-sm">
        [ภาพขนาดใหญ่จะแสดงที่นี่]
      </div>
      <div class="p-4 text-sm text-gray-500">คำอธิบายภาพ</div>
    </div>`,
};

function showModal(type) {
  document.getElementById('modal-content').innerHTML = modals[type];
  document.getElementById('modal-overlay').classList.remove('hidden');
  document.body.style.overflow = 'hidden';
}

function closeAll() {
  document.getElementById('modal-overlay').classList.add('hidden');
  document.body.style.overflow = '';
}

function openDrawer() {
  document.getElementById('drawer-overlay').classList.remove('hidden');
  document.body.style.overflow = 'hidden';
  requestAnimationFrame(() => document.getElementById('drawer-panel').style.transform = 'translateX(0)');
}

function closeDrawer() {
  document.getElementById('drawer-panel').style.transform = 'translateX(100%)';
  setTimeout(() => { document.getElementById('drawer-overlay').classList.add('hidden'); document.body.style.overflow = ''; }, 300);
}

document.addEventListener('keydown', e => { if (e.key === 'Escape') { closeAll(); closeDrawer(); } });
</script>

</body>
</html>
```

---

## 📝 สรุป Part 28

| Modal Type | Pattern |
|-----------|---------|
| Basic | `fixed inset-0 z-50 flex` + backdrop |
| Animated | `@keyframes` + class animation |
| Confirm | centered text + 2 action buttons |
| Drawer | `translate-x-full` → `translate-x-0` |
| Dynamic | innerHTML swap per type |

---

*Part 28 — Modal & Dialog | Steps 271–280 จาก 1,000 Steps*
