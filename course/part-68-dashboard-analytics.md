# Part 68: Dashboard Analytics

## เป้าหมาย
- KPI cards พร้อม trend indicators
- Charts ด้วย pure CSS + SVG
- Data tables แบบ sortable/filterable
- Real-time update simulation
- Steps 671–680

---

## Steps 671–680: Analytics Dashboard Workshop

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Analytics Dashboard - Steps 671-680</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    @keyframes countUp { from { opacity: 0; transform: translateY(8px); } to { opacity: 1; transform: translateY(0); } }
    .count-animate { animation: countUp 0.5s ease forwards; }
    @keyframes barGrow { from { height: 0; } to { height: var(--bar-h); } }
    .bar-grow { animation: barGrow 0.8s ease-out forwards; }
    @keyframes lineAppear { from { stroke-dashoffset: 1000; } to { stroke-dashoffset: 0; } }
    .line-path { stroke-dasharray: 1000; animation: lineAppear 1.2s ease-out forwards; }
    .sparkline-up { background: linear-gradient(to top, rgba(99,102,241,.15), transparent); }
    .sparkline-down { background: linear-gradient(to top, rgba(239,68,68,.1), transparent); }
  </style>
</head>
<body class="bg-gray-100 min-h-screen">

  <!-- Sidebar -->
  <div class="flex h-screen overflow-hidden">
    <aside class="w-56 bg-gray-900 flex flex-col py-5 flex-shrink-0">
      <div class="px-5 mb-6">
        <span class="font-extrabold text-white text-lg">📊 Analytics</span>
      </div>
      <nav class="flex-1 px-3 space-y-1">
        <a href="#" class="flex items-center gap-2.5 px-3 py-2 text-sm font-medium rounded-xl bg-indigo-600 text-white">📈 Overview</a>
        <a href="#" class="flex items-center gap-2.5 px-3 py-2 text-sm font-medium rounded-xl text-gray-400 hover:bg-gray-800 hover:text-white transition-colors">👥 Audience</a>
        <a href="#" class="flex items-center gap-2.5 px-3 py-2 text-sm font-medium rounded-xl text-gray-400 hover:bg-gray-800 hover:text-white transition-colors">🛒 Revenue</a>
        <a href="#" class="flex items-center gap-2.5 px-3 py-2 text-sm font-medium rounded-xl text-gray-400 hover:bg-gray-800 hover:text-white transition-colors">🔗 Acquisition</a>
        <a href="#" class="flex items-center gap-2.5 px-3 py-2 text-sm font-medium rounded-xl text-gray-400 hover:bg-gray-800 hover:text-white transition-colors">⚙️ Settings</a>
      </nav>
      <div class="px-3">
        <div class="bg-gray-800 rounded-xl p-3">
          <p class="text-xs text-gray-400">Data updated</p>
          <p id="lastUpdated" class="text-xs text-gray-300 font-semibold">just now</p>
        </div>
      </div>
    </aside>

    <!-- Main -->
    <main class="flex-1 overflow-y-auto p-6">

      <!-- Header -->
      <div class="flex items-center justify-between mb-6">
        <div>
          <h1 class="font-extrabold text-xl text-gray-900">Overview</h1>
          <p class="text-sm text-gray-500">Sep 1 – Sep 30, 2024</p>
        </div>
        <div class="flex gap-2">
          <select class="text-sm border border-gray-300 rounded-xl px-3 py-2 focus:outline-none focus:ring-2 focus:ring-indigo-400 bg-white">
            <option>Last 30 days</option>
            <option>Last 7 days</option>
            <option>Last 90 days</option>
            <option>This year</option>
          </select>
          <button onclick="refreshData()" class="bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors flex items-center gap-1.5">
            <span id="refreshIcon">↻</span> Refresh
          </button>
        </div>
      </div>

      <!-- KPI Grid -->
      <div class="grid grid-cols-2 lg:grid-cols-4 gap-4 mb-6">
        <div class="bg-white rounded-2xl p-4 border hover:shadow-md transition-shadow">
          <div class="flex items-start justify-between mb-2">
            <div>
              <p class="text-xs font-semibold text-gray-500 uppercase tracking-wide">Total Revenue</p>
              <p id="kpi-revenue" class="text-2xl font-extrabold text-gray-900 count-animate">฿284,560</p>
            </div>
            <div class="w-9 h-9 bg-indigo-100 rounded-xl flex items-center justify-center text-lg">💰</div>
          </div>
          <div class="flex items-center gap-1.5">
            <span class="text-xs font-bold text-emerald-600 bg-emerald-50 px-1.5 py-0.5 rounded-full">+18.2%</span>
            <span class="text-xs text-gray-400">vs last month</span>
          </div>
          <div id="spark-revenue" class="h-8 mt-3 flex items-end gap-0.5"></div>
        </div>

        <div class="bg-white rounded-2xl p-4 border hover:shadow-md transition-shadow">
          <div class="flex items-start justify-between mb-2">
            <div>
              <p class="text-xs font-semibold text-gray-500 uppercase tracking-wide">Total Users</p>
              <p id="kpi-users" class="text-2xl font-extrabold text-gray-900 count-animate">12,847</p>
            </div>
            <div class="w-9 h-9 bg-blue-100 rounded-xl flex items-center justify-center text-lg">👥</div>
          </div>
          <div class="flex items-center gap-1.5">
            <span class="text-xs font-bold text-emerald-600 bg-emerald-50 px-1.5 py-0.5 rounded-full">+9.4%</span>
            <span class="text-xs text-gray-400">vs last month</span>
          </div>
          <div id="spark-users" class="h-8 mt-3 flex items-end gap-0.5"></div>
        </div>

        <div class="bg-white rounded-2xl p-4 border hover:shadow-md transition-shadow">
          <div class="flex items-start justify-between mb-2">
            <div>
              <p class="text-xs font-semibold text-gray-500 uppercase tracking-wide">Conversion</p>
              <p id="kpi-conv" class="text-2xl font-extrabold text-gray-900 count-animate">3.24%</p>
            </div>
            <div class="w-9 h-9 bg-emerald-100 rounded-xl flex items-center justify-center text-lg">🎯</div>
          </div>
          <div class="flex items-center gap-1.5">
            <span class="text-xs font-bold text-red-600 bg-red-50 px-1.5 py-0.5 rounded-full">-1.2%</span>
            <span class="text-xs text-gray-400">vs last month</span>
          </div>
          <div id="spark-conv" class="h-8 mt-3 flex items-end gap-0.5"></div>
        </div>

        <div class="bg-white rounded-2xl p-4 border hover:shadow-md transition-shadow">
          <div class="flex items-start justify-between mb-2">
            <div>
              <p class="text-xs font-semibold text-gray-500 uppercase tracking-wide">Avg. Order</p>
              <p id="kpi-aov" class="text-2xl font-extrabold text-gray-900 count-animate">฿1,245</p>
            </div>
            <div class="w-9 h-9 bg-yellow-100 rounded-xl flex items-center justify-center text-lg">🛍</div>
          </div>
          <div class="flex items-center gap-1.5">
            <span class="text-xs font-bold text-emerald-600 bg-emerald-50 px-1.5 py-0.5 rounded-full">+5.6%</span>
            <span class="text-xs text-gray-400">vs last month</span>
          </div>
          <div id="spark-aov" class="h-8 mt-3 flex items-end gap-0.5"></div>
        </div>
      </div>

      <!-- Charts row -->
      <div class="grid grid-cols-1 lg:grid-cols-3 gap-4 mb-6">

        <!-- Line Chart -->
        <div class="lg:col-span-2 bg-white rounded-2xl border p-5">
          <div class="flex items-center justify-between mb-4">
            <h2 class="font-bold text-gray-900">Revenue & Orders</h2>
            <div class="flex text-xs gap-3">
              <span class="flex items-center gap-1"><span class="w-2 h-2 rounded-full bg-indigo-500 inline-block"></span>Revenue</span>
              <span class="flex items-center gap-1"><span class="w-2 h-2 rounded-full bg-emerald-400 inline-block"></span>Orders</span>
            </div>
          </div>
          <svg id="lineChart" class="w-full" style="height:180px" viewBox="0 0 600 180" preserveAspectRatio="none">
            <!-- Grid lines -->
            <line x1="0" y1="36" x2="600" y2="36" stroke="#f3f4f6" stroke-width="1"/>
            <line x1="0" y1="72" x2="600" y2="72" stroke="#f3f4f6" stroke-width="1"/>
            <line x1="0" y1="108" x2="600" y2="108" stroke="#f3f4f6" stroke-width="1"/>
            <line x1="0" y1="144" x2="600" y2="144" stroke="#f3f4f6" stroke-width="1"/>
            <!-- Revenue line -->
            <polyline id="revLine" fill="none" stroke="#6366f1" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round" class="line-path"/>
            <!-- Orders line -->
            <polyline id="ordLine" fill="none" stroke="#34d399" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="line-path"/>
            <!-- Dots revenue -->
            <g id="revDots"></g>
            <!-- Dots orders -->
            <g id="ordDots"></g>
          </svg>
          <div class="flex justify-between mt-2 text-xs text-gray-400">
            <span>ม.ค.</span><span>ก.พ.</span><span>มี.ค.</span><span>เม.ย.</span><span>พ.ค.</span><span>มิ.ย.</span><span>ก.ค.</span><span>ส.ค.</span><span>ก.ย.</span><span>ต.ค.</span><span>พ.ย.</span><span>ธ.ค.</span>
          </div>
        </div>

        <!-- Donut Chart -->
        <div class="bg-white rounded-2xl border p-5">
          <h2 class="font-bold text-gray-900 mb-4">Traffic Sources</h2>
          <div class="relative flex justify-center mb-4">
            <svg viewBox="0 0 100 100" class="w-32 h-32 -rotate-90">
              <circle cx="50" cy="50" r="40" fill="none" stroke="#e5e7eb" stroke-width="16"/>
              <circle id="d1" cx="50" cy="50" r="40" fill="none" stroke="#6366f1" stroke-width="16" stroke-dasharray="251.2" stroke-dashoffset="0" style="transition: stroke-dashoffset 1s ease;"/>
              <circle id="d2" cx="50" cy="50" r="40" fill="none" stroke="#34d399" stroke-width="16" stroke-dasharray="251.2" stroke-dashoffset="0" style="transition: stroke-dashoffset 1s ease 0.2s;"/>
              <circle id="d3" cx="50" cy="50" r="40" fill="none" stroke="#fbbf24" stroke-width="16" stroke-dasharray="251.2" stroke-dashoffset="0" style="transition: stroke-dashoffset 1s ease 0.4s;"/>
              <circle id="d4" cx="50" cy="50" r="40" fill="none" stroke="#f87171" stroke-width="16" stroke-dasharray="251.2" stroke-dashoffset="0" style="transition: stroke-dashoffset 1s ease 0.6s;"/>
            </svg>
            <div class="absolute inset-0 flex flex-col items-center justify-center rotate-0">
              <span class="text-xl font-extrabold text-gray-900">100%</span>
              <span class="text-xs text-gray-400">Traffic</span>
            </div>
          </div>
          <div class="space-y-2" id="donutLegend"></div>
        </div>
      </div>

      <!-- Bar chart + Table -->
      <div class="grid grid-cols-1 lg:grid-cols-2 gap-4">

        <!-- Bar Chart -->
        <div class="bg-white rounded-2xl border p-5">
          <h2 class="font-bold text-gray-900 mb-4">Top Products</h2>
          <div id="barChart" class="space-y-3"></div>
        </div>

        <!-- Data Table -->
        <div class="bg-white rounded-2xl border p-5">
          <div class="flex items-center justify-between mb-4">
            <h2 class="font-bold text-gray-900">Recent Transactions</h2>
            <input placeholder="ค้นหา..." id="txSearch" oninput="filterTx()" class="text-xs border border-gray-200 rounded-lg px-2.5 py-1.5 focus:outline-none focus:ring-1 focus:ring-indigo-400">
          </div>
          <div class="overflow-x-auto">
            <table class="w-full text-sm">
              <thead>
                <tr class="border-b">
                  <th class="text-left pb-2 text-xs font-semibold text-gray-500 cursor-pointer hover:text-gray-700" onclick="sortTx('id')">ID ↕</th>
                  <th class="text-left pb-2 text-xs font-semibold text-gray-500 cursor-pointer hover:text-gray-700" onclick="sortTx('name')">Customer ↕</th>
                  <th class="text-right pb-2 text-xs font-semibold text-gray-500 cursor-pointer hover:text-gray-700" onclick="sortTx('amount')">Amount ↕</th>
                  <th class="text-center pb-2 text-xs font-semibold text-gray-500">Status</th>
                </tr>
              </thead>
              <tbody id="txBody"></tbody>
            </table>
          </div>
          <div class="flex items-center justify-between mt-3">
            <span id="txInfo" class="text-xs text-gray-400"></span>
            <div class="flex gap-1">
              <button onclick="prevPage()" class="w-6 h-6 border rounded-lg text-xs hover:bg-gray-50 transition-colors">‹</button>
              <button onclick="nextPage()" class="w-6 h-6 border rounded-lg text-xs hover:bg-gray-50 transition-colors">›</button>
            </div>
          </div>
        </div>
      </div>

    </main>
  </div>

  <script>
    // Data
    const revenueData = [42, 58, 51, 73, 65, 89, 78, 95, 108, 92, 115, 102];
    const ordersData =  [20, 28, 24, 35, 30, 42, 38, 48, 52, 45, 58, 50];
    const sources = [
      { name: 'Organic Search', pct: 38, color: '#6366f1' },
      { name: 'Direct', pct: 26, color: '#34d399' },
      { name: 'Social Media', pct: 22, color: '#fbbf24' },
      { name: 'Referral', pct: 14, color: '#f87171' },
    ];
    const products = [
      { name: 'iPhone 15 Pro', sales: 284 },
      { name: 'MacBook Pro', sales: 217 },
      { name: 'AirPods Pro', sales: 189 },
      { name: 'iPad Air', sales: 156 },
      { name: 'Apple Watch', sales: 124 },
    ];
    const allTx = Array.from({length: 32}, (_, i) => ({
      id: `TX${String(i+1001).slice(1)}`,
      name: ['สมชาย วงค์', 'นภา มี', 'ธนพล จันทร์', 'พรทิพย์ แก้ว', 'วิชัย สุข', 'ศิริ มั่น', 'อรุณ บัว', 'มณี ทอง'][i % 8],
      amount: Math.floor(Math.random() * 8000) + 500,
      status: ['paid', 'paid', 'paid', 'pending', 'failed'][Math.floor(Math.random() * 5)],
    }));

    let txPage = 0, txSort = 'id', txAsc = true, txFiltered = [...allTx];
    const PER_PAGE = 6;

    // Sparklines
    function renderSparklines() {
      ['revenue', 'users', 'conv', 'aov'].forEach(key => {
        const el = document.getElementById(`spark-${key}`);
        if (!el) return;
        const data = Array.from({length: 12}, () => Math.random());
        const max = Math.max(...data);
        el.innerHTML = data.map(v => {
          const h = Math.round((v / max) * 28) + 4;
          const isUp = key !== 'conv';
          return `<div style="height:${h}px" class="flex-1 rounded-sm ${isUp ? 'bg-indigo-200' : 'bg-red-200'} last:${isUp ? 'bg-indigo-500' : 'bg-red-400'}"></div>`;
        }).join('');
      });
    }

    // Line chart
    function renderLineChart() {
      const W = 600, H = 180, PAD = 10;
      const maxV = 120;
      const pts = (data) => data.map((v, i) => {
        const x = PAD + (i / (data.length - 1)) * (W - PAD*2);
        const y = H - PAD - (v / maxV) * (H - PAD*2);
        return `${x},${y}`;
      }).join(' ');

      document.getElementById('revLine').setAttribute('points', pts(revenueData));
      document.getElementById('ordLine').setAttribute('points', pts(ordersData));

      const rDots = document.getElementById('revDots');
      const oDots = document.getElementById('ordDots');
      rDots.innerHTML = revenueData.map((v, i) => {
        const x = PAD + (i / (revenueData.length-1)) * (W - PAD*2);
        const y = H - PAD - (v / maxV) * (H - PAD*2);
        return `<circle cx="${x}" cy="${y}" r="3" fill="#6366f1" class="opacity-0 hover:opacity-100 transition-opacity cursor-pointer" title="${v}"/>`;
      }).join('');
      oDots.innerHTML = ordersData.map((v, i) => {
        const x = PAD + (i / (ordersData.length-1)) * (W - PAD*2);
        const y = H - PAD - (v / maxV) * (H - PAD*2);
        return `<circle cx="${x}" cy="${y}" r="2.5" fill="#34d399"/>`;
      }).join('');
    }

    // Donut chart
    function renderDonut() {
      const C = 2 * Math.PI * 40; // circumference
      let offset = 0;
      const circles = ['d1','d2','d3','d4'];
      sources.forEach((s, i) => {
        const dash = (s.pct / 100) * C;
        const el = document.getElementById(circles[i]);
        el.style.strokeDasharray = `${dash} ${C - dash}`;
        el.style.strokeDashoffset = -offset;
        offset += dash;
      });
      document.getElementById('donutLegend').innerHTML = sources.map(s => `
        <div class="flex items-center justify-between">
          <div class="flex items-center gap-2">
            <span class="w-2.5 h-2.5 rounded-full flex-shrink-0" style="background:${s.color}"></span>
            <span class="text-xs text-gray-600">${s.name}</span>
          </div>
          <span class="text-xs font-bold text-gray-800">${s.pct}%</span>
        </div>
      `).join('');
    }

    // Bar chart
    function renderBarChart() {
      const max = products[0].sales;
      document.getElementById('barChart').innerHTML = products.map((p, i) => `
        <div>
          <div class="flex justify-between text-xs mb-1">
            <span class="text-gray-600">${p.name}</span>
            <span class="font-semibold text-gray-800">${p.sales} units</span>
          </div>
          <div class="h-2 bg-gray-100 rounded-full overflow-hidden">
            <div class="h-full rounded-full transition-all duration-700" style="width: ${(p.sales/max)*100}%; background: hsl(${240 + i*20}, 70%, 60%)"></div>
          </div>
        </div>
      `).join('');
    }

    // Transaction table
    function filterTx() {
      const q = document.getElementById('txSearch').value.toLowerCase();
      txFiltered = allTx.filter(t => t.name.toLowerCase().includes(q) || t.id.toLowerCase().includes(q));
      txPage = 0;
      renderTx();
    }

    function sortTx(col) {
      if (txSort === col) txAsc = !txAsc;
      else { txSort = col; txAsc = true; }
      txFiltered.sort((a, b) => {
        let va = a[col], vb = b[col];
        if (col === 'amount') return txAsc ? va - vb : vb - va;
        return txAsc ? String(va).localeCompare(String(vb)) : String(vb).localeCompare(String(va));
      });
      renderTx();
    }

    function renderTx() {
      const start = txPage * PER_PAGE;
      const page = txFiltered.slice(start, start + PER_PAGE);
      document.getElementById('txInfo').textContent = `แสดง ${start+1}–${Math.min(start+PER_PAGE, txFiltered.length)} จาก ${txFiltered.length}`;
      const statusClass = { paid: 'bg-emerald-100 text-emerald-700', pending: 'bg-yellow-100 text-yellow-700', failed: 'bg-red-100 text-red-600' };
      document.getElementById('txBody').innerHTML = page.map(t => `
        <tr class="border-b last:border-0 hover:bg-gray-50 transition-colors">
          <td class="py-2 text-xs font-mono text-gray-500">${t.id}</td>
          <td class="py-2 text-xs text-gray-800 font-medium">${t.name}</td>
          <td class="py-2 text-xs text-right font-semibold text-gray-900">฿${t.amount.toLocaleString()}</td>
          <td class="py-2 text-center"><span class="text-[10px] font-bold px-2 py-0.5 rounded-full ${statusClass[t.status]}">${t.status}</span></td>
        </tr>
      `).join('');
    }

    function prevPage() { if (txPage > 0) { txPage--; renderTx(); } }
    function nextPage() { if ((txPage+1)*PER_PAGE < txFiltered.length) { txPage++; renderTx(); } }

    function refreshData() {
      const icon = document.getElementById('refreshIcon');
      icon.style.animation = 'spin 0.8s linear infinite';
      icon.style.display = 'inline-block';
      setTimeout(() => {
        icon.style.animation = '';
        document.getElementById('lastUpdated').textContent = new Date().toLocaleTimeString('th-TH');
        renderSparklines();
      }, 800);
    }

    // Init
    renderSparklines();
    renderLineChart();
    renderDonut();
    renderBarChart();
    renderTx();

    // Simulate real-time updates every 8s
    setInterval(() => {
      const kpis = ['kpi-revenue', 'kpi-users', 'kpi-conv', 'kpi-aov'];
      kpis.forEach(id => {
        const el = document.getElementById(id);
        el.style.opacity = '0';
        setTimeout(() => { el.style.opacity = '1'; el.style.transition = 'opacity 0.3s'; }, 200);
      });
    }, 8000);
  </script>
</body>
</html>
```

---

## สรุป Part 68

| Step | เนื้อหา |
|------|---------|
| 671 | Dashboard layout — sidebar + main content grid |
| 672 | KPI cards พร้อม sparklines, trend badges |
| 673 | Sparkline mini charts ด้วย CSS bars |
| 674 | SVG line chart — 12 เดือน, dual series |
| 675 | Donut/pie chart ด้วย SVG stroke-dasharray |
| 676 | Horizontal bar chart สำหรับ top products |
| 677 | Data table — sortable columns |
| 678 | Search filter + pagination |
| 679 | Traffic sources legend |
| 680 | Workshop: Complete Analytics Dashboard — KPI/charts/table/real-time |

**Part ถัดไป:** Part 69 — Authentication UI (Steps 681–690)
