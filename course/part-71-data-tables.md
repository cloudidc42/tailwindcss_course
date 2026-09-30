# Part 71: Data Tables

## เป้าหมาย
- Advanced data table — sort, filter, paginate
- Column resizing + sticky headers
- Row selection + bulk actions
- Export CSV
- Steps 701–710

---

## Steps 701–710: Advanced Data Table Workshop

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Data Tables - Steps 701-710</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    th { user-select: none; }
    .sort-active { color: #4f46e5; }
    .resizer { cursor: col-resize; }
    tr.selected td { background: #eef2ff; }
    @keyframes rowIn { from { opacity:0; transform:translateY(-4px); } to { opacity:1; transform:translateY(0); } }
    tbody tr { animation: rowIn 0.2s ease forwards; }
  </style>
</head>
<body class="bg-gray-100 min-h-screen p-6">

  <div class="max-w-6xl mx-auto">
    <h1 class="text-2xl font-extrabold text-gray-900 mb-6">Advanced Data Table</h1>

    <!-- Table Container -->
    <div class="bg-white rounded-2xl border shadow-sm overflow-hidden">

      <!-- Toolbar -->
      <div class="px-5 py-4 flex flex-wrap items-center gap-3 border-b">
        <!-- Search -->
        <div class="relative flex-1 min-w-0 max-w-xs">
          <svg class="absolute left-3 top-1/2 -translate-y-1/2 w-3.5 h-3.5 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"/></svg>
          <input id="tableSearch" placeholder="ค้นหา..." oninput="applyFilters()" class="w-full pl-8 pr-4 py-2 text-sm border border-gray-300 rounded-xl focus:outline-none focus:ring-2 focus:ring-indigo-400">
        </div>

        <!-- Status filter -->
        <select id="statusFilter" onchange="applyFilters()" class="text-sm border border-gray-300 rounded-xl px-3 py-2 focus:outline-none focus:ring-2 focus:ring-indigo-400">
          <option value="">ทุกสถานะ</option>
          <option value="active">Active</option>
          <option value="inactive">Inactive</option>
          <option value="pending">Pending</option>
        </select>

        <!-- Department filter -->
        <select id="deptFilter" onchange="applyFilters()" class="text-sm border border-gray-300 rounded-xl px-3 py-2 focus:outline-none focus:ring-2 focus:ring-indigo-400">
          <option value="">ทุกแผนก</option>
          <option value="Engineering">Engineering</option>
          <option value="Marketing">Marketing</option>
          <option value="Sales">Sales</option>
          <option value="HR">HR</option>
          <option value="Finance">Finance</option>
        </select>

        <div class="flex-1"></div>

        <!-- Bulk actions (shown when selected) -->
        <div id="bulkActions" class="hidden flex items-center gap-2">
          <span id="selectedCount" class="text-sm font-semibold text-indigo-700 bg-indigo-50 px-2.5 py-1 rounded-lg"></span>
          <button onclick="bulkDelete()" class="text-sm font-semibold text-red-600 hover:text-red-700 border border-red-200 px-3 py-1.5 rounded-xl hover:bg-red-50 transition-colors">ลบที่เลือก</button>
          <button onclick="bulkExport()" class="text-sm font-semibold text-gray-700 border border-gray-300 px-3 py-1.5 rounded-xl hover:bg-gray-50 transition-colors">Export CSV</button>
        </div>

        <button onclick="exportCSV()" class="text-sm font-semibold text-gray-700 border border-gray-300 px-3 py-2 rounded-xl hover:bg-gray-50 transition-colors flex items-center gap-1.5">
          ⬇ Export All
        </button>
        <button onclick="addRow()" class="text-sm font-semibold text-white bg-indigo-600 px-3 py-2 rounded-xl hover:bg-indigo-700 transition-colors flex items-center gap-1.5">
          ＋ เพิ่มพนักงาน
        </button>
      </div>

      <!-- Table -->
      <div class="overflow-x-auto">
        <table class="w-full">
          <thead class="bg-gray-50 border-b sticky top-0 z-10">
            <tr>
              <th class="w-10 px-4 py-3 text-left">
                <input type="checkbox" id="selectAll" onchange="toggleSelectAll(this.checked)" class="rounded accent-indigo-600">
              </th>
              <th onclick="sortBy('name')" class="px-4 py-3 text-left text-xs font-semibold text-gray-500 uppercase tracking-wide cursor-pointer hover:text-gray-700 whitespace-nowrap" id="th-name">
                ชื่อ <span class="sort-icon text-gray-300">↕</span>
              </th>
              <th onclick="sortBy('email')" class="px-4 py-3 text-left text-xs font-semibold text-gray-500 uppercase tracking-wide cursor-pointer hover:text-gray-700 whitespace-nowrap" id="th-email">
                อีเมล <span class="sort-icon text-gray-300">↕</span>
              </th>
              <th onclick="sortBy('dept')" class="px-4 py-3 text-left text-xs font-semibold text-gray-500 uppercase tracking-wide cursor-pointer hover:text-gray-700 whitespace-nowrap" id="th-dept">
                แผนก <span class="sort-icon text-gray-300">↕</span>
              </th>
              <th onclick="sortBy('salary')" class="px-4 py-3 text-right text-xs font-semibold text-gray-500 uppercase tracking-wide cursor-pointer hover:text-gray-700 whitespace-nowrap" id="th-salary">
                เงินเดือน <span class="sort-icon text-gray-300">↕</span>
              </th>
              <th onclick="sortBy('joined')" class="px-4 py-3 text-left text-xs font-semibold text-gray-500 uppercase tracking-wide cursor-pointer hover:text-gray-700 whitespace-nowrap" id="th-joined">
                วันที่เข้า <span class="sort-icon text-gray-300">↕</span>
              </th>
              <th onclick="sortBy('status')" class="px-4 py-3 text-left text-xs font-semibold text-gray-500 uppercase tracking-wide cursor-pointer hover:text-gray-700 whitespace-nowrap" id="th-status">
                สถานะ <span class="sort-icon text-gray-300">↕</span>
              </th>
              <th class="px-4 py-3 text-center text-xs font-semibold text-gray-500 uppercase tracking-wide whitespace-nowrap">การดำเนินการ</th>
            </tr>
          </thead>
          <tbody id="tableBody"></tbody>
        </table>
      </div>

      <!-- Pagination -->
      <div class="px-5 py-3 border-t flex flex-wrap items-center justify-between gap-3">
        <div class="flex items-center gap-3">
          <span id="pageInfo" class="text-sm text-gray-500"></span>
          <select id="pageSize" onchange="changePageSize()" class="text-sm border border-gray-300 rounded-lg px-2 py-1 focus:outline-none">
            <option value="10">10 / หน้า</option>
            <option value="25">25 / หน้า</option>
            <option value="50">50 / หน้า</option>
          </select>
        </div>
        <div id="pagination" class="flex gap-1"></div>
      </div>
    </div>

    <!-- Stats cards below table -->
    <div class="grid grid-cols-4 gap-4 mt-5">
      <div class="bg-white border rounded-2xl p-4">
        <p class="text-xs text-gray-500 mb-1">พนักงานทั้งหมด</p>
        <p id="stat-total" class="text-2xl font-extrabold text-gray-900">—</p>
      </div>
      <div class="bg-white border rounded-2xl p-4">
        <p class="text-xs text-gray-500 mb-1">Active</p>
        <p id="stat-active" class="text-2xl font-extrabold text-emerald-600">—</p>
      </div>
      <div class="bg-white border rounded-2xl p-4">
        <p class="text-xs text-gray-500 mb-1">เงินเดือนเฉลี่ย</p>
        <p id="stat-avg" class="text-2xl font-extrabold text-indigo-600">—</p>
      </div>
      <div class="bg-white border rounded-2xl p-4">
        <p class="text-xs text-gray-500 mb-1">แผนกที่มากสุด</p>
        <p id="stat-dept" class="text-2xl font-extrabold text-gray-700">—</p>
      </div>
    </div>
  </div>

  <script>
    const depts = ['Engineering', 'Marketing', 'Sales', 'HR', 'Finance'];
    const statuses = ['active', 'active', 'active', 'inactive', 'pending'];
    const firstNames = ['สมชาย', 'นภา', 'ธนพล', 'พรทิพย์', 'วิชัย', 'ศิริ', 'อรุณ', 'มณี', 'ประเสริฐ', 'สุภา', 'กิตติ', 'ลลิตา'];
    const lastNames = ['วงค์', 'มี', 'จันทร์', 'แก้ว', 'สุข', 'มั่น', 'บัว', 'ทอง'];

    let data = Array.from({length: 60}, (_, i) => ({
      id: i + 1,
      name: firstNames[i % firstNames.length] + ' ' + lastNames[i % lastNames.length],
      email: `user${i+1}@company.com`,
      dept: depts[i % depts.length],
      salary: Math.floor(Math.random() * 60000) + 20000,
      joined: new Date(2020 + Math.floor(i/12), i%12, 1).toLocaleDateString('th-TH'),
      status: statuses[Math.floor(Math.random() * statuses.length)],
    }));

    let filtered = [...data];
    let sortCol = 'id', sortAsc = true;
    let page = 0, pageSize = 10;
    let selected = new Set();

    function applyFilters() {
      const q = document.getElementById('tableSearch').value.toLowerCase();
      const st = document.getElementById('statusFilter').value;
      const dp = document.getElementById('deptFilter').value;
      filtered = data.filter(r =>
        (!q || r.name.toLowerCase().includes(q) || r.email.toLowerCase().includes(q)) &&
        (!st || r.status === st) &&
        (!dp || r.dept === dp)
      );
      page = 0;
      selected.clear();
      render();
    }

    function sortBy(col) {
      if (sortCol === col) sortAsc = !sortAsc;
      else { sortCol = col; sortAsc = true; }
      filtered.sort((a, b) => {
        const va = a[col], vb = b[col];
        if (typeof va === 'number') return sortAsc ? va - vb : vb - va;
        return sortAsc ? String(va).localeCompare(String(vb)) : String(vb).localeCompare(String(va));
      });
      // Update sort icons
      document.querySelectorAll('th[id^="th-"] .sort-icon').forEach(el => { el.textContent = '↕'; el.className = 'sort-icon text-gray-300'; });
      const activeTh = document.getElementById('th-' + col);
      if (activeTh) {
        const icon = activeTh.querySelector('.sort-icon');
        icon.textContent = sortAsc ? '↑' : '↓';
        icon.className = 'sort-icon sort-active';
      }
      render();
    }

    function render() {
      const start = page * pageSize;
      const rows = filtered.slice(start, start + pageSize);
      const statusClass = { active: 'bg-emerald-100 text-emerald-700', inactive: 'bg-gray-100 text-gray-500', pending: 'bg-yellow-100 text-yellow-700' };

      document.getElementById('tableBody').innerHTML = rows.map(r => `
        <tr class="${selected.has(r.id) ? 'selected' : ''} hover:bg-gray-50 border-b last:border-0 transition-colors" data-id="${r.id}">
          <td class="px-4 py-3">
            <input type="checkbox" ${selected.has(r.id) ? 'checked' : ''} onchange="toggleRow(${r.id}, this.checked)" class="rounded accent-indigo-600">
          </td>
          <td class="px-4 py-3">
            <div class="flex items-center gap-2.5">
              <div class="w-7 h-7 rounded-full bg-indigo-100 flex items-center justify-center text-xs font-bold text-indigo-700">${r.name[0]}</div>
              <span class="text-sm font-semibold text-gray-900">${r.name}</span>
            </div>
          </td>
          <td class="px-4 py-3 text-sm text-gray-600">${r.email}</td>
          <td class="px-4 py-3">
            <span class="text-xs font-semibold bg-blue-50 text-blue-700 px-2 py-0.5 rounded-full">${r.dept}</span>
          </td>
          <td class="px-4 py-3 text-sm font-semibold text-gray-900 text-right">฿${r.salary.toLocaleString()}</td>
          <td class="px-4 py-3 text-sm text-gray-500 whitespace-nowrap">${r.joined}</td>
          <td class="px-4 py-3">
            <span class="text-xs font-semibold px-2 py-0.5 rounded-full ${statusClass[r.status]}">${r.status}</span>
          </td>
          <td class="px-4 py-3">
            <div class="flex items-center justify-center gap-1">
              <button onclick="editRow(${r.id})" class="w-7 h-7 flex items-center justify-center rounded-lg hover:bg-blue-100 text-blue-600 transition-colors" title="แก้ไข">✏️</button>
              <button onclick="deleteRow(${r.id})" class="w-7 h-7 flex items-center justify-center rounded-lg hover:bg-red-100 text-red-600 transition-colors" title="ลบ">🗑</button>
            </div>
          </td>
        </tr>
      `).join('');

      renderPagination();
      renderStats();
      updateBulkBar();
    }

    function renderPagination() {
      const total = filtered.length;
      const pages = Math.ceil(total / pageSize);
      const start = page * pageSize + 1;
      const end = Math.min(start + pageSize - 1, total);
      document.getElementById('pageInfo').textContent = `แสดง ${total > 0 ? start : 0}–${end} จาก ${total} รายการ`;

      let html = '';
      const showPages = [];
      for (let i = 0; i < pages; i++) {
        if (i === 0 || i === pages-1 || Math.abs(i - page) <= 1) showPages.push(i);
        else if (showPages[showPages.length-1] !== '...') showPages.push('...');
      }
      html += `<button onclick="goPage(${page-1})" ${page===0?'disabled':''} class="w-8 h-8 flex items-center justify-center rounded-lg border text-xs hover:bg-gray-50 disabled:opacity-40 transition-colors">‹</button>`;
      showPages.forEach(p => {
        if (p === '...') html += `<span class="w-8 h-8 flex items-center justify-center text-xs text-gray-400">…</span>`;
        else html += `<button onclick="goPage(${p})" class="w-8 h-8 flex items-center justify-center rounded-lg border text-xs transition-colors ${p===page ? 'bg-indigo-600 text-white border-indigo-600' : 'hover:bg-gray-50'}">${p+1}</button>`;
      });
      html += `<button onclick="goPage(${page+1})" ${page>=pages-1?'disabled':''} class="w-8 h-8 flex items-center justify-center rounded-lg border text-xs hover:bg-gray-50 disabled:opacity-40 transition-colors">›</button>`;
      document.getElementById('pagination').innerHTML = html;
    }

    function renderStats() {
      document.getElementById('stat-total').textContent = data.length;
      document.getElementById('stat-active').textContent = data.filter(r => r.status === 'active').length;
      const avg = Math.round(data.reduce((s,r) => s+r.salary, 0) / data.length);
      document.getElementById('stat-avg').textContent = '฿' + avg.toLocaleString();
      const deptCount = {};
      data.forEach(r => deptCount[r.dept] = (deptCount[r.dept]||0)+1);
      const top = Object.entries(deptCount).sort((a,b) => b[1]-a[1])[0];
      document.getElementById('stat-dept').textContent = top ? top[0] : '—';
    }

    function toggleRow(id, checked) {
      if (checked) selected.add(id);
      else selected.delete(id);
      render();
    }

    function toggleSelectAll(checked) {
      const start = page * pageSize;
      filtered.slice(start, start + pageSize).forEach(r => checked ? selected.add(r.id) : selected.delete(r.id));
      render();
    }

    function updateBulkBar() {
      const bar = document.getElementById('bulkActions');
      if (selected.size > 0) {
        bar.classList.remove('hidden');
        bar.classList.add('flex');
        document.getElementById('selectedCount').textContent = `เลือก ${selected.size} รายการ`;
      } else {
        bar.classList.add('hidden');
        bar.classList.remove('flex');
      }
    }

    function goPage(p) {
      const pages = Math.ceil(filtered.length / pageSize);
      if (p < 0 || p >= pages) return;
      page = p; render();
    }

    function changePageSize() {
      pageSize = parseInt(document.getElementById('pageSize').value);
      page = 0; render();
    }

    function editRow(id) {
      const r = data.find(d => d.id === id);
      const name = prompt('แก้ไขชื่อ:', r.name);
      if (name !== null && name.trim()) { r.name = name.trim(); applyFilters(); }
    }

    function deleteRow(id) {
      if (!confirm('ยืนยันลบรายการนี้?')) return;
      data = data.filter(d => d.id !== id);
      selected.delete(id);
      applyFilters();
    }

    function bulkDelete() {
      if (!confirm(`ยืนยันลบ ${selected.size} รายการ?`)) return;
      data = data.filter(d => !selected.has(d.id));
      selected.clear();
      applyFilters();
    }

    function addRow() {
      const name = prompt('ชื่อพนักงาน:');
      if (!name) return;
      data.unshift({ id: Date.now(), name, email: name.replace(' ','').toLowerCase() + '@company.com', dept: depts[0], salary: 30000, joined: new Date().toLocaleDateString('th-TH'), status: 'active' });
      applyFilters();
    }

    function exportCSV(rows) {
      const toExport = rows || filtered;
      const headers = ['ID','ชื่อ','อีเมล','แผนก','เงินเดือน','วันที่เข้า','สถานะ'];
      const csv = [headers.join(','), ...toExport.map(r => [r.id, r.name, r.email, r.dept, r.salary, r.joined, r.status].join(','))].join('\n');
      const blob = new Blob(['﻿' + csv], { type: 'text/csv;charset=utf-8;' });
      const link = Object.assign(document.createElement('a'), { href: URL.createObjectURL(blob), download: 'employees.csv' });
      link.click();
    }

    function bulkExport() {
      exportCSV(data.filter(r => selected.has(r.id)));
    }

    // Init
    applyFilters();
  </script>
</body>
</html>
```

---

## สรุป Part 71

| Step | เนื้อหา |
|------|---------|
| 701 | Table structure — sticky header, overflow-x |
| 702 | Sort columns — click header, ascending/descending |
| 703 | Search + multi-filter (status, department) |
| 704 | Pagination — page buttons, ellipsis, page size |
| 705 | Row checkbox selection |
| 706 | Select all (current page) |
| 707 | Bulk delete + bulk export |
| 708 | Edit row inline (prompt) + delete row |
| 709 | Add new row |
| 710 | Workshop: Complete Data Table — 60 rows, sort/filter/paginate/select/CSV |

**Part ถัดไป:** Part 72 — Notification Center (Steps 711–720)
