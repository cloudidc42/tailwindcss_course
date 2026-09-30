# Part 75: Advanced Animation & Micro-interactions

## เป้าหมาย
- CSS keyframe animations ขั้นสูง
- Micro-interactions — hover, focus, click
- Scroll-triggered animations
- Stagger effects + orchestrated sequences
- Steps 741–750

---

## Steps 741–750: Advanced Animation Workshop

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Advanced Animation - Steps 741-750</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    /* === Core animations === */
    @keyframes float      { 0%,100%{transform:translateY(0)} 50%{transform:translateY(-12px)} }
    @keyframes pulse-ring { 0%{transform:scale(1);opacity:1} 100%{transform:scale(2);opacity:0} }
    @keyframes shimmer    { 0%{background-position:-200% 0} 100%{background-position:200% 0} }
    @keyframes bounce-in  { 0%{transform:scale(0);opacity:0} 60%{transform:scale(1.15)} 80%{transform:scale(0.95)} 100%{transform:scale(1);opacity:1} }
    @keyframes elastic    { 0%{transform:scaleX(1)} 25%{transform:scaleX(1.15) scaleY(0.85)} 50%{transform:scaleX(0.9) scaleY(1.1)} 75%{transform:scaleX(1.05) scaleY(0.97)} 100%{transform:scaleX(1)} }
    @keyframes typewriter { from{width:0} to{width:100%} }
    @keyframes blink      { 0%,100%{opacity:1} 50%{opacity:0} }
    @keyframes orbit      { from{transform:rotate(0deg) translateX(60px) rotate(0deg)} to{transform:rotate(360deg) translateX(60px) rotate(-360deg)} }
    @keyframes morph      { 0%{border-radius:60% 40% 30% 70%/60% 30% 70% 40%} 50%{border-radius:30% 60% 70% 40%/50% 60% 30% 60%} 100%{border-radius:60% 40% 30% 70%/60% 30% 70% 40%} }
    @keyframes drawLine   { from{stroke-dashoffset:500} to{stroke-dashoffset:0} }
    @keyframes countNum   { from{opacity:0;transform:translateY(20px)} to{opacity:1;transform:translateY(0)} }
    @keyframes ripple     { 0%{transform:scale(0);opacity:0.6} 100%{transform:scale(4);opacity:0} }
    @keyframes wave       { 0%,100%{transform:rotate(0)} 25%{transform:rotate(20deg)} 75%{transform:rotate(-20deg)} }
    @keyframes spotlight  { 0%,100%{opacity:0.3} 50%{opacity:1} }

    .float        { animation: float 3s ease-in-out infinite; }
    .shimmer-bg   { background: linear-gradient(90deg,#f3f4f6 25%,#e5e7eb 50%,#f3f4f6 75%); background-size:200%; animation:shimmer 1.5s infinite; }
    .bounce-in    { animation: bounce-in 0.6s cubic-bezier(.34,1.56,.64,1) forwards; }
    .morph-shape  { animation: morph 6s ease-in-out infinite; }
    .orbit-dot    { animation: orbit 3s linear infinite; }
    .wave-hand    { display:inline-block; animation:wave 0.8s ease-in-out; animation-iteration-count:3; }

    /* Button ripple */
    .ripple-btn   { position:relative; overflow:hidden; }
    .ripple-btn .ripple-effect { position:absolute; border-radius:50%; background:rgba(255,255,255,0.4); animation:ripple 0.6s ease-out forwards; pointer-events:none; }

    /* Typewriter */
    .typewriter   { overflow:hidden; white-space:nowrap; border-right:3px solid #6366f1; animation:typewriter 2s steps(30) forwards, blink 0.75s step-end infinite 2s; width:0; display:inline-block; }

    /* Draw SVG */
    .draw-path    { stroke-dasharray:500; animation:drawLine 2s ease forwards; }

    /* Scroll reveal */
    .reveal { opacity:0; transform:translateY(24px); transition:opacity 0.6s ease, transform 0.6s ease; }
    .reveal.visible { opacity:1; transform:translateY(0); }

    /* Stagger */
    .stagger > * { opacity:0; transform:translateY(16px); }
    .stagger.go > *:nth-child(1) { animation:countNum 0.4s 0.0s ease forwards; }
    .stagger.go > *:nth-child(2) { animation:countNum 0.4s 0.1s ease forwards; }
    .stagger.go > *:nth-child(3) { animation:countNum 0.4s 0.2s ease forwards; }
    .stagger.go > *:nth-child(4) { animation:countNum 0.4s 0.3s ease forwards; }
    .stagger.go > *:nth-child(5) { animation:countNum 0.4s 0.4s ease forwards; }
    .stagger.go > *:nth-child(6) { animation:countNum 0.4s 0.5s ease forwards; }
  </style>
</head>
<body class="bg-gray-950 text-white min-h-screen">

  <div class="max-w-4xl mx-auto px-6 py-12">
    <h1 class="text-3xl font-extrabold text-white mb-2 text-center">
      Advanced Animation
    </h1>
    <p class="text-center text-gray-400 mb-12">สาธิต CSS animation เทคนิคขั้นสูง</p>

    <!-- Typewriter -->
    <section class="bg-gray-900 rounded-3xl p-8 mb-6 border border-gray-800">
      <h2 class="text-sm font-semibold text-indigo-400 mb-4 uppercase tracking-wide">Step 741 — Typewriter Effect</h2>
      <div class="font-mono text-xl">
        <span class="typewriter" id="tw">สวัสดี ฉันคือ Claude</span>
      </div>
      <button onclick="restartTypewriter()" class="mt-4 text-xs text-indigo-400 hover:text-indigo-300 border border-indigo-800 px-3 py-1.5 rounded-lg transition-colors">↺ เล่นซ้ำ</button>
    </section>

    <!-- Floating + Morph -->
    <section class="bg-gray-900 rounded-3xl p-8 mb-6 border border-gray-800">
      <h2 class="text-sm font-semibold text-indigo-400 mb-4 uppercase tracking-wide">Step 742 — Float & Morph</h2>
      <div class="flex items-center justify-around">
        <div class="float w-16 h-16 bg-indigo-500 rounded-2xl flex items-center justify-center text-2xl shadow-lg shadow-indigo-500/30">🚀</div>
        <div class="float w-16 h-16 bg-emerald-500 rounded-full flex items-center justify-center text-2xl shadow-lg shadow-emerald-500/30" style="animation-delay:0.5s">💎</div>
        <div class="morph-shape w-16 h-16 bg-gradient-to-br from-pink-500 to-indigo-500 flex items-center justify-center text-2xl shadow-lg">⭐</div>
        <div class="float w-16 h-16 bg-yellow-500 rounded-2xl flex items-center justify-center text-2xl shadow-lg shadow-yellow-500/30" style="animation-delay:1s">🌈</div>
      </div>
    </section>

    <!-- Shimmer Loading -->
    <section class="bg-gray-900 rounded-3xl p-8 mb-6 border border-gray-800">
      <h2 class="text-sm font-semibold text-indigo-400 mb-4 uppercase tracking-wide">Step 743 — Shimmer Skeleton</h2>
      <div class="flex items-start gap-3 p-4 bg-gray-800 rounded-2xl">
        <div class="shimmer-bg w-10 h-10 rounded-full flex-shrink-0"></div>
        <div class="flex-1 space-y-2.5 pt-1">
          <div class="shimmer-bg h-3 rounded-full w-1/2"></div>
          <div class="shimmer-bg h-2.5 rounded-full w-3/4"></div>
          <div class="shimmer-bg h-2.5 rounded-full w-2/3"></div>
        </div>
      </div>
      <button id="loadBtn" onclick="toggleSkeleton()" class="mt-4 text-xs text-indigo-400 hover:text-indigo-300 border border-indigo-800 px-3 py-1.5 rounded-lg transition-colors">Toggle loaded</button>
    </section>

    <!-- Ripple Button -->
    <section class="bg-gray-900 rounded-3xl p-8 mb-6 border border-gray-800">
      <h2 class="text-sm font-semibold text-indigo-400 mb-4 uppercase tracking-wide">Step 744 — Ripple + Elastic</h2>
      <div class="flex flex-wrap gap-4">
        <button class="ripple-btn bg-indigo-600 text-white px-5 py-2.5 rounded-xl font-semibold hover:bg-indigo-700 transition-colors" onclick="createRipple(event)">Ripple Click</button>
        <button class="bg-emerald-600 text-white px-5 py-2.5 rounded-xl font-semibold hover:bg-emerald-700 transition-colors" style="transition:transform 0.1s" onmousedown="this.style.transform='scale(0.94)'" onmouseup="this.style.transform='scale(1)'" onmouseleave="this.style.transform='scale(1)'">Press Scale</button>
        <button class="bg-pink-600 text-white px-5 py-2.5 rounded-xl font-semibold" onclick="this.style.animation='elastic 0.5s ease'; setTimeout(()=>this.style.animation='',600)">Elastic</button>
        <button class="bg-yellow-500 text-white px-5 py-2.5 rounded-xl font-semibold hover:rotate-3 hover:scale-105 transition-all duration-150">Tilt</button>
      </div>
    </section>

    <!-- Orbit -->
    <section class="bg-gray-900 rounded-3xl p-8 mb-6 border border-gray-800">
      <h2 class="text-sm font-semibold text-indigo-400 mb-4 uppercase tracking-wide">Step 745 — Orbit</h2>
      <div class="flex justify-center">
        <div class="relative w-32 h-32 flex items-center justify-center">
          <div class="w-10 h-10 bg-indigo-500 rounded-full flex items-center justify-center text-lg font-bold shadow-lg shadow-indigo-500/40">🌍</div>
          <!-- Orbit rings -->
          <div class="absolute inset-0 rounded-full border border-indigo-800 opacity-50"></div>
          <div class="absolute w-24 h-24 rounded-full border border-indigo-700 opacity-40"></div>
          <div class="absolute" style="top:50%;left:50%;margin-top:-4px;margin-left:-4px;">
            <div class="orbit-dot w-3 h-3 bg-yellow-400 rounded-full -mt-1 -ml-1" style="animation-duration:2s">🌕</div>
          </div>
          <div class="absolute" style="top:50%;left:50%;margin-top:-4px;margin-left:-4px;">
            <div class="orbit-dot w-2 h-2 bg-red-400 rounded-full" style="animation-duration:3.5s;animation-direction:reverse">🛸</div>
          </div>
        </div>
      </div>
    </section>

    <!-- Draw SVG -->
    <section class="bg-gray-900 rounded-3xl p-8 mb-6 border border-gray-800">
      <h2 class="text-sm font-semibold text-indigo-400 mb-4 uppercase tracking-wide">Step 746 — SVG Draw</h2>
      <div class="flex justify-center">
        <svg width="200" height="100" viewBox="0 0 200 100">
          <path d="M10,80 L50,30 L100,60 L150,20 L190,50" fill="none" stroke="#6366f1" stroke-width="3" stroke-linecap="round" stroke-linejoin="round" class="draw-path" id="svgPath"/>
          <circle r="4" fill="#6366f1" id="endDot" cx="190" cy="50"/>
        </svg>
      </div>
      <button onclick="redrawSVG()" class="block mx-auto mt-3 text-xs text-indigo-400 hover:text-indigo-300 border border-indigo-800 px-3 py-1.5 rounded-lg transition-colors">↺ วาดใหม่</button>
    </section>

    <!-- Stagger reveal -->
    <section class="bg-gray-900 rounded-3xl p-8 mb-6 border border-gray-800">
      <h2 class="text-sm font-semibold text-indigo-400 mb-4 uppercase tracking-wide">Step 747 — Stagger</h2>
      <div class="stagger grid grid-cols-3 gap-3" id="staggerGrid">
        <div class="bg-gray-800 rounded-xl p-3 text-center">
          <div class="text-2xl mb-1">🎨</div>
          <p class="text-xs text-gray-300">Design</p>
        </div>
        <div class="bg-gray-800 rounded-xl p-3 text-center">
          <div class="text-2xl mb-1">💻</div>
          <p class="text-xs text-gray-300">Code</p>
        </div>
        <div class="bg-gray-800 rounded-xl p-3 text-center">
          <div class="text-2xl mb-1">🚀</div>
          <p class="text-xs text-gray-300">Deploy</p>
        </div>
        <div class="bg-gray-800 rounded-xl p-3 text-center">
          <div class="text-2xl mb-1">📊</div>
          <p class="text-xs text-gray-300">Analytics</p>
        </div>
        <div class="bg-gray-800 rounded-xl p-3 text-center">
          <div class="text-2xl mb-1">🔒</div>
          <p class="text-xs text-gray-300">Security</p>
        </div>
        <div class="bg-gray-800 rounded-xl p-3 text-center">
          <div class="text-2xl mb-1">⚡</div>
          <p class="text-xs text-gray-300">Speed</p>
        </div>
      </div>
      <button onclick="triggerStagger()" class="mt-4 text-xs text-indigo-400 hover:text-indigo-300 border border-indigo-800 px-3 py-1.5 rounded-lg transition-colors">▶ เล่น Stagger</button>
    </section>

    <!-- Scroll reveal (Steps 748-750) -->
    <section class="bg-gray-900 rounded-3xl p-8 mb-6 border border-gray-800">
      <h2 class="text-sm font-semibold text-indigo-400 mb-6 uppercase tracking-wide">Steps 748–750 — Scroll Reveal + Counter + Pulse Ring</h2>
      <div class="space-y-4">
        <div class="reveal bg-gray-800 rounded-2xl p-5 flex items-center gap-4">
          <div class="relative w-14 h-14 flex-shrink-0">
            <div class="w-14 h-14 bg-indigo-500 rounded-2xl flex items-center justify-center text-2xl">📈</div>
            <div class="absolute inset-0 bg-indigo-500 rounded-2xl" style="animation:pulse-ring 2s infinite"></div>
          </div>
          <div>
            <p class="text-xs text-gray-400">Revenue</p>
            <p class="text-2xl font-extrabold text-white" id="counter1">0</p>
          </div>
        </div>
        <div class="reveal bg-gray-800 rounded-2xl p-5 flex items-center gap-4" style="transition-delay:0.1s">
          <div class="relative w-14 h-14 flex-shrink-0">
            <div class="w-14 h-14 bg-emerald-500 rounded-2xl flex items-center justify-center text-2xl">👥</div>
          </div>
          <div>
            <p class="text-xs text-gray-400">Users</p>
            <p class="text-2xl font-extrabold text-white" id="counter2">0</p>
          </div>
        </div>
        <div class="reveal bg-gray-800 rounded-2xl p-5 flex items-center gap-4" style="transition-delay:0.2s">
          <div class="w-14 h-14 bg-yellow-500 rounded-2xl flex items-center justify-center text-2xl flex-shrink-0">⭐</div>
          <div>
            <p class="text-xs text-gray-400">Rating</p>
            <p class="text-2xl font-extrabold text-white" id="counter3">0</p>
          </div>
        </div>
      </div>
    </section>

  </div>

  <script>
    // Ripple
    function createRipple(e) {
      const btn = e.currentTarget;
      const rect = btn.getBoundingClientRect();
      const size = Math.max(rect.width, rect.height);
      const x = e.clientX - rect.left - size/2;
      const y = e.clientY - rect.top - size/2;
      const ripple = Object.assign(document.createElement('span'), {
        className: 'ripple-effect',
        style: `width:${size}px;height:${size}px;left:${x}px;top:${y}px;`
      });
      btn.appendChild(ripple);
      setTimeout(() => ripple.remove(), 700);
    }

    // Typewriter restart
    function restartTypewriter() {
      const el = document.getElementById('tw');
      el.style.animation = 'none';
      el.offsetHeight; // reflow
      el.style.animation = '';
    }

    // Skeleton toggle
    let isLoaded = false;
    function toggleSkeleton() {
      isLoaded = !isLoaded;
      const btn = document.querySelector('#loadBtn').previousElementSibling;
      if (isLoaded) {
        btn.innerHTML = `
          <div class="flex items-start gap-3">
            <div class="w-10 h-10 rounded-full bg-indigo-600 flex items-center justify-center text-sm font-bold flex-shrink-0 bounce-in">สม</div>
            <div class="flex-1 bounce-in">
              <p class="font-semibold text-white text-sm">สมชาย วงค์</p>
              <p class="text-gray-400 text-xs mt-0.5">Full Stack Developer</p>
              <p class="text-gray-500 text-xs mt-1">ยินดีที่ได้รู้จัก!</p>
            </div>
          </div>`;
      } else {
        btn.innerHTML = `
          <div class="flex items-start gap-3 p-4 bg-gray-800 rounded-2xl">
            <div class="shimmer-bg w-10 h-10 rounded-full flex-shrink-0"></div>
            <div class="flex-1 space-y-2.5 pt-1">
              <div class="shimmer-bg h-3 rounded-full w-1/2"></div>
              <div class="shimmer-bg h-2.5 rounded-full w-3/4"></div>
              <div class="shimmer-bg h-2.5 rounded-full w-2/3"></div>
            </div>
          </div>`;
      }
    }

    // SVG redraw
    function redrawSVG() {
      const path = document.getElementById('svgPath');
      path.style.animation = 'none';
      path.offsetHeight;
      path.style.animation = '';
    }

    // Stagger
    function triggerStagger() {
      const grid = document.getElementById('staggerGrid');
      grid.classList.remove('go');
      [...grid.children].forEach(el => { el.style.animation = 'none'; el.style.opacity = '0'; });
      requestAnimationFrame(() => requestAnimationFrame(() => grid.classList.add('go')));
    }

    // Scroll reveal + counter
    const counters = [
      { id: 'counter1', target: 284560, prefix: '฿', suffix: '' },
      { id: 'counter2', target: 12847, prefix: '', suffix: '' },
      { id: 'counter3', target: 4.9, prefix: '', suffix: '/5', dec: 1 },
    ];

    function animateCounter(id, target, prefix, suffix, dec=0) {
      const el = document.getElementById(id);
      let start = 0;
      const step = target / 60;
      const timer = setInterval(() => {
        start += step;
        if (start >= target) { start = target; clearInterval(timer); }
        el.textContent = prefix + (dec ? start.toFixed(dec) : Math.floor(start).toLocaleString()) + suffix;
      }, 16);
    }

    // Intersection Observer for reveal
    const io = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          entry.target.classList.add('visible');
          // Trigger counters when first stat is visible
          if (entry.target.querySelector('#counter1')) {
            counters.forEach(c => animateCounter(c.id, c.target, c.prefix, c.suffix, c.dec));
          }
        }
      });
    }, { threshold: 0.2 });

    document.querySelectorAll('.reveal').forEach(el => io.observe(el));

    // Auto stagger on load
    setTimeout(triggerStagger, 500);
  </script>
</body>
</html>
```

---

## สรุป Part 75

| Step | เนื้อหา |
|------|---------|
| 741 | Typewriter effect + cursor blink |
| 742 | Float animation + morph blob shape |
| 743 | Shimmer skeleton loading |
| 744 | Ripple click + elastic press + tilt hover |
| 745 | Orbit — planet/moon rotation |
| 746 | SVG draw path animation (stroke-dashoffset) |
| 747 | Stagger grid reveal (animation-delay per nth-child) |
| 748 | Scroll reveal ด้วย IntersectionObserver |
| 749 | Animated counter (requestAnimationFrame) |
| 750 | Workshop: Full animation showcase — dark bg, all effects combined |

**Part ถัดไป:** Part 76 — Empty States & Error Pages (Steps 751–760)
