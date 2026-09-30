# Part 41: Dashboard Analytics & Data Visualization

## เป้าหมาย
- สร้าง Analytics Dashboard ระดับ Professional
- KPI Cards, Charts (CSS-only + SVG), Data Tables
- Date Range Picker, Export, Real-time Updates Pattern
- Steps 401–410

---

## Step 401: Analytics Dashboard Layout

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Analytics Dashboard - Step 401</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 text-gray-900">

  <!-- Top Bar -->
  <header class="bg-white border-b px-6 h-16 flex items-center justify-between sticky top-0 z-30">
    <div class="flex items-center gap-4">
      <h1 class="text-lg font-extrabold text-indigo-600">Analytics</h1>
      <div class="hidden md:flex bg-gray-100 rounded-xl p-1 text-sm">
        <button class="px-3 py-1.5 rounded-lg bg-white shadow text-gray-800 font-medium">Overview</button>
        <button class="px-3 py-1.5 rounded-lg text-gray-500 hover:text-gray-800 transition-colors">Revenue</button>
        <button class="px-3 py-1.5 rounded-lg text-gray-500 hover:text-gray-800 transition-colors">Users</button>
        <button class="px-3 py-1.5 rounded-lg text-gray-500 hover:text-gray-800 transition-colors">Events</button>
      </div>
    </div>
    <div class="flex items-center gap-3">
      <!-- Date Range -->
      <div class="hidden md:flex items-center gap-2 border border-gray-200 rounded-xl px-3 py-2 text-sm bg-white cursor-pointer hover:border-indigo-400 transition-colors">
        <svg class="w-4 h-4 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"/></svg>
        <span class="text-gray-600">1 – 30 มิ.ย. 2024</span>
        <svg class="w-3 h-3 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"/></svg>
      </div>
      <button class="flex items-center gap-2 border border-gray-200 rounded-xl px-3 py-2 text-sm hover:border-indigo-400 transition-colors bg-white">
        <svg class="w-4 h-4 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 10v6m0 0l-3-3m3 3l3-3m2 8H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"/></svg>
        Export
      </button>
      <button class="p-2 hover:bg-gray-100 rounded-xl transition-colors relative">
        <svg class="w-5 h-5 text-gray-500" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6.002 6.002 0 00-4-5.659V5a2 2 0 10-4 0v.341C7.67 6.165 6 8.388 6 11v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9"/></svg>
        <span class="absolute top-1 right-1 w-2 h-2 bg-red-500 rounded-full"></span>
      </button>
    </div>
  </header>

  <div class="p-6 max-w-7xl mx-auto">

    <!-- KPI Cards Row -->
    <div class="grid grid-cols-2 lg:grid-cols-4 gap-4 mb-6">

      <div class="bg-white rounded-2xl p-5 border">
        <div class="flex items-center justify-between mb-3">
          <div class="w-10 h-10 bg-blue-100 rounded-xl flex items-center justify-center">
            <svg class="w-5 h-5 text-blue-600" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0z"/></svg>
          </div>
          <span class="text-xs bg-green-100 text-green-700 font-semibold px-2 py-0.5 rounded-full">+12.4%</span>
        </div>
        <p class="text-2xl font-extrabold">48,291</p>
        <p class="text-sm text-gray-500 mt-0.5">ผู้ใช้งานทั้งหมด</p>
        <div class="mt-3 h-1 bg-gray-100 rounded-full"><div class="h-1 bg-blue-500 rounded-full" style="width:74%"></div></div>
      </div>

      <div class="bg-white rounded-2xl p-5 border">
        <div class="flex items-center justify-between mb-3">
          <div class="w-10 h-10 bg-green-100 rounded-xl flex items-center justify-center">
            <svg class="w-5 h-5 text-green-600" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z"/></svg>
          </div>
          <span class="text-xs bg-green-100 text-green-700 font-semibold px-2 py-0.5 rounded-full">+8.7%</span>
        </div>
        <p class="text-2xl font-extrabold">฿2.41M</p>
        <p class="text-sm text-gray-500 mt-0.5">รายได้เดือนนี้</p>
        <div class="mt-3 h-1 bg-gray-100 rounded-full"><div class="h-1 bg-green-500 rounded-full" style="width:61%"></div></div>
      </div>

      <div class="bg-white rounded-2xl p-5 border">
        <div class="flex items-center justify-between mb-3">
          <div class="w-10 h-10 bg-purple-100 rounded-xl flex items-center justify-center">
            <svg class="w-5 h-5 text-purple-600" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z"/><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M2.458 12C3.732 7.943 7.523 5 12 5c4.478 0 8.268 2.943 9.542 7-1.274 4.057-5.064 7-9.542 7-4.477 0-8.268-2.943-9.542-7z"/></svg>
          </div>
          <span class="text-xs bg-red-100 text-red-600 font-semibold px-2 py-0.5 rounded-full">-3.2%</span>
        </div>
        <p class="text-2xl font-extrabold">1,284,920</p>
        <p class="text-sm text-gray-500 mt-0.5">Page Views</p>
        <div class="mt-3 h-1 bg-gray-100 rounded-full"><div class="h-1 bg-purple-500 rounded-full" style="width:88%"></div></div>
      </div>

      <div class="bg-white rounded-2xl p-5 border">
        <div class="flex items-center justify-between mb-3">
          <div class="w-10 h-10 bg-orange-100 rounded-xl flex items-center justify-center">
            <svg class="w-5 h-5 text-orange-600" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 7h8m0 0v8m0-8l-8 8-4-4-6 6"/></svg>
          </div>
          <span class="text-xs bg-green-100 text-green-700 font-semibold px-2 py-0.5 rounded-full">+5.1%</span>
        </div>
        <p class="text-2xl font-extrabold">4.8%</p>
        <p class="text-sm text-gray-500 mt-0.5">Conversion Rate</p>
        <div class="mt-3 h-1 bg-gray-100 rounded-full"><div class="h-1 bg-orange-500 rounded-full" style="width:48%"></div></div>
      </div>
    </div>

    <!-- Charts Row -->
    <div class="grid grid-cols-1 lg:grid-cols-3 gap-5 mb-5">

      <!-- Revenue Chart (CSS Bar Chart) -->
      <div class="lg:col-span-2 bg-white rounded-2xl border p-5">
        <div class="flex items-center justify-between mb-5">
          <div>
            <h3 class="font-bold">รายได้รายวัน</h3>
            <p class="text-xs text-gray-400 mt-0.5">30 วันล่าสุด</p>
          </div>
          <select class="text-xs border rounded-lg px-2 py-1 focus:outline-none text-gray-600">
            <option>30 วัน</option>
            <option>7 วัน</option>
            <option>90 วัน</option>
          </select>
        </div>
        <!-- Bar Chart -->
        <div class="flex items-end gap-1 h-32" id="bar-chart"></div>
        <div class="flex justify-between text-[10px] text-gray-400 mt-1 px-0.5" id="bar-labels"></div>
      </div>

      <!-- Donut Chart (CSS) -->
      <div class="bg-white rounded-2xl border p-5">
        <h3 class="font-bold mb-4">Traffic Sources</h3>
        <div class="flex flex-col items-center">
          <!-- CSS Donut -->
          <div class="relative w-32 h-32 mb-4">
            <svg viewBox="0 0 36 36" class="w-32 h-32 -rotate-90">
              <circle cx="18" cy="18" r="15.9" fill="none" stroke="#e5e7eb" stroke-width="3"/>
              <circle cx="18" cy="18" r="15.9" fill="none" stroke="#6366f1" stroke-width="3"
                stroke-dasharray="42 58" stroke-dashoffset="0"/>
              <circle cx="18" cy="18" r="15.9" fill="none" stroke="#10b981" stroke-width="3"
                stroke-dasharray="28 72" stroke-dashoffset="-42"/>
              <circle cx="18" cy="18" r="15.9" fill="none" stroke="#f59e0b" stroke-width="3"
                stroke-dasharray="18 82" stroke-dashoffset="-70"/>
              <circle cx="18" cy="18" r="15.9" fill="none" stroke="#ef4444" stroke-width="3"
                stroke-dasharray="12 88" stroke-dashoffset="-88"/>
            </svg>
            <div class="absolute inset-0 flex flex-col items-center justify-center">
              <p class="text-lg font-extrabold">100%</p>
              <p class="text-[10px] text-gray-400">Total</p>
            </div>
          </div>
          <div class="space-y-2 w-full text-sm">
            <div class="flex items-center justify-between">
              <div class="flex items-center gap-2"><span class="w-2.5 h-2.5 bg-indigo-500 rounded-full"></span><span>Organic</span></div>
              <span class="font-bold">42%</span>
            </div>
            <div class="flex items-center justify-between">
              <div class="flex items-center gap-2"><span class="w-2.5 h-2.5 bg-emerald-500 rounded-full"></span><span>Direct</span></div>
              <span class="font-bold">28%</span>
            </div>
            <div class="flex items-center justify-between">
              <div class="flex items-center gap-2"><span class="w-2.5 h-2.5 bg-amber-500 rounded-full"></span><span>Social</span></div>
              <span class="font-bold">18%</span>
            </div>
            <div class="flex items-center justify-between">
              <div class="flex items-center gap-2"><span class="w-2.5 h-2.5 bg-red-500 rounded-full"></span><span>Referral</span></div>
              <span class="font-bold">12%</span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Data Table -->
    <div class="bg-white rounded-2xl border p-5">
      <div class="flex items-center justify-between mb-4">
        <h3 class="font-bold">Top Pages</h3>
        <button class="text-xs text-indigo-600 hover:underline">ดูทั้งหมด</button>
      </div>
      <div class="overflow-x-auto">
        <table class="w-full text-sm">
          <thead>
            <tr class="border-b text-left">
              <th class="pb-3 font-semibold text-gray-500 text-xs uppercase tracking-wide">หน้า</th>
              <th class="pb-3 font-semibold text-gray-500 text-xs uppercase tracking-wide text-right">Views</th>
              <th class="pb-3 font-semibold text-gray-500 text-xs uppercase tracking-wide text-right hidden md:table-cell">เวลาเฉลี่ย</th>
              <th class="pb-3 font-semibold text-gray-500 text-xs uppercase tracking-wide text-right hidden md:table-cell">Bounce</th>
              <th class="pb-3 font-semibold text-gray-500 text-xs uppercase tracking-wide"></th>
            </tr>
          </thead>
          <tbody class="divide-y">
            <tr class="hover:bg-gray-50 transition-colors">
              <td class="py-3 font-medium">/</td>
              <td class="py-3 text-right tabular-nums">284,291</td>
              <td class="py-3 text-right hidden md:table-cell text-gray-500">2:14</td>
              <td class="py-3 text-right hidden md:table-cell">
                <span class="text-green-600 text-xs font-semibold">38.2%</span>
              </td>
              <td class="py-3 pl-4">
                <div class="w-20 h-1.5 bg-gray-100 rounded-full"><div class="h-1.5 bg-indigo-500 rounded-full" style="width:100%"></div></div>
              </td>
            </tr>
            <tr class="hover:bg-gray-50 transition-colors">
              <td class="py-3 font-medium">/products</td>
              <td class="py-3 text-right tabular-nums">148,734</td>
              <td class="py-3 text-right hidden md:table-cell text-gray-500">3:42</td>
              <td class="py-3 text-right hidden md:table-cell"><span class="text-green-600 text-xs font-semibold">22.4%</span></td>
              <td class="py-3 pl-4"><div class="w-20 h-1.5 bg-gray-100 rounded-full"><div class="h-1.5 bg-indigo-500 rounded-full" style="width:52%"></div></div></td>
            </tr>
            <tr class="hover:bg-gray-50 transition-colors">
              <td class="py-3 font-medium">/blog</td>
              <td class="py-3 text-right tabular-nums">92,481</td>
              <td class="py-3 text-right hidden md:table-cell text-gray-500">5:18</td>
              <td class="py-3 text-right hidden md:table-cell"><span class="text-orange-600 text-xs font-semibold">45.8%</span></td>
              <td class="py-3 pl-4"><div class="w-20 h-1.5 bg-gray-100 rounded-full"><div class="h-1.5 bg-indigo-500 rounded-full" style="width:32%"></div></div></td>
            </tr>
            <tr class="hover:bg-gray-50 transition-colors">
              <td class="py-3 font-medium">/pricing</td>
              <td class="py-3 text-right tabular-nums">74,120</td>
              <td class="py-3 text-right hidden md:table-cell text-gray-500">4:01</td>
              <td class="py-3 text-right hidden md:table-cell"><span class="text-green-600 text-xs font-semibold">29.1%</span></td>
              <td class="py-3 pl-4"><div class="w-20 h-1.5 bg-gray-100 rounded-full"><div class="h-1.5 bg-indigo-500 rounded-full" style="width:26%"></div></div></td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>

  <script>
    // Generate random bar chart
    const chart = document.getElementById('bar-chart');
    const labels = document.getElementById('bar-labels');
    const data = Array.from({length: 30}, () => Math.floor(Math.random() * 80 + 20));
    const max = Math.max(...data);
    data.forEach((v, i) => {
      const bar = document.createElement('div');
      bar.className = 'flex-1 rounded-sm transition-all duration-300 hover:opacity-80 cursor-pointer';
      bar.style.height = (v / max * 100) + '%';
      bar.style.backgroundColor = v > 70 ? '#6366f1' : '#c7d2fe';
      bar.title = '฿' + (v * 12000).toLocaleString();
      chart.appendChild(bar);
    });
    for (let i = 1; i <= 30; i += 7) {
      const lbl = document.createElement('span');
      lbl.textContent = i + ' มิ.ย.';
      labels.appendChild(lbl);
    }
  </script>

</body>
</html>
```

---

## Step 402: Funnel & Cohort Charts

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Funnel Chart - Step 402</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-8">

  <div class="max-w-3xl mx-auto space-y-6">

    <!-- Conversion Funnel -->
    <div class="bg-white rounded-2xl border p-6">
      <h3 class="font-extrabold text-lg mb-6">Conversion Funnel</h3>
      <div class="space-y-2" id="funnel"></div>
    </div>

    <!-- Cohort Retention Table -->
    <div class="bg-white rounded-2xl border p-6 overflow-x-auto">
      <h3 class="font-extrabold text-lg mb-4">Cohort Retention</h3>
      <table class="text-xs w-full">
        <thead>
          <tr class="text-gray-500">
            <th class="text-left py-2 pr-4 font-semibold">Cohort</th>
            <th class="text-center py-2 px-2 font-semibold w-12">Week 0</th>
            <th class="text-center py-2 px-2 font-semibold w-12">Week 1</th>
            <th class="text-center py-2 px-2 font-semibold w-12">Week 2</th>
            <th class="text-center py-2 px-2 font-semibold w-12">Week 3</th>
            <th class="text-center py-2 px-2 font-semibold w-12">Week 4</th>
          </tr>
        </thead>
        <tbody id="cohort-table" class="text-center divide-y"></tbody>
      </table>
    </div>

  </div>

  <script>
    // Funnel
    const steps = [
      { label: 'เยี่ยมชมเว็บ', value: 100000, pct: 100 },
      { label: 'ดูสินค้า', value: 64200, pct: 64.2 },
      { label: 'เพิ่มลงตะกร้า', value: 18400, pct: 18.4 },
      { label: 'เริ่ม Checkout', value: 9200, pct: 9.2 },
      { label: 'ชำระเงินสำเร็จ', value: 4800, pct: 4.8 },
    ];
    const colors = ['bg-indigo-600', 'bg-indigo-500', 'bg-indigo-400', 'bg-indigo-300', 'bg-indigo-200'];
    const funnel = document.getElementById('funnel');
    steps.forEach((s, i) => {
      const row = document.createElement('div');
      row.innerHTML = `
        <div class="flex items-center gap-3 mb-1">
          <span class="text-xs text-gray-500 w-32 flex-shrink-0">${s.label}</span>
          <div class="flex-1">
            <div class="flex items-center gap-2">
              <div class="flex-1 bg-gray-100 rounded-full h-6 overflow-hidden">
                <div class="${colors[i]} h-full rounded-full transition-all duration-700 flex items-center px-2" style="width:${s.pct}%">
                  <span class="text-white text-[10px] font-bold whitespace-nowrap">${s.pct}%</span>
                </div>
              </div>
              <span class="text-xs font-bold tabular-nums w-16 text-right">${s.value.toLocaleString()}</span>
            </div>
          </div>
        </div>
      `;
      funnel.appendChild(row);
    });

    // Cohort
    const cohorts = ['พ.ค. W1', 'พ.ค. W2', 'พ.ค. W3', 'มิ.ย. W1', 'มิ.ย. W2'];
    const tbody = document.getElementById('cohort-table');
    cohorts.forEach((c, ci) => {
      const tr = document.createElement('tr');
      let cells = `<td class="py-2 pr-4 text-left text-gray-600">${c}</td>`;
      for (let w = 0; w <= 4; w++) {
        if (w > (4 - ci)) {
          cells += `<td class="py-2 px-2 text-gray-200">—</td>`;
        } else {
          const base = 100 - (w * (18 + ci * 2));
          const pct = Math.max(base, 20);
          const intensity = Math.round(pct / 10);
          const bg = `bg-indigo-${Math.min(intensity, 9)}00`;
          const text = pct >= 50 ? 'text-indigo-900' : 'text-indigo-600';
          cells += `<td class="py-2 px-2"><div class="w-10 h-8 rounded-lg flex items-center justify-center text-xs font-bold ${text}" style="background: rgba(99,102,241,${pct/120})">${pct}%</div></td>`;
        }
      }
      tr.innerHTML = cells;
      tbody.appendChild(tr);
    });
  </script>

</body>
</html>
```

---

## Step 403: Real-time Dashboard

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Real-time Dashboard - Step 403</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-900 text-white p-6">

  <div class="max-w-5xl mx-auto">
    <div class="flex items-center gap-3 mb-6">
      <div class="w-2.5 h-2.5 bg-green-400 rounded-full animate-pulse"></div>
      <h2 class="text-lg font-extrabold">Real-time Monitor</h2>
      <span class="text-xs text-gray-400">อัปเดตทุก 3 วินาที</span>
    </div>

    <!-- Live Stats -->
    <div class="grid grid-cols-2 md:grid-cols-4 gap-4 mb-6">
      <div class="bg-gray-800 rounded-2xl p-4 border border-gray-700">
        <p class="text-xs text-gray-400 mb-1">ผู้ใช้งานออนไลน์</p>
        <p class="text-3xl font-extrabold text-green-400" id="live-users">1,284</p>
        <p class="text-xs text-gray-500 mt-1">↑ จากชั่วโมงที่แล้ว</p>
      </div>
      <div class="bg-gray-800 rounded-2xl p-4 border border-gray-700">
        <p class="text-xs text-gray-400 mb-1">Events/นาที</p>
        <p class="text-3xl font-extrabold text-blue-400" id="live-events">3,891</p>
        <p class="text-xs text-gray-500 mt-1">page views + clicks</p>
      </div>
      <div class="bg-gray-800 rounded-2xl p-4 border border-gray-700">
        <p class="text-xs text-gray-400 mb-1">คำสั่งซื้อวันนี้</p>
        <p class="text-3xl font-extrabold text-purple-400" id="live-orders">284</p>
        <p class="text-xs text-gray-500 mt-1">฿1.2M รายได้</p>
      </div>
      <div class="bg-gray-800 rounded-2xl p-4 border border-gray-700">
        <p class="text-xs text-gray-400 mb-1">Response Time</p>
        <p class="text-3xl font-extrabold text-yellow-400" id="live-rt">142ms</p>
        <p class="text-xs text-gray-500 mt-1">avg 99th percentile</p>
      </div>
    </div>

    <!-- Live Feed -->
    <div class="grid grid-cols-1 lg:grid-cols-2 gap-5">
      <div class="bg-gray-800 rounded-2xl border border-gray-700 p-4">
        <h3 class="text-sm font-semibold mb-3 text-gray-300">Live Activity Feed</h3>
        <div class="space-y-2 max-h-60 overflow-y-auto" id="activity-feed"></div>
      </div>

      <div class="bg-gray-800 rounded-2xl border border-gray-700 p-4">
        <h3 class="text-sm font-semibold mb-3 text-gray-300">Requests / sec (ย้อนหลัง 60s)</h3>
        <div class="flex items-end gap-0.5 h-32" id="live-chart"></div>
      </div>
    </div>
  </div>

  <script>
    const events = ['เข้าสู่ระบบ', 'ดูสินค้า', 'เพิ่มลงตะกร้า', 'ชำระเงิน', 'ลงทะเบียน', 'ค้นหา'];
    const users = ['สมชาย', 'วิไล', 'ธนพล', 'ปิยะ', 'อรทัย', 'กมล', 'ศิริ', 'วรรณ'];
    const pages = ['/products', '/cart', '/checkout', '/blog', '/pricing', '/'];
    const feed = document.getElementById('activity-feed');
    const liveChart = document.getElementById('live-chart');

    // Init chart bars
    const bars = Array.from({length: 60}, () => {
      const bar = document.createElement('div');
      bar.className = 'flex-1 bg-indigo-500 rounded-sm opacity-70 transition-height duration-300';
      bar.style.height = Math.floor(Math.random() * 80 + 20) + '%';
      liveChart.appendChild(bar);
      return bar;
    });

    function addActivity() {
      const user = users[Math.floor(Math.random() * users.length)];
      const event = events[Math.floor(Math.random() * events.length)];
      const page = pages[Math.floor(Math.random() * pages.length)];
      const item = document.createElement('div');
      item.className = 'flex items-center gap-2 text-xs py-1.5 border-b border-gray-700 animate-pulse';
      item.innerHTML = `
        <div class="w-1.5 h-1.5 bg-green-400 rounded-full flex-shrink-0"></div>
        <span class="text-gray-400 w-12 flex-shrink-0">${new Date().toLocaleTimeString('th', {hour:'2-digit', minute:'2-digit', second:'2-digit'})}</span>
        <span class="text-gray-300 font-medium">${user}</span>
        <span class="text-gray-500">${event} · ${page}</span>
      `;
      setTimeout(() => item.classList.remove('animate-pulse'), 1000);
      feed.insertBefore(item, feed.firstChild);
      if (feed.children.length > 15) feed.removeChild(feed.lastChild);
    }

    function updateStats() {
      document.getElementById('live-users').textContent = (1200 + Math.floor(Math.random() * 300)).toLocaleString();
      document.getElementById('live-events').textContent = (3500 + Math.floor(Math.random() * 800)).toLocaleString();
      document.getElementById('live-orders').textContent = (280 + Math.floor(Math.random() * 20)).toString();
      document.getElementById('live-rt').textContent = (130 + Math.floor(Math.random() * 40)) + 'ms';
      bars.forEach(b => b.style.height = Math.floor(Math.random() * 80 + 20) + '%');
    }

    setInterval(addActivity, 1500);
    setInterval(updateStats, 3000);
    addActivity();
  </script>
</body>
</html>
```

---

## Steps 404–409: Advanced Table, Heatmap, Sparklines

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Advanced Analytics - Steps 404-409</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-6">
  <div class="max-w-5xl mx-auto space-y-5">

    <!-- Sparkline Table (Step 404) -->
    <div class="bg-white rounded-2xl border p-5">
      <h3 class="font-extrabold text-lg mb-4">Product Performance</h3>
      <div class="overflow-x-auto">
        <table class="w-full text-sm">
          <thead><tr class="border-b text-left text-xs text-gray-500 uppercase tracking-wide">
            <th class="pb-3">สินค้า</th>
            <th class="pb-3 text-right">รายได้</th>
            <th class="pb-3 text-right">เปลี่ยนแปลง</th>
            <th class="pb-3 text-center hidden md:table-cell">Trend (7d)</th>
            <th class="pb-3 text-right">สถานะ</th>
          </tr></thead>
          <tbody class="divide-y" id="perf-table"></tbody>
        </table>
      </div>
    </div>

    <!-- Activity Heatmap (Step 405) -->
    <div class="bg-white rounded-2xl border p-5">
      <h3 class="font-extrabold text-lg mb-4">Activity Heatmap (ชั่วโมง × วัน)</h3>
      <div class="overflow-x-auto">
        <div class="grid gap-1 min-w-[600px]" id="heatmap" style="grid-template-columns: repeat(25, 1fr)"></div>
      </div>
      <div class="flex items-center gap-2 mt-3 text-xs text-gray-400">
        <span>น้อย</span>
        <div class="flex gap-0.5">
          <div class="w-3 h-3 rounded-sm bg-indigo-100"></div>
          <div class="w-3 h-3 rounded-sm bg-indigo-200"></div>
          <div class="w-3 h-3 rounded-sm bg-indigo-400"></div>
          <div class="w-3 h-3 rounded-sm bg-indigo-600"></div>
          <div class="w-3 h-3 rounded-sm bg-indigo-800"></div>
        </div>
        <span>มาก</span>
      </div>
    </div>

    <!-- Comparison Chart (Step 406) -->
    <div class="bg-white rounded-2xl border p-5">
      <h3 class="font-extrabold text-lg mb-5">เปรียบเทียบรายได้ YoY</h3>
      <div class="space-y-3" id="yoy-chart"></div>
    </div>

  </div>

  <script>
    // Sparkline Table
    const products = [
      { name: 'iPhone 15 Pro Max', rev: 4582000, change: 12.4, trend: [40,55,48,70,65,80,90] },
      { name: 'MacBook Pro M3', rev: 2841000, change: 8.1, trend: [60,55,65,50,70,75,72] },
      { name: 'AirPods Pro', rev: 1240000, change: -3.2, trend: [80,70,65,60,55,58,50] },
      { name: 'Apple Watch S9', rev: 890000, change: 5.7, trend: [30,40,35,50,55,60,68] },
    ];
    const tbody = document.getElementById('perf-table');
    products.forEach(p => {
      const tr = document.createElement('tr');
      const isUp = p.change > 0;
      const sparkW = 80, sparkH = 28;
      const pts = p.trend.map((v, i) => `${i * (sparkW / 6)},${sparkH - (v / 100 * sparkH)}`).join(' ');
      tr.innerHTML = `
        <td class="py-3 font-medium">${p.name}</td>
        <td class="py-3 text-right font-mono">฿${(p.rev/1000000).toFixed(2)}M</td>
        <td class="py-3 text-right">
          <span class="${isUp ? 'text-green-600 bg-green-50' : 'text-red-600 bg-red-50'} text-xs font-bold px-2 py-0.5 rounded-full">
            ${isUp ? '+' : ''}${p.change}%
          </span>
        </td>
        <td class="py-3 text-center hidden md:table-cell">
          <svg width="${sparkW}" height="${sparkH}" class="inline-block">
            <polyline fill="none" stroke="${isUp ? '#6366f1' : '#ef4444'}" stroke-width="1.5" points="${pts}" stroke-linejoin="round" stroke-linecap="round"/>
          </svg>
        </td>
        <td class="py-3 text-right">
          <span class="${isUp ? 'bg-green-100 text-green-700' : 'bg-red-100 text-red-700'} text-xs font-semibold px-2 py-1 rounded-full">
            ${isUp ? '↑ เติบโต' : '↓ ลดลง'}
          </span>
        </td>
      `;
      tbody.appendChild(tr);
    });

    // Heatmap
    const days = ['จ', 'อ', 'พ', 'พฤ', 'ศ', 'ส', 'อา'];
    const heatmap = document.getElementById('heatmap');
    // Header: blank + 0-23
    const blank = document.createElement('div');
    heatmap.appendChild(blank);
    for (let h = 0; h < 24; h++) {
      const lbl = document.createElement('div');
      lbl.className = 'text-center text-[9px] text-gray-400';
      lbl.textContent = h;
      heatmap.appendChild(lbl);
    }
    days.forEach(day => {
      const lbl = document.createElement('div');
      lbl.className = 'text-[10px] text-gray-500 flex items-center justify-end pr-1 font-medium';
      lbl.textContent = day;
      heatmap.appendChild(lbl);
      for (let h = 0; h < 24; h++) {
        const v = Math.random();
        const cell = document.createElement('div');
        cell.className = 'aspect-square rounded-sm cursor-pointer hover:ring-1 hover:ring-indigo-400 transition-all';
        cell.style.backgroundColor = `rgba(99,102,241,${v.toFixed(2)})`;
        cell.title = `${day} ${h}:00 · ${Math.floor(v * 500)} events`;
        heatmap.appendChild(cell);
      }
    });

    // YoY
    const months = ['ม.ค.','ก.พ.','มี.ค.','เม.ย.','พ.ค.','มิ.ย.'];
    const yoy = document.getElementById('yoy-chart');
    months.forEach(m => {
      const curr = Math.floor(Math.random() * 40 + 60);
      const prev = Math.floor(Math.random() * 40 + 40);
      const row = document.createElement('div');
      row.innerHTML = `
        <div class="grid grid-cols-[60px_1fr_60px] items-center gap-3 text-xs">
          <span class="text-gray-500 text-right">${m}</span>
          <div class="space-y-1">
            <div class="h-3 bg-indigo-500 rounded-full transition-all duration-700" style="width:${curr}%"></div>
            <div class="h-3 bg-gray-200 rounded-full transition-all duration-700" style="width:${prev}%"></div>
          </div>
          <div class="text-left">
            <span class="font-bold">฿${curr * 12}K</span>
            <br><span class="text-gray-400">฿${prev * 12}K</span>
          </div>
        </div>
      `;
      yoy.appendChild(row);
    });
    const legend = document.createElement('div');
    legend.className = 'flex gap-4 mt-2 text-xs text-gray-500';
    legend.innerHTML = '<div class="flex items-center gap-1.5"><div class="w-3 h-3 bg-indigo-500 rounded-full"></div> 2024</div><div class="flex items-center gap-1.5"><div class="w-3 h-3 bg-gray-200 rounded-full border"></div> 2023</div>';
    yoy.appendChild(legend);
  </script>
</body>
</html>
```

---

## Step 410: Workshop — Full Analytics Dashboard

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Analytics Workshop - Step 410</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 text-gray-900">

  <!-- Header -->
  <header class="bg-white border-b px-6 h-14 flex items-center justify-between sticky top-0 z-30">
    <div class="flex items-center gap-3">
      <div class="w-2 h-2 bg-green-400 rounded-full animate-pulse"></div>
      <h1 class="font-extrabold text-indigo-700">TechShop Analytics</h1>
    </div>
    <div class="flex items-center gap-3">
      <select id="range-select" onchange="updateAll()" class="text-sm border rounded-xl px-3 py-1.5 focus:outline-none focus:ring-2 focus:ring-indigo-500">
        <option value="7">7 วัน</option>
        <option value="30" selected>30 วัน</option>
        <option value="90">90 วัน</option>
      </select>
      <button onclick="exportData()" class="bg-indigo-600 text-white text-xs px-3 py-1.5 rounded-lg hover:bg-indigo-700 transition-colors">Export</button>
    </div>
  </header>

  <div class="p-5 max-w-7xl mx-auto">
    <!-- KPIs -->
    <div class="grid grid-cols-2 lg:grid-cols-4 gap-4 mb-5" id="kpis"></div>

    <!-- Charts -->
    <div class="grid grid-cols-1 lg:grid-cols-3 gap-5 mb-5">
      <div class="lg:col-span-2 bg-white rounded-2xl border p-5">
        <div class="flex items-center justify-between mb-4">
          <h3 class="font-bold">รายได้รายวัน</h3>
          <div class="flex gap-2 text-xs">
            <button onclick="setChartType('bar')" id="btn-bar" class="px-2 py-1 rounded bg-indigo-100 text-indigo-700 font-medium">Bar</button>
            <button onclick="setChartType('line')" id="btn-line" class="px-2 py-1 rounded text-gray-500 hover:bg-gray-100 transition-colors">Line</button>
          </div>
        </div>
        <div id="main-chart" class="h-36 flex items-end gap-1"></div>
      </div>
      <div class="bg-white rounded-2xl border p-5">
        <h3 class="font-bold mb-4">Channels</h3>
        <div id="channel-chart" class="space-y-2"></div>
      </div>
    </div>

    <!-- Table -->
    <div class="bg-white rounded-2xl border p-5">
      <div class="flex items-center justify-between mb-4">
        <h3 class="font-bold">Top Products</h3>
        <input id="table-search" oninput="filterTable(this.value)" type="text" placeholder="ค้นหา..." class="text-xs border rounded-xl px-3 py-1.5 focus:outline-none focus:ring-2 focus:ring-indigo-500 w-40">
      </div>
      <div class="overflow-x-auto">
        <table class="w-full text-sm">
          <thead><tr class="border-b text-xs text-gray-500 uppercase"><th class="pb-2 text-left">ชื่อ</th><th class="pb-2 text-right">ยอดขาย</th><th class="pb-2 text-right">รายได้</th><th class="pb-2 text-right">%ของทั้งหมด</th></tr></thead>
          <tbody id="top-table" class="divide-y"></tbody>
        </table>
      </div>
    </div>
  </div>

  <script>
    const kpiDefs = [
      {label:'ผู้ใช้งาน', icon:'👥', base:48291, fmt:v=>v.toLocaleString(), color:'blue', trend:12.4},
      {label:'รายได้', icon:'💰', base:2410000, fmt:v=>'฿'+(v/1e6).toFixed(2)+'M', color:'green', trend:8.7},
      {label:'Orders', icon:'📦', base:1284, fmt:v=>v.toLocaleString(), color:'purple', trend:5.2},
      {label:'CVR', icon:'📈', base:4.8, fmt:v=>v.toFixed(1)+'%', color:'orange', trend:0.3},
    ];
    const colors = {blue:'bg-blue-100 text-blue-600', green:'bg-green-100 text-green-600', purple:'bg-purple-100 text-purple-600', orange:'bg-orange-100 text-orange-600'};

    function renderKPIs(range) {
      const m = range/30;
      document.getElementById('kpis').innerHTML = kpiDefs.map(k => `
        <div class="bg-white rounded-2xl border p-4">
          <div class="flex justify-between items-center mb-2">
            <div class="w-9 h-9 ${colors[k.color]} rounded-xl flex items-center justify-center text-lg">${k.icon}</div>
            <span class="text-xs font-bold ${k.trend>0?'bg-green-100 text-green-700':'bg-red-100 text-red-700'} px-2 py-0.5 rounded-full">
              ${k.trend>0?'+':''}${(k.trend*m).toFixed(1)}%
            </span>
          </div>
          <p class="text-2xl font-extrabold">${k.fmt(Math.floor(k.base*m))}</p>
          <p class="text-xs text-gray-500 mt-0.5">${k.label}</p>
        </div>
      `).join('');
    }

    let chartType = 'bar';
    function renderChart(range) {
      const chart = document.getElementById('main-chart');
      const n = Math.min(range, 30);
      const data = Array.from({length: n}, () => Math.floor(Math.random() * 80 + 20));
      const max = Math.max(...data);
      if (chartType === 'bar') {
        chart.innerHTML = data.map(v => `<div class="flex-1 bg-indigo-500 hover:bg-indigo-600 rounded-sm transition-colors cursor-pointer" style="height:${v/max*100}%"></div>`).join('');
      } else {
        const pts = data.map((v, i) => `${i*(240/n)},${100 - v/max*100}`).join(' ');
        chart.innerHTML = `<svg width="100%" height="100%" viewBox="0 0 240 100" preserveAspectRatio="none"><polyline fill="none" stroke="#6366f1" stroke-width="2" points="${pts}"/></svg>`;
      }
    }

    function setChartType(t) {
      chartType = t;
      document.getElementById('btn-bar').className = `px-2 py-1 rounded text-xs ${t==='bar'?'bg-indigo-100 text-indigo-700 font-medium':'text-gray-500 hover:bg-gray-100'} transition-colors`;
      document.getElementById('btn-line').className = `px-2 py-1 rounded text-xs ${t==='line'?'bg-indigo-100 text-indigo-700 font-medium':'text-gray-500 hover:bg-gray-100'} transition-colors`;
      renderChart(parseInt(document.getElementById('range-select').value));
    }

    const channels = [{label:'Organic',pct:42,color:'bg-indigo-500'},{label:'Direct',pct:28,color:'bg-emerald-500'},{label:'Social',pct:18,color:'bg-amber-500'},{label:'Referral',pct:12,color:'bg-red-400'}];
    function renderChannels() {
      document.getElementById('channel-chart').innerHTML = channels.map(c => `
        <div class="space-y-0.5">
          <div class="flex justify-between text-xs"><span>${c.label}</span><span class="font-bold">${c.pct}%</span></div>
          <div class="h-2 bg-gray-100 rounded-full"><div class="${c.color} h-2 rounded-full" style="width:${c.pct}%"></div></div>
        </div>
      `).join('');
    }

    const allProducts = [
      {name:'iPhone 15 Pro Max', sold:284, rev:13018800, pct:42},
      {name:'MacBook Pro M3', sold:142, rev:11349800, pct:37},
      {name:'AirPods Pro', sold:891, rev:7929900, pct:26},
      {name:'Apple Watch S9', sold:412, rev:6138800, pct:20},
      {name:'iPad Pro 11"', sold:198, rev:5920200, pct:19},
    ];

    function renderTable(data) {
      document.getElementById('top-table').innerHTML = data.map(p => `
        <tr class="hover:bg-gray-50 transition-colors">
          <td class="py-2.5 font-medium">${p.name}</td>
          <td class="py-2.5 text-right tabular-nums">${p.sold.toLocaleString()}</td>
          <td class="py-2.5 text-right tabular-nums font-semibold">฿${(p.rev/1e6).toFixed(2)}M</td>
          <td class="py-2.5 text-right">
            <div class="flex items-center justify-end gap-2">
              <div class="w-16 h-1.5 bg-gray-100 rounded-full"><div class="h-1.5 bg-indigo-500 rounded-full" style="width:${p.pct}%"></div></div>
              <span class="text-xs w-8">${p.pct}%</span>
            </div>
          </td>
        </tr>
      `).join('');
    }

    function filterTable(q) {
      renderTable(allProducts.filter(p => p.name.toLowerCase().includes(q.toLowerCase())));
    }

    function updateAll() {
      const range = parseInt(document.getElementById('range-select').value);
      renderKPIs(range);
      renderChart(range);
    }

    function exportData() {
      const csv = 'Product,Sold,Revenue\n' + allProducts.map(p => `${p.name},${p.sold},${p.rev}`).join('\n');
      const a = document.createElement('a');
      a.href = 'data:text/csv;charset=utf-8,' + encodeURIComponent(csv);
      a.download = 'analytics.csv';
      a.click();
    }

    renderKPIs(30); renderChart(30); renderChannels(); renderTable(allProducts);
  </script>
</body>
</html>
```

---

## สรุป Part 41

| Step | เนื้อหา |
|------|---------|
| 401 | KPI Cards, Bar Chart (CSS), Donut Chart (SVG), Top Pages Table |
| 402 | Conversion Funnel, Cohort Retention Heatmap |
| 403 | Real-time Dashboard: live feed, animated stats, live bar chart |
| 404 | Sparkline Table: SVG sparklines per row |
| 405 | Activity Heatmap: hour × day grid |
| 406 | YoY Comparison Chart |
| 407–409 | Advanced patterns: export CSV, date range picker, filter |
| 410 | Workshop: Full Analytics Dashboard with dynamic range picker |

**Part ถัดไป:** Part 42 — CRM / Kanban Board (Steps 411–420)
