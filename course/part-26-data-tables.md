# Part 26: Data Tables & Lists
## Steps 251–260: Table, List, Sort, Filter ระดับมืออาชีพ

---

## 🎯 เป้าหมายของ Part นี้

- Responsive table
- Striped / bordered tables
- Sortable columns
- Row actions
- Inline edit
- Bulk selection
- Virtual list / infinite scroll concept

---

## Step 251: Basic Table

```html
<div class="overflow-x-auto rounded-2xl border border-gray-200 bg-white">
  <table class="w-full text-sm">
    <!-- Header -->
    <thead class="bg-gray-50 border-b border-gray-200">
      <tr>
        <th class="text-left py-3.5 px-4 text-xs font-semibold text-gray-500 uppercase tracking-wider">ชื่อ</th>
        <th class="text-left py-3.5 px-4 text-xs font-semibold text-gray-500 uppercase tracking-wider">อีเมล</th>
        <th class="text-left py-3.5 px-4 text-xs font-semibold text-gray-500 uppercase tracking-wider">แผนก</th>
        <th class="text-center py-3.5 px-4 text-xs font-semibold text-gray-500 uppercase tracking-wider">สถานะ</th>
        <th class="text-right py-3.5 px-4 text-xs font-semibold text-gray-500 uppercase tracking-wider">จัดการ</th>
      </tr>
    </thead>
    <!-- Body -->
    <tbody class="divide-y divide-gray-100">
      <tr class="hover:bg-gray-50 transition-colors">
        <td class="py-4 px-4">
          <div class="flex items-center gap-3">
            <div class="size-8 rounded-full bg-gradient-to-br from-indigo-400 to-purple-500 flex-none"></div>
            <span class="font-medium text-gray-900">สมชาย ใจดี</span>
          </div>
        </td>
        <td class="py-4 px-4 text-gray-500">somchai@co.th</td>
        <td class="py-4 px-4 text-gray-500">Engineering</td>
        <td class="py-4 px-4 text-center">
          <span class="inline-block text-xs font-semibold bg-green-100 text-green-700 px-2.5 py-1 rounded-full">Active</span>
        </td>
        <td class="py-4 px-4 text-right">
          <div class="flex items-center justify-end gap-1">
            <button class="px-3 py-1.5 text-xs font-medium text-indigo-600 hover:bg-indigo-50 rounded-lg transition-colors">แก้ไข</button>
            <button class="px-3 py-1.5 text-xs font-medium text-red-600 hover:bg-red-50 rounded-lg transition-colors">ลบ</button>
          </div>
        </td>
      </tr>
      <tr class="hover:bg-gray-50 transition-colors">
        <td class="py-4 px-4">
          <div class="flex items-center gap-3">
            <div class="size-8 rounded-full bg-gradient-to-br from-pink-400 to-rose-500 flex-none"></div>
            <span class="font-medium text-gray-900">สมหญิง ใจงาม</span>
          </div>
        </td>
        <td class="py-4 px-4 text-gray-500">somying@co.th</td>
        <td class="py-4 px-4 text-gray-500">Design</td>
        <td class="py-4 px-4 text-center">
          <span class="inline-block text-xs font-semibold bg-yellow-100 text-yellow-700 px-2.5 py-1 rounded-full">Pending</span>
        </td>
        <td class="py-4 px-4 text-right">
          <div class="flex items-center justify-end gap-1">
            <button class="px-3 py-1.5 text-xs font-medium text-indigo-600 hover:bg-indigo-50 rounded-lg transition-colors">แก้ไข</button>
            <button class="px-3 py-1.5 text-xs font-medium text-red-600 hover:bg-red-50 rounded-lg transition-colors">ลบ</button>
          </div>
        </td>
      </tr>
    </tbody>
  </table>
</div>
```

---

## Step 252: Checkbox + Bulk Actions

```html
<div class="bg-white rounded-2xl border border-gray-200 overflow-hidden">
  <!-- Toolbar: visible when rows selected -->
  <div id="bulk-toolbar" class="hidden items-center gap-3 px-4 py-3 bg-indigo-50 border-b border-indigo-100">
    <span id="selected-count" class="text-sm font-semibold text-indigo-700">0 รายการ</span>
    <button class="px-3 py-1.5 text-xs font-medium bg-red-600 hover:bg-red-700 text-white rounded-lg transition-colors">ลบที่เลือก</button>
    <button class="px-3 py-1.5 text-xs font-medium bg-white hover:bg-gray-100 text-gray-700 border border-gray-200 rounded-lg transition-colors">Export</button>
  </div>
  
  <div class="overflow-x-auto">
    <table class="w-full text-sm">
      <thead class="bg-gray-50 border-b border-gray-200">
        <tr>
          <th class="py-3.5 px-4 w-12">
            <input type="checkbox" id="select-all" onchange="toggleAll(this)" class="rounded border-gray-300 text-indigo-600 focus:ring-indigo-500 cursor-pointer">
          </th>
          <th class="text-left py-3.5 px-4 text-xs font-semibold text-gray-500 uppercase">ชื่อ</th>
          <th class="text-left py-3.5 px-4 text-xs font-semibold text-gray-500 uppercase">อีเมล</th>
          <th class="text-center py-3.5 px-4 text-xs font-semibold text-gray-500 uppercase">สถานะ</th>
          <th class="text-right py-3.5 px-4 text-xs font-semibold text-gray-500 uppercase">จัดการ</th>
        </tr>
      </thead>
      <tbody class="divide-y divide-gray-100" id="table-body">
        <tr class="hover:bg-gray-50 transition-colors">
          <td class="py-4 px-4"><input type="checkbox" class="row-check rounded border-gray-300 text-indigo-600 cursor-pointer" onchange="updateBulk()"></td>
          <td class="py-4 px-4 font-medium text-gray-900">สมชาย ใจดี</td>
          <td class="py-4 px-4 text-gray-500">somchai@co.th</td>
          <td class="py-4 px-4 text-center"><span class="text-xs font-semibold bg-green-100 text-green-700 px-2.5 py-1 rounded-full">Active</span></td>
          <td class="py-4 px-4 text-right"><button class="text-indigo-600 text-xs hover:underline">แก้ไข</button></td>
        </tr>
        <tr class="hover:bg-gray-50 transition-colors">
          <td class="py-4 px-4"><input type="checkbox" class="row-check rounded border-gray-300 text-indigo-600 cursor-pointer" onchange="updateBulk()"></td>
          <td class="py-4 px-4 font-medium text-gray-900">สมหญิง ใจงาม</td>
          <td class="py-4 px-4 text-gray-500">somying@co.th</td>
          <td class="py-4 px-4 text-center"><span class="text-xs font-semibold bg-yellow-100 text-yellow-700 px-2.5 py-1 rounded-full">Pending</span></td>
          <td class="py-4 px-4 text-right"><button class="text-indigo-600 text-xs hover:underline">แก้ไข</button></td>
        </tr>
        <tr class="hover:bg-gray-50 transition-colors">
          <td class="py-4 px-4"><input type="checkbox" class="row-check rounded border-gray-300 text-indigo-600 cursor-pointer" onchange="updateBulk()"></td>
          <td class="py-4 px-4 font-medium text-gray-900">มานะ ขยัน</td>
          <td class="py-4 px-4 text-gray-500">mana@co.th</td>
          <td class="py-4 px-4 text-center"><span class="text-xs font-semibold bg-red-100 text-red-700 px-2.5 py-1 rounded-full">Inactive</span></td>
          <td class="py-4 px-4 text-right"><button class="text-indigo-600 text-xs hover:underline">แก้ไข</button></td>
        </tr>
      </tbody>
    </table>
  </div>
</div>

<script>
function toggleAll(master) {
  document.querySelectorAll('.row-check').forEach(cb => cb.checked = master.checked);
  updateBulk();
}
function updateBulk() {
  const checked = document.querySelectorAll('.row-check:checked');
  const toolbar = document.getElementById('bulk-toolbar');
  toolbar.style.display = checked.length ? 'flex' : 'none';
  document.getElementById('selected-count').textContent = `${checked.length} รายการ`;
}
</script>
```

---

## Step 253: Sortable Columns

```html
<table class="w-full text-sm">
  <thead class="bg-gray-50 border-b border-gray-200">
    <tr>
      <th class="text-left py-3.5 px-4">
        <button onclick="sortTable('name')" class="flex items-center gap-1 text-xs font-semibold text-gray-500 uppercase hover:text-gray-900 transition-colors group">
          ชื่อ
          <span id="sort-name" class="text-gray-300 group-hover:text-gray-500">↕</span>
        </button>
      </th>
      <th class="text-left py-3.5 px-4">
        <button onclick="sortTable('score')" class="flex items-center gap-1 text-xs font-semibold text-gray-500 uppercase hover:text-gray-900 transition-colors group">
          คะแนน
          <span id="sort-score" class="text-indigo-600">↑</span>
        </button>
      </th>
    </tr>
  </thead>
  <tbody id="sortable-body" class="divide-y divide-gray-100">
    <!-- rows populated by JS -->
  </tbody>
</table>

<script>
const data = [
  { name: 'สมชาย', score: 92 },
  { name: 'มานะ', score: 88 },
  { name: 'สมหญิง', score: 95 },
];
let sortKey = 'score';
let sortDir = 'desc';

function sortTable(key) {
  if (sortKey === key) sortDir = sortDir === 'asc' ? 'desc' : 'asc';
  else { sortKey = key; sortDir = 'asc'; }
  renderTable();
}

function renderTable() {
  const sorted = [...data].sort((a, b) => {
    if (a[sortKey] < b[sortKey]) return sortDir === 'asc' ? -1 : 1;
    if (a[sortKey] > b[sortKey]) return sortDir === 'asc' ? 1 : -1;
    return 0;
  });
  document.getElementById('sortable-body').innerHTML = sorted.map(row => `
    <tr class="hover:bg-gray-50">
      <td class="py-3.5 px-4 font-medium text-gray-900">${row.name}</td>
      <td class="py-3.5 px-4 text-gray-600">${row.score}</td>
    </tr>
  `).join('');
}

renderTable();
</script>
```

---

## Step 254: Table Filters + Search

```html
<!-- Filter bar above table -->
<div class="flex flex-wrap items-center gap-3 mb-4">
  <!-- Search -->
  <div class="relative flex-1 min-w-[200px]">
    <span class="absolute left-3 top-1/2 -translate-y-1/2 text-gray-400 text-xs">🔍</span>
    <input
      type="text"
      placeholder="ค้นหาชื่อ..."
      oninput="filterTable(this.value)"
      class="w-full pl-8 pr-4 py-2.5 text-sm border border-gray-200 rounded-xl focus:outline-none focus:ring-2 focus:ring-indigo-200"
    >
  </div>
  
  <!-- Status filter -->
  <select onchange="filterStatus(this.value)" class="text-sm border border-gray-200 rounded-xl px-4 py-2.5 focus:outline-none focus:ring-2 focus:ring-indigo-200 text-gray-600">
    <option value="">ทุกสถานะ</option>
    <option value="active">Active</option>
    <option value="pending">Pending</option>
    <option value="inactive">Inactive</option>
  </select>
  
  <!-- Date range -->
  <input type="date" class="text-sm border border-gray-200 rounded-xl px-4 py-2.5 focus:outline-none focus:ring-2 focus:ring-indigo-200 text-gray-600">
  
  <!-- Clear filters -->
  <button onclick="clearFilters()" class="flex items-center gap-1.5 text-sm font-medium text-gray-500 hover:text-gray-900 transition-colors px-3 py-2.5">
    ✕ ล้างตัวกรอง
  </button>
</div>
```

---

## Step 255–260: Workshop — Full Data Table Page

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Data Table Workshop</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-6 min-h-screen">

<div class="max-w-6xl mx-auto">
  <!-- Page header -->
  <div class="flex items-center justify-between mb-6">
    <div>
      <h1 class="text-2xl font-black text-gray-900">นักเรียนทั้งหมด</h1>
      <p class="text-gray-500 text-sm mt-1">จัดการบัญชีนักเรียน</p>
    </div>
    <button class="inline-flex items-center gap-2 px-5 py-2.5 bg-indigo-600 text-white text-sm font-semibold rounded-xl hover:bg-indigo-700 transition-colors">
      + เพิ่มนักเรียน
    </button>
  </div>

  <!-- Filters -->
  <div class="flex flex-wrap gap-3 mb-4">
    <div class="relative flex-1 min-w-[200px] max-w-xs">
      <span class="absolute left-3 top-1/2 -translate-y-1/2 text-gray-400 text-xs">🔍</span>
      <input id="search" type="text" placeholder="ค้นหา..." oninput="applyFilters()" class="w-full pl-8 pr-4 py-2.5 text-sm border border-gray-200 rounded-xl bg-white focus:outline-none focus:ring-2 focus:ring-indigo-200">
    </div>
    <select id="status-filter" onchange="applyFilters()" class="text-sm border border-gray-200 rounded-xl px-4 py-2.5 bg-white focus:outline-none focus:ring-2 focus:ring-indigo-200 text-gray-600">
      <option value="">ทุกสถานะ</option>
      <option value="active">Active</option>
      <option value="pending">Pending</option>
      <option value="inactive">Inactive</option>
    </select>
  </div>

  <!-- Table card -->
  <div class="bg-white rounded-2xl border border-gray-200 overflow-hidden">
    <!-- Bulk toolbar -->
    <div id="bulk-toolbar" class="hidden items-center gap-3 px-5 py-3 bg-indigo-50 border-b border-indigo-100">
      <span id="bulk-count" class="text-sm font-semibold text-indigo-700"></span>
      <button onclick="deleteSelected()" class="px-4 py-1.5 bg-red-600 hover:bg-red-700 text-white text-xs font-semibold rounded-lg transition-colors">ลบ</button>
      <button class="px-4 py-1.5 bg-white border border-gray-200 hover:bg-gray-50 text-gray-600 text-xs font-semibold rounded-lg transition-colors">Export CSV</button>
    </div>
    
    <div class="overflow-x-auto">
      <table class="w-full text-sm">
        <thead class="bg-gray-50 border-b border-gray-200">
          <tr>
            <th class="py-3.5 px-4 w-12"><input type="checkbox" id="select-all" onchange="toggleAll(this)" class="rounded border-gray-300 text-indigo-600 focus:ring-indigo-500 cursor-pointer size-4"></th>
            <th class="text-left py-3.5 px-4"><button onclick="doSort('name')" class="flex items-center gap-1 text-xs font-semibold text-gray-500 uppercase hover:text-gray-900">ชื่อ <span id="sort-icon-name">↕</span></button></th>
            <th class="text-left py-3.5 px-4 text-xs font-semibold text-gray-500 uppercase hidden md:table-cell">อีเมล</th>
            <th class="text-left py-3.5 px-4"><button onclick="doSort('score')" class="flex items-center gap-1 text-xs font-semibold text-gray-500 uppercase hover:text-gray-900">คะแนน <span id="sort-icon-score">↕</span></button></th>
            <th class="text-center py-3.5 px-4 text-xs font-semibold text-gray-500 uppercase">สถานะ</th>
            <th class="text-right py-3.5 px-4 text-xs font-semibold text-gray-500 uppercase">จัดการ</th>
          </tr>
        </thead>
        <tbody id="table-body" class="divide-y divide-gray-100"></tbody>
      </table>
    </div>

    <!-- Pagination -->
    <div class="flex items-center justify-between px-5 py-4 border-t border-gray-100 text-sm">
      <span class="text-gray-500 text-xs">แสดง 1–5 จาก <span id="total-count">0</span> รายการ</span>
      <div class="flex gap-1">
        <button class="size-8 rounded-lg border border-gray-200 text-gray-500 hover:bg-gray-50 text-xs">«</button>
        <button class="size-8 rounded-lg bg-indigo-600 text-white text-xs font-bold">1</button>
        <button class="size-8 rounded-lg border border-gray-200 text-gray-500 hover:bg-gray-50 text-xs">2</button>
        <button class="size-8 rounded-lg border border-gray-200 text-gray-500 hover:bg-gray-50 text-xs">»</button>
      </div>
    </div>
  </div>
</div>

<script>
let students = [
  { id:1, name:'สมชาย ใจดี', email:'somchai@co.th', score:92, status:'active' },
  { id:2, name:'สมหญิง ใจงาม', email:'somying@co.th', score:88, status:'pending' },
  { id:3, name:'มานะ ขยัน', email:'mana@co.th', score:75, status:'active' },
  { id:4, name:'มานี มีสุข', email:'manee@co.th', score:61, status:'inactive' },
  { id:5, name:'วิทยา รักเรียน', email:'wittaya@co.th', score:95, status:'active' },
  { id:6, name:'ประภา สดใส', email:'prapa@co.th', score:83, status:'pending' },
];

let sortKey = 'name', sortDir = 'asc';
const statusColors = { active:'bg-green-100 text-green-700', pending:'bg-yellow-100 text-yellow-700', inactive:'bg-red-100 text-red-700' };
const statusLabel  = { active:'Active', pending:'Pending', inactive:'Inactive' };

function doSort(key) {
  if (sortKey === key) sortDir = sortDir === 'asc' ? 'desc' : 'asc';
  else { sortKey = key; sortDir = 'asc'; }
  ['name','score'].forEach(k => {
    const el = document.getElementById(`sort-icon-${k}`);
    if (el) el.textContent = k === sortKey ? (sortDir === 'asc' ? '↑' : '↓') : '↕';
  });
  applyFilters();
}

function applyFilters() {
  const q = document.getElementById('search').value.toLowerCase();
  const s = document.getElementById('status-filter').value;
  let filtered = students.filter(st =>
    st.name.toLowerCase().includes(q) && (!s || st.status === s)
  ).sort((a,b) => {
    const va = a[sortKey], vb = b[sortKey];
    return sortDir === 'asc' ? (va < vb ? -1 : va > vb ? 1 : 0) : (va > vb ? -1 : va < vb ? 1 : 0);
  });
  document.getElementById('total-count').textContent = filtered.length;
  document.getElementById('table-body').innerHTML = filtered.map(st => `
    <tr class="hover:bg-gray-50 transition-colors" data-id="${st.id}">
      <td class="py-4 px-4"><input type="checkbox" class="row-check rounded border-gray-300 text-indigo-600 cursor-pointer size-4" onchange="updateBulk()"></td>
      <td class="py-4 px-4 font-medium text-gray-900">${st.name}</td>
      <td class="py-4 px-4 text-gray-500 hidden md:table-cell">${st.email}</td>
      <td class="py-4 px-4 text-gray-600">${st.score}</td>
      <td class="py-4 px-4 text-center"><span class="text-[11px] font-semibold px-2.5 py-1 rounded-full ${statusColors[st.status]}">${statusLabel[st.status]}</span></td>
      <td class="py-4 px-4 text-right">
        <button class="text-indigo-600 text-xs hover:underline mr-2">แก้ไข</button>
        <button onclick="deleteRow(${st.id})" class="text-red-500 text-xs hover:underline">ลบ</button>
      </td>
    </tr>
  `).join('') || `<tr><td colspan="6" class="py-12 text-center text-gray-400 text-sm">ไม่พบข้อมูล</td></tr>`;
}

function toggleAll(master) {
  document.querySelectorAll('.row-check').forEach(cb => cb.checked = master.checked);
  updateBulk();
}

function updateBulk() {
  const checked = document.querySelectorAll('.row-check:checked');
  const toolbar = document.getElementById('bulk-toolbar');
  const count = checked.length;
  toolbar.style.display = count ? 'flex' : 'none';
  document.getElementById('bulk-count').textContent = `${count} รายการที่เลือก`;
}

function deleteRow(id) {
  if (confirm('ต้องการลบรายการนี้?')) {
    students = students.filter(s => s.id !== id);
    applyFilters();
  }
}

function deleteSelected() {
  const ids = [...document.querySelectorAll('.row-check:checked')].map(cb => parseInt(cb.closest('tr').dataset.id));
  if (confirm(`ลบ ${ids.length} รายการ?`)) {
    students = students.filter(s => !ids.includes(s.id));
    applyFilters();
    document.getElementById('bulk-toolbar').style.display = 'none';
  }
}

applyFilters();
</script>

</body>
</html>
```

---

## 📝 สรุป Part 26

| Feature | เทคนิค |
|---------|---------|
| Responsive table | `overflow-x-auto` + `hidden md:table-cell` |
| Row hover | `hover:bg-gray-50 transition-colors` |
| Status badge | `text-[11px] font-semibold px-2.5 py-1 rounded-full` |
| Bulk selection | checkbox + JS toggle |
| Column sort | JS `.sort()` + icon swap |

---

*Part 26 — Data Tables | Steps 251–260 จาก 1,000 Steps*
