# Part 24: Component Patterns
## Steps 231–240: Design Patterns ที่นักพัฒนาใช้จริง

---

## 🎯 เป้าหมายของ Part นี้

- @apply directive
- Component abstraction
- Variant pattern
- Compound component design
- Headless component styling

---

## Step 231: @apply Directive

```css
/* styles.css */
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer components {
  /* Button component */
  .btn {
    @apply inline-flex items-center justify-center gap-2;
    @apply font-semibold text-sm;
    @apply px-5 py-2.5 rounded-xl;
    @apply transition-all duration-150;
    @apply cursor-pointer select-none;
    @apply focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-offset-2;
    @apply disabled:opacity-50 disabled:cursor-not-allowed;
  }
  
  .btn-primary {
    @apply bg-indigo-600 text-white;
    @apply hover:bg-indigo-700;
    @apply active:scale-[0.98] active:bg-indigo-800;
    @apply focus-visible:ring-indigo-500;
  }
  
  .btn-secondary {
    @apply bg-gray-100 text-gray-900;
    @apply hover:bg-gray-200;
    @apply active:scale-[0.98];
    @apply focus-visible:ring-gray-500;
  }
  
  .btn-outline {
    @apply border-2 border-indigo-600 text-indigo-600;
    @apply hover:bg-indigo-600 hover:text-white;
    @apply focus-visible:ring-indigo-500;
  }
  
  /* Size variants */
  .btn-sm { @apply text-xs px-3 py-1.5 rounded-lg; }
  .btn-lg { @apply text-base px-6 py-3; }
  .btn-xl { @apply text-lg px-8 py-4 rounded-2xl; }
  
  /* Input component */
  .input {
    @apply w-full px-4 py-3 rounded-xl;
    @apply border border-gray-300;
    @apply bg-white text-gray-900;
    @apply placeholder:text-gray-400;
    @apply focus:outline-none focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200;
    @apply transition-all;
    @apply disabled:bg-gray-100 disabled:cursor-not-allowed;
  }
  
  .input-error {
    @apply border-red-400 focus:border-red-500 focus:ring-red-200;
  }
  
  .input-success {
    @apply border-green-400 focus:border-green-500 focus:ring-green-200;
  }
  
  /* Card component */
  .card {
    @apply bg-white rounded-2xl border border-gray-200 shadow-sm overflow-hidden;
  }
  
  .card-body {
    @apply p-6;
  }
  
  .card-hover {
    @apply hover:shadow-xl hover:-translate-y-0.5 transition-all duration-300;
  }
  
  /* Badge component */
  .badge {
    @apply inline-flex items-center gap-1;
    @apply text-xs font-semibold px-2.5 py-1 rounded-full;
  }
  
  .badge-primary { @apply bg-indigo-100 text-indigo-700; }
  .badge-success  { @apply bg-green-100 text-green-700; }
  .badge-warning  { @apply bg-yellow-100 text-yellow-700; }
  .badge-danger   { @apply bg-red-100 text-red-700; }
  .badge-gray     { @apply bg-gray-100 text-gray-600; }
}

@layer utilities {
  .text-balance { text-wrap: balance; }
  .glass {
    @apply bg-white/10 backdrop-blur-md border border-white/20;
  }
  .gradient-text {
    @apply bg-gradient-to-r from-indigo-600 to-purple-600 bg-clip-text text-transparent;
  }
}
```

```html
<!-- ใช้ @apply classes -->
<button class="btn btn-primary">Primary</button>
<button class="btn btn-secondary btn-sm">Small</button>
<input class="input" placeholder="Text...">
<input class="input input-error" placeholder="Error state">
<div class="card card-hover"><div class="card-body">Content</div></div>
<span class="badge badge-success">Active</span>
```

---

## Step 232: CVA Pattern (Class Variance Authority)

```javascript
// lib/variants.js — Class Variance Authority pattern (pure JS)
function cn(...classes) {
  return classes.filter(Boolean).join(' ')
}

// Button variant config
const buttonVariants = {
  base: [
    'inline-flex items-center justify-center gap-2',
    'font-semibold text-sm rounded-xl',
    'transition-all duration-150 cursor-pointer',
    'focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-offset-2',
    'disabled:opacity-50 disabled:cursor-not-allowed',
  ],
  
  variants: {
    variant: {
      primary:   'bg-indigo-600 hover:bg-indigo-700 active:scale-[0.98] text-white focus-visible:ring-indigo-500',
      secondary: 'bg-gray-100 hover:bg-gray-200 active:scale-[0.98] text-gray-900',
      outline:   'border-2 border-indigo-600 text-indigo-600 hover:bg-indigo-600 hover:text-white',
      ghost:     'text-indigo-600 hover:bg-indigo-50',
      danger:    'bg-red-600 hover:bg-red-700 text-white focus-visible:ring-red-500',
    },
    size: {
      sm:  'text-xs px-3 py-1.5 rounded-lg',
      md:  'px-5 py-2.5',
      lg:  'text-base px-6 py-3',
      xl:  'text-lg px-8 py-4 rounded-2xl',
      icon: 'size-10',
    },
  },
  
  defaults: {
    variant: 'primary',
    size: 'md',
  },
}

function button({ variant = 'primary', size = 'md', className = '' } = {}) {
  return cn(
    ...buttonVariants.base,
    buttonVariants.variants.variant[variant],
    buttonVariants.variants.size[size],
    className
  )
}

// Usage
const primaryBtn = button()
const smallOutline = button({ variant: 'outline', size: 'sm' })
const customDanger = button({ variant: 'danger', size: 'lg', className: 'w-full' })
```

```html
<!-- ใช้ใน HTML (static generation) -->
<button class="inline-flex items-center justify-center gap-2 font-semibold text-sm rounded-xl transition-all duration-150 cursor-pointer focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-offset-2 bg-indigo-600 hover:bg-indigo-700 active:scale-[0.98] text-white focus-visible:ring-indigo-500 px-5 py-2.5">
  Button
</button>
```

---

## Step 233: Compound Component Pattern

```html
<!-- Compound Card Component -->
<!-- ทำด้วยการ group classes เป็น logical sections -->

<!-- Card -->
<article class="bg-white rounded-2xl border border-gray-200 overflow-hidden shadow-sm group">
  <!-- Card Image -->
  <div class="relative overflow-hidden aspect-video">
    <div class="w-full h-full bg-gradient-to-br from-indigo-400 to-purple-600 group-hover:scale-105 transition-transform duration-500"></div>
    <div class="absolute top-3 left-3 flex gap-2">
      <span class="text-xs font-bold bg-white px-2.5 py-1 rounded-full text-indigo-700 shadow">Level 1</span>
    </div>
    <div class="absolute top-3 right-3">
      <button class="size-8 bg-white/80 hover:bg-white rounded-full flex items-center justify-center text-sm shadow transition-colors" aria-label="Save">
        ♡
      </button>
    </div>
  </div>
  
  <!-- Card Content -->
  <div class="p-5">
    <!-- Meta -->
    <div class="flex items-center gap-2 text-xs text-gray-400 mb-2">
      <span class="flex items-center gap-1">👤 อาจารย์สมชาย</span>
      <span>•</span>
      <span>10 Steps</span>
      <span>•</span>
      <span>45 นาที</span>
    </div>
    
    <!-- Title -->
    <h3 class="font-bold text-gray-900 group-hover:text-indigo-600 transition-colors leading-snug text-base">
      Introduction to Tailwind CSS
    </h3>
    
    <!-- Description -->
    <p class="text-gray-500 text-sm mt-2 line-clamp-2 leading-relaxed">
      เรียนรู้ utility-first CSS framework ที่ทรงพลังที่สุดในปัจจุบัน
    </p>
    
    <!-- Progress (optional) -->
    <div class="mt-3 space-y-1">
      <div class="flex justify-between text-xs">
        <span class="text-gray-400">Progress</span>
        <span class="text-green-600 font-bold">100%</span>
      </div>
      <div class="h-1.5 bg-gray-100 rounded-full overflow-hidden">
        <div class="h-full w-full bg-green-500 rounded-full"></div>
      </div>
    </div>
  </div>
  
  <!-- Card Footer -->
  <div class="px-5 py-3 bg-gray-50 border-t border-gray-100 flex items-center justify-between">
    <div class="flex items-center gap-1 text-xs text-yellow-500">
      ★ <span class="text-gray-600 font-medium">4.9</span>
      <span class="text-gray-400">(48 รีวิว)</span>
    </div>
    <button class="flex items-center gap-1 text-indigo-600 text-xs font-medium hover:text-indigo-800 transition-colors">
      เรียนต่อ →
    </button>
  </div>
</article>
```

---

## Step 234: Data Attribute Styling

```html
<!-- ใช้ data attributes แทน JS class toggle -->

<!-- Accordion item (data-open) -->
<div 
  data-state="closed"
  onclick="this.dataset.state = this.dataset.state === 'open' ? 'closed' : 'open'"
  class="border border-gray-200 rounded-xl overflow-hidden cursor-pointer group"
>
  <div class="flex items-center justify-between px-5 py-4 bg-white hover:bg-gray-50 transition-colors">
    <span class="font-medium text-gray-900">คำถามที่พบบ่อย</span>
    <svg 
      class="size-5 text-gray-500 transition-transform duration-300 group-data-[state=open]:rotate-180"
      fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"
    >
      <path stroke-linecap="round" stroke-linejoin="round" d="M19 9l-7 7-7-7"/>
    </svg>
  </div>
  <div class="overflow-hidden grid transition-all duration-300 grid-rows-[0fr] data-[state=open]:grid-rows-[1fr]">
    <div class="overflow-hidden">
      <div class="px-5 py-4 text-sm text-gray-600 bg-gray-50 border-t border-gray-100">
        เนื้อหาคำตอบที่ปรากฏเมื่อ open
      </div>
    </div>
  </div>
</div>

<!-- Tabs (data-active) -->
<div class="flex gap-1 p-1 bg-gray-100 rounded-xl">
  <button 
    data-active
    class="px-4 py-2 text-sm font-medium rounded-lg data-[active]:bg-white data-[active]:shadow data-[active]:text-gray-900 text-gray-500 transition-all"
    onclick="activateTab(this)"
  >
    Tab 1
  </button>
  <button 
    class="px-4 py-2 text-sm font-medium rounded-lg data-[active]:bg-white data-[active]:shadow data-[active]:text-gray-900 text-gray-500 transition-all"
    onclick="activateTab(this)"
  >
    Tab 2
  </button>
  <button 
    class="px-4 py-2 text-sm font-medium rounded-lg data-[active]:bg-white data-[active]:shadow data-[active]:text-gray-900 text-gray-500 transition-all"
    onclick="activateTab(this)"
  >
    Tab 3
  </button>
</div>

<script>
function activateTab(btn) {
  btn.closest('.flex').querySelectorAll('button').forEach(b => delete b.dataset.active);
  btn.dataset.active = '';
}
</script>
```

---

## Step 235: Skeleton Loading Pattern

```html
<!-- Skeleton pulse animation -->
<style>
  @keyframes shimmer {
    0% { background-position: -200% 0; }
    100% { background-position: 200% 0; }
  }
  .skeleton {
    background: linear-gradient(90deg, #f0f0f0 25%, #e0e0e0 50%, #f0f0f0 75%);
    background-size: 200% 100%;
    animation: shimmer 1.5s linear infinite;
    border-radius: 0.5rem;
  }
</style>

<!-- Course card skeleton -->
<div class="bg-white rounded-2xl border border-gray-200 overflow-hidden">
  <div class="skeleton aspect-video"></div>
  <div class="p-5 space-y-3">
    <div class="skeleton h-3 w-1/3 rounded-full"></div>
    <div class="skeleton h-5 w-4/5"></div>
    <div class="skeleton h-3 w-full"></div>
    <div class="skeleton h-3 w-3/4"></div>
    <div class="flex items-center justify-between mt-4 pt-4 border-t border-gray-100">
      <div class="skeleton h-3 w-1/4 rounded-full"></div>
      <div class="skeleton h-3 w-1/5 rounded-full"></div>
    </div>
  </div>
</div>

<!-- List item skeleton -->
<div class="flex items-center gap-4 p-4 bg-white rounded-xl border border-gray-200">
  <div class="skeleton size-12 rounded-xl flex-none"></div>
  <div class="flex-1 space-y-2">
    <div class="skeleton h-4 w-3/4"></div>
    <div class="skeleton h-3 w-1/2"></div>
  </div>
  <div class="skeleton h-6 w-16 rounded-full flex-none"></div>
</div>

<!-- Tailwind animate-pulse version -->
<div class="bg-white rounded-2xl p-5 space-y-3 animate-pulse">
  <div class="h-4 bg-gray-200 rounded w-3/4"></div>
  <div class="h-3 bg-gray-200 rounded"></div>
  <div class="h-3 bg-gray-200 rounded w-5/6"></div>
  <div class="flex gap-3 mt-4">
    <div class="size-10 bg-gray-200 rounded-full"></div>
    <div class="flex-1 space-y-2">
      <div class="h-3 bg-gray-200 rounded w-1/2"></div>
      <div class="h-3 bg-gray-200 rounded w-1/3"></div>
    </div>
  </div>
</div>
```

---

## Step 236: Scroll Animations (CSS + Tailwind)

```html
<!-- Intersection Observer + Tailwind -->
<style>
  .fade-in-up {
    opacity: 0;
    transform: translateY(30px);
    transition: opacity 0.6s ease, transform 0.6s ease;
  }
  .fade-in-up.visible {
    opacity: 1;
    transform: translateY(0);
  }
</style>

<div class="space-y-6">
  <div class="fade-in-up bg-white rounded-2xl p-6 border border-gray-200">Card 1</div>
  <div class="fade-in-up bg-white rounded-2xl p-6 border border-gray-200" style="transition-delay: 0.1s">Card 2</div>
  <div class="fade-in-up bg-white rounded-2xl p-6 border border-gray-200" style="transition-delay: 0.2s">Card 3</div>
</div>

<script>
  const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        entry.target.classList.add('visible');
      }
    });
  }, { threshold: 0.1 });
  
  document.querySelectorAll('.fade-in-up').forEach(el => observer.observe(el));
</script>
```

---

## Step 237–240: Workshop — Full Component Library Demo

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Component Library Demo</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    @keyframes shimmer { 0% { background-position: -200% 0; } 100% { background-position: 200% 0; } }
    .skeleton { background: linear-gradient(90deg, #f0f0f0 25%, #e0e0e0 50%, #f0f0f0 75%); background-size: 200% 100%; animation: shimmer 1.5s linear infinite; border-radius: 0.5rem; }
  </style>
</head>
<body class="bg-gray-50 p-6 min-h-screen">

<div class="max-w-5xl mx-auto space-y-10">

  <h1 class="text-3xl font-black text-gray-900">Component Library</h1>

  <!-- Buttons -->
  <section class="bg-white rounded-2xl p-6 border border-gray-200">
    <h2 class="font-bold text-lg text-gray-900 mb-5">Buttons</h2>
    <div class="flex flex-wrap gap-3 mb-4">
      <button class="inline-flex items-center gap-2 font-semibold text-sm px-5 py-2.5 rounded-xl transition-all bg-indigo-600 hover:bg-indigo-700 active:scale-[0.98] text-white">Primary</button>
      <button class="inline-flex items-center gap-2 font-semibold text-sm px-5 py-2.5 rounded-xl transition-all bg-gray-100 hover:bg-gray-200 text-gray-900">Secondary</button>
      <button class="inline-flex items-center gap-2 font-semibold text-sm px-5 py-2.5 rounded-xl transition-all border-2 border-indigo-600 text-indigo-600 hover:bg-indigo-600 hover:text-white">Outline</button>
      <button class="inline-flex items-center gap-2 font-semibold text-sm px-5 py-2.5 rounded-xl transition-all text-indigo-600 hover:bg-indigo-50">Ghost</button>
      <button class="inline-flex items-center gap-2 font-semibold text-sm px-5 py-2.5 rounded-xl transition-all bg-red-600 hover:bg-red-700 text-white">Danger</button>
    </div>
    <div class="flex flex-wrap gap-3 items-center">
      <button class="inline-flex items-center font-semibold text-xs px-3 py-1.5 rounded-lg bg-indigo-600 text-white hover:bg-indigo-700">SM</button>
      <button class="inline-flex items-center font-semibold text-sm px-5 py-2.5 rounded-xl bg-indigo-600 text-white hover:bg-indigo-700">MD</button>
      <button class="inline-flex items-center font-semibold text-base px-6 py-3 rounded-xl bg-indigo-600 text-white hover:bg-indigo-700">LG</button>
      <button class="inline-flex items-center font-semibold text-lg px-8 py-4 rounded-2xl bg-indigo-600 text-white hover:bg-indigo-700">XL</button>
    </div>
  </section>

  <!-- Accordion -->
  <section class="bg-white rounded-2xl p-6 border border-gray-200">
    <h2 class="font-bold text-lg text-gray-900 mb-5">Accordion</h2>
    <div class="space-y-2">
      <div data-state="open" onclick="this.dataset.state = this.dataset.state === 'open' ? 'closed' : 'open'" class="border border-gray-200 rounded-xl overflow-hidden cursor-pointer group">
        <div class="flex items-center justify-between px-5 py-4 bg-gray-50 group-data-[state=open]:bg-white">
          <span class="font-medium text-gray-900 text-sm">Tailwind CSS คืออะไร?</span>
          <span class="text-gray-400 text-sm transition-transform duration-300 group-data-[state=open]:rotate-180">▼</span>
        </div>
        <div class="grid transition-all duration-300 grid-rows-[1fr] group-data-[state=closed]:grid-rows-[0fr] overflow-hidden">
          <div class="overflow-hidden">
            <div class="px-5 py-4 text-sm text-gray-600 border-t border-gray-100 bg-white">
              Tailwind CSS คือ utility-first CSS framework ที่ให้คุณสร้าง UI โดยใช้ class ขนาดเล็กๆ แทนการเขียน CSS เอง
            </div>
          </div>
        </div>
      </div>
      
      <div data-state="closed" onclick="this.dataset.state = this.dataset.state === 'open' ? 'closed' : 'open'" class="border border-gray-200 rounded-xl overflow-hidden cursor-pointer group">
        <div class="flex items-center justify-between px-5 py-4 bg-gray-50 group-data-[state=open]:bg-white">
          <span class="font-medium text-gray-900 text-sm">ต้องมีพื้นฐาน CSS ก่อนไหม?</span>
          <span class="text-gray-400 text-sm transition-transform duration-300 group-data-[state=open]:rotate-180">▼</span>
        </div>
        <div class="grid transition-all duration-300 grid-rows-[0fr] group-data-[state=open]:grid-rows-[1fr] overflow-hidden">
          <div class="overflow-hidden">
            <div class="px-5 py-4 text-sm text-gray-600 border-t border-gray-100 bg-white">
              ควรรู้ HTML/CSS พื้นฐาน แต่ Tailwind ก็จะช่วยให้เข้าใจ CSS มากขึ้นระหว่างเรียน
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- Skeleton Loading -->
  <section class="bg-white rounded-2xl p-6 border border-gray-200">
    <h2 class="font-bold text-lg text-gray-900 mb-5">Skeleton Loading</h2>
    <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
      <div class="space-y-3">
        <div class="skeleton h-32 rounded-xl"></div>
        <div class="skeleton h-4 w-3/4"></div>
        <div class="skeleton h-3"></div>
        <div class="skeleton h-3 w-5/6"></div>
      </div>
      <div class="space-y-3">
        <div class="skeleton h-32 rounded-xl"></div>
        <div class="skeleton h-4 w-3/4"></div>
        <div class="skeleton h-3"></div>
        <div class="skeleton h-3 w-5/6"></div>
      </div>
      <div class="space-y-3">
        <div class="skeleton h-32 rounded-xl"></div>
        <div class="skeleton h-4 w-3/4"></div>
        <div class="skeleton h-3"></div>
        <div class="skeleton h-3 w-5/6"></div>
      </div>
    </div>
  </section>

</div>

</body>
</html>
```

---

## 📝 สรุป Part 24

| Pattern | เมื่อใช้ |
|---------|---------|
| `@apply` | สร้าง component classes ใน CSS |
| CVA pattern | สร้าง typed variants ใน JS |
| Data attributes | State management ไม่ต้อง toggle JS class |
| Skeleton | Placeholder ระหว่างโหลด data |
| Intersection Observer | Scroll-triggered animations |

---

*Part 24 — จาก 100 Parts | Steps 231–240 จาก 1,000 Steps*
