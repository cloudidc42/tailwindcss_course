# Part 73: Drag & Drop UI

## เป้าหมาย
- Kanban board ด้วย HTML5 Drag & Drop API
- Card reorder + column swap
- Visual drop zones + feedback
- Steps 721–730

---

## Steps 721–730: Kanban Drag & Drop Workshop

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Drag & Drop Kanban - Steps 721-730</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    .card { cursor: grab; transition: box-shadow 0.15s, transform 0.15s, opacity 0.15s; }
    .card:active { cursor: grabbing; }
    .card.dragging { opacity: 0.4; transform: scale(0.96); box-shadow: 0 8px 24px rgba(0,0,0,.15); }
    .drop-zone { transition: background 0.15s, border-color 0.15s; min-height: 48px; }
    .drop-zone.drag-over { background: #eef2ff; border: 2px dashed #6366f1; border-radius: 0.75rem; }
    .column { transition: box-shadow 0.2s; }
    .column.col-over { box-shadow: 0 0 0 2px #6366f1; }
    @keyframes cardIn { from { opacity:0; transform:translateY(-10px); } to { opacity:1; transform:translateY(0); } }
    .card-in { animation: cardIn 0.25s ease forwards; }
    .priority-high   { border-left: 3px solid #ef4444; }
    .priority-medium { border-left: 3px solid #f59e0b; }
    .priority-low    { border-left: 3px solid #22c55e; }
  </style>
</head>
<body class="bg-gray-100 min-h-screen p-6">

  <div class="max-w-6xl mx-auto">
    <!-- Header -->
    <div class="flex items-center justify-between mb-6">
      <div>
        <h1 class="text-2xl font-extrabold text-gray-900">Project Board</h1>
        <p class="text-sm text-gray-500 mt-0.5">ลาก Card ข้ามคอลัมน์ได้เลย</p>
      </div>
      <div class="flex gap-2">
        <button onclick="addCard()" class="bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">+ เพิ่ม Card</button>
        <select id="priorityNewCard" class="text-sm border border-gray-300 rounded-xl px-3 py-2 focus:outline-none">
          <option value="high">High 🔴</option>
          <option value="medium" selected>Medium 🟡</option>
          <option value="low">Low 🟢</option>
        </select>
      </div>
    </div>

    <!-- Stats -->
    <div class="flex gap-3 mb-5 overflow-x-auto pb-1">
      <div id="stat-todo" class="bg-white rounded-xl border px-4 py-2 flex items-center gap-2 flex-shrink-0">
        <span class="w-2.5 h-2.5 rounded-full bg-gray-400"></span>
        <span class="text-sm font-semibold text-gray-700">Todo</span>
        <span class="text-sm font-bold text-gray-900" id="cnt-todo">0</span>
      </div>
      <div id="stat-progress" class="bg-white rounded-xl border px-4 py-2 flex items-center gap-2 flex-shrink-0">
        <span class="w-2.5 h-2.5 rounded-full bg-blue-500"></span>
        <span class="text-sm font-semibold text-gray-700">In Progress</span>
        <span class="text-sm font-bold text-gray-900" id="cnt-progress">0</span>
      </div>
      <div id="stat-review" class="bg-white rounded-xl border px-4 py-2 flex items-center gap-2 flex-shrink-0">
        <span class="w-2.5 h-2.5 rounded-full bg-yellow-500"></span>
        <span class="text-sm font-semibold text-gray-700">Review</span>
        <span class="text-sm font-bold text-gray-900" id="cnt-review">0</span>
      </div>
      <div id="stat-done" class="bg-white rounded-xl border px-4 py-2 flex items-center gap-2 flex-shrink-0">
        <span class="w-2.5 h-2.5 rounded-full bg-emerald-500"></span>
        <span class="text-sm font-semibold text-gray-700">Done</span>
        <span class="text-sm font-bold text-gray-900" id="cnt-done">0</span>
      </div>
    </div>

    <!-- Kanban Board -->
    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4" id="board">

      <!-- Todo column -->
      <div class="column bg-white rounded-2xl border" id="col-todo"
           ondragover="handleColOver(event, 'todo')" ondragleave="handleColLeave('todo')" ondrop="handleColDrop(event, 'todo')">
        <div class="flex items-center justify-between px-4 py-3 border-b">
          <div class="flex items-center gap-2">
            <span class="w-2.5 h-2.5 rounded-full bg-gray-400"></span>
            <h3 class="font-bold text-gray-800 text-sm">Todo</h3>
          </div>
          <span id="col-cnt-todo" class="text-xs font-bold text-gray-500 bg-gray-100 px-2 py-0.5 rounded-full">0</span>
        </div>
        <div class="p-3 space-y-2 drop-zone min-h-[120px]" id="zone-todo"
             ondragover="event.preventDefault(); event.currentTarget.classList.add('drag-over')"
             ondragleave="event.currentTarget.classList.remove('drag-over')"
             ondrop="handleDrop(event, 'todo')"></div>
        <div class="px-3 pb-3">
          <button onclick="quickAdd('todo')" class="w-full flex items-center gap-1 text-xs text-gray-400 hover:text-gray-600 py-1.5 rounded-lg hover:bg-gray-50 transition-colors pl-1">
            <span>+</span> เพิ่ม Card
          </button>
        </div>
      </div>

      <!-- In Progress -->
      <div class="column bg-white rounded-2xl border" id="col-progress"
           ondragover="handleColOver(event, 'progress')" ondragleave="handleColLeave('progress')" ondrop="handleColDrop(event, 'progress')">
        <div class="flex items-center justify-between px-4 py-3 border-b">
          <div class="flex items-center gap-2">
            <span class="w-2.5 h-2.5 rounded-full bg-blue-500"></span>
            <h3 class="font-bold text-gray-800 text-sm">In Progress</h3>
          </div>
          <span id="col-cnt-progress" class="text-xs font-bold text-blue-600 bg-blue-50 px-2 py-0.5 rounded-full">0</span>
        </div>
        <div class="p-3 space-y-2 drop-zone min-h-[120px]" id="zone-progress"
             ondragover="event.preventDefault(); event.currentTarget.classList.add('drag-over')"
             ondragleave="event.currentTarget.classList.remove('drag-over')"
             ondrop="handleDrop(event, 'progress')"></div>
        <div class="px-3 pb-3">
          <button onclick="quickAdd('progress')" class="w-full flex items-center gap-1 text-xs text-gray-400 hover:text-gray-600 py-1.5 rounded-lg hover:bg-gray-50 transition-colors pl-1">
            <span>+</span> เพิ่ม Card
          </button>
        </div>
      </div>

      <!-- Review -->
      <div class="column bg-white rounded-2xl border" id="col-review"
           ondragover="handleColOver(event, 'review')" ondragleave="handleColLeave('review')" ondrop="handleColDrop(event, 'review')">
        <div class="flex items-center justify-between px-4 py-3 border-b">
          <div class="flex items-center gap-2">
            <span class="w-2.5 h-2.5 rounded-full bg-yellow-500"></span>
            <h3 class="font-bold text-gray-800 text-sm">Review</h3>
          </div>
          <span id="col-cnt-review" class="text-xs font-bold text-yellow-700 bg-yellow-50 px-2 py-0.5 rounded-full">0</span>
        </div>
        <div class="p-3 space-y-2 drop-zone min-h-[120px]" id="zone-review"
             ondragover="event.preventDefault(); event.currentTarget.classList.add('drag-over')"
             ondragleave="event.currentTarget.classList.remove('drag-over')"
             ondrop="handleDrop(event, 'review')"></div>
        <div class="px-3 pb-3">
          <button onclick="quickAdd('review')" class="w-full flex items-center gap-1 text-xs text-gray-400 hover:text-gray-600 py-1.5 rounded-lg hover:bg-gray-50 transition-colors pl-1">
            <span>+</span> เพิ่ม Card
          </button>
        </div>
      </div>

      <!-- Done -->
      <div class="column bg-white rounded-2xl border" id="col-done"
           ondragover="handleColOver(event, 'done')" ondragleave="handleColLeave('done')" ondrop="handleColDrop(event, 'done')">
        <div class="flex items-center justify-between px-4 py-3 border-b">
          <div class="flex items-center gap-2">
            <span class="w-2.5 h-2.5 rounded-full bg-emerald-500"></span>
            <h3 class="font-bold text-gray-800 text-sm">Done</h3>
          </div>
          <span id="col-cnt-done" class="text-xs font-bold text-emerald-700 bg-emerald-50 px-2 py-0.5 rounded-full">0</span>
        </div>
        <div class="p-3 space-y-2 drop-zone min-h-[120px]" id="zone-done"
             ondragover="event.preventDefault(); event.currentTarget.classList.add('drag-over')"
             ondragleave="event.currentTarget.classList.remove('drag-over')"
             ondrop="handleDrop(event, 'done')"></div>
        <div class="px-3 pb-3">
          <button onclick="quickAdd('done')" class="w-full flex items-center gap-1 text-xs text-gray-400 hover:text-gray-600 py-1.5 rounded-lg hover:bg-gray-50 transition-colors pl-1">
            <span>+</span> เพิ่ม Card
          </button>
        </div>
      </div>
    </div>
  </div>

  <script>
    let cards = [
      { id:1, title:'ออกแบบ UI หน้าหลัก', assignee:'สม', priority:'high',   col:'todo',     tags:['Design','UI'] },
      { id:2, title:'สร้าง API endpoint', assignee:'นภ', priority:'medium', col:'todo',     tags:['Backend'] },
      { id:3, title:'เขียน unit tests',   assignee:'ธน', priority:'low',    col:'todo',     tags:['Testing'] },
      { id:4, title:'Integrate Stripe',   assignee:'พร', priority:'high',   col:'progress', tags:['Payment'] },
      { id:5, title:'Dashboard charts',   assignee:'วิ', priority:'medium', col:'progress', tags:['Frontend'] },
      { id:6, title:'User auth flow',     assignee:'สม', priority:'high',   col:'review',   tags:['Auth'] },
      { id:7, title:'Deploy to staging',  assignee:'ธน', priority:'medium', col:'done',     tags:['DevOps'] },
      { id:8, title:'Fix login bug',      assignee:'นภ', priority:'high',   col:'done',     tags:['Bug'] },
    ];
    let nextId = 9;
    let draggingId = null;

    const tagColors = { Design:'bg-pink-100 text-pink-700', UI:'bg-purple-100 text-purple-700', Backend:'bg-blue-100 text-blue-700', Testing:'bg-green-100 text-green-700', Payment:'bg-yellow-100 text-yellow-700', Frontend:'bg-indigo-100 text-indigo-700', Auth:'bg-orange-100 text-orange-700', DevOps:'bg-gray-100 text-gray-700', Bug:'bg-red-100 text-red-700' };

    function renderBoard() {
      const cols = ['todo','progress','review','done'];
      cols.forEach(col => {
        const zone = document.getElementById('zone-' + col);
        const colCards = cards.filter(c => c.col === col);
        zone.innerHTML = colCards.map(c => createCardHTML(c)).join('');
        document.getElementById('col-cnt-' + col).textContent = colCards.length;
        document.getElementById('cnt-' + col).textContent = colCards.length;
      });
    }

    function createCardHTML(c) {
      const priorityColors = { high: 'text-red-600', medium: 'text-yellow-600', low: 'text-emerald-600' };
      const priorityLabels = { high: '🔴', medium: '🟡', low: '🟢' };
      return `
        <div class="card card-in priority-${c.priority} bg-white rounded-xl border p-3 shadow-sm hover:shadow-md"
             draggable="true" id="card-${c.id}"
             ondragstart="handleDragStart(event, ${c.id})"
             ondragend="handleDragEnd(event)">
          <div class="flex items-start justify-between mb-2">
            <p class="text-sm font-semibold text-gray-900 flex-1 mr-2 leading-snug">${c.title}</p>
            <button onclick="deleteCard(${c.id})" class="text-gray-200 hover:text-red-400 transition-colors text-xs flex-shrink-0">✕</button>
          </div>
          <div class="flex flex-wrap gap-1 mb-2">
            ${c.tags.map(t => `<span class="text-[10px] font-semibold px-1.5 py-0.5 rounded-full ${tagColors[t] || 'bg-gray-100 text-gray-600'}">${t}</span>`).join('')}
          </div>
          <div class="flex items-center justify-between">
            <div class="w-5 h-5 rounded-full bg-indigo-200 flex items-center justify-center text-[10px] font-bold text-indigo-700">${c.assignee}</div>
            <span class="text-xs ${priorityColors[c.priority]}">${priorityLabels[c.priority]} ${c.priority}</span>
          </div>
        </div>
      `;
    }

    function handleDragStart(e, id) {
      draggingId = id;
      e.dataTransfer.effectAllowed = 'move';
      e.dataTransfer.setData('text/plain', id);
      setTimeout(() => document.getElementById('card-' + id)?.classList.add('dragging'), 0);
    }

    function handleDragEnd(e) {
      document.querySelectorAll('.card').forEach(el => el.classList.remove('dragging'));
      document.querySelectorAll('.drop-zone').forEach(el => el.classList.remove('drag-over'));
      document.querySelectorAll('.column').forEach(el => el.classList.remove('col-over'));
    }

    function handleDrop(e, col) {
      e.preventDefault(); e.stopPropagation();
      e.currentTarget.classList.remove('drag-over');
      if (draggingId === null) return;
      const card = cards.find(c => c.id === draggingId);
      if (card) card.col = col;
      draggingId = null;
      renderBoard();
    }

    function handleColOver(e, col) { e.preventDefault(); document.getElementById('col-' + col).classList.add('col-over'); }
    function handleColLeave(col) { document.getElementById('col-' + col).classList.remove('col-over'); }
    function handleColDrop(e, col) { e.stopPropagation(); document.getElementById('col-' + col).classList.remove('col-over'); }

    function addCard() {
      const title = prompt('ชื่องาน:');
      if (!title) return;
      const priority = document.getElementById('priorityNewCard').value;
      cards.push({ id: nextId++, title, assignee: 'ใหม่', priority, col: 'todo', tags: [] });
      renderBoard();
    }

    function quickAdd(col) {
      const title = prompt(`เพิ่มงานใน ${col}:`);
      if (!title) return;
      cards.push({ id: nextId++, title, assignee: 'ใหม่', priority: 'medium', col, tags: [] });
      renderBoard();
    }

    function deleteCard(id) {
      cards = cards.filter(c => c.id !== id);
      renderBoard();
    }

    renderBoard();
  </script>
</body>
</html>
```

---

## สรุป Part 73

| Step | เนื้อหา |
|------|---------|
| 721 | HTML5 dragstart / dragend events |
| 722 | Drop zone — dragover / drop |
| 723 | Visual feedback — dragging opacity, scale |
| 724 | Drop zone highlight — dashed border |
| 725 | Kanban column — 4 states |
| 726 | Card structure — title, tags, assignee, priority |
| 727 | Priority border colors (left border indicator) |
| 728 | Column counts + stat bar |
| 729 | Add card (global + per-column quick add) + delete |
| 730 | Workshop: Complete Kanban Board — drag/drop/color/stats |

**Part ถัดไป:** Part 74 — Form Wizard & Multi-step (Steps 731–740)
