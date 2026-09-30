# Part 34: Advanced Animation & Microinteractions
## Steps 331–340: Animation ระดับมืออาชีพ

---

## 🎯 เป้าหมายของ Part นี้

- CSS keyframe animations ขั้นสูง
- Stagger animations
- Scroll-triggered animations (Intersection Observer)
- FLIP animations
- Page transitions
- Reduced motion

---

## Step 331: Custom Keyframes in Tailwind Config

```javascript
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      keyframes: {
        // Entrance animations
        'fade-in':       { from: { opacity: '0' }, to: { opacity: '1' } },
        'fade-in-up':    { from: { opacity: '0', transform: 'translateY(20px)' }, to: { opacity: '1', transform: 'translateY(0)' } },
        'fade-in-down':  { from: { opacity: '0', transform: 'translateY(-20px)' }, to: { opacity: '1', transform: 'translateY(0)' } },
        'fade-in-left':  { from: { opacity: '0', transform: 'translateX(20px)' }, to: { opacity: '1', transform: 'translateX(0)' } },
        'fade-in-right': { from: { opacity: '0', transform: 'translateX(-20px)' }, to: { opacity: '1', transform: 'translateX(0)' } },
        'scale-in':      { from: { opacity: '0', transform: 'scale(0.9)' }, to: { opacity: '1', transform: 'scale(1)' } },
        'scale-in-center': { from: { opacity: '0', transform: 'scale(0.8)' }, to: { opacity: '1', transform: 'scale(1)' } },
        
        // Attention animations
        'shake': {
          '0%,100%': { transform: 'translateX(0)' },
          '10%,50%,90%': { transform: 'translateX(-4px)' },
          '30%,70%': { transform: 'translateX(4px)' },
        },
        'pulse-ring': {
          '0%': { transform: 'scale(1)', opacity: '0.8' },
          '100%': { transform: 'scale(1.8)', opacity: '0' },
        },
        'float': {
          '0%,100%': { transform: 'translateY(0px)' },
          '50%': { transform: 'translateY(-12px)' },
        },
        'wiggle': {
          '0%,100%': { transform: 'rotate(-3deg)' },
          '50%': { transform: 'rotate(3deg)' },
        },
        
        // Morphing
        'gradient-x': {
          '0%,100%': { 'background-position': '0% 50%' },
          '50%': { 'background-position': '100% 50%' },
        },
        'typewriter': {
          from: { width: '0' },
          to:   { width: '100%' },
        },
        'blink': {
          '0%,100%': { opacity: '1' },
          '50%': { opacity: '0' },
        },
      },
      animation: {
        'fade-in':        'fade-in 0.5s ease-out',
        'fade-in-up':     'fade-in-up 0.5s ease-out',
        'fade-in-down':   'fade-in-down 0.5s ease-out',
        'fade-in-left':   'fade-in-left 0.5s ease-out',
        'fade-in-right':  'fade-in-right 0.5s ease-out',
        'scale-in':       'scale-in 0.3s ease-out',
        'shake':          'shake 0.6s ease-in-out',
        'float':          'float 3s ease-in-out infinite',
        'wiggle':         'wiggle 0.5s ease-in-out infinite',
        'pulse-ring':     'pulse-ring 1.5s ease-out infinite',
        'gradient-x':     'gradient-x 3s ease infinite',
        'typewriter':     'typewriter 2s steps(20) forwards',
        'blink':          'blink 1s ease-in-out infinite',
        // Stagger delays
        'delay-75':       'fade-in-up 0.5s ease-out 75ms both',
        'delay-150':      'fade-in-up 0.5s ease-out 150ms both',
        'delay-300':      'fade-in-up 0.5s ease-out 300ms both',
        'delay-500':      'fade-in-up 0.5s ease-out 500ms both',
      },
    }
  }
}
```

---

## Step 332: Stagger Animations

```html
<script src="https://cdn.tailwindcss.com"></script>
<script>
tailwind.config = {
  theme: {
    extend: {
      keyframes: {
        'fade-in-up': { from:{ opacity:'0', transform:'translateY(20px)' }, to:{ opacity:'1', transform:'translateY(0)' } },
      },
      animation: {
        'fade-in-up': 'fade-in-up 0.5s ease-out both',
      }
    }
  }
}
</script>

<!-- Stagger with animation-delay -->
<div class="grid grid-cols-3 gap-4 p-6">
  <div class="bg-indigo-100 rounded-2xl p-5 animate-[fade-in-up_0.5s_ease-out_0ms_both] text-center">
    <div class="text-3xl mb-2">🎨</div>
    <p class="font-bold text-sm">Card 1</p>
  </div>
  <div class="bg-purple-100 rounded-2xl p-5 animate-[fade-in-up_0.5s_ease-out_100ms_both] text-center">
    <div class="text-3xl mb-2">⚡</div>
    <p class="font-bold text-sm">Card 2</p>
  </div>
  <div class="bg-pink-100 rounded-2xl p-5 animate-[fade-in-up_0.5s_ease-out_200ms_both] text-center">
    <div class="text-3xl mb-2">🚀</div>
    <p class="font-bold text-sm">Card 3</p>
  </div>
</div>

<!-- JS-driven stagger -->
<div id="stagger-grid" class="grid grid-cols-4 gap-3 p-6">
  <!-- items inserted by JS -->
</div>

<script>
const items = ['🎨','⚡','🚀','🔒','📊','🛠️','🌐','💎'];
const grid = document.getElementById('stagger-grid');
items.forEach((icon, i) => {
  const div = document.createElement('div');
  div.className = 'bg-white rounded-xl border border-gray-200 p-4 text-center opacity-0 translate-y-4 transition-all duration-500';
  div.innerHTML = `<div class="text-2xl">${icon}</div>`;
  div.style.transitionDelay = `${i * 80}ms`;
  grid.appendChild(div);
  
  requestAnimationFrame(() => {
    requestAnimationFrame(() => {
      div.classList.remove('opacity-0', 'translate-y-4');
    });
  });
});
</script>
```

---

## Step 333: Scroll-triggered Animations

```html
<!-- Cards animate in as they enter viewport -->
<style>
  .scroll-reveal {
    opacity: 0;
    transform: translateY(30px);
    transition: opacity 0.6s ease, transform 0.6s ease;
  }
  .scroll-reveal.revealed {
    opacity: 1;
    transform: translateY(0);
  }
</style>

<div class="space-y-6 py-20 px-6 max-w-2xl mx-auto">
  <div class="scroll-reveal bg-white rounded-2xl border border-gray-200 p-6">Card 1 — slides up on scroll</div>
  <div class="scroll-reveal bg-white rounded-2xl border border-gray-200 p-6" style="transition-delay: 0.1s">Card 2 — with delay</div>
  <div class="scroll-reveal bg-white rounded-2xl border border-gray-200 p-6" style="transition-delay: 0.2s">Card 3</div>
  <div class="scroll-reveal bg-white rounded-2xl border border-gray-200 p-6">Card 4</div>
  <div class="scroll-reveal bg-white rounded-2xl border border-gray-200 p-6">Card 5</div>
</div>

<script>
const revealObserver = new IntersectionObserver(
  (entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        entry.target.classList.add('revealed');
        revealObserver.unobserve(entry.target); // Reveal once only
      }
    });
  },
  { 
    threshold: 0.15,
    rootMargin: '0px 0px -50px 0px',
  }
);

document.querySelectorAll('.scroll-reveal').forEach(el => revealObserver.observe(el));
</script>
```

---

## Step 334: Pulse Ring / Status Indicators

```html
<!-- Live indicator with pulse ring -->
<div class="flex items-center gap-2 p-4">
  <!-- Animated pulse dot -->
  <div class="relative flex">
    <span class="absolute inline-flex size-3 rounded-full bg-green-400 opacity-75 animate-ping"></span>
    <span class="relative inline-flex size-3 rounded-full bg-green-500"></span>
  </div>
  <span class="text-sm text-gray-700 font-medium">Live — 247 คนกำลังเรียน</span>
</div>

<!-- Full pulse ring around element -->
<div class="relative inline-flex items-center justify-center">
  <div class="absolute size-16 rounded-full bg-indigo-400 animate-ping opacity-30"></div>
  <div class="absolute size-12 rounded-full bg-indigo-400 animate-ping opacity-50" style="animation-delay: 0.5s"></div>
  <div class="relative size-10 rounded-full bg-indigo-600 flex items-center justify-center text-white text-sm font-bold">▶</div>
</div>
```

---

## Step 335: Typewriter Effect

```html
<style>
  @keyframes typewriter { from { width: 0 } to { width: 100% } }
  @keyframes blink { 0%,100%{border-color:transparent} 50%{border-color:currentColor} }
  
  .typewriter {
    display: inline-block;
    overflow: hidden;
    white-space: nowrap;
    border-right: 2px solid;
    animation: typewriter 2s steps(20) forwards, blink 0.8s step-end infinite;
  }
</style>

<div class="p-6 bg-gray-900 text-white">
  <p class="text-3xl font-black">
    <span class="typewriter text-indigo-400">สวัสดีชาว Tailwind!</span>
  </p>
</div>

<!-- Multiple lines typewriter -->
<div id="multi-type" class="p-6 font-mono text-green-400 bg-gray-950 text-sm"></div>
<script>
const lines = [
  '> npm install tailwindcss',
  '> npx tailwindcss init',
  '> Ready! Start building 🚀',
];
let currentLine = 0, currentChar = 0;
const el = document.getElementById('multi-type');
let lineEl;

function typeLine() {
  if (!lineEl || currentChar === 0) {
    lineEl = document.createElement('p');
    lineEl.className = 'leading-relaxed';
    el.appendChild(lineEl);
  }
  
  if (currentChar < lines[currentLine].length) {
    lineEl.textContent += lines[currentLine][currentChar];
    currentChar++;
    setTimeout(typeLine, 60);
  } else {
    currentLine++;
    currentChar = 0;
    if (currentLine < lines.length) setTimeout(typeLine, 400);
  }
}
setTimeout(typeLine, 500);
</script>
```

---

## Step 336: Reduced Motion

```html
<!-- @media prefers-reduced-motion — always handle this! -->
<style>
  /* Default: animate */
  .hero-blob {
    animation: float 4s ease-in-out infinite;
  }
  
  /* Reduced motion: disable or simplify */
  @media (prefers-reduced-motion: reduce) {
    .hero-blob { animation: none; }
    *, ::before, ::after {
      animation-duration: 0.01ms !important;
      animation-iteration-count: 1 !important;
      transition-duration: 0.01ms !important;
    }
  }
</style>

<!-- Tailwind: motion-safe / motion-reduce -->
<div class="motion-safe:animate-bounce motion-reduce:animate-none">
  🎈 Bounces only when motion is safe
</div>

<button class="motion-safe:hover:scale-105 motion-reduce:hover:opacity-90 transition-all duration-200 px-5 py-2.5 bg-indigo-600 text-white rounded-xl">
  Hover Me (accessible animation)
</button>
```

---

## Step 337–340: Workshop — Microinteraction Showcase

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Animation Workshop</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    @keyframes fade-up   { from{opacity:0;transform:translateY(24px)} to{opacity:1;transform:translateY(0)} }
    @keyframes scale-in  { from{opacity:0;transform:scale(0.9)} to{opacity:1;transform:scale(1)} }
    @keyframes float     { 0%,100%{transform:translateY(0)} 50%{transform:translateY(-10px)} }
    @keyframes slide-r   { from{opacity:0;transform:translateX(-20px)} to{opacity:1;transform:translateX(0)} }
    @keyframes gradient-bg { 0%,100%{background-position:0% 50%} 50%{background-position:100% 50%} }
    @keyframes spin-slow { from{transform:rotate(0)} to{transform:rotate(360deg)} }
    .anim-fade-up  { animation: fade-up  0.6s ease-out both; }
    .anim-scale-in { animation: scale-in 0.4s ease-out both; }
    .anim-float    { animation: float    3s  ease-in-out infinite; }
    .anim-slide-r  { animation: slide-r  0.5s ease-out both; }
    .anim-grad     { animation: gradient-bg 5s ease infinite; background-size:300% 300%; }
    .anim-spin     { animation: spin-slow 8s linear infinite; }
    
    .scroll-reveal { opacity:0; transform:translateY(28px); transition:opacity .5s ease, transform .5s ease; }
    .scroll-reveal.in { opacity:1; transform:none; }
    
    @media(prefers-reduced-motion:reduce) {
      *, *::before, *::after { animation-duration:.01ms!important; transition-duration:.01ms!important; }
    }
  </style>
</head>
<body class="bg-gray-50 overflow-x-hidden">

  <!-- Hero animated -->
  <section class="relative bg-gray-950 text-white py-24 px-6 overflow-hidden">
    <!-- Background blobs -->
    <div class="absolute top-0 left-0 size-96 rounded-full bg-indigo-600/30 blur-3xl anim-float" style="animation-delay:0s"></div>
    <div class="absolute bottom-0 right-0 size-80 rounded-full bg-purple-600/30 blur-3xl anim-float" style="animation-delay:1.5s"></div>
    
    <div class="relative text-center max-w-3xl mx-auto">
      <p class="text-indigo-400 text-sm font-bold uppercase tracking-widest mb-4 anim-fade-up" style="animation-delay:.1s">Advanced Animations</p>
      <h1 class="text-5xl sm:text-7xl font-black leading-tight mb-6 anim-fade-up" style="animation-delay:.2s">
        <span class="anim-grad bg-gradient-to-r from-indigo-400 via-purple-400 to-pink-400 bg-clip-text text-transparent">Micro</span><br>Interactions
      </h1>
      <p class="text-gray-400 text-lg anim-fade-up" style="animation-delay:.3s">UI ที่มีชีวิตชีวาด้วย animation</p>
      
      <div class="flex items-center justify-center gap-4 mt-8 anim-fade-up" style="animation-delay:.4s">
        <button class="px-6 py-3 bg-indigo-600 text-white font-bold rounded-2xl hover:bg-indigo-700 hover:scale-105 active:scale-95 transition-all duration-200 shadow-lg shadow-indigo-500/30">
          เริ่มเรียน →
        </button>
        <button class="px-6 py-3 border border-white/20 text-white font-bold rounded-2xl hover:bg-white/10 transition-all duration-200 backdrop-blur-sm">
          ดูตัวอย่าง
        </button>
      </div>
    </div>
  </section>

  <!-- Animation examples -->
  <section class="max-w-5xl mx-auto py-16 px-6 space-y-16">
    
    <!-- Entrance animations -->
    <div>
      <h2 class="text-2xl font-bold text-gray-900 mb-6">Entrance Animations</h2>
      <div class="grid grid-cols-2 sm:grid-cols-4 gap-4">
        <div class="bg-white rounded-2xl border border-gray-200 p-5 text-center anim-fade-up" style="animation-delay:0ms">
          <p class="text-2xl mb-2">⬆️</p><p class="text-xs font-bold text-gray-700">Fade Up</p>
        </div>
        <div class="bg-white rounded-2xl border border-gray-200 p-5 text-center anim-scale-in" style="animation-delay:100ms">
          <p class="text-2xl mb-2">🔍</p><p class="text-xs font-bold text-gray-700">Scale In</p>
        </div>
        <div class="bg-white rounded-2xl border border-gray-200 p-5 text-center anim-slide-r" style="animation-delay:200ms">
          <p class="text-2xl mb-2">➡️</p><p class="text-xs font-bold text-gray-700">Slide Right</p>
        </div>
        <div class="bg-white rounded-2xl border border-gray-200 p-5 text-center anim-fade-up" style="animation-delay:300ms">
          <p class="text-2xl mb-2">✨</p><p class="text-xs font-bold text-gray-700">Fade + Delay</p>
        </div>
      </div>
    </div>

    <!-- Hover microinteractions -->
    <div>
      <h2 class="text-2xl font-bold text-gray-900 mb-6">Hover Microinteractions</h2>
      <div class="grid grid-cols-2 sm:grid-cols-4 gap-4">
        <button class="bg-white rounded-2xl border border-gray-200 p-5 text-center hover:scale-105 hover:shadow-xl hover:shadow-indigo-500/10 transition-all duration-300 cursor-pointer">
          <p class="text-2xl mb-2">🚀</p><p class="text-xs font-bold text-gray-700">Scale Up</p>
        </button>
        <button class="bg-white rounded-2xl border border-gray-200 p-5 text-center hover:-translate-y-2 hover:shadow-lg transition-all duration-300 cursor-pointer">
          <p class="text-2xl mb-2">⬆️</p><p class="text-xs font-bold text-gray-700">Lift Up</p>
        </button>
        <button class="bg-indigo-600 rounded-2xl p-5 text-center text-white hover:bg-indigo-700 hover:rotate-1 transition-all duration-300 cursor-pointer group">
          <p class="text-2xl mb-2 group-hover:scale-110 transition-transform">⚡</p><p class="text-xs font-bold">Rotate</p>
        </button>
        <button class="relative bg-white rounded-2xl border border-gray-200 p-5 text-center overflow-hidden cursor-pointer group">
          <div class="absolute inset-0 bg-gradient-to-r from-indigo-500 to-purple-500 translate-y-full group-hover:translate-y-0 transition-transform duration-300 rounded-2xl"></div>
          <div class="relative z-10"><p class="text-2xl mb-2 group-hover:text-white transition-colors">🎨</p><p class="text-xs font-bold text-gray-700 group-hover:text-white transition-colors">Fill</p></div>
        </button>
      </div>
    </div>

    <!-- Scroll reveal -->
    <div>
      <h2 class="text-2xl font-bold text-gray-900 mb-6">Scroll-triggered Animations</h2>
      <div class="space-y-4">
        <div class="scroll-reveal bg-white rounded-2xl border border-gray-200 p-5 flex items-center gap-4">
          <div class="size-12 rounded-xl bg-indigo-100 flex items-center justify-center text-2xl flex-none">🎓</div>
          <div><p class="font-bold text-gray-900">เรียนรู้ Tailwind CSS</p><p class="text-sm text-gray-400">Scroll down เพื่อดู animation</p></div>
        </div>
        <div class="scroll-reveal bg-white rounded-2xl border border-gray-200 p-5 flex items-center gap-4" style="transition-delay:.1s">
          <div class="size-12 rounded-xl bg-purple-100 flex items-center justify-center text-2xl flex-none">⚡</div>
          <div><p class="font-bold text-gray-900">Build Fast UIs</p><p class="text-sm text-gray-400">Utility-first approach</p></div>
        </div>
        <div class="scroll-reveal bg-white rounded-2xl border border-gray-200 p-5 flex items-center gap-4" style="transition-delay:.2s">
          <div class="size-12 rounded-xl bg-green-100 flex items-center justify-center text-2xl flex-none">🌐</div>
          <div><p class="font-bold text-gray-900">Fully Responsive</p><p class="text-sm text-gray-400">Mobile-first design</p></div>
        </div>
      </div>
    </div>
    
  </section>

<script>
const obs = new IntersectionObserver(entries => {
  entries.forEach(e => { if(e.isIntersecting) { e.target.classList.add('in'); obs.unobserve(e.target); } });
}, { threshold: 0.15 });
document.querySelectorAll('.scroll-reveal').forEach(el => obs.observe(el));
</script>

</body>
</html>
```

---

## 📝 สรุป Part 34

| Concept | เทคนิค |
|---------|---------|
| Custom keyframes | `theme.extend.keyframes` |
| Stagger | `animation-delay` increments |
| Scroll trigger | `IntersectionObserver` |
| Pulse ring | `animate-ping` |
| Reduced motion | `motion-safe:` / `motion-reduce:` |
| Typewriter | `steps()` timing function |

---

*Part 34 — Advanced Animation | Steps 331–340 จาก 1,000 Steps*
