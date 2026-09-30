# Part 49: Custom Tailwind Config & Theming

## เป้าหมาย
- ขยาย Tailwind ด้วย `tailwind.config` ผ่าน CDN script tag
- Custom colors, fonts, spacing, animations
- Multi-theme support ด้วย CSS variables
- Steps 481–490

---

## Step 481: Custom Config via CDN

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Custom Config - Step 481</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    // Custom config ผ่าน tailwind.config global
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            brand: {
              50:  '#eff6ff',
              100: '#dbeafe',
              200: '#bfdbfe',
              300: '#93c5fd',
              400: '#60a5fa',
              500: '#3b82f6',
              600: '#2563eb',  // primary
              700: '#1d4ed8',
              800: '#1e40af',
              900: '#1e3a8a',
            },
            success: '#22c55e',
            danger:  '#ef4444',
            warning: '#f59e0b',
          },
          fontFamily: {
            sans: ['IBM Plex Sans Thai', 'Sarabun', 'sans-serif'],
            mono: ['JetBrains Mono', 'monospace'],
          },
          borderRadius: {
            '4xl': '2rem',
            '5xl': '2.5rem',
          },
          spacing: {
            '18': '4.5rem',
            '22': '5.5rem',
            '72': '18rem',
            '84': '21rem',
          },
          animation: {
            'fade-in': 'fadeIn 0.5s ease-out',
            'slide-up': 'slideUp 0.4s ease-out',
            'wiggle': 'wiggle 0.3s ease-in-out',
          },
          keyframes: {
            fadeIn: {
              '0%': { opacity: '0' },
              '100%': { opacity: '1' },
            },
            slideUp: {
              '0%': { opacity: '0', transform: 'translateY(16px)' },
              '100%': { opacity: '1', transform: 'translateY(0)' },
            },
            wiggle: {
              '0%, 100%': { transform: 'rotate(-3deg)' },
              '50%': { transform: 'rotate(3deg)' },
            },
          },
          boxShadow: {
            'card': '0 2px 8px rgba(0,0,0,0.08)',
            'card-hover': '0 8px 24px rgba(0,0,0,0.12)',
            'glow': '0 0 20px rgba(37, 99, 235, 0.4)',
          },
        },
      },
    };
  </script>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Sans+Thai:wght@300;400;500;600;700&display=swap" rel="stylesheet">
</head>
<body class="bg-gray-50 p-8 font-sans">
  <div class="max-w-4xl mx-auto space-y-8">
    <h1 class="text-2xl font-extrabold">Custom Tailwind Config</h1>

    <!-- Brand colors -->
    <section class="bg-white rounded-4xl shadow-card p-6">
      <h2 class="font-bold mb-4">Brand Colors</h2>
      <div class="grid grid-cols-10 gap-1">
        <div><div class="h-10 rounded-xl bg-brand-50"></div><p class="text-[10px] text-center mt-1 text-gray-400">50</p></div>
        <div><div class="h-10 rounded-xl bg-brand-100"></div><p class="text-[10px] text-center mt-1 text-gray-400">100</p></div>
        <div><div class="h-10 rounded-xl bg-brand-200"></div><p class="text-[10px] text-center mt-1 text-gray-400">200</p></div>
        <div><div class="h-10 rounded-xl bg-brand-300"></div><p class="text-[10px] text-center mt-1 text-gray-400">300</p></div>
        <div><div class="h-10 rounded-xl bg-brand-400"></div><p class="text-[10px] text-center mt-1 text-gray-400">400</p></div>
        <div><div class="h-10 rounded-xl bg-brand-500"></div><p class="text-[10px] text-center mt-1 text-gray-400">500</p></div>
        <div><div class="h-10 rounded-xl bg-brand-600 ring-2 ring-offset-1 ring-brand-600"></div><p class="text-[10px] text-center mt-1 text-brand-600 font-bold">600✓</p></div>
        <div><div class="h-10 rounded-xl bg-brand-700"></div><p class="text-[10px] text-center mt-1 text-gray-400">700</p></div>
        <div><div class="h-10 rounded-xl bg-brand-800"></div><p class="text-[10px] text-center mt-1 text-gray-400">800</p></div>
        <div><div class="h-10 rounded-xl bg-brand-900"></div><p class="text-[10px] text-center mt-1 text-gray-400">900</p></div>
      </div>
      <div class="flex gap-3 mt-4">
        <div class="flex-1 p-3 bg-success text-white rounded-xl text-sm text-center font-medium">success</div>
        <div class="flex-1 p-3 bg-danger text-white rounded-xl text-sm text-center font-medium">danger</div>
        <div class="flex-1 p-3 bg-warning text-white rounded-xl text-sm text-center font-medium">warning</div>
      </div>
    </section>

    <!-- Custom animations -->
    <section class="bg-white rounded-4xl shadow-card p-6">
      <h2 class="font-bold mb-4">Custom Animations</h2>
      <div class="flex flex-wrap gap-3">
        <div class="animate-fade-in bg-brand-100 text-brand-700 px-4 py-2 rounded-xl text-sm font-medium">animate-fade-in</div>
        <div class="animate-slide-up bg-green-100 text-green-700 px-4 py-2 rounded-xl text-sm font-medium">animate-slide-up</div>
        <button class="hover:animate-wiggle bg-purple-100 text-purple-700 px-4 py-2 rounded-xl text-sm font-medium" onmouseover="this.classList.add('animate-wiggle')" onmouseout="this.classList.remove('animate-wiggle')">hover:wiggle</button>
      </div>
    </section>

    <!-- Custom shadows -->
    <section class="bg-white rounded-4xl shadow-card p-6">
      <h2 class="font-bold mb-4">Custom Shadows</h2>
      <div class="grid grid-cols-3 gap-4">
        <div class="bg-white rounded-2xl shadow-card p-4 text-center text-sm text-gray-600 hover:shadow-card-hover transition-shadow cursor-pointer">shadow-card</div>
        <div class="bg-white rounded-2xl shadow-card-hover p-4 text-center text-sm text-gray-600">shadow-card-hover</div>
        <div class="bg-brand-600 text-white rounded-2xl shadow-glow p-4 text-center text-sm">shadow-glow</div>
      </div>
    </section>

    <!-- Custom border-radius -->
    <section class="bg-white rounded-4xl shadow-card p-6">
      <h2 class="font-bold mb-4">Custom Border Radius</h2>
      <div class="flex flex-wrap gap-3 items-center">
        <div class="w-20 h-20 bg-brand-200 rounded-xl text-center text-xs flex items-center justify-center">xl</div>
        <div class="w-20 h-20 bg-brand-300 rounded-2xl text-center text-xs flex items-center justify-center">2xl</div>
        <div class="w-20 h-20 bg-brand-400 rounded-3xl text-center text-xs flex items-center justify-center text-white">3xl</div>
        <div class="w-20 h-20 bg-brand-500 rounded-4xl text-center text-xs flex items-center justify-center text-white">4xl</div>
        <div class="w-20 h-20 bg-brand-600 rounded-5xl text-center text-xs flex items-center justify-center text-white">5xl</div>
      </div>
    </section>

  </div>
</body>
</html>
```

---

## Steps 482–484: CSS Variables Theming

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>CSS Variables Theme - Steps 482-484</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            primary: 'rgb(var(--color-primary) / <alpha-value>)',
            secondary: 'rgb(var(--color-secondary) / <alpha-value>)',
            surface: 'rgb(var(--color-surface) / <alpha-value>)',
            'on-surface': 'rgb(var(--color-on-surface) / <alpha-value>)',
          },
        },
      },
    };
  </script>
  <style>
    /* Default (Indigo) theme */
    :root {
      --color-primary: 99 102 241;
      --color-secondary: 139 92 246;
      --color-surface: 255 255 255;
      --color-on-surface: 17 24 39;
    }
    /* Theme: Emerald */
    [data-theme="emerald"] {
      --color-primary: 16 185 129;
      --color-secondary: 5 150 105;
      --color-surface: 255 255 255;
      --color-on-surface: 17 24 39;
    }
    /* Theme: Rose */
    [data-theme="rose"] {
      --color-primary: 244 63 94;
      --color-secondary: 225 29 72;
      --color-surface: 255 255 255;
      --color-on-surface: 17 24 39;
    }
    /* Theme: Amber */
    [data-theme="amber"] {
      --color-primary: 245 158 11;
      --color-secondary: 217 119 6;
      --color-surface: 255 255 255;
      --color-on-surface: 17 24 39;
    }
    /* Dark + Indigo */
    [data-theme="dark"] {
      --color-primary: 129 140 248;
      --color-secondary: 167 139 250;
      --color-surface: 31 41 55;
      --color-on-surface: 243 244 246;
    }
  </style>
</head>
<body class="bg-gray-100 p-8 transition-all duration-300">
  <div class="max-w-3xl mx-auto space-y-6">
    <div class="flex items-center justify-between">
      <h2 class="text-xl font-extrabold">CSS Variable Theming</h2>
      <!-- Theme picker -->
      <div class="flex gap-2">
        <button onclick="setTheme('')" class="w-8 h-8 bg-indigo-500 rounded-full ring-2 ring-offset-2 ring-indigo-500" title="Indigo"></button>
        <button onclick="setTheme('emerald')" class="w-8 h-8 bg-emerald-500 rounded-full hover:ring-2 hover:ring-offset-2 hover:ring-emerald-500 transition-all" title="Emerald"></button>
        <button onclick="setTheme('rose')" class="w-8 h-8 bg-rose-500 rounded-full hover:ring-2 hover:ring-offset-2 hover:ring-rose-500 transition-all" title="Rose"></button>
        <button onclick="setTheme('amber')" class="w-8 h-8 bg-amber-500 rounded-full hover:ring-2 hover:ring-offset-2 hover:ring-amber-500 transition-all" title="Amber"></button>
        <button onclick="setTheme('dark')" class="w-8 h-8 bg-gray-800 rounded-full hover:ring-2 hover:ring-offset-2 hover:ring-gray-800 transition-all" title="Dark"></button>
      </div>
    </div>

    <!-- Cards using theme colors -->
    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
      <div class="bg-surface text-on-surface rounded-2xl border p-5 shadow-sm">
        <div class="w-10 h-10 bg-primary/10 rounded-xl flex items-center justify-center text-primary mb-3">📊</div>
        <h3 class="font-bold">Theme Card</h3>
        <p class="text-sm opacity-60 mt-1">Card ที่ใช้ CSS variable colors</p>
        <button class="mt-4 bg-primary text-white px-4 py-2 rounded-xl text-sm font-semibold hover:opacity-90 transition-opacity">Action</button>
      </div>
      <div class="bg-primary text-white rounded-2xl p-5 shadow-sm">
        <h3 class="font-bold">Primary Card</h3>
        <p class="text-sm opacity-80 mt-1">สีเปลี่ยนตาม theme</p>
        <button class="mt-4 bg-white/20 hover:bg-white/30 px-4 py-2 rounded-xl text-sm font-semibold transition-colors">Action</button>
      </div>
    </div>

    <!-- Form -->
    <div class="bg-surface text-on-surface rounded-2xl border p-5 shadow-sm">
      <h3 class="font-bold mb-4">Form</h3>
      <input type="text" placeholder="กรอกข้อความ..." class="w-full border rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-primary mb-3">
      <button class="bg-primary text-white px-5 py-2.5 rounded-xl text-sm font-semibold hover:opacity-90 transition-opacity">บันทึก</button>
    </div>

    <!-- Badges -->
    <div class="bg-surface rounded-2xl border p-5 flex flex-wrap gap-2">
      <span class="bg-primary/10 text-primary text-xs font-semibold px-3 py-1 rounded-full">Primary Badge</span>
      <span class="bg-secondary/10 text-secondary text-xs font-semibold px-3 py-1 rounded-full">Secondary Badge</span>
    </div>

  </div>

  <script>
    function setTheme(theme) {
      document.documentElement.setAttribute('data-theme', theme);
      document.body.classList.toggle('bg-gray-900', theme === 'dark');
      document.body.classList.toggle('bg-gray-100', theme !== 'dark');
    }
  </script>
</body>
</html>
```

---

## Steps 485–490: Multi-Brand Theme Workshop

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Multi-Brand Theme - Steps 485-490</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            t: {
              primary: 'rgb(var(--t-primary) / <alpha-value>)',
              'primary-dark': 'rgb(var(--t-primary-dark) / <alpha-value>)',
              accent: 'rgb(var(--t-accent) / <alpha-value>)',
              bg: 'rgb(var(--t-bg) / <alpha-value>)',
              surface: 'rgb(var(--t-surface) / <alpha-value>)',
              text: 'rgb(var(--t-text) / <alpha-value>)',
              muted: 'rgb(var(--t-muted) / <alpha-value>)',
              border: 'rgb(var(--t-border) / <alpha-value>)',
            },
          },
          fontFamily: {
            brand: ['var(--t-font)', 'sans-serif'],
          },
        },
      },
    };
  </script>
  <style>
    /* Brand: TechCorp (default) */
    :root, [data-brand="techcorp"] {
      --t-primary: 99 102 241;
      --t-primary-dark: 79 70 229;
      --t-accent: 139 92 246;
      --t-bg: 249 250 251;
      --t-surface: 255 255 255;
      --t-text: 17 24 39;
      --t-muted: 107 114 128;
      --t-border: 229 231 235;
      --t-font: 'Inter';
    }
    /* Brand: GreenLeaf */
    [data-brand="greenleaf"] {
      --t-primary: 16 185 129;
      --t-primary-dark: 5 150 105;
      --t-accent: 52 211 153;
      --t-bg: 240 253 244;
      --t-surface: 255 255 255;
      --t-text: 6 78 59;
      --t-muted: 52 134 88;
      --t-border: 209 250 229;
      --t-font: 'Nunito';
    }
    /* Brand: SunsetPay */
    [data-brand="sunsetpay"] {
      --t-primary: 239 68 68;
      --t-primary-dark: 220 38 38;
      --t-accent: 251 146 60;
      --t-bg: 255 241 242;
      --t-surface: 255 255 255;
      --t-text: 127 29 29;
      --t-muted: 185 28 28;
      --t-border: 254 226 226;
      --t-font: 'Poppins';
    }
    /* Brand: NightOwl (dark) */
    [data-brand="nightowl"] {
      --t-primary: 129 140 248;
      --t-primary-dark: 99 102 241;
      --t-accent: 196 181 253;
      --t-bg: 15 23 42;
      --t-surface: 30 41 59;
      --t-text: 226 232 240;
      --t-muted: 148 163 184;
      --t-border: 51 65 85;
      --t-font: 'JetBrains Mono';
    }
    body { background-color: rgb(var(--t-bg)); color: rgb(var(--t-text)); }
    * { transition: background-color 0.3s, border-color 0.3s, color 0.3s; }
  </style>
</head>
<body>

  <!-- Header -->
  <header class="bg-t-surface border-b border-t-border px-6 h-14 flex items-center justify-between sticky top-0 z-10">
    <div class="flex items-center gap-2">
      <div class="w-7 h-7 bg-t-primary rounded-lg flex items-center justify-center text-white font-bold text-xs">B</div>
      <span id="brand-name" class="font-extrabold text-t-text">TechCorp</span>
    </div>

    <!-- Brand switcher -->
    <div class="flex items-center gap-2 text-xs">
      <span class="text-t-muted hidden sm:block">Brand:</span>
      <div class="flex gap-1">
        <button onclick="setBrand('techcorp','TechCorp','🔷')" class="px-2.5 py-1 rounded-lg bg-indigo-100 text-indigo-700 font-medium hover:bg-indigo-200 transition-colors">TechCorp</button>
        <button onclick="setBrand('greenleaf','GreenLeaf','🌿')" class="px-2.5 py-1 rounded-lg bg-green-100 text-green-700 font-medium hover:bg-green-200 transition-colors">GreenLeaf</button>
        <button onclick="setBrand('sunsetpay','SunsetPay','🌅')" class="px-2.5 py-1 rounded-lg bg-red-100 text-red-700 font-medium hover:bg-red-200 transition-colors">SunsetPay</button>
        <button onclick="setBrand('nightowl','NightOwl','🦉')" class="px-2.5 py-1 rounded-lg bg-gray-800 text-gray-200 font-medium hover:bg-gray-700 transition-colors">NightOwl</button>
      </div>
    </div>
  </header>

  <main class="max-w-5xl mx-auto p-6 space-y-8">

    <!-- Hero -->
    <section class="bg-t-primary rounded-2xl p-8 text-white text-center">
      <span id="brand-emoji" class="text-4xl">🔷</span>
      <h1 class="text-2xl font-extrabold mt-3 mb-2" id="hero-title">ยินดีต้อนรับสู่ TechCorp</h1>
      <p class="opacity-80 text-sm" id="hero-sub">Platform สำหรับธุรกิจยุคใหม่</p>
      <div class="flex gap-3 justify-center mt-5">
        <button class="bg-white text-t-primary-dark px-5 py-2.5 rounded-xl text-sm font-bold hover:bg-opacity-90 transition-opacity">เริ่มต้นฟรี</button>
        <button class="border border-white/40 text-white px-5 py-2.5 rounded-xl text-sm font-semibold hover:bg-white/10 transition-colors">ดูตัวอย่าง</button>
      </div>
    </section>

    <!-- Features -->
    <section class="grid grid-cols-1 md:grid-cols-3 gap-4">
      <div class="bg-t-surface border border-t-border rounded-2xl p-5">
        <div class="w-10 h-10 bg-t-primary/10 rounded-xl flex items-center justify-center text-t-primary mb-3 text-xl">⚡</div>
        <h3 class="font-bold text-t-text">รวดเร็ว</h3>
        <p class="text-t-muted text-xs mt-1">Performance สูงสุดทุก platform</p>
      </div>
      <div class="bg-t-surface border border-t-border rounded-2xl p-5">
        <div class="w-10 h-10 bg-t-primary/10 rounded-xl flex items-center justify-center text-t-primary mb-3 text-xl">🔒</div>
        <h3 class="font-bold text-t-text">ปลอดภัย</h3>
        <p class="text-t-muted text-xs mt-1">Enterprise-grade security</p>
      </div>
      <div class="bg-t-surface border border-t-border rounded-2xl p-5">
        <div class="w-10 h-10 bg-t-primary/10 rounded-xl flex items-center justify-center text-t-primary mb-3 text-xl">📱</div>
        <h3 class="font-bold text-t-text">ทุกอุปกรณ์</h3>
        <p class="text-t-muted text-xs mt-1">Responsive ทุก breakpoint</p>
      </div>
    </section>

    <!-- Stats -->
    <section class="grid grid-cols-2 md:grid-cols-4 gap-4">
      <div class="bg-t-surface border border-t-border rounded-2xl p-4 text-center">
        <p class="text-2xl font-extrabold text-t-primary">10K+</p>
        <p class="text-xs text-t-muted mt-1">ลูกค้า</p>
      </div>
      <div class="bg-t-surface border border-t-border rounded-2xl p-4 text-center">
        <p class="text-2xl font-extrabold text-t-primary">99.9%</p>
        <p class="text-xs text-t-muted mt-1">Uptime</p>
      </div>
      <div class="bg-t-surface border border-t-border rounded-2xl p-4 text-center">
        <p class="text-2xl font-extrabold text-t-primary">50+</p>
        <p class="text-xs text-t-muted mt-1">ประเทศ</p>
      </div>
      <div class="bg-t-surface border border-t-border rounded-2xl p-4 text-center">
        <p class="text-2xl font-extrabold text-t-primary">4.9</p>
        <p class="text-xs text-t-muted mt-1">Rating</p>
      </div>
    </section>

    <!-- Pricing -->
    <section>
      <h2 class="text-xl font-extrabold text-t-text text-center mb-6">แผนราคา</h2>
      <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
        <div class="bg-t-surface border border-t-border rounded-2xl p-6">
          <h3 class="font-extrabold text-t-text">Free</h3>
          <p class="text-3xl font-extrabold text-t-text mt-2">฿0 <span class="text-sm font-normal text-t-muted">/เดือน</span></p>
          <ul class="mt-4 space-y-2 text-sm text-t-muted">
            <li class="flex gap-2"><span class="text-t-primary">✓</span> 5 Projects</li>
            <li class="flex gap-2"><span class="text-t-primary">✓</span> 1 GB Storage</li>
            <li class="flex gap-2 opacity-40"><span>✗</span> Priority Support</li>
          </ul>
          <button class="mt-5 w-full border-2 border-t-primary text-t-primary py-2.5 rounded-xl text-sm font-semibold hover:bg-t-primary/5 transition-colors">เริ่มฟรี</button>
        </div>
        <div class="bg-t-primary rounded-2xl p-6 text-white relative overflow-hidden">
          <span class="absolute top-3 right-3 text-xs bg-white/20 px-2 py-0.5 rounded-full font-medium">แนะนำ</span>
          <h3 class="font-extrabold">Pro</h3>
          <p class="text-3xl font-extrabold mt-2">฿890 <span class="text-sm font-normal opacity-80">/เดือน</span></p>
          <ul class="mt-4 space-y-2 text-sm opacity-90">
            <li class="flex gap-2"><span>✓</span> Unlimited Projects</li>
            <li class="flex gap-2"><span>✓</span> 50 GB Storage</li>
            <li class="flex gap-2"><span>✓</span> Priority Support</li>
          </ul>
          <button class="mt-5 w-full bg-white text-t-primary py-2.5 rounded-xl text-sm font-bold hover:bg-white/90 transition-opacity">สมัครเลย</button>
        </div>
        <div class="bg-t-surface border border-t-border rounded-2xl p-6">
          <h3 class="font-extrabold text-t-text">Enterprise</h3>
          <p class="text-3xl font-extrabold text-t-text mt-2">Custom</p>
          <ul class="mt-4 space-y-2 text-sm text-t-muted">
            <li class="flex gap-2"><span class="text-t-primary">✓</span> Unlimited Everything</li>
            <li class="flex gap-2"><span class="text-t-primary">✓</span> SLA Guarantee</li>
            <li class="flex gap-2"><span class="text-t-primary">✓</span> Dedicated Support</li>
          </ul>
          <button class="mt-5 w-full bg-t-primary text-white py-2.5 rounded-xl text-sm font-semibold hover:opacity-90 transition-opacity">ติดต่อเรา</button>
        </div>
      </div>
    </section>

    <!-- CTA -->
    <section class="bg-t-surface border-2 border-t-primary rounded-2xl p-8 text-center">
      <h2 class="text-xl font-extrabold text-t-text mb-2">พร้อมเริ่มต้นแล้วหรือยัง?</h2>
      <p class="text-t-muted text-sm mb-5">ลองใช้ฟรี 14 วัน ไม่ต้องใช้บัตรเครดิต</p>
      <div class="flex gap-3 justify-center">
        <input type="email" placeholder="อีเมลของคุณ" class="border border-t-border bg-t-bg text-t-text rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-t-primary w-64">
        <button class="bg-t-primary text-white px-5 py-2.5 rounded-xl text-sm font-semibold hover:opacity-90 transition-opacity">เริ่มทดลองใช้</button>
      </div>
    </section>

  </main>

  <script>
    const brandData = {
      techcorp:  { name: 'TechCorp',  emoji: '🔷', sub: 'Platform สำหรับธุรกิจยุคใหม่' },
      greenleaf: { name: 'GreenLeaf', emoji: '🌿', sub: 'ยั่งยืน เป็นมิตรกับสิ่งแวดล้อม' },
      sunsetpay: { name: 'SunsetPay', emoji: '🌅', sub: 'ชำระเงินง่าย ทุกที่ทุกเวลา' },
      nightowl:  { name: 'NightOwl',  emoji: '🦉', sub: 'Developer tools สำหรับคืนดึก' },
    };

    function setBrand(key, name, emoji) {
      document.documentElement.setAttribute('data-brand', key);
      document.body.style.backgroundColor = '';
      const d = brandData[key];
      document.getElementById('brand-name').textContent = d.name;
      document.getElementById('brand-emoji').textContent = d.emoji;
      document.getElementById('hero-title').textContent = 'ยินดีต้อนรับสู่ ' + d.name;
      document.getElementById('hero-sub').textContent = d.sub;
    }
  </script>

</body>
</html>
```

---

## สรุป Part 49

| Step | เนื้อหา |
|------|---------|
| 481 | Custom config via CDN: colors, fontFamily, borderRadius, spacing, animation, boxShadow |
| 482 | CSS variables theming: `--color-primary` → Tailwind class |
| 483 | Multiple themes via `[data-theme="..."]` selector |
| 484 | Theme picker UI with CSS variable switch |
| 485 | Multi-brand setup: CSS variable tokens per brand |
| 486 | Brand switcher hero section |
| 487 | Feature cards + stats with theme tokens |
| 488 | Pricing cards with theme-aware styling |
| 489 | CTA form with theme colors |
| 490 | Workshop: Complete multi-brand site (TechCorp/GreenLeaf/SunsetPay/NightOwl) |

**Part ถัดไป:** Part 50 — Performance & Production Best Practices (Steps 491–500)
