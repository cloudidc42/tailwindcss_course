# Part 62: Multi-Brand Enterprise System

## เป้าหมาย
- สร้างระบบ multi-brand ด้วย CSS custom properties
- Brand switching ผ่าน data attribute
- Enterprise theming patterns
- Steps 611–620

---

## Step 611: Brand Token Architecture

```
แต่ละ brand override CSS variables เดียวกัน:

[data-brand="brand-a"] { --brand-primary: #6366f1; }
[data-brand="brand-b"] { --brand-primary: #0ea5e9; }
[data-brand="brand-c"] { --brand-primary: #10b981; }
```

---

## Step 612: Brand Definitions

```css
/* Base tokens */
:root {
  --brand-primary:    #6366f1;
  --brand-primary-h:  #4f46e5;
  --brand-secondary:  #e0e7ff;
  --brand-accent:     #f59e0b;
  --brand-name:       "Default";
  --brand-radius:     0.75rem;
  --brand-font:       'Inter', sans-serif;
  --surface-bg:       #f9fafb;
  --surface-card:     #ffffff;
  --text-primary:     #111827;
  --text-secondary:   #6b7280;
  --border:           #e5e7eb;
}

/* TechCorp — Indigo/Professional */
[data-brand="techcorp"] {
  --brand-primary:    #6366f1;
  --brand-primary-h:  #4f46e5;
  --brand-secondary:  #e0e7ff;
  --brand-accent:     #f59e0b;
  --brand-radius:     0.75rem;
  --brand-font:       'Inter', sans-serif;
}

/* EcoGreen — Emerald/Sustainable */
[data-brand="ecogreen"] {
  --brand-primary:    #10b981;
  --brand-primary-h:  #059669;
  --brand-secondary:  #d1fae5;
  --brand-accent:     #84cc16;
  --brand-radius:     1.5rem;
  --brand-font:       'Nunito', sans-serif;
}

/* SunsetPay — Orange/Financial */
[data-brand="sunsetpay"] {
  --brand-primary:    #f97316;
  --brand-primary-h:  #ea580c;
  --brand-secondary:  #ffedd5;
  --brand-accent:     #fbbf24;
  --brand-radius:     0.5rem;
  --brand-font:       'Poppins', sans-serif;
}

/* NightOwl — Dark/Gaming */
[data-brand="nightowl"] {
  --brand-primary:    #a855f7;
  --brand-primary-h:  #9333ea;
  --brand-secondary:  #3b0764;
  --brand-accent:     #ec4899;
  --brand-radius:     0.5rem;
  --brand-font:       'Rajdhani', sans-serif;
  --surface-bg:       #0a0a0f;
  --surface-card:     #13131f;
  --text-primary:     #f1f5f9;
  --text-secondary:   #94a3b8;
  --border:           #1e1b4b;
}

/* OceanBlue — Blue/Corporate */
[data-brand="oceanblue"] {
  --brand-primary:    #0ea5e9;
  --brand-primary-h:  #0284c7;
  --brand-secondary:  #e0f2fe;
  --brand-accent:     #06b6d4;
  --brand-radius:     1rem;
  --brand-font:       'DM Sans', sans-serif;
}
```

---

## Steps 613–620: Multi-Brand Workshop

```html
<!DOCTYPE html>
<html lang="th" data-brand="techcorp">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Multi-Brand Enterprise - Steps 613-620</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&family=Nunito:wght@400;600;700;800&family=Poppins:wght@400;500;600;700&family=DM+Sans:wght@400;500;600;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --brand-primary:    #6366f1;
      --brand-primary-h:  #4f46e5;
      --brand-secondary:  #e0e7ff;
      --brand-accent:     #f59e0b;
      --brand-radius:     0.75rem;
      --brand-font:       'Inter', sans-serif;
      --surface-bg:       #f9fafb;
      --surface-card:     #ffffff;
      --text-primary:     #111827;
      --text-secondary:   #6b7280;
      --border:           #e5e7eb;
    }
    [data-brand="techcorp"] {
      --brand-primary:   #6366f1; --brand-primary-h:  #4f46e5;
      --brand-secondary: #e0e7ff; --brand-accent:     #f59e0b;
      --brand-radius:    0.75rem; --brand-font:       'Inter', sans-serif;
      --surface-bg: #f9fafb; --surface-card: #ffffff;
      --text-primary: #111827; --text-secondary: #6b7280; --border: #e5e7eb;
    }
    [data-brand="ecogreen"] {
      --brand-primary:   #10b981; --brand-primary-h:  #059669;
      --brand-secondary: #d1fae5; --brand-accent:     #84cc16;
      --brand-radius:    1.5rem;  --brand-font:       'Nunito', sans-serif;
      --surface-bg: #f0fdf4; --surface-card: #ffffff;
      --text-primary: #064e3b; --text-secondary: #065f46; --border: #d1fae5;
    }
    [data-brand="sunsetpay"] {
      --brand-primary:   #f97316; --brand-primary-h:  #ea580c;
      --brand-secondary: #ffedd5; --brand-accent:     #fbbf24;
      --brand-radius:    0.5rem;  --brand-font:       'Poppins', sans-serif;
      --surface-bg: #fff7ed; --surface-card: #ffffff;
      --text-primary: #431407; --text-secondary: #7c2d12; --border: #fed7aa;
    }
    [data-brand="nightowl"] {
      --brand-primary:   #a855f7; --brand-primary-h:  #9333ea;
      --brand-secondary: #3b0764; --brand-accent:     #ec4899;
      --brand-radius:    0.5rem;  --brand-font:       'Inter', sans-serif;
      --surface-bg: #0a0a0f; --surface-card: #13131f;
      --text-primary: #f1f5f9; --text-secondary: #94a3b8; --border: #1e1b4b;
    }
    [data-brand="oceanblue"] {
      --brand-primary:   #0ea5e9; --brand-primary-h:  #0284c7;
      --brand-secondary: #e0f2fe; --brand-accent:     #06b6d4;
      --brand-radius:    1rem;    --brand-font:       'DM Sans', sans-serif;
      --surface-bg: #f0f9ff; --surface-card: #ffffff;
      --text-primary: #0c4a6e; --text-secondary: #075985; --border: #bae6fd;
    }

    * { font-family: var(--brand-font) !important; }
    body { background: var(--surface-bg); color: var(--text-primary); transition: background 0.3s, color 0.3s; }

    .brand-card  { background: var(--surface-card); border: 1px solid var(--border); border-radius: var(--brand-radius); transition: background 0.3s, border 0.3s; }
    .brand-btn   { background: var(--brand-primary); color: #fff; border-radius: calc(var(--brand-radius) * 0.9); padding: 0.6rem 1.25rem; font-size: 0.875rem; font-weight: 600; cursor: pointer; transition: all 0.2s; border: none; }
    .brand-btn:hover { background: var(--brand-primary-h); transform: translateY(-1px); box-shadow: 0 4px 12px rgba(0,0,0,0.15); }
    .brand-btn-outline { background: transparent; color: var(--brand-primary); border: 2px solid var(--brand-primary); border-radius: calc(var(--brand-radius) * 0.9); padding: 0.55rem 1.2rem; font-size: 0.875rem; font-weight: 600; cursor: pointer; transition: all 0.2s; }
    .brand-btn-outline:hover { background: var(--brand-secondary); }
    .brand-badge { background: var(--brand-secondary); color: var(--brand-primary); border-radius: 9999px; padding: 0.2rem 0.75rem; font-size: 0.75rem; font-weight: 600; }
    .brand-input { background: var(--surface-card); border: 1.5px solid var(--border); border-radius: var(--brand-radius); padding: 0.6rem 1rem; font-size: 0.875rem; color: var(--text-primary); width: 100%; outline: none; transition: border 0.2s; }
    .brand-input:focus { border-color: var(--brand-primary); box-shadow: 0 0 0 3px rgba(var(--brand-primary-rgb, 99, 102, 241), 0.15); }
    .brand-link { color: var(--brand-primary); font-weight: 500; text-decoration: none; }
    .brand-link:hover { text-decoration: underline; }
    .text-primary { color: var(--text-primary) }
    .text-secondary { color: var(--text-secondary) }
    .brand-divider { border-color: var(--border); }

    /* Progress bar */
    .brand-progress-bg { background: var(--brand-secondary); border-radius: 9999px; overflow: hidden; }
    .brand-progress-fill { background: var(--brand-primary); border-radius: 9999px; transition: width 0.5s; }

    /* Nav active */
    .brand-nav-active { background: var(--brand-secondary); color: var(--brand-primary); font-weight: 600; border-radius: var(--brand-radius); }
  </style>
</head>
<body class="min-h-screen">

  <!-- Brand Switcher -->
  <div style="background: var(--brand-primary)" class="px-6 py-3 flex items-center gap-3 flex-wrap">
    <span class="text-white text-xs font-semibold mr-2">Brand:</span>
    <button onclick="setBrand('techcorp','TechCorp','🔷')" class="px-3 py-1 bg-white/20 hover:bg-white/30 text-white text-xs rounded-full transition-colors font-semibold">🔷 TechCorp</button>
    <button onclick="setBrand('ecogreen','EcoGreen','🌿')" class="px-3 py-1 bg-white/20 hover:bg-white/30 text-white text-xs rounded-full transition-colors font-semibold">🌿 EcoGreen</button>
    <button onclick="setBrand('sunsetpay','SunsetPay','🌅')" class="px-3 py-1 bg-white/20 hover:bg-white/30 text-white text-xs rounded-full transition-colors font-semibold">🌅 SunsetPay</button>
    <button onclick="setBrand('nightowl','NightOwl','🦉')" class="px-3 py-1 bg-white/20 hover:bg-white/30 text-white text-xs rounded-full transition-colors font-semibold">🦉 NightOwl</button>
    <button onclick="setBrand('oceanblue','OceanBlue','🌊')" class="px-3 py-1 bg-white/20 hover:bg-white/30 text-white text-xs rounded-full transition-colors font-semibold">🌊 OceanBlue</button>
  </div>

  <div class="flex h-[calc(100vh-44px)] overflow-hidden">
    <!-- Sidebar -->
    <aside class="w-52 flex flex-col flex-shrink-0" style="background: var(--surface-card); border-right: 1px solid var(--border)">
      <div class="px-4 py-5 border-b brand-divider">
        <div id="brandLogo" class="flex items-center gap-2">
          <span class="text-2xl" id="brandEmoji">🔷</span>
          <span class="font-extrabold text-sm" id="brandName" style="color: var(--brand-primary)">TechCorp</span>
        </div>
      </div>
      <nav class="p-2 space-y-0.5 flex-1">
        <button onclick="setPage('dashboard')" id="nav-dashboard" class="w-full flex items-center gap-2.5 px-3 py-2 text-sm text-left brand-nav-active transition-colors">
          🏠 <span>Dashboard</span>
        </button>
        <button onclick="setPage('users')" id="nav-users" class="w-full flex items-center gap-2.5 px-3 py-2 text-sm text-left text-secondary rounded-lg hover:opacity-70 transition-colors">
          👥 <span>Users</span>
        </button>
        <button onclick="setPage('products')" id="nav-products" class="w-full flex items-center gap-2.5 px-3 py-2 text-sm text-left text-secondary rounded-lg hover:opacity-70 transition-colors">
          📦 <span>Products</span>
        </button>
        <button onclick="setPage('settings')" id="nav-settings" class="w-full flex items-center gap-2.5 px-3 py-2 text-sm text-left text-secondary rounded-lg hover:opacity-70 transition-colors">
          ⚙️ <span>Settings</span>
        </button>
      </nav>
    </aside>

    <!-- Main -->
    <div class="flex-1 flex flex-col overflow-hidden">
      <!-- Topbar -->
      <header class="h-14 px-6 flex items-center justify-between flex-shrink-0" style="background: var(--surface-card); border-bottom: 1px solid var(--border)">
        <p id="pageTitle" class="font-bold text-sm text-primary">Dashboard</p>
        <div class="flex items-center gap-3">
          <button class="brand-btn" style="padding: 0.4rem 0.9rem; font-size: 0.75rem">+ New</button>
          <div class="w-8 h-8 flex items-center justify-center rounded-lg text-sm font-bold text-white" style="background: var(--brand-primary)">SK</div>
        </div>
      </header>

      <!-- Content -->
      <main class="flex-1 overflow-y-auto p-6">

        <!-- Dashboard page -->
        <div id="page-dashboard" class="space-y-6">
          <div class="grid grid-cols-2 xl:grid-cols-4 gap-4">
            <div class="brand-card p-4">
              <p class="text-xs text-secondary font-semibold mb-2">Revenue</p>
              <p class="text-2xl font-extrabold text-primary">฿284K</p>
              <p class="text-xs mt-1" style="color: var(--brand-primary)">▲ +12%</p>
            </div>
            <div class="brand-card p-4">
              <p class="text-xs text-secondary font-semibold mb-2">Users</p>
              <p class="text-2xl font-extrabold text-primary">1,284</p>
              <p class="text-xs mt-1" style="color: var(--brand-primary)">▲ +8%</p>
            </div>
            <div class="brand-card p-4">
              <p class="text-xs text-secondary font-semibold mb-2">Orders</p>
              <p class="text-2xl font-extrabold text-primary">391</p>
              <p class="text-xs mt-1 text-red-500">▼ -3%</p>
            </div>
            <div class="brand-card p-4">
              <p class="text-xs text-secondary font-semibold mb-2">NPS Score</p>
              <p class="text-2xl font-extrabold text-primary">72</p>
              <p class="text-xs mt-1" style="color: var(--brand-primary)">▲ +4pts</p>
            </div>
          </div>

          <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
            <div class="lg:col-span-2 brand-card p-5">
              <p class="font-bold text-sm text-primary mb-4">Sales Performance</p>
              <div class="flex items-end gap-1.5 h-28">
                <div v-for="v in [60,75,55,88,70,95,80,92,72,100,68,90]">
                </div>
                <script>
                  // render bars inline
                  const barData = [60,75,55,88,70,95,80,92,72,100,68,90];
                  document.currentScript.insertAdjacentHTML('beforebegin',
                    barData.map((v,i) =>
                      `<div style="height:${v}%; flex:1; background:${i===11?'var(--brand-primary)':'var(--brand-secondary)'}; border-radius:4px 4px 0 0; transition: background 0.3s"></div>`
                    ).join('')
                  );
                </script>
              </div>
            </div>
            <div class="brand-card p-5">
              <p class="font-bold text-sm text-primary mb-4">Top Products</p>
              <div class="space-y-3">
                <div>
                  <div class="flex justify-between text-xs mb-1"><span class="text-primary font-medium">Pro Plan</span><span class="text-secondary">84%</span></div>
                  <div class="brand-progress-bg h-1.5"><div class="brand-progress-fill h-full" style="width:84%"></div></div>
                </div>
                <div>
                  <div class="flex justify-between text-xs mb-1"><span class="text-primary font-medium">Enterprise</span><span class="text-secondary">62%</span></div>
                  <div class="brand-progress-bg h-1.5"><div class="brand-progress-fill h-full" style="width:62%"></div></div>
                </div>
                <div>
                  <div class="flex justify-between text-xs mb-1"><span class="text-primary font-medium">Starter</span><span class="text-secondary">38%</span></div>
                  <div class="brand-progress-bg h-1.5"><div class="brand-progress-fill h-full" style="width:38%"></div></div>
                </div>
              </div>
            </div>
          </div>

          <!-- Buttons & Components preview -->
          <div class="brand-card p-5">
            <p class="font-bold text-sm text-primary mb-4">Brand Components</p>
            <div class="flex flex-wrap gap-3">
              <button class="brand-btn">Primary Button</button>
              <button class="brand-btn-outline">Outline Button</button>
              <span class="brand-badge">Active</span>
              <span class="brand-badge" style="background: #dcfce7; color: #16a34a">Success</span>
              <span class="brand-badge" style="background: #fee2e2; color: #dc2626">Error</span>
            </div>
            <div class="mt-4 max-w-sm">
              <input class="brand-input" placeholder="Brand-styled input...">
            </div>
          </div>
        </div>

        <!-- Users page -->
        <div id="page-users" class="hidden space-y-4">
          <div class="brand-card overflow-hidden" style="padding: 0">
            <div class="px-5 py-4 border-b brand-divider flex justify-between items-center">
              <p class="font-bold text-sm text-primary">Users</p>
              <button class="brand-btn" style="padding: 0.35rem 0.9rem; font-size: 0.75rem">+ Add</button>
            </div>
            <table class="w-full text-sm">
              <thead>
                <tr style="background: var(--surface-bg)">
                  <th class="px-5 py-3 text-left text-xs font-semibold text-secondary">Name</th>
                  <th class="px-5 py-3 text-left text-xs font-semibold text-secondary">Role</th>
                  <th class="px-5 py-3 text-left text-xs font-semibold text-secondary">Status</th>
                </tr>
              </thead>
              <tbody>
                <tr class="border-t brand-divider">
                  <td class="px-5 py-3 text-primary font-medium">สมชาย เทคโน</td>
                  <td class="px-5 py-3"><span class="brand-badge">Admin</span></td>
                  <td class="px-5 py-3"><span class="brand-badge" style="background: #dcfce7; color: #16a34a">Active</span></td>
                </tr>
                <tr class="border-t brand-divider">
                  <td class="px-5 py-3 text-primary font-medium">วิชัย ดีงาม</td>
                  <td class="px-5 py-3"><span class="brand-badge">Editor</span></td>
                  <td class="px-5 py-3"><span class="brand-badge" style="background: #dcfce7; color: #16a34a">Active</span></td>
                </tr>
                <tr class="border-t brand-divider">
                  <td class="px-5 py-3 text-primary font-medium">นิดา สวยงาม</td>
                  <td class="px-5 py-3"><span class="brand-badge">User</span></td>
                  <td class="px-5 py-3"><span class="brand-badge" style="background: var(--surface-bg); color: var(--text-secondary)">Inactive</span></td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>

        <!-- Products page -->
        <div id="page-products" class="hidden">
          <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-5">
            <div v-for="p in products"></div>
            <script>
              const products = [
                { name: 'Pro Plan', price: '฿299/mo', features: ['Unlimited projects','100GB storage','Priority support'] },
                { name: 'Enterprise', price: 'Custom', features: ['Everything in Pro','SLA guarantee','Dedicated manager'] },
                { name: 'Starter', price: '฿99/mo', features: ['5 projects','10GB storage','Email support'] },
              ];
              document.currentScript.insertAdjacentHTML('beforebegin', products.map(p => `
                <div class="brand-card p-5">
                  <p class="font-extrabold text-lg text-primary mb-1">${p.name}</p>
                  <p style="color: var(--brand-primary)" class="font-bold text-xl mb-3">${p.price}</p>
                  <ul class="space-y-1.5">
                    ${p.features.map(f => `<li class="text-sm text-secondary flex items-center gap-2">✓ ${f}</li>`).join('')}
                  </ul>
                  <button class="brand-btn w-full mt-4" style="justify-content:center; display:flex">เลือกแผนนี้</button>
                </div>
              `).join(''));
            </script>
          </div>
        </div>

        <!-- Settings page -->
        <div id="page-settings" class="hidden">
          <div class="brand-card p-5 max-w-md space-y-4">
            <p class="font-bold text-sm text-primary">การตั้งค่า</p>
            <div class="space-y-3">
              <div>
                <label class="text-xs font-semibold text-secondary block mb-1">ชื่อบริษัท</label>
                <input class="brand-input" value="TechCorp Ltd.">
              </div>
              <div>
                <label class="text-xs font-semibold text-secondary block mb-1">อีเมลหลัก</label>
                <input type="email" class="brand-input" value="admin@techcorp.co">
              </div>
            </div>
            <div class="flex gap-3 pt-2">
              <button class="brand-btn-outline flex-1">ยกเลิก</button>
              <button class="brand-btn flex-1">บันทึก</button>
            </div>
          </div>
        </div>

      </main>
    </div>
  </div>

  <script>
    function setBrand(key, name, emoji) {
      document.documentElement.setAttribute('data-brand', key);
      document.getElementById('brandName').textContent = name;
      document.getElementById('brandEmoji').textContent = emoji;
    }

    function setPage(id) {
      const pages = ['dashboard','users','products','settings'];
      const titles = { dashboard:'Dashboard', users:'Users', products:'Products', settings:'Settings' };
      pages.forEach(p => {
        document.getElementById('page-' + p).classList.toggle('hidden', p !== id);
        const nav = document.getElementById('nav-' + p);
        if (p === id) {
          nav.classList.add('brand-nav-active');
          nav.classList.remove('text-secondary');
        } else {
          nav.classList.remove('brand-nav-active');
          nav.classList.add('text-secondary');
        }
      });
      document.getElementById('pageTitle').textContent = titles[id];
    }
  </script>
</body>
</html>
```

---

## สรุป Part 62

| Step | เนื้อหา |
|------|---------|
| 611 | Brand token architecture concept |
| 612 | 5 brand definitions: TechCorp/EcoGreen/SunsetPay/NightOwl/OceanBlue |
| 613 | Brand switcher toolbar |
| 614 | Brand-aware sidebar navigation |
| 615 | Dashboard page with brand tokens |
| 616 | Bar chart + progress bars ด้วย brand colors |
| 617 | Button/Badge/Input brand components |
| 618 | Users table with brand styling |
| 619 | Products/Settings pages |
| 620 | Workshop: Complete 5-brand enterprise dashboard |

**Part ถัดไป:** Part 63 — Testing Tailwind Components (Steps 621–630)
