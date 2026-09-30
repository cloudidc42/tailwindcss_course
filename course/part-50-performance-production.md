# Part 50: Performance & Production Best Practices

## เป้าหมาย
- Optimize Tailwind สำหรับ production
- Purge CSS, class organization, และ code splitting
- Accessibility (a11y) best practices
- Performance patterns สำหรับ web apps จริง
- Steps 491–500

---

## Step 491: Tailwind Production Optimization

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Production Patterns - Step 491</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-8">
  <div class="max-w-4xl mx-auto space-y-8">
    <h2 class="text-xl font-extrabold">Production Best Practices</h2>

    <!-- Build tool setup guide -->
    <section class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4">Production Setup (Vite + Tailwind)</h3>
      <div class="bg-gray-900 rounded-xl overflow-hidden text-xs">
        <div class="flex items-center gap-2 px-4 py-2 bg-gray-800 border-b border-gray-700">
          <div class="w-3 h-3 rounded-full bg-red-500"></div>
          <div class="w-3 h-3 rounded-full bg-yellow-500"></div>
          <div class="w-3 h-3 rounded-full bg-green-500"></div>
          <span class="ml-2 text-gray-400">terminal</span>
        </div>
        <pre class="p-4 text-green-400 overflow-x-auto"><code><span class="text-yellow-400"># 1. Create project</span>
npm create vite@latest my-app -- --template vanilla
cd my-app

<span class="text-yellow-400"># 2. Install Tailwind</span>
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p

<span class="text-yellow-400"># 3. tailwind.config.js</span>
module.exports = {
  content: ['./index.html', './src/**/*.{js,ts,jsx,tsx}'],
  theme: { extend: {} },
  plugins: [],
};

<span class="text-yellow-400"># 4. src/index.css</span>
@tailwind base;
@tailwind components;
@tailwind utilities;

<span class="text-yellow-400"># 5. Build (เหลือแค่ class ที่ใช้จริง)</span>
npm run build  <span class="text-gray-500"># CSS ขนาดเล็กลง ~95%</span></code></pre>
      </div>
    </section>

    <!-- Class organization -->
    <section class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4">Class Organization (Recommended Order)</h3>
      <div class="bg-gray-900 rounded-xl p-4 text-xs font-mono text-green-400 overflow-x-auto">
        <pre><code><span class="text-gray-500">/* แนะนำจัดเรียง class ตามลำดับนี้ */</span>
&lt;div class="
  <span class="text-blue-400">/* Layout */</span>
  flex items-center justify-between
  <span class="text-yellow-400">/* Spacing */</span>
  px-4 py-2.5 gap-3 space-x-2
  <span class="text-green-400">/* Sizing */</span>
  w-full h-14 max-w-xl
  <span class="text-purple-400">/* Typography */</span>
  text-sm font-semibold leading-tight
  <span class="text-red-400">/* Colors */</span>
  bg-white text-gray-900
  <span class="text-orange-400">/* Borders */</span>
  border border-gray-200 rounded-xl
  <span class="text-pink-400">/* Effects */</span>
  shadow-sm hover:shadow-md
  <span class="text-cyan-400">/* Transitions */</span>
  transition-all duration-200
  <span class="text-gray-400">/* Responsive */</span>
  md:px-6 lg:max-w-2xl
  <span class="text-gray-400">/* Dark mode */</span>
  dark:bg-gray-800 dark:text-white
"&gt;</code></pre>
      </div>
    </section>

    <!-- @apply for components -->
    <section class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4">@apply สำหรับ Component Classes</h3>
      <div class="bg-gray-900 rounded-xl p-4 text-xs font-mono overflow-x-auto">
        <pre class="text-green-400"><code><span class="text-gray-500">/* index.css - สร้าง reusable component classes */</span>
@layer components &#123;
  .btn &#123;
    @apply px-4 py-2 rounded-xl text-sm font-semibold transition-colors;
  &#125;
  .btn-primary &#123;
    @apply btn bg-indigo-600 text-white hover:bg-indigo-700;
  &#125;
  .btn-outline &#123;
    @apply btn border-2 border-indigo-600 text-indigo-600 hover:bg-indigo-50;
  &#125;
  .card &#123;
    @apply bg-white border border-gray-200 rounded-2xl p-5 shadow-sm;
  &#125;
  .input &#123;
    @apply w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm
      focus:outline-none focus:ring-2 focus:ring-indigo-500;
  &#125;
&#125;

<span class="text-gray-500">/* ใน HTML */</span>
&lt;button class="<span class="text-yellow-300">btn-primary</span>"&gt;Save&lt;/button&gt;
&lt;div class="<span class="text-yellow-300">card</span>"&gt;...&lt;/div&gt;</code></pre>
      </div>
    </section>

    <!-- Avoid common mistakes -->
    <section class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4">ข้อผิดพลาดที่พบบ่อย</h3>
      <div class="space-y-3">
        <div class="bg-red-50 border border-red-200 rounded-xl p-4">
          <p class="text-xs font-bold text-red-700 mb-1">❌ อย่าทำ: Dynamic class names</p>
          <code class="text-xs text-red-600">const color = 'indigo'; → `bg-${color}-600` ← Tailwind จะ purge!</code>
        </div>
        <div class="bg-green-50 border border-green-200 rounded-xl p-4">
          <p class="text-xs font-bold text-green-700 mb-1">✅ ทำแบบนี้แทน: Full class names</p>
          <code class="text-xs text-green-600">const classes = &#123; indigo: 'bg-indigo-600', red: 'bg-red-600' &#125;[color]</code>
        </div>
        <div class="bg-red-50 border border-red-200 rounded-xl p-4">
          <p class="text-xs font-bold text-red-700 mb-1">❌ อย่าทำ: ใช้ important ฟุ่มเฟือย</p>
          <code class="text-xs text-red-600">class="!text-red-500 !font-bold !p-4" ← ทำให้ debug ยาก</code>
        </div>
        <div class="bg-green-50 border border-green-200 rounded-xl p-4">
          <p class="text-xs font-bold text-green-700 mb-1">✅ ทำแบบนี้แทน: จัดลำดับ specificity ให้ถูกต้อง</p>
          <code class="text-xs text-green-600">ใช้ @layer utilities สำหรับ override</code>
        </div>
      </div>
    </section>

  </div>
</body>
</html>
```

---

## Steps 492–495: Accessibility (a11y) Best Practices

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Accessibility - Steps 492-495</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-8">
  <div class="max-w-4xl mx-auto space-y-8">
    <h2 class="text-xl font-extrabold">Accessibility (a11y)</h2>

    <!-- Focus visible (Step 492) -->
    <section class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4">Focus Visible — Keyboard Navigation</h3>
      <p class="text-sm text-gray-600 mb-4">กด Tab เพื่อเลื่อน focus ระหว่าง elements</p>
      <div class="flex flex-wrap gap-3">
        <button class="bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm font-semibold
          focus:outline-none focus-visible:ring-2 focus-visible:ring-indigo-500 focus-visible:ring-offset-2">
          Button 1
        </button>
        <button class="border border-gray-300 px-4 py-2 rounded-xl text-sm
          focus:outline-none focus-visible:ring-2 focus-visible:ring-indigo-500 focus-visible:ring-offset-2">
          Button 2
        </button>
        <a href="#" class="text-indigo-600 underline text-sm
          focus:outline-none focus-visible:ring-2 focus-visible:ring-indigo-500 focus-visible:ring-offset-2 rounded">
          Link
        </a>
        <input type="text" placeholder="Input..." class="border border-gray-300 rounded-xl px-3 py-2 text-sm
          focus:outline-none focus-visible:ring-2 focus-visible:ring-indigo-500">
      </div>
    </section>

    <!-- Screen reader text (Step 493) -->
    <section class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4">Screen Reader Text (sr-only)</h3>
      <div class="space-y-3">
        <!-- Icon button with sr-only label -->
        <div class="flex gap-3">
          <button class="w-9 h-9 bg-gray-100 rounded-xl flex items-center justify-center hover:bg-gray-200 transition-colors" aria-label="ค้นหา">
            <span class="sr-only">ค้นหา</span>
            <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"/></svg>
          </button>
          <button class="w-9 h-9 bg-gray-100 rounded-xl flex items-center justify-center hover:bg-gray-200 transition-colors" aria-label="การแจ้งเตือน">
            <span class="sr-only">การแจ้งเตือน</span>
            🔔
          </button>
          <button class="w-9 h-9 bg-red-100 rounded-xl flex items-center justify-center hover:bg-red-200 transition-colors" aria-label="ลบรายการ">
            <span class="sr-only">ลบรายการ</span>
            🗑️
          </button>
        </div>
        <p class="text-xs text-gray-500">icon buttons ต้องมี aria-label หรือ sr-only text</p>

        <!-- Form labels -->
        <div class="mt-4">
          <label for="email-a11y" class="block text-sm font-medium text-gray-700 mb-1">
            อีเมล <span class="text-red-500" aria-label="จำเป็น">*</span>
          </label>
          <input id="email-a11y" type="email" required aria-required="true" aria-describedby="email-hint"
            class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
          <p id="email-hint" class="text-xs text-gray-500 mt-1">ใช้สำหรับ login และรับการแจ้งเตือน</p>
        </div>
      </div>
    </section>

    <!-- Color contrast (Step 494) -->
    <section class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4">Color Contrast (WCAG AA)</h3>
      <div class="grid grid-cols-2 md:grid-cols-4 gap-3 text-center text-sm font-medium">
        <div class="p-4 bg-indigo-600 text-white rounded-xl">
          <p>✅ PASS</p>
          <p class="text-[10px] opacity-70 mt-1">4.5:1+</p>
        </div>
        <div class="p-4 bg-indigo-100 text-indigo-900 rounded-xl">
          <p>✅ PASS</p>
          <p class="text-[10px] opacity-70 mt-1">dark on light</p>
        </div>
        <div class="p-4 bg-yellow-400 text-gray-900 rounded-xl">
          <p>✅ PASS</p>
          <p class="text-[10px] opacity-70 mt-1">dark text ดีกว่า</p>
        </div>
        <div class="p-4 bg-yellow-200 text-yellow-400 rounded-xl">
          <p>❌ FAIL</p>
          <p class="text-[10px] opacity-70 mt-1">contrast ต่ำ</p>
        </div>
      </div>
      <p class="text-xs text-gray-500 mt-3">WCAG AA ต้องการ contrast ratio 4.5:1 สำหรับ normal text</p>
    </section>

    <!-- ARIA roles and live regions (Step 495) -->
    <section class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4">ARIA Roles & Live Regions</h3>

      <!-- Alert/Status -->
      <div class="space-y-3">
        <div role="alert" aria-live="assertive" class="bg-red-50 border border-red-200 rounded-xl p-4 flex gap-3">
          <span aria-hidden="true">❌</span>
          <p class="text-sm text-red-800">role="alert" — อ่านทันที (สำหรับ errors)</p>
        </div>
        <div role="status" aria-live="polite" class="bg-green-50 border border-green-200 rounded-xl p-4 flex gap-3">
          <span aria-hidden="true">✅</span>
          <p class="text-sm text-green-800">role="status" — รออ่าน (สำหรับ updates)</p>
        </div>

        <!-- Modal accessibility -->
        <div class="bg-gray-50 rounded-xl p-4 text-xs font-mono text-gray-700">
          <p class="font-bold mb-2 text-sm">Modal ที่ถูกต้องตาม a11y:</p>
          <pre class="text-xs overflow-x-auto"><code>&lt;div role="dialog"
     aria-modal="true"
     aria-labelledby="modal-title"
     aria-describedby="modal-desc"&gt;
  &lt;h2 id="modal-title"&gt;หัวข้อ Modal&lt;/h2&gt;
  &lt;p id="modal-desc"&gt;คำอธิบาย...&lt;/p&gt;
  &lt;button autofocus&gt;Primary Action&lt;/button&gt;
&lt;/div&gt;</code></pre>
        </div>
      </div>
    </section>

  </div>
</body>
</html>
```

---

## Steps 496–500: Performance Patterns Workshop

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Performance Workshop - Steps 496-500</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      theme: {
        extend: {
          keyframes: {
            fadeUp: { '0%': { opacity: '0', transform: 'translateY(20px)' }, '100%': { opacity: '1', transform: 'translateY(0)' } },
          },
          animation: {
            'fade-up': 'fadeUp 0.5s ease-out',
          },
        },
      },
    };
  </script>
  <style>
    /* Lazy-load patterns */
    .lazy-hidden { opacity: 0; transform: translateY(20px); transition: all 0.5s ease-out; }
    .lazy-visible { opacity: 1; transform: translateY(0); }
    /* Reduce motion */
    @media (prefers-reduced-motion: reduce) {
      *, *::before, *::after { animation-duration: 0.01ms !important; transition-duration: 0.01ms !important; }
    }
  </style>
</head>
<body class="bg-gray-50">

  <header class="bg-white border-b px-6 h-14 flex items-center justify-between sticky top-0 z-20">
    <h1 class="font-extrabold text-indigo-600">Performance Patterns</h1>
    <div class="flex items-center gap-2">
      <span id="perf-score" class="text-xs bg-green-100 text-green-700 px-2.5 py-1 rounded-full font-semibold">Score: 100</span>
    </div>
  </header>

  <main class="max-w-5xl mx-auto p-6 space-y-8">

    <!-- Lazy scroll reveal (Step 496) -->
    <section class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4">Scroll-Triggered Animations (Step 496)</h3>
      <p class="text-sm text-gray-600 mb-4">Elements แสดงผลเมื่อเลื่อนลงมาถึง viewport</p>
      <div class="space-y-4">
        <div class="lazy-hidden p-4 bg-indigo-50 rounded-xl"><p class="font-semibold text-indigo-700">Item 1 — Fade up</p></div>
        <div class="lazy-hidden p-4 bg-green-50 rounded-xl"><p class="font-semibold text-green-700">Item 2 — Fade up</p></div>
        <div class="lazy-hidden p-4 bg-purple-50 rounded-xl"><p class="font-semibold text-purple-700">Item 3 — Fade up</p></div>
      </div>
    </section>

    <!-- Virtual list pattern (Step 497) -->
    <section class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-2">Virtual List (Virtualization) Pattern (Step 497)</h3>
      <p class="text-sm text-gray-600 mb-4">แสดงเฉพาะ items ที่อยู่ใน viewport</p>
      <div class="flex gap-3 mb-3">
        <button onclick="addItems(100)" class="bg-indigo-600 text-white px-3 py-1.5 rounded-xl text-sm">+100 items</button>
        <button onclick="addItems(1000)" class="border border-gray-300 px-3 py-1.5 rounded-xl text-sm">+1000 items</button>
        <span class="text-sm text-gray-500 self-center" id="item-count">0 items</span>
      </div>
      <div id="virtual-container" class="h-48 overflow-y-auto border rounded-xl p-2 space-y-1" onscroll="onVScroll()"></div>
    </section>

    <!-- Debounced search (Step 498) -->
    <section class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4">Debounced Search (Step 498)</h3>
      <div class="relative">
        <svg class="absolute left-3 top-1/2 -translate-y-1/2 w-4 h-4 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"/></svg>
        <input id="search-input" type="text" placeholder="ค้นหา (debounced 300ms)..." oninput="debouncedSearch(this.value)" class="w-full border border-gray-300 rounded-xl pl-9 pr-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
      </div>
      <div id="search-results" class="mt-3 space-y-1.5 min-h-12"></div>
    </section>

    <!-- Intersection Observer (Step 499) -->
    <section class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4">Lazy Image Loading (Step 499)</h3>
      <div class="grid grid-cols-2 md:grid-cols-4 gap-3">
        <div class="lazy-img aspect-square bg-gray-200 rounded-xl overflow-hidden" data-src="placeholder">
          <div class="w-full h-full flex items-center justify-center text-gray-400 text-xs">Loading...</div>
        </div>
        <div class="lazy-img aspect-square bg-gray-200 rounded-xl overflow-hidden" data-src="placeholder">
          <div class="w-full h-full flex items-center justify-center text-gray-400 text-xs">Loading...</div>
        </div>
        <div class="lazy-img aspect-square bg-gray-200 rounded-xl overflow-hidden" data-src="placeholder">
          <div class="w-full h-full flex items-center justify-center text-gray-400 text-xs">Loading...</div>
        </div>
        <div class="lazy-img aspect-square bg-gray-200 rounded-xl overflow-hidden" data-src="placeholder">
          <div class="w-full h-full flex items-center justify-center text-gray-400 text-xs">Loading...</div>
        </div>
      </div>
    </section>

    <!-- Production checklist (Step 500 Workshop) -->
    <section class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4">Production Launch Checklist (Step 500)</h3>
      <div class="space-y-2" id="checklist">
        <!-- filled by JS -->
      </div>
      <div class="mt-5 p-4 bg-indigo-50 rounded-xl border border-indigo-200 text-center hidden" id="launch-ready">
        <p class="text-2xl mb-1">🚀</p>
        <p class="font-extrabold text-indigo-800">Ready to Launch!</p>
        <p class="text-xs text-indigo-600 mt-1">ผ่านทุก checklist แล้ว</p>
      </div>
      <button onclick="launchCheck()" class="mt-4 bg-indigo-600 text-white px-5 py-2.5 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">Run Check</button>
    </section>

  </main>

  <script>
    // Scroll reveal
    const lazyEls = document.querySelectorAll('.lazy-hidden');
    const io = new IntersectionObserver(entries => {
      entries.forEach(e => { if (e.isIntersecting) { e.target.classList.add('lazy-visible'); io.unobserve(e.target); } });
    }, { threshold: 0.1 });
    lazyEls.forEach(el => io.observe(el));

    // Virtual list
    let allItems = [];
    function addItems(n) {
      for (let i = 0; i < n; i++) allItems.push({ id: allItems.length + 1, name: 'รายการ #' + (allItems.length + 1) });
      document.getElementById('item-count').textContent = allItems.length + ' items';
      renderVisible();
    }
    function renderVisible() {
      const c = document.getElementById('virtual-container');
      const scrollTop = c.scrollTop;
      const rowH = 40;
      const visible = Math.ceil(c.clientHeight / rowH) + 2;
      const startIdx = Math.max(0, Math.floor(scrollTop / rowH) - 1);
      const endIdx = Math.min(allItems.length, startIdx + visible);
      c.innerHTML = '';
      // Spacer top
      const topSpacer = document.createElement('div');
      topSpacer.style.height = (startIdx * rowH) + 'px';
      c.appendChild(topSpacer);
      // Visible items
      allItems.slice(startIdx, endIdx).forEach(item => {
        const d = document.createElement('div');
        d.className = 'h-9 flex items-center px-3 bg-gray-50 rounded-lg text-sm text-gray-700';
        d.textContent = item.name;
        c.appendChild(d);
      });
      // Spacer bottom
      const botSpacer = document.createElement('div');
      botSpacer.style.height = Math.max(0, (allItems.length - endIdx) * rowH) + 'px';
      c.appendChild(botSpacer);
    }
    function onVScroll() { renderVisible(); }

    // Debounced search
    const items = ['Tailwind CSS', 'TailwindUI', 'Taylor Swift', 'Technical SEO', 'React', 'Vue', 'Angular', 'Svelte', 'Next.js', 'Nuxt.js'];
    let debTimer;
    function debouncedSearch(q) {
      clearTimeout(debTimer);
      debTimer = setTimeout(() => {
        const res = document.getElementById('search-results');
        if (!q) { res.innerHTML = '<p class="text-xs text-gray-400 py-1">พิมพ์เพื่อค้นหา...</p>'; return; }
        const found = items.filter(x => x.toLowerCase().includes(q.toLowerCase()));
        if (!found.length) { res.innerHTML = '<p class="text-xs text-gray-400 py-1">ไม่พบผลลัพธ์</p>'; return; }
        res.innerHTML = found.map(x => `<div class="flex items-center gap-2 px-3 py-2 bg-gray-50 rounded-xl text-sm hover:bg-indigo-50 hover:text-indigo-700 cursor-pointer transition-colors">🔍 ${x}</div>`).join('');
      }, 300);
    }

    // Lazy images
    const lazyImgs = document.querySelectorAll('.lazy-img');
    const imgObs = new IntersectionObserver(entries => {
      entries.forEach((e, i) => {
        if (e.isIntersecting) {
          const colors = ['bg-indigo-200', 'bg-green-200', 'bg-yellow-200', 'bg-purple-200'];
          const icons = ['🌄', '🌿', '🌅', '🎨'];
          e.target.classList.remove('bg-gray-200');
          e.target.classList.add(colors[i % 4]);
          e.target.innerHTML = `<div class="w-full h-full flex flex-col items-center justify-center text-3xl gap-1"><span>${icons[i % 4]}</span><span class="text-xs font-medium text-gray-600">Loaded</span></div>`;
          imgObs.unobserve(e.target);
        }
      });
    }, { threshold: 0.1 });
    lazyImgs.forEach(el => imgObs.observe(el));

    // Checklist
    const checks = [
      { id: 'purge', label: 'CSS Purge/PurgeCSS enabled', group: 'Build' },
      { id: 'minify', label: 'HTML/CSS/JS minified', group: 'Build' },
      { id: 'compress', label: 'Gzip/Brotli compression', group: 'Server' },
      { id: 'cache', label: 'Cache headers ถูกต้อง', group: 'Server' },
      { id: 'a11y', label: 'Focus visible บน interactive elements', group: 'a11y' },
      { id: 'contrast', label: 'Color contrast WCAG AA ผ่าน', group: 'a11y' },
      { id: 'alt', label: 'img tags มี alt text', group: 'a11y' },
      { id: 'responsive', label: 'Test บนทุก breakpoint', group: 'Responsive' },
      { id: 'dark', label: 'Dark mode ทำงานถูกต้อง', group: 'Dark Mode' },
      { id: 'perf', label: 'Lighthouse score ≥ 90', group: 'Performance' },
    ];

    const cl = document.getElementById('checklist');
    checks.forEach(c => {
      const d = document.createElement('label');
      d.className = 'flex items-center gap-3 p-3 rounded-xl hover:bg-gray-50 cursor-pointer transition-colors';
      d.innerHTML = `
        <input type="checkbox" class="w-4 h-4 rounded text-indigo-600 focus:ring-indigo-500" onchange="updateScore()">
        <div class="flex-1 min-w-0">
          <span class="text-sm font-medium">${c.label}</span>
        </div>
        <span class="text-[10px] bg-gray-100 text-gray-500 px-1.5 py-0.5 rounded-full flex-shrink-0">${c.group}</span>
      `;
      cl.appendChild(d);
    });

    function updateScore() {
      const total = document.querySelectorAll('#checklist input').length;
      const checked = document.querySelectorAll('#checklist input:checked').length;
      const pct = Math.round((checked / total) * 100);
      const score = document.getElementById('perf-score');
      score.textContent = `Score: ${pct}%`;
      score.className = `text-xs px-2.5 py-1 rounded-full font-semibold ${pct === 100 ? 'bg-green-100 text-green-700' : pct >= 70 ? 'bg-yellow-100 text-yellow-700' : 'bg-red-100 text-red-700'}`;
      document.getElementById('launch-ready').classList.toggle('hidden', pct < 100);
    }

    function launchCheck() {
      document.querySelectorAll('#checklist input').forEach(cb => { cb.checked = Math.random() > 0.3; });
      updateScore();
    }
  </script>

</body>
</html>
```

---

## สรุป Part 50

| Step | เนื้อหา |
|------|---------|
| 491 | Production setup: Vite + Tailwind, purge CSS, class organization, @apply, avoid dynamic classes |
| 492 | Accessibility: focus-visible, keyboard navigation |
| 493 | Screen reader: sr-only, aria-label, aria-describedby, form labels |
| 494 | Color contrast WCAG AA, semantic colors |
| 495 | ARIA roles: role="dialog/alert/status", aria-modal, aria-live |
| 496 | Scroll-triggered animations with IntersectionObserver |
| 497 | Virtual list pattern for large datasets |
| 498 | Debounced search with clearTimeout |
| 499 | Lazy image loading with IntersectionObserver |
| 500 | Workshop: Production launch checklist + live score |

---

## 🎉 Level 4 Complete! (Steps 401–500)

**Parts 41–50 ครอบคลุม:**
- Advanced dashboards & analytics
- Kanban & CRM systems
- Chat & messaging UI
- File manager & media gallery
- Design system components
- Animations & micro-interactions
- Responsive design mastery
- Dark mode implementation
- Custom config & multi-brand theming
- Performance & accessibility production

**Part ถัดไป:** Part 51 — React + Tailwind Integration (Steps 501–510)
