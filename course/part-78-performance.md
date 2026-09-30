# Part 78: Performance Optimization

## เป้าหมาย
- Tailwind CSS ขนาดเล็ก — purge, JIT, minimal config
- Critical CSS + lazy loading
- Image optimization + responsive images
- Core Web Vitals — LCP, FID, CLS
- Steps 771–780

---

## Step 771: Tailwind ขนาดเล็กที่สุด

### Development vs Production

```
Development (CDN):     ~4MB — ทุก class
Production (built):    ~5–20KB — เฉพาะ class ที่ใช้จริง
```

### Production config

`tailwind.config.ts`:
```ts
import type { Config } from 'tailwindcss'

export default {
  // ระบุ content ให้แม่นยำ — อย่าใช้ ** กว้างเกิน
  content: [
    './src/app/**/*.{ts,tsx}',
    './src/components/**/*.{ts,tsx}',
    // ❌ ห้ามใส่ node_modules
  ],
  theme: {
    // ตัด font families ที่ไม่ใช้
    fontFamily: {
      sans: ['Inter', 'system-ui', 'sans-serif'],
      // ❌ ไม่ต้องมี mono ถ้าไม่ใช้
    },
    extend: {
      // เฉพาะ custom values ที่ใช้จริง
      colors: {
        brand: { 600: '#4f46e5' },
      },
    },
  },
  // Core plugins เท่านั้น — ปิด plugins ไม่ใช้
  corePlugins: {
    float: false,        // ถ้าไม่ใช้ float
    clear: false,
    skew: false,
    // ...
  },
} satisfies Config
```

---

## Step 772: JIT Mode & Dynamic Classes

JIT (Just-in-Time) เปิดอยู่ใน Tailwind v3+ โดย default

```html
<!-- ❌ Dynamic classes แบบนี้จะถูก purge -->
<div class="text-{{ color }}-500">...</div>

<!-- ✅ ใช้ safelist หรือ map ก่อน -->
```

```ts
// tailwind.config.ts — safelist สำหรับ dynamic classes
export default {
  safelist: [
    // Exact classes
    'bg-red-500', 'bg-green-500',
    // Pattern
    { pattern: /bg-(red|green|blue)-(100|500|700)/ },
    { pattern: /text-(red|green|blue)-(600|700)/ },
  ],
}
```

```tsx
// ✅ Pattern ที่ purge ไม่ตัด — ใช้ full class name
const statusColors = {
  active:   'bg-emerald-100 text-emerald-700',
  inactive: 'bg-gray-100 text-gray-500',
  pending:  'bg-yellow-100 text-yellow-700',
}
// Tailwind เห็น full string → ไม่ถูก purge
<span className={statusColors[status]}>...</span>
```

---

## Step 773: Critical CSS

```html
<!-- Critical CSS inline ใน <head> — block render ให้น้อยที่สุด -->
<head>
  <style>
    /* Critical: layout + above-the-fold styles */
    *, *::before, *::after { box-sizing: border-box; }
    body { margin: 0; font-family: Inter, system-ui, sans-serif; }
    .nav { display: flex; height: 56px; background: #fff; border-bottom: 1px solid #e5e7eb; }
    .hero { min-height: 60vh; display: flex; align-items: center; }
  </style>
  <!-- Non-critical CSS defer -->
  <link rel="preload" href="/app.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
  <noscript><link rel="stylesheet" href="/app.css"></noscript>
</head>
```

---

## Step 774: Image Optimization

```html
<!-- ✅ responsive images ด้วย srcset + sizes -->
<img
  src="/hero-800.webp"
  srcset="/hero-400.webp 400w, /hero-800.webp 800w, /hero-1200.webp 1200w"
  sizes="(max-width: 640px) 100vw, (max-width: 1024px) 80vw, 1200px"
  width="1200"
  height="600"
  alt="Hero image"
  loading="eager"
  decoding="async"
  class="w-full h-auto object-cover rounded-2xl"
>

<!-- ✅ Lazy loading สำหรับ images below fold -->
<img
  src="/product.webp"
  loading="lazy"
  decoding="async"
  width="400"
  height="400"
  alt="Product"
  class="w-full aspect-square object-cover"
>

<!-- ✅ Blur placeholder ก่อน load จริง -->
<div class="relative overflow-hidden">
  <img src="data:image/svg+xml;base64,..." class="absolute inset-0 w-full h-full scale-110 blur-xl" aria-hidden="true">
  <img src="/product.webp" loading="lazy" class="relative z-10 w-full h-full object-cover">
</div>
```

---

## Step 775: Core Web Vitals

### LCP (Largest Contentful Paint) — target < 2.5s

```html
<!-- Preload hero image — สำคัญมากสำหรับ LCP -->
<link rel="preload" as="image" href="/hero.webp" fetchpriority="high">

<!-- ✅ fetchpriority="high" บน LCP image -->
<img src="/hero.webp" fetchpriority="high" loading="eager" alt="Hero">
```

### CLS (Cumulative Layout Shift) — target < 0.1

```html
<!-- ✅ กำหนด width/height ทุก image เสมอ — ป้องกัน CLS -->
<img src="/product.webp" width="400" height="400" alt="..." class="w-full h-auto">

<!-- ✅ Font display swap ป้องกัน FOUT -->
<style>
  @font-face {
    font-family: 'Inter';
    src: url('/inter.woff2') format('woff2');
    font-display: swap; /* หรือ optional */
  }
</style>

<!-- ✅ Skeleton placeholder ป้องกัน layout shift -->
<div class="h-48 bg-gray-200 rounded-2xl animate-pulse" aria-hidden="true"></div>
```

### FCP / TTI

```html
<!-- Preconnect origins ที่ใช้บ่อย -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="dns-prefetch" href="https://api.myservice.com">

<!-- Defer non-critical JS -->
<script src="/analytics.js" defer></script>
<!-- หรือ async สำหรับ independent scripts -->
<script src="/chat.js" async></script>
```

---

## Step 776: Tailwind CSS Bundle Analysis

```bash
# ดู CSS size หลัง build
npm run build
ls -lh dist/assets/*.css

# Expected production sizes
# Minimal (few components):  5–8 KB gzip
# Medium app:               8–15 KB gzip
# Large app with safelist: 15–25 KB gzip
```

```ts
// next.config.ts — Bundle analyzer
import { withBundleAnalyzer } from '@next/bundle-analyzer'
const withAnalyzer = withBundleAnalyzer({ enabled: process.env.ANALYZE === 'true' })
export default withAnalyzer({ /* next config */ })
```

---

## Step 777: Lazy Components + Code Splitting

```tsx
// React lazy loading
import { lazy, Suspense } from 'react'

const HeavyDashboard = lazy(() => import('./Dashboard'))
const Chart = lazy(() => import('./Chart'))

// Skeleton fallback ขณะโหลด
function App() {
  return (
    <Suspense fallback={
      <div className="animate-pulse space-y-4 p-6">
        <div className="h-8 bg-gray-200 rounded-xl w-1/3"></div>
        <div className="h-48 bg-gray-200 rounded-2xl"></div>
      </div>
    }>
      <HeavyDashboard />
    </Suspense>
  )
}

// Next.js dynamic import
import dynamic from 'next/dynamic'
const DynamicMap = dynamic(() => import('./Map'), {
  loading: () => <div className="h-64 bg-gray-200 rounded-2xl animate-pulse" />,
  ssr: false, // client-only component
})
```

---

## Step 778: CSS Containment

```css
/* CSS containment ช่วย performance ของ browser layout */
.card {
  contain: layout style; /* บอก browser ว่า card ไม่ affect layout นอก */
}

/* content-visibility: auto — skip render ของ off-screen content */
.long-list-item {
  content-visibility: auto;
  contain-intrinsic-size: 0 80px; /* hint ขนาดก่อน render */
}
```

```html
<!-- Tailwind equivalent ด้วย @layer + custom CSS -->
<style>
  @layer utilities {
    .contain-layout { contain: layout; }
    .contain-strict  { contain: strict; }
    .cv-auto         { content-visibility: auto; contain-intrinsic-size: 0 200px; }
  }
</style>
```

---

## Step 779: Optimistic UI

```tsx
// อัปเดต UI ก่อน server response — รู้สึกเร็วขึ้น
function TodoList() {
  const [todos, setTodos] = useState(initialTodos)

  async function addTodo(text: string) {
    // 1. อัปเดต UI ทันที (optimistic)
    const tempId = Date.now()
    setTodos(prev => [...prev, { id: tempId, text, done: false, pending: true }])

    try {
      // 2. ส่ง API request
      const created = await createTodo(text)
      // 3. เปลี่ยน temp id เป็น real id
      setTodos(prev => prev.map(t => t.id === tempId ? { ...created, pending: false } : t))
    } catch {
      // 4. Rollback ถ้า error
      setTodos(prev => prev.filter(t => t.id !== tempId))
      // แสดง error toast
    }
  }

  return (
    <ul>
      {todos.map(todo => (
        <li key={todo.id} className={`flex items-center gap-2 py-2 ${todo.pending ? 'opacity-50' : ''}`}>
          {todo.pending && <span className="w-3 h-3 rounded-full bg-indigo-400 animate-pulse"></span>}
          {todo.text}
        </li>
      ))}
    </ul>
  )
}
```

---

## Step 780: Performance Workshop — Complete Audit

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Performance Workshop</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Preconnect + DNS prefetch -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <style>
    @keyframes pulse { 0%,100%{opacity:1} 50%{opacity:.4} }
    .animate-pulse { animation: pulse 1.8s ease-in-out infinite; }
    .contain-layout { contain: layout; }
  </style>
</head>
<body class="bg-gray-100 min-h-screen p-6">
  <div class="max-w-3xl mx-auto">
    <h1 class="text-2xl font-extrabold text-gray-900 mb-6">Performance Checklist</h1>
    <div class="space-y-3" id="checklist"></div>
    <div class="mt-6 bg-white rounded-2xl border p-5">
      <h2 class="font-bold text-gray-900 mb-3">Lighthouse Score Simulator</h2>
      <div class="grid grid-cols-4 gap-3 text-center">
        <div><div id="perf" class="text-3xl font-black text-emerald-600">—</div><p class="text-xs text-gray-500">Performance</p></div>
        <div><div id="acc"  class="text-3xl font-black text-emerald-600">—</div><p class="text-xs text-gray-500">Accessibility</p></div>
        <div><div id="bp"   class="text-3xl font-black text-emerald-600">—</div><p class="text-xs text-gray-500">Best Practices</p></div>
        <div><div id="seo"  class="text-3xl font-black text-emerald-600">—</div><p class="text-xs text-gray-500">SEO</p></div>
      </div>
      <button onclick="runAudit()" class="mt-4 bg-indigo-600 text-white px-5 py-2.5 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors w-full">▶ Run Audit</button>
    </div>
  </div>
  <script>
    const items = [
      { done: true,  text: 'Tailwind content array แม่นยำ' },
      { done: true,  text: 'Dynamic classes ใน safelist' },
      { done: true,  text: 'Images มี width/height' },
      { done: true,  text: 'loading="lazy" สำหรับ images below fold' },
      { done: true,  text: 'fetchpriority="high" บน LCP image' },
      { done: false, text: 'Critical CSS inline ใน <head>' },
      { done: true,  text: 'font-display: swap' },
      { done: false, text: 'Bundle analyzer ตรวจสอบ' },
      { done: true,  text: 'Code splitting lazy imports' },
      { done: true,  text: 'Optimistic UI สำหรับ mutations' },
      { done: false, text: 'content-visibility: auto สำหรับ long lists' },
      { done: true,  text: 'preconnect สำหรับ 3rd party origins' },
    ];
    document.getElementById('checklist').innerHTML = items.map(i => `
      <div class="flex items-center gap-3 bg-white border rounded-xl px-4 py-3 ${i.done ? 'border-emerald-200' : 'border-gray-200'}">
        <span class="text-lg">${i.done ? '✅' : '⬜'}</span>
        <span class="text-sm ${i.done ? 'text-gray-800' : 'text-gray-400 line-through'}">${i.text}</span>
      </div>
    `).join('');

    function runAudit() {
      ['perf','acc','bp','seo'].forEach(id => document.getElementById(id).textContent = '...');
      setTimeout(() => {
        document.getElementById('perf').textContent = 94;
        document.getElementById('acc').textContent = 98;
        document.getElementById('bp').textContent = 96;
        document.getElementById('seo').textContent = 100;
      }, 1500);
    }
  </script>
</body>
</html>
```

---

## สรุป Part 78

| Step | เนื้อหา |
|------|---------|
| 771 | Tailwind config ขนาดเล็ก — content, corePlugins |
| 772 | JIT + safelist สำหรับ dynamic classes |
| 773 | Critical CSS inline + defer non-critical |
| 774 | Image optimization — srcset, sizes, loading, decoding |
| 775 | Core Web Vitals — LCP, CLS, FCP/TTI strategies |
| 776 | Bundle analysis — CSS size targets |
| 777 | Lazy components + Suspense skeleton |
| 778 | CSS containment + content-visibility |
| 779 | Optimistic UI pattern |
| 780 | Workshop: Performance checklist + Lighthouse simulator |

**Part ถัดไป:** Part 79 — Theming System (Steps 781–790)
