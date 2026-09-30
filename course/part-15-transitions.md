# Part 15: Transition และ Animation พื้นฐาน
## Steps 141–150: Motion Design

---

## 🎯 เป้าหมายของ Part นี้

- Transition utilities ทั้งหมด
- Transform utilities ครบ
- Keyframe animations
- Animate on hover/state
- Performance best practices

---

## Step 141: Transition Basics

```html
<!-- transition เปิดใช้ transition บน all properties -->
<button class="bg-blue-500 hover:bg-blue-700 text-white px-4 py-2 rounded-lg transition-all duration-300">
  All properties transition
</button>

<!-- transition-colors เฉพาะ color properties -->
<button class="bg-blue-500 hover:bg-blue-700 text-white px-4 py-2 rounded-lg transition-colors">
  Color transition only
</button>

<!-- transition-transform เฉพาะ transform -->
<div class="hover:scale-110 transition-transform duration-200 cursor-pointer">
  Scale on hover
</div>

<!-- transition-opacity -->
<div class="opacity-50 hover:opacity-100 transition-opacity duration-200">
  Fade in on hover
</div>

<!-- transition-shadow -->
<div class="shadow hover:shadow-xl transition-shadow duration-300 p-4 bg-white rounded-xl">
  Shadow grows
</div>
```

### Transition Properties

```
transition         → all properties
transition-colors  → color, background-color, border-color, text-decoration-color, fill, stroke
transition-opacity → opacity
transition-shadow  → box-shadow
transition-transform → transform
transition-none    → no transition
```

---

## Step 142: Duration and Easing

```html
<!-- Duration -->
<div class="hover:scale-110 transition duration-75">75ms (very fast)</div>
<div class="hover:scale-110 transition duration-100">100ms</div>
<div class="hover:scale-110 transition duration-150">150ms (default)</div>
<div class="hover:scale-110 transition duration-200">200ms</div>
<div class="hover:scale-110 transition duration-300">300ms</div>
<div class="hover:scale-110 transition duration-500">500ms</div>
<div class="hover:scale-110 transition duration-700">700ms</div>
<div class="hover:scale-110 transition duration-1000">1s</div>

<!-- Easing (timing function) -->
<div class="hover:translate-x-10 transition ease-linear duration-300">Linear</div>
<div class="hover:translate-x-10 transition ease-in duration-300">Ease In (เริ่มช้า)</div>
<div class="hover:translate-x-10 transition ease-out duration-300">Ease Out (ช้าตอนจบ)</div>
<div class="hover:translate-x-10 transition ease-in-out duration-300">Ease In-Out (ช้าทั้งคู่)</div>

<!-- Delay -->
<div class="hover:scale-110 transition duration-300 delay-100">Delay 100ms</div>
<div class="hover:scale-110 transition duration-300 delay-200">Delay 200ms</div>
<div class="hover:scale-110 transition duration-300 delay-500">Delay 500ms</div>
```

---

## Step 143: Transform — Scale

```html
<!-- Scale up -->
<div class="hover:scale-110 transition-transform duration-200 cursor-pointer">
  Scale 110%
</div>

<!-- Scale down -->
<button class="active:scale-95 transition-transform">Press me</button>

<!-- Scale X or Y -->
<div class="hover:scale-x-125 transition-transform">Scale X only</div>
<div class="hover:scale-y-110 transition-transform">Scale Y only</div>

<!-- Scale values -->
<!-- scale-0, scale-50, scale-75, scale-90, scale-95, scale-100, scale-105, scale-110, scale-125, scale-150 -->

<!-- Scale with origin -->
<div class="hover:scale-125 origin-top-left transition-transform">From top-left</div>
<div class="hover:scale-125 origin-bottom-right transition-transform">From bottom-right</div>
<div class="hover:scale-125 origin-center transition-transform">From center (default)</div>
```

---

## Step 144: Transform — Rotate

```html
<!-- Rotate -->
<div class="hover:rotate-12 transition-transform duration-300 cursor-pointer">
  Rotate 12°
</div>
<div class="hover:-rotate-12 transition-transform duration-300 cursor-pointer">
  Rotate -12°
</div>
<div class="hover:rotate-45 transition-transform duration-300">45°</div>
<div class="hover:rotate-90 transition-transform duration-300">90°</div>
<div class="hover:rotate-180 transition-transform duration-300">180°</div>

<!-- Arrow icon rotate -->
<div class="flex items-center gap-2 cursor-pointer group">
  <span>Toggle</span>
  <svg class="size-4 group-hover:rotate-180 transition-transform duration-300" fill="none" viewBox="0 0 24 24" stroke="currentColor">
    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"/>
  </svg>
</div>

<!-- Spin icon (เมื่อ loading) -->
<div class="size-8 border-3 border-gray-200 border-t-indigo-600 rounded-full animate-spin"></div>
```

---

## Step 145: Transform — Translate

```html
<!-- Translate X and Y -->
<div class="hover:translate-x-2 transition-transform">Move right 8px</div>
<div class="hover:-translate-x-2 transition-transform">Move left 8px</div>
<div class="hover:translate-y-2 transition-transform">Move down 8px</div>
<div class="hover:-translate-y-2 transition-transform">Move up 8px</div>

<!-- translate-1/2 = 50% of size -->
<div class="absolute top-1/2 -translate-y-1/2 left-1/2 -translate-x-1/2">
  Perfect center
</div>

<!-- Slide in from bottom on hover -->
<div class="overflow-hidden relative h-32 group cursor-pointer bg-gray-100 rounded-xl">
  <div class="absolute inset-x-0 bottom-0 bg-indigo-600 text-white p-3 translate-y-full group-hover:translate-y-0 transition-transform duration-300">
    <p class="text-sm font-medium">Hover info revealed!</p>
  </div>
  <div class="p-4">Card content</div>
</div>

<!-- Lift card on hover -->
<div class="bg-white rounded-xl p-6 shadow hover:-translate-y-1 hover:shadow-xl transition-all duration-300 cursor-pointer">
  Lift card effect
</div>
```

---

## Step 146: Transform — Skew

```html
<!-- Skew X and Y -->
<div class="hover:skew-x-3 transition-transform cursor-pointer">Skew X 3°</div>
<div class="hover:-skew-x-3 transition-transform cursor-pointer">Skew -X 3°</div>
<div class="hover:skew-y-3 transition-transform cursor-pointer">Skew Y 3°</div>

<!-- skew values: 0, 1, 2, 3, 6, 12 degrees -->

<!-- Creative skew badge -->
<span class="
  inline-block bg-indigo-600 text-white text-xs font-bold 
  px-3 py-1 -skew-x-6
">
  <span class="inline-block skew-x-6">NEW</span>
</span>
```

---

## Step 147: Built-in Animations

```html
<!-- animate-spin: loading spinner -->
<div class="
  size-8 rounded-full 
  border-4 border-gray-200 border-t-indigo-600 
  animate-spin
"></div>

<!-- animate-ping: notification badge -->
<div class="relative size-3">
  <div class="absolute inset-0 bg-red-400 rounded-full animate-ping opacity-75"></div>
  <div class="relative size-3 bg-red-500 rounded-full"></div>
</div>

<!-- animate-pulse: skeleton/loading -->
<div class="space-y-3">
  <div class="h-4 bg-gray-200 rounded animate-pulse"></div>
  <div class="h-4 bg-gray-200 rounded animate-pulse w-3/4"></div>
  <div class="h-4 bg-gray-200 rounded animate-pulse w-1/2"></div>
</div>

<!-- animate-bounce: scroll indicator -->
<div class="animate-bounce size-8 flex items-center justify-center text-gray-400">
  ↓
</div>

<!-- Pause animation -->
<div class="animate-spin hover:pause">Pausable spinner</div>
```

---

## Step 148: Custom Animations in Config

```javascript
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      keyframes: {
        'fade-in': {
          '0%': { opacity: '0', transform: 'translateY(10px)' },
          '100%': { opacity: '1', transform: 'translateY(0)' },
        },
        'slide-in-left': {
          '0%': { transform: 'translateX(-100%)' },
          '100%': { transform: 'translateX(0)' },
        },
        'slide-in-right': {
          '0%': { transform: 'translateX(100%)' },
          '100%': { transform: 'translateX(0)' },
        },
        'zoom-in': {
          '0%': { transform: 'scale(0.95)', opacity: '0' },
          '100%': { transform: 'scale(1)', opacity: '1' },
        },
        'shake': {
          '0%, 100%': { transform: 'translateX(0)' },
          '10%, 30%, 50%, 70%, 90%': { transform: 'translateX(-4px)' },
          '20%, 40%, 60%, 80%': { transform: 'translateX(4px)' },
        },
        'float': {
          '0%, 100%': { transform: 'translateY(0)' },
          '50%': { transform: 'translateY(-10px)' },
        },
      },
      animation: {
        'fade-in': 'fade-in 0.5s ease-out',
        'slide-in-left': 'slide-in-left 0.4s ease-out',
        'slide-in-right': 'slide-in-right 0.4s ease-out',
        'zoom-in': 'zoom-in 0.3s ease-out',
        'shake': 'shake 0.5s ease-in-out',
        'float': 'float 3s ease-in-out infinite',
      },
    },
  },
}
```

```html
<!-- ใช้ custom animations -->
<div class="animate-fade-in">Fade in on load</div>
<div class="animate-float">Floating element</div>
<button class="animate-shake">Shake button</button>
```

---

## Step 149: Animation Timing & Iteration

```javascript
// tailwind.config.js — extended duration, delay
module.exports = {
  theme: {
    extend: {
      animationDuration: {
        '2000': '2000ms',
        '3000': '3000ms',
        '5000': '5000ms',
      },
      animationDelay: {
        '100': '100ms',
        '200': '200ms',
        '300': '300ms',
        '400': '400ms',
        '500': '500ms',
      },
    },
  },
}
```

```html
<!-- Staggered animation (ใช้ inline style สำหรับ delay) -->
<div class="space-y-2">
  <div class="animate-fade-in" style="animation-delay: 0ms">Item 1</div>
  <div class="animate-fade-in" style="animation-delay: 100ms">Item 2</div>
  <div class="animate-fade-in" style="animation-delay: 200ms">Item 3</div>
</div>

<!-- animate-none: ปิด animation -->
<div class="animate-spin motion-reduce:animate-none">
  Respects user's reduce motion preference
</div>

<!-- motion-reduce: accessibility -->
<div class="hover:-translate-y-1 transition motion-reduce:hover:translate-y-0 motion-reduce:transition-none">
  No motion for reduce-motion users
</div>
```

---

## Step 150: Workshop — Animated Landing Hero

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Animation Workshop</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    @keyframes fadeInUp {
      from { opacity: 0; transform: translateY(30px); }
      to   { opacity: 1; transform: translateY(0); }
    }
    @keyframes float {
      0%, 100% { transform: translateY(0); }
      50% { transform: translateY(-15px); }
    }
    @keyframes pulse-ring {
      0% { transform: scale(0.8); opacity: 0.8; }
      100% { transform: scale(2); opacity: 0; }
    }
    @keyframes shimmer {
      0% { background-position: -200% 0; }
      100% { background-position: 200% 0; }
    }
    .animate-fade-up { animation: fadeInUp 0.6s ease-out forwards; }
    .animate-float   { animation: float 4s ease-in-out infinite; }
    .delay-100 { animation-delay: 100ms; }
    .delay-200 { animation-delay: 200ms; }
    .delay-300 { animation-delay: 300ms; }
    .delay-400 { animation-delay: 400ms; }
    .shimmer {
      background: linear-gradient(90deg, #f0f0f0 25%, #e0e0e0 50%, #f0f0f0 75%);
      background-size: 200% 100%;
      animation: shimmer 1.5s infinite;
    }
  </style>
</head>
<body class="bg-slate-900 text-white min-h-screen overflow-x-hidden">

  <!-- Animated background blobs -->
  <div class="fixed inset-0 pointer-events-none overflow-hidden">
    <div class="absolute top-1/4 -left-40 size-96 bg-indigo-600/20 rounded-full blur-3xl animate-float"></div>
    <div class="absolute bottom-1/4 -right-40 size-96 bg-purple-600/20 rounded-full blur-3xl animate-float delay-200"></div>
    <div class="absolute top-3/4 left-1/2 size-80 bg-cyan-600/15 rounded-full blur-3xl animate-float" style="animation-delay:1s"></div>
  </div>

  <!-- Navbar -->
  <nav class="relative z-10 border-b border-white/10 bg-white/5 backdrop-blur-md">
    <div class="max-w-7xl mx-auto px-6 py-4 flex items-center justify-between">
      <div class="font-black text-xl bg-gradient-to-r from-indigo-400 to-purple-400 bg-clip-text text-transparent">
        TailwindPro
      </div>
      <div class="hidden md:flex items-center gap-6 text-sm text-white/70">
        <a href="#" class="hover:text-white transition-colors">หลักสูตร</a>
        <a href="#" class="hover:text-white transition-colors">โปรเจกต์</a>
        <a href="#" class="hover:text-white transition-colors">บล็อก</a>
        <button class="bg-indigo-600 hover:bg-indigo-500 active:scale-95 text-white text-sm px-4 py-2 rounded-lg transition-all">
          เริ่มเรียน
        </button>
      </div>
    </div>
  </nav>

  <!-- Hero -->
  <main class="relative z-10 max-w-7xl mx-auto px-6 pt-24 pb-20">
    <div class="flex flex-col lg:flex-row items-center gap-16">
      
      <!-- Text -->
      <div class="flex-1 text-center lg:text-left">
        <div class="opacity-0 animate-fade-up">
          <span class="inline-flex items-center gap-2 text-sm bg-white/10 text-indigo-300 px-3 py-1.5 rounded-full border border-white/20 mb-6">
            <span class="relative flex h-2 w-2">
              <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-green-400 opacity-75"></span>
              <span class="relative inline-flex rounded-full h-2 w-2 bg-green-500"></span>
            </span>
            อัพเดตใหม่ — Tailwind CSS v4 มาแล้ว!
          </span>
        </div>
        
        <h1 class="text-5xl sm:text-6xl font-black leading-tight opacity-0 animate-fade-up delay-100">
          เรียน Tailwind CSS<br>
          <span class="bg-gradient-to-r from-indigo-400 via-purple-400 to-pink-400 bg-clip-text text-transparent">
            อย่างมืออาชีพ
          </span>
        </h1>
        
        <p class="mt-5 text-white/60 text-lg max-w-lg mx-auto lg:mx-0 leading-relaxed opacity-0 animate-fade-up delay-200">
          100+ บทเรียน • 1,000 Steps • Project จริงทุกบท
          ตั้งแต่พื้นฐานจนถึงระดับโลก
        </p>
        
        <div class="mt-8 flex flex-col sm:flex-row gap-3 justify-center lg:justify-start opacity-0 animate-fade-up delay-300">
          <button class="
            bg-indigo-600 hover:bg-indigo-500 
            active:scale-95
            text-white font-semibold px-6 py-3.5 rounded-xl 
            transition-all shadow-lg shadow-indigo-600/30 hover:shadow-indigo-500/40
          ">
            เริ่มเรียนฟรี →
          </button>
          <button class="
            bg-white/10 hover:bg-white/20 
            border border-white/20 hover:border-white/30
            text-white font-semibold px-6 py-3.5 rounded-xl 
            transition-all
          ">
            ดูหลักสูตร
          </button>
        </div>
        
        <!-- Stats -->
        <div class="mt-10 flex gap-8 justify-center lg:justify-start opacity-0 animate-fade-up delay-400">
          <div>
            <p class="text-2xl font-black">1,200+</p>
            <p class="text-white/50 text-sm">นักเรียน</p>
          </div>
          <div class="w-px bg-white/20"></div>
          <div>
            <p class="text-2xl font-black">100+</p>
            <p class="text-white/50 text-sm">บทเรียน</p>
          </div>
          <div class="w-px bg-white/20"></div>
          <div>
            <p class="text-2xl font-black">4.9★</p>
            <p class="text-white/50 text-sm">คะแนน</p>
          </div>
        </div>
      </div>
      
      <!-- Floating cards -->
      <div class="relative flex-none w-full max-w-xs lg:max-w-sm">
        <!-- Main card - floating -->
        <div class="
          bg-white/10 backdrop-blur-sm border border-white/20 
          rounded-2xl p-6 shadow-2xl animate-float
        ">
          <div class="flex items-center gap-3 mb-5">
            <div class="size-10 rounded-xl bg-gradient-to-br from-indigo-400 to-purple-500 flex items-center justify-center text-xl">🎨</div>
            <div>
              <h3 class="font-bold text-sm">Part 15: Animations</h3>
              <p class="text-white/50 text-xs">กำลังเรียน</p>
            </div>
          </div>
          
          <!-- Progress -->
          <div class="space-y-3">
            <div>
              <div class="flex justify-between text-xs text-white/60 mb-1">
                <span>Progress</span>
                <span>75%</span>
              </div>
              <div class="h-2 bg-white/10 rounded-full overflow-hidden">
                <div class="h-full w-3/4 bg-gradient-to-r from-indigo-500 to-purple-500 rounded-full"></div>
              </div>
            </div>
            <div class="grid grid-cols-3 gap-2">
              <div class="bg-white/10 rounded-lg p-2 text-center">
                <p class="text-sm font-bold">Step 147</p>
                <p class="text-xs text-white/50">Done</p>
              </div>
              <div class="bg-white/10 rounded-lg p-2 text-center">
                <p class="text-sm font-bold">10</p>
                <p class="text-xs text-white/50">Steps left</p>
              </div>
              <div class="bg-indigo-600/50 rounded-lg p-2 text-center">
                <p class="text-sm font-bold">1,000</p>
                <p class="text-xs text-white/50">Total</p>
              </div>
            </div>
          </div>
        </div>
        
        <!-- Floating badge -->
        <div class="
          absolute -top-4 -right-4 
          bg-green-500 text-white text-xs font-bold 
          px-3 py-1.5 rounded-full shadow-lg shadow-green-500/30
          animate-bounce
        ">
          🎉 +150 XP
        </div>
        
        <!-- Mini card -->
        <div class="
          absolute -bottom-6 -left-6 
          bg-white/10 backdrop-blur-sm border border-white/20 
          rounded-xl p-3 shadow-xl
          animate-float delay-200
        " style="animation-delay: 1s">
          <div class="flex items-center gap-2">
            <div class="size-7 bg-yellow-400/20 rounded-lg flex items-center justify-center">⚡</div>
            <div>
              <p class="text-xs font-bold">Streak</p>
              <p class="text-xs text-white/50">7 วัน</p>
            </div>
          </div>
        </div>
      </div>
      
    </div>
  </main>

  <!-- Skeleton loading example -->
  <section class="relative z-10 max-w-7xl mx-auto px-6 pb-20">
    <h2 class="text-2xl font-bold mb-6">Skeleton Loading Example</h2>
    <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
      <div class="bg-white/5 border border-white/10 rounded-xl p-4 space-y-3">
        <div class="h-32 rounded-lg shimmer opacity-20"></div>
        <div class="h-4 rounded shimmer opacity-20"></div>
        <div class="h-3 rounded shimmer opacity-20 w-3/4"></div>
        <div class="h-3 rounded shimmer opacity-20 w-1/2"></div>
      </div>
      <div class="bg-white/5 border border-white/10 rounded-xl p-4 space-y-3">
        <div class="h-32 rounded-lg shimmer opacity-20"></div>
        <div class="h-4 rounded shimmer opacity-20"></div>
        <div class="h-3 rounded shimmer opacity-20 w-3/4"></div>
        <div class="h-3 rounded shimmer opacity-20 w-1/2"></div>
      </div>
      <div class="bg-white/5 border border-white/10 rounded-xl p-4 space-y-3">
        <div class="h-32 rounded-lg shimmer opacity-20"></div>
        <div class="h-4 rounded shimmer opacity-20"></div>
        <div class="h-3 rounded shimmer opacity-20 w-3/4"></div>
        <div class="h-3 rounded shimmer opacity-20 w-1/2"></div>
      </div>
    </div>
  </section>

</body>
</html>
```

---

## 📝 สรุป Part 15

| Utility | หมวด | ตัวอย่าง |
|---------|------|---------|
| `transition` | Transition | `transition-all duration-300` |
| `duration-{n}` | Duration | `duration-300` |
| `ease-{type}` | Easing | `ease-in-out` |
| `delay-{n}` | Delay | `delay-200` |
| `scale-{n}` | Transform | `hover:scale-110` |
| `rotate-{n}` | Transform | `hover:rotate-12` |
| `translate-x/y` | Transform | `hover:-translate-y-1` |
| `animate-spin` | Animation | loading spinner |
| `animate-pulse` | Animation | skeleton loading |
| `animate-bounce` | Animation | scroll indicator |
| `animate-ping` | Animation | notification badge |

---

*Part 15 — จาก 100 Parts | Steps 141–150 จาก 1,000 Steps*
