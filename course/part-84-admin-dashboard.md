# Part 84: Admin Dashboard

## เป้าหมาย
- Full admin dashboard layout
- Sidebar nav, header, KPI cards, charts, data table
- Notifications, user profile, breadcrumbs
- Steps 831–840

---

## Step 831: Dashboard Shell

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Admin Dashboard</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 h-screen flex overflow-hidden">

  <!-- Sidebar -->
  <aside id="sidebar" class="w-60 flex-shrink-0 bg-gray-900 flex flex-col h-full">
    <!-- Logo -->
    <div class="h-14 flex items-center px-5 border-b border-gray-800">
      <span class="font-extrabold text-white text-lg">AdminPro</span>
    </div>
    <!-- Nav -->
    <nav class="flex-1 p-3 space-y-0.5 overflow-y-auto">
      <a href="#" class="flex items-center gap-2.5 px-3 py-2 rounded-xl bg-indigo-600 text-white text-sm font-medium">
        <span>🏠</span> Dashboard
      </a>
      <a href="#" class="flex items-center gap-2.5 px-3 py-2 rounded-xl text-gray-400 hover:bg-gray-800 hover:text-white text-sm transition-colors">
        <span>👥</span> Users
      </a>
      <a href="#" class="flex items-center gap-2.5 px-3 py-2 rounded-xl text-gray-400 hover:bg-gray-800 hover:text-white text-sm transition-colors">
        <span>📦</span> Products
      </a>
      <a href="#" class="flex items-center gap-2.5 px-3 py-2 rounded-xl text-gray-400 hover:bg-gray-800 hover:text-white text-sm transition-colors">
        <span>🛒</span> Orders
        <span class="ml-auto bg-red-500 text-white text-xs px-1.5 py-0.5 rounded-full">8</span>
      </a>
      <a href="#" class="flex items-center gap-2.5 px-3 py-2 rounded-xl text-gray-400 hover:bg-gray-800 hover:text-white text-sm transition-colors">
        <span>📊</span> Analytics
      </a>
      <a href="#" class="flex items-center gap-2.5 px-3 py-2 rounded-xl text-gray-400 hover:bg-gray-800 hover:text-white text-sm transition-colors">
        <span>📢</span> Marketing
      </a>
      <div class="pt-3 border-t border-gray-800 mt-3">
        <a href="#" class="flex items-center gap-2.5 px-3 py-2 rounded-xl text-gray-400 hover:bg-gray-800 hover:text-white text-sm transition-colors">
          <span>⚙️</span> Settings
        </a>
        <a href="#" class="flex items-center gap-2.5 px-3 py-2 rounded-xl text-gray-400 hover:bg-gray-800 hover:text-white text-sm transition-colors">
          <span>❓</span> Help
        </a>
      </div>
    </nav>
    <!-- User -->
    <div class="p-3 border-t border-gray-800">
      <div class="flex items-center gap-2.5 px-3 py-2 rounded-xl hover:bg-gray-800 cursor-pointer transition-colors">
        <div class="w-7 h-7 rounded-full bg-indigo-500 flex items-center justify-center text-white text-xs font-bold flex-shrink-0">JD</div>
        <div class="min-w-0">
          <p class="text-white text-xs font-semibold truncate">John Doe</p>
          <p class="text-gray-500 text-xs truncate">Admin</p>
        </div>
      </div>
    </div>
  </aside>

  <!-- Main -->
  <div class="flex-1 flex flex-col min-w-0 overflow-hidden">
    <!-- Header -->
    <header class="h-14 bg-white border-b flex items-center justify-between px-6 flex-shrink-0">
      <div>
        <nav class="flex items-center gap-1.5 text-sm text-gray-400">
          <span class="hover:text-gray-700 cursor-pointer">Home</span>
          <span>/</span>
          <span class="text-gray-900 font-medium">Dashboard</span>
        </nav>
      </div>
      <div class="flex items-center gap-3">
        <div class="relative">
          <input type="text" placeholder="Search..." class="bg-gray-100 border-0 rounded-xl px-4 py-1.5 text-sm text-gray-700 placeholder-gray-400 focus:outline-none focus:ring-2 focus:ring-indigo-500 w-48">
        </div>
        <button class="relative p-2 rounded-xl hover:bg-gray-100 transition-colors">
          🔔
          <span class="absolute top-1 right-1 w-2 h-2 bg-red-500 rounded-full"></span>
        </button>
        <div class="w-8 h-8 rounded-full bg-indigo-600 flex items-center justify-center text-white text-xs font-bold cursor-pointer">JD</div>
      </div>
    </header>

    <!-- Page content -->
    <main class="flex-1 overflow-y-auto p-6">
      <!-- KPI cards -->
      <div class="grid grid-cols-2 xl:grid-cols-4 gap-4 mb-6">
        <div class="bg-white rounded-2xl border p-5">
          <p class="text-xs font-semibold text-gray-400 uppercase tracking-wide">Revenue</p>
          <p class="text-2xl font-extrabold text-gray-900 mt-1">$84,293</p>
          <p class="text-xs text-emerald-600 font-medium mt-1">↑ 12.5% vs last month</p>
        </div>
        <div class="bg-white rounded-2xl border p-5">
          <p class="text-xs font-semibold text-gray-400 uppercase tracking-wide">Orders</p>
          <p class="text-2xl font-extrabold text-gray-900 mt-1">2,841</p>
          <p class="text-xs text-emerald-600 font-medium mt-1">↑ 8.2% vs last month</p>
        </div>
        <div class="bg-white rounded-2xl border p-5">
          <p class="text-xs font-semibold text-gray-400 uppercase tracking-wide">Customers</p>
          <p class="text-2xl font-extrabold text-gray-900 mt-1">18,492</p>
          <p class="text-xs text-red-500 font-medium mt-1">↓ 2.1% vs last month</p>
        </div>
        <div class="bg-white rounded-2xl border p-5">
          <p class="text-xs font-semibold text-gray-400 uppercase tracking-wide">Conversion</p>
          <p class="text-2xl font-extrabold text-gray-900 mt-1">4.28%</p>
          <p class="text-xs text-emerald-600 font-medium mt-1">↑ 0.4pp vs last month</p>
        </div>
      </div>

      <!-- Charts row -->
      <div class="grid grid-cols-1 lg:grid-cols-3 gap-4 mb-6">
        <div class="lg:col-span-2 bg-white rounded-2xl border p-5">
          <div class="flex items-center justify-between mb-4">
            <h3 class="font-bold text-gray-900">Revenue Over Time</h3>
            <select class="text-xs border rounded-lg px-2 py-1 text-gray-600 focus:outline-none">
              <option>Last 7 days</option><option>Last 30 days</option><option>Last 90 days</option>
            </select>
          </div>
          <div id="chart" class="h-40"></div>
        </div>
        <div class="bg-white rounded-2xl border p-5">
          <h3 class="font-bold text-gray-900 mb-4">Top Categories</h3>
          <div class="space-y-3" id="categories"></div>
        </div>
      </div>

      <!-- Recent orders table -->
      <div class="bg-white rounded-2xl border">
        <div class="flex items-center justify-between p-5 border-b">
          <h3 class="font-bold text-gray-900">Recent Orders</h3>
          <button class="text-sm text-indigo-600 font-medium hover:text-indigo-700">View all →</button>
        </div>
        <div class="overflow-x-auto">
          <table class="w-full text-sm">
            <thead class="bg-gray-50 border-b">
              <tr>
                <th class="text-left px-5 py-3 font-semibold text-gray-500">Order</th>
                <th class="text-left px-5 py-3 font-semibold text-gray-500">Customer</th>
                <th class="text-left px-5 py-3 font-semibold text-gray-500">Status</th>
                <th class="text-left px-5 py-3 font-semibold text-gray-500">Date</th>
                <th class="text-right px-5 py-3 font-semibold text-gray-500">Amount</th>
              </tr>
            </thead>
            <tbody class="divide-y divide-gray-100" id="order-table"></tbody>
          </table>
        </div>
      </div>
    </main>
  </div>

  <script>
    // SVG chart
    const data = [40,65,45,80,70,90,85];
    const w=400, h=120, pad=10;
    const maxV=Math.max(...data);
    const pts = data.map((v,i) => `${pad+i*(w-pad*2)/6},${h-pad-(v/maxV)*(h-pad*2)}`).join(' ');
    const fillPts = `${pad},${h-pad} ` + pts + ` ${w-pad},${h-pad}`;
    document.getElementById('chart').innerHTML = `
      <svg viewBox="0 0 ${w} ${h}" class="w-full h-full">
        <defs><linearGradient id="g" x1="0" y1="0" x2="0" y2="1"><stop offset="0%" stop-color="#6366f1" stop-opacity=".2"/><stop offset="100%" stop-color="#6366f1" stop-opacity="0"/></linearGradient></defs>
        <polygon points="${fillPts}" fill="url(#g)"/>
        <polyline points="${pts}" fill="none" stroke="#6366f1" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
        ${data.map((v,i) => `<circle cx="${pad+i*(w-pad*2)/6}" cy="${h-pad-(v/maxV)*(h-pad*2)}" r="3" fill="#6366f1"/>`).join('')}
      </svg>`;

    // Categories
    const cats = [{name:'Electronics',pct:42,color:'bg-indigo-500'},{name:'Clothing',pct:28,color:'bg-purple-500'},{name:'Home',pct:18,color:'bg-amber-500'},{name:'Other',pct:12,color:'bg-gray-300'}];
    document.getElementById('categories').innerHTML = cats.map(c => `
      <div>
        <div class="flex justify-between text-xs mb-1"><span class="font-medium text-gray-700">${c.name}</span><span class="text-gray-500">${c.pct}%</span></div>
        <div class="h-1.5 bg-gray-100 rounded-full"><div class="${c.color} h-full rounded-full" style="width:${c.pct}%"></div></div>
      </div>`).join('');

    // Orders
    const statColors = {delivered:'bg-emerald-100 text-emerald-700',pending:'bg-yellow-100 text-yellow-700',processing:'bg-blue-100 text-blue-700',cancelled:'bg-red-100 text-red-700'};
    const orders = [{id:'#4821',cust:'John Doe',status:'delivered',date:'Sep 28',amt:'$189.00'},{id:'#4820',cust:'Sarah A.',status:'pending',date:'Sep 27',amt:'$94.50'},{id:'#4819',cust:'Mike K.',status:'processing',date:'Sep 26',amt:'$342.00'},{id:'#4818',cust:'Anna L.',status:'cancelled',date:'Sep 25',amt:'$67.00'},{id:'#4817',cust:'Tom W.',status:'delivered',date:'Sep 24',amt:'$215.00'}];
    document.getElementById('order-table').innerHTML = orders.map(o => `
      <tr class="hover:bg-gray-50 transition-colors">
        <td class="px-5 py-3 font-semibold text-gray-900">${o.id}</td>
        <td class="px-5 py-3 text-gray-600">${o.cust}</td>
        <td class="px-5 py-3"><span class="px-2.5 py-0.5 rounded-full text-xs font-semibold ${statColors[o.status]}">${o.status}</span></td>
        <td class="px-5 py-3 text-gray-500">${o.date}</td>
        <td class="px-5 py-3 text-right font-semibold text-gray-900">${o.amt}</td>
      </tr>`).join('');
  </script>
</body>
</html>
```

---

## Step 832: Activity Feed

```html
<!-- Activity feed widget -->
<div class="bg-white rounded-2xl border p-5">
  <h3 class="font-bold text-gray-900 mb-4">Recent Activity</h3>
  <div class="space-y-4" id="feed"></div>
</div>
<script>
  const activities = [
    { icon:'🛒', text:'New order #4822 from <strong>Alice Chen</strong>', time:'2m ago',   color:'bg-indigo-100' },
    { icon:'👤', text:'New user <strong>Bob Smith</strong> registered',    time:'15m ago',  color:'bg-emerald-100' },
    { icon:'⚠️', text:'Low stock alert: <strong>Product XL-200</strong>',  time:'1h ago',   color:'bg-amber-100' },
    { icon:'💳', text:'Payment of <strong>$1,240</strong> received',       time:'2h ago',   color:'bg-blue-100' },
    { icon:'🔄', text:'Order #4816 status updated to <strong>Shipped</strong>', time:'3h ago', color:'bg-purple-100' },
  ];
  document.getElementById('feed').innerHTML = activities.map(a => `
    <div class="flex items-start gap-3">
      <div class="w-8 h-8 rounded-xl ${a.color} flex items-center justify-center text-sm flex-shrink-0">${a.icon}</div>
      <div class="flex-1 min-w-0">
        <p class="text-sm text-gray-700">${a.text}</p>
        <p class="text-xs text-gray-400 mt-0.5">${a.time}</p>
      </div>
    </div>`).join('');
</script>
```

---

## Step 833: Settings Page

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Settings</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 min-h-screen p-6">
<div class="max-w-3xl mx-auto space-y-4">
  <h1 class="text-xl font-extrabold text-gray-900">Settings</h1>

  <!-- Profile section -->
  <div class="bg-white rounded-2xl border divide-y">
    <div class="p-5">
      <h2 class="font-bold text-gray-900 mb-4">Profile</h2>
      <div class="flex items-center gap-4 mb-4">
        <div class="w-16 h-16 rounded-2xl bg-indigo-600 flex items-center justify-center text-white text-2xl font-extrabold">JD</div>
        <button class="px-4 py-2 border text-sm font-medium rounded-xl hover:bg-gray-50 transition-colors">Change photo</button>
      </div>
      <div class="grid grid-cols-2 gap-3">
        <div><label class="block text-xs font-semibold text-gray-500 mb-1">First Name</label><input value="John" class="w-full px-3 py-2 border rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500"></div>
        <div><label class="block text-xs font-semibold text-gray-500 mb-1">Last Name</label><input value="Doe" class="w-full px-3 py-2 border rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500"></div>
        <div class="col-span-2"><label class="block text-xs font-semibold text-gray-500 mb-1">Email</label><input type="email" value="john@example.com" class="w-full px-3 py-2 border rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500"></div>
      </div>
    </div>
    <div class="p-5 flex justify-end">
      <button class="px-5 py-2 bg-indigo-600 text-white text-sm font-semibold rounded-xl hover:bg-indigo-700 transition-colors">Save changes</button>
    </div>
  </div>

  <!-- Notifications section -->
  <div class="bg-white rounded-2xl border p-5">
    <h2 class="font-bold text-gray-900 mb-4">Notifications</h2>
    <div class="space-y-3">
      <div class="flex items-center justify-between py-2">
        <div><p class="text-sm font-medium text-gray-900">Email notifications</p><p class="text-xs text-gray-500">Receive updates via email</p></div>
        <button onclick="this.dataset.on=this.dataset.on==='1'?'0':'1'" data-on="1" class="relative w-11 h-6 rounded-full transition-colors bg-indigo-600 focus:outline-none focus-visible:ring-2 focus-visible:ring-indigo-500" id="t1" role="switch" aria-checked="true">
          <span class="absolute left-1 top-1 w-4 h-4 bg-white rounded-full shadow transition-transform translate-x-5"></span>
        </button>
      </div>
      <div class="flex items-center justify-between py-2">
        <div><p class="text-sm font-medium text-gray-900">Push notifications</p><p class="text-xs text-gray-500">Browser push notifications</p></div>
        <button class="relative w-11 h-6 rounded-full transition-colors bg-gray-200 focus:outline-none" role="switch" aria-checked="false">
          <span class="absolute left-1 top-1 w-4 h-4 bg-white rounded-full shadow transition-transform"></span>
        </button>
      </div>
    </div>
  </div>

  <!-- Danger zone -->
  <div class="bg-white rounded-2xl border border-red-200 p-5">
    <h2 class="font-bold text-red-600 mb-1">Danger Zone</h2>
    <p class="text-gray-500 text-sm mb-4">การดำเนินการเหล่านี้ไม่สามารถย้อนกลับได้</p>
    <button class="px-4 py-2 bg-red-50 border border-red-300 text-red-700 text-sm font-semibold rounded-xl hover:bg-red-100 transition-colors">Delete Account</button>
  </div>
</div>
</body>
</html>
```

---

## Step 834: User Management Page

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Users</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 min-h-screen p-6">
<div class="max-w-5xl mx-auto">
  <div class="flex items-center justify-between mb-5">
    <h1 class="text-xl font-extrabold text-gray-900">Users</h1>
    <button class="px-4 py-2 bg-indigo-600 text-white text-sm font-semibold rounded-xl hover:bg-indigo-700 transition-colors">+ Invite User</button>
  </div>
  <!-- Filters -->
  <div class="bg-white rounded-2xl border p-4 mb-4 flex flex-wrap gap-3 items-center">
    <input type="text" placeholder="Search users..." class="flex-1 min-w-[200px] px-3 py-2 border rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
    <select class="px-3 py-2 border rounded-xl text-sm text-gray-600 focus:outline-none bg-white">
      <option>All roles</option><option>Admin</option><option>Member</option><option>Viewer</option>
    </select>
    <select class="px-3 py-2 border rounded-xl text-sm text-gray-600 focus:outline-none bg-white">
      <option>All status</option><option>Active</option><option>Inactive</option>
    </select>
  </div>
  <!-- Table -->
  <div class="bg-white rounded-2xl border overflow-hidden">
    <table class="w-full text-sm">
      <thead class="bg-gray-50 border-b">
        <tr>
          <th class="text-left px-5 py-3 font-semibold text-gray-500">User</th>
          <th class="text-left px-5 py-3 font-semibold text-gray-500">Role</th>
          <th class="text-left px-5 py-3 font-semibold text-gray-500">Status</th>
          <th class="text-left px-5 py-3 font-semibold text-gray-500">Joined</th>
          <th class="px-5 py-3"></th>
        </tr>
      </thead>
      <tbody class="divide-y divide-gray-100">
        <tr class="hover:bg-gray-50">
          <td class="px-5 py-3.5"><div class="flex items-center gap-3"><div class="w-8 h-8 rounded-full bg-indigo-600 flex items-center justify-center text-white text-xs font-bold">JD</div><div><p class="font-semibold text-gray-900">John Doe</p><p class="text-xs text-gray-400">john@example.com</p></div></div></td>
          <td class="px-5 py-3.5"><span class="px-2.5 py-0.5 rounded-full text-xs font-semibold bg-indigo-100 text-indigo-700">Admin</span></td>
          <td class="px-5 py-3.5"><span class="flex items-center gap-1.5 text-xs font-semibold text-emerald-700"><span class="w-1.5 h-1.5 rounded-full bg-emerald-500"></span>Active</span></td>
          <td class="px-5 py-3.5 text-gray-500">Jan 2024</td>
          <td class="px-5 py-3.5 text-right"><button class="text-gray-400 hover:text-gray-600">•••</button></td>
        </tr>
        <tr class="hover:bg-gray-50">
          <td class="px-5 py-3.5"><div class="flex items-center gap-3"><div class="w-8 h-8 rounded-full bg-rose-500 flex items-center justify-center text-white text-xs font-bold">SA</div><div><p class="font-semibold text-gray-900">Sarah Ann</p><p class="text-xs text-gray-400">sarah@example.com</p></div></div></td>
          <td class="px-5 py-3.5"><span class="px-2.5 py-0.5 rounded-full text-xs font-semibold bg-gray-100 text-gray-600">Member</span></td>
          <td class="px-5 py-3.5"><span class="flex items-center gap-1.5 text-xs font-semibold text-emerald-700"><span class="w-1.5 h-1.5 rounded-full bg-emerald-500"></span>Active</span></td>
          <td class="px-5 py-3.5 text-gray-500">Mar 2024</td>
          <td class="px-5 py-3.5 text-right"><button class="text-gray-400 hover:text-gray-600">•••</button></td>
        </tr>
        <tr class="hover:bg-gray-50">
          <td class="px-5 py-3.5"><div class="flex items-center gap-3"><div class="w-8 h-8 rounded-full bg-gray-400 flex items-center justify-center text-white text-xs font-bold">MK</div><div><p class="font-semibold text-gray-900">Mike Kim</p><p class="text-xs text-gray-400">mike@example.com</p></div></div></td>
          <td class="px-5 py-3.5"><span class="px-2.5 py-0.5 rounded-full text-xs font-semibold bg-gray-100 text-gray-600">Viewer</span></td>
          <td class="px-5 py-3.5"><span class="flex items-center gap-1.5 text-xs font-semibold text-gray-500"><span class="w-1.5 h-1.5 rounded-full bg-gray-300"></span>Inactive</span></td>
          <td class="px-5 py-3.5 text-gray-500">Jun 2024</td>
          <td class="px-5 py-3.5 text-right"><button class="text-gray-400 hover:text-gray-600">•••</button></td>
        </tr>
      </tbody>
    </table>
  </div>
</div>
</body>
</html>
```

---

## Steps 835–840: Dashboard Patterns Summary

```
835 — Sidebar collapse (icon-only mode)
836 — Notification dropdown with mark-all-read
837 — Breadcrumb navigation
838 — Empty dashboard state (no data)
839 — Mobile dashboard (bottom tab nav)
840 — Workshop: Complete admin dashboard
```

```html
<!-- Step 835: Sidebar collapse -->
<script>
function toggleSidebar() {
  const sidebar = document.getElementById('sidebar');
  const labels  = sidebar.querySelectorAll('.nav-label');
  const collapsed = sidebar.classList.toggle('w-14');
  sidebar.classList.toggle('w-60');
  labels.forEach(el => el.classList.toggle('hidden', collapsed));
}
</script>

<!-- Step 839: Mobile bottom nav -->
<nav class="fixed bottom-0 left-0 right-0 bg-white border-t flex md:hidden z-40">
  <a href="#" class="flex-1 flex flex-col items-center py-2.5 text-indigo-600">
    <span class="text-xl">🏠</span><span class="text-xs font-medium mt-0.5">Home</span>
  </a>
  <a href="#" class="flex-1 flex flex-col items-center py-2.5 text-gray-400">
    <span class="text-xl">📦</span><span class="text-xs font-medium mt-0.5">Orders</span>
  </a>
  <a href="#" class="flex-1 flex flex-col items-center py-2.5 text-gray-400">
    <span class="text-xl">📊</span><span class="text-xs font-medium mt-0.5">Stats</span>
  </a>
  <a href="#" class="flex-1 flex flex-col items-center py-2.5 text-gray-400">
    <span class="text-xl">⚙️</span><span class="text-xs font-medium mt-0.5">Settings</span>
  </a>
</nav>
```

---

## สรุป Part 84

| Step | เนื้อหา |
|------|---------|
| 831 | Full admin dashboard shell — sidebar, header, KPIs, chart, table |
| 832 | Activity feed widget |
| 833 | Settings page — profile, notifications toggle, danger zone |
| 834 | User management page — filters, table, role badges |
| 835 | Sidebar collapse (icon-only mode) |
| 836 | Notification dropdown |
| 837 | Breadcrumb navigation |
| 838 | Empty state for dashboard |
| 839 | Mobile bottom tab navigation |
| 840 | Workshop: Full admin dashboard |

**Part ถัดไป:** Part 85 — Testing & Quality Assurance (Steps 841–850)
