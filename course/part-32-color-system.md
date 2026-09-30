# Part 32: Color System & Theming
## Steps 311–320: ระบบสีระดับมืออาชีพ

---

## 🎯 เป้าหมายของ Part นี้

- Tailwind color palette ทั้งหมด
- Semantic color system
- Multi-brand theming
- CSS custom properties color system
- Color accessibility (contrast)
- Dynamic color generation

---

## Step 311: Tailwind Color Palette

```html
<!-- ทุก Tailwind color ใช้ได้ผ่าน {color}-{shade} -->
<!-- Shades: 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950 -->

<div class="grid grid-cols-11 gap-1 p-4">
  <!-- slate -->
  <div class="col-span-11 text-xs text-gray-500 font-bold uppercase tracking-wider mb-1">Slate</div>
  <div class="h-10 bg-slate-50 rounded"></div>
  <div class="h-10 bg-slate-100 rounded"></div>
  <div class="h-10 bg-slate-200 rounded"></div>
  <div class="h-10 bg-slate-300 rounded"></div>
  <div class="h-10 bg-slate-400 rounded"></div>
  <div class="h-10 bg-slate-500 rounded"></div>
  <div class="h-10 bg-slate-600 rounded"></div>
  <div class="h-10 bg-slate-700 rounded"></div>
  <div class="h-10 bg-slate-800 rounded"></div>
  <div class="h-10 bg-slate-900 rounded"></div>
  <div class="h-10 bg-slate-950 rounded"></div>
</div>

<!-- Special colors -->
<div class="flex gap-2 p-4">
  <div class="h-10 w-10 bg-black rounded"></div>
  <div class="h-10 w-10 bg-white rounded border"></div>
  <div class="h-10 w-10 bg-transparent rounded border-2 border-dashed"></div>
  <div class="h-10 w-10 bg-current rounded"></div>
  <div class="h-10 w-10 bg-inherit rounded border"></div>
</div>
```

---

## Step 312: Semantic Color System

```javascript
// tailwind.config.js — Semantic Color System
module.exports = {
  theme: {
    extend: {
      colors: {
        // Brand
        brand: {
          50:  'hsl(var(--brand-50))',
          100: 'hsl(var(--brand-100))',
          200: 'hsl(var(--brand-200))',
          300: 'hsl(var(--brand-300))',
          400: 'hsl(var(--brand-400))',
          500: 'hsl(var(--brand-500))',
          600: 'hsl(var(--brand-600))',
          700: 'hsl(var(--brand-700))',
          800: 'hsl(var(--brand-800))',
          900: 'hsl(var(--brand-900))',
          DEFAULT: 'hsl(var(--brand-500))',
        },
        
        // Semantic
        surface: {
          DEFAULT: 'hsl(var(--surface))',
          raised:  'hsl(var(--surface-raised))',
          overlay: 'hsl(var(--surface-overlay))',
          sunken:  'hsl(var(--surface-sunken))',
        },
        
        content: {
          DEFAULT:   'hsl(var(--content))',
          secondary: 'hsl(var(--content-secondary))',
          tertiary:  'hsl(var(--content-tertiary))',
          disabled:  'hsl(var(--content-disabled))',
          inverse:   'hsl(var(--content-inverse))',
        },
        
        border: {
          DEFAULT:  'hsl(var(--border))',
          strong:   'hsl(var(--border-strong))',
          focus:    'hsl(var(--border-focus))',
        },
        
        // Status
        status: {
          success: 'hsl(var(--status-success))',
          warning: 'hsl(var(--status-warning))',
          error:   'hsl(var(--status-error))',
          info:    'hsl(var(--status-info))',
        },
      }
    }
  }
}
```

```css
/* CSS Variables — Light Theme */
:root {
  --brand-50:  220 100% 97%;
  --brand-100: 220 100% 93%;
  --brand-200: 220 95% 85%;
  --brand-300: 220 85% 72%;
  --brand-400: 220 75% 60%;
  --brand-500: 220 70% 50%;
  --brand-600: 220 65% 42%;
  --brand-700: 220 60% 35%;
  --brand-800: 220 55% 28%;
  --brand-900: 220 50% 22%;
  
  --surface:         0 0% 100%;
  --surface-raised:  0 0% 98%;
  --surface-overlay: 0 0% 96%;
  --surface-sunken:  0 0% 94%;
  
  --content:           220 15% 10%;
  --content-secondary: 220 10% 35%;
  --content-tertiary:  220 8% 55%;
  --content-disabled:  220 5% 70%;
  --content-inverse:   0 0% 100%;
  
  --border:       220 13% 88%;
  --border-strong: 220 13% 75%;
  --border-focus: 220 70% 50%;
  
  --status-success: 142 72% 45%;
  --status-warning: 38 95% 50%;
  --status-error:   4 86% 58%;
  --status-info:    199 89% 48%;
}

/* Dark Theme */
.dark, [data-theme="dark"] {
  --surface:         220 20% 8%;
  --surface-raised:  220 20% 11%;
  --surface-overlay: 220 20% 14%;
  --surface-sunken:  220 20% 5%;
  
  --content:           0 0% 95%;
  --content-secondary: 220 5% 65%;
  --content-tertiary:  220 5% 45%;
  --content-disabled:  220 5% 30%;
  --content-inverse:   220 15% 10%;
  
  --border:        220 13% 20%;
  --border-strong: 220 13% 30%;
}
```

---

## Step 313: Multi-Brand Theming

```css
/* Brand A: Indigo */
[data-brand="indigo"] {
  --brand-500: 239 84% 67%;
  --brand-600: 239 72% 57%;
  --brand-700: 239 65% 50%;
  --border-focus: 239 84% 67%;
}

/* Brand B: Rose */
[data-brand="rose"] {
  --brand-500: 346 77% 60%;
  --brand-600: 346 70% 52%;
  --brand-700: 346 65% 44%;
  --border-focus: 346 77% 60%;
}

/* Brand C: Emerald */
[data-brand="emerald"] {
  --brand-500: 160 84% 39%;
  --brand-600: 160 76% 32%;
  --brand-700: 160 70% 26%;
  --border-focus: 160 84% 39%;
}

/* Brand D: Amber */
[data-brand="amber"] {
  --brand-500: 45 96% 50%;
  --brand-600: 45 88% 42%;
  --brand-700: 45 80% 35%;
  --border-focus: 45 96% 50%;
}
```

```html
<!-- Brand switcher demo -->
<div data-brand="indigo" id="brand-demo" class="p-6 bg-white rounded-2xl border border-gray-200">
  <div class="flex gap-2 mb-6">
    <button onclick="setBrand('indigo')" class="px-3 py-1.5 text-xs font-semibold rounded-lg bg-indigo-600 text-white">Indigo</button>
    <button onclick="setBrand('rose')" class="px-3 py-1.5 text-xs font-semibold rounded-lg bg-rose-600 text-white">Rose</button>
    <button onclick="setBrand('emerald')" class="px-3 py-1.5 text-xs font-semibold rounded-lg bg-emerald-600 text-white">Emerald</button>
    <button onclick="setBrand('amber')" class="px-3 py-1.5 text-xs font-semibold rounded-lg bg-amber-500 text-white">Amber</button>
  </div>
  
  <button class="px-5 py-2.5 bg-[hsl(var(--brand-600,239_84%_67%))] text-white font-semibold rounded-xl hover:bg-[hsl(var(--brand-700,239_65%_50%))] transition-colors">
    Brand Button
  </button>
</div>

<script>
function setBrand(name) {
  document.getElementById('brand-demo').dataset.brand = name;
}
</script>
```

---

## Step 314: Color Accessibility (Contrast)

```html
<!-- Contrast checker visualization -->
<div class="grid grid-cols-2 gap-4 p-6">
  <!-- PASS: AA -->
  <div class="bg-gray-900 p-4 rounded-xl">
    <p class="text-white font-medium text-sm">White on Gray-900</p>
    <p class="text-gray-400 text-xs mt-1">Contrast: 19.1:1 ✅ AAA</p>
    <div class="mt-2 h-1.5 bg-green-400 rounded-full" style="width:95%"></div>
  </div>
  
  <!-- PASS: AA -->
  <div class="bg-indigo-600 p-4 rounded-xl">
    <p class="text-white font-medium text-sm">White on Indigo-600</p>
    <p class="text-indigo-200 text-xs mt-1">Contrast: 4.5:1 ✅ AA</p>
    <div class="mt-2 h-1.5 bg-green-400 rounded-full" style="width:70%"></div>
  </div>
  
  <!-- FAIL: Too low -->
  <div class="bg-yellow-300 p-4 rounded-xl">
    <p class="text-yellow-500 font-medium text-sm">Yellow-500 on Yellow-300</p>
    <p class="text-yellow-700 text-xs mt-1">Contrast: 1.8:1 ❌ FAIL</p>
    <div class="mt-2 h-1.5 bg-red-400 rounded-full" style="width:20%"></div>
  </div>
  
  <!-- PASS: Large text -->
  <div class="bg-blue-100 p-4 rounded-xl">
    <p class="text-blue-700 font-bold text-xl">Blue-700 on Blue-100</p>
    <p class="text-blue-600 text-xs mt-1">Contrast: 4.5:1 ✅ AA Large</p>
    <div class="mt-2 h-1.5 bg-green-400 rounded-full" style="width:70%"></div>
  </div>
</div>

<!-- Best practice combinations -->
<div class="grid grid-cols-3 gap-3 p-6">
  <div class="bg-gray-900 text-white p-4 rounded-xl text-sm font-medium text-center">Dark Surface</div>
  <div class="bg-indigo-600 text-white p-4 rounded-xl text-sm font-medium text-center">Indigo Brand</div>
  <div class="bg-white border border-gray-200 text-gray-900 p-4 rounded-xl text-sm font-medium text-center">Light Card</div>
  <div class="bg-red-600 text-white p-4 rounded-xl text-sm font-medium text-center">Error Red</div>
  <div class="bg-green-700 text-white p-4 rounded-xl text-sm font-medium text-center">Success Green</div>
  <div class="bg-amber-400 text-amber-900 p-4 rounded-xl text-sm font-medium text-center">Warning</div>
</div>
```

---

## Step 315–320: Workshop — Color Palette Generator

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Color System Workshop</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-6 min-h-screen">

<div class="max-w-5xl mx-auto space-y-8">
  <h1 class="text-3xl font-black text-gray-900">Color System</h1>

  <!-- Tailwind Color Palette -->
  <div class="bg-white rounded-2xl border border-gray-200 p-6">
    <h2 class="font-bold text-gray-900 mb-5">Tailwind Palette</h2>
    <div class="space-y-3" id="palette"></div>
  </div>

  <!-- Brand Theming -->
  <div class="bg-white rounded-2xl border border-gray-200 p-6">
    <h2 class="font-bold text-gray-900 mb-4">Brand Themes</h2>
    <div class="flex flex-wrap gap-2 mb-6">
      <button onclick="applyTheme('indigo')" class="px-4 py-2 bg-indigo-600 text-white text-sm font-semibold rounded-xl">Indigo</button>
      <button onclick="applyTheme('rose')" class="px-4 py-2 bg-rose-600 text-white text-sm font-semibold rounded-xl">Rose</button>
      <button onclick="applyTheme('emerald')" class="px-4 py-2 bg-emerald-600 text-white text-sm font-semibold rounded-xl">Emerald</button>
      <button onclick="applyTheme('violet')" class="px-4 py-2 bg-violet-600 text-white text-sm font-semibold rounded-xl">Violet</button>
      <button onclick="applyTheme('orange')" class="px-4 py-2 bg-orange-500 text-white text-sm font-semibold rounded-xl">Orange</button>
    </div>
    
    <!-- Theme preview card -->
    <div id="theme-preview" class="rounded-2xl overflow-hidden" style="background: hsl(var(--p-600,239 84% 67%))">
      <div class="p-6" style="background: hsl(var(--p-50, 220 100% 97%))">
        <div class="flex items-center gap-3 mb-4">
          <div class="size-10 rounded-xl flex items-center justify-center text-white text-base" style="background: hsl(var(--p-600, 239 84% 67%))">T</div>
          <div>
            <p class="font-bold text-gray-900 text-sm">TailwindPro</p>
            <p class="text-xs text-gray-500">Branded Theme Preview</p>
          </div>
        </div>
        <button class="px-5 py-2.5 text-white text-sm font-semibold rounded-xl transition-opacity hover:opacity-90" style="background: hsl(var(--p-600, 239 84% 67%))">
          Primary Button
        </button>
        <button class="ml-3 px-5 py-2.5 text-sm font-semibold rounded-xl border-2 transition-colors" style="border-color: hsl(var(--p-600, 239 84% 67%)); color: hsl(var(--p-600, 239 84% 67%))">
          Outline Button
        </button>
      </div>
    </div>
  </div>
</div>

<script>
// Tailwind colors to display
const colors = {
  slate:  ['#f8fafc','#f1f5f9','#e2e8f0','#cbd5e1','#94a3b8','#64748b','#475569','#334155','#1e293b','#0f172a','#020617'],
  gray:   ['#f9fafb','#f3f4f6','#e5e7eb','#d1d5db','#9ca3af','#6b7280','#4b5563','#374151','#1f2937','#111827','#030712'],
  red:    ['#fef2f2','#fee2e2','#fecaca','#fca5a5','#f87171','#ef4444','#dc2626','#b91c1c','#991b1b','#7f1d1d','#450a0a'],
  orange: ['#fff7ed','#ffedd5','#fed7aa','#fdba74','#fb923c','#f97316','#ea580c','#c2410c','#9a3412','#7c2d12','#431407'],
  yellow: ['#fefce8','#fef9c3','#fef08a','#fde047','#facc15','#eab308','#ca8a04','#a16207','#854d0e','#713f12','#422006'],
  green:  ['#f0fdf4','#dcfce7','#bbf7d0','#86efac','#4ade80','#22c55e','#16a34a','#15803d','#166534','#14532d','#052e16'],
  blue:   ['#eff6ff','#dbeafe','#bfdbfe','#93c5fd','#60a5fa','#3b82f6','#2563eb','#1d4ed8','#1e40af','#1e3a8a','#172554'],
  indigo: ['#eef2ff','#e0e7ff','#c7d2fe','#a5b4fc','#818cf8','#6366f1','#4f46e5','#4338ca','#3730a3','#312e81','#1e1b4b'],
  purple: ['#faf5ff','#f3e8ff','#e9d5ff','#d8b4fe','#c084fc','#a855f7','#9333ea','#7e22ce','#6b21a8','#581c87','#3b0764'],
  pink:   ['#fdf2f8','#fce7f3','#fbcfe8','#f9a8d4','#f472b6','#ec4899','#db2777','#be185d','#9d174d','#831843','#500724'],
};

const shades = ['50','100','200','300','400','500','600','700','800','900','950'];

const paletteEl = document.getElementById('palette');
Object.entries(colors).forEach(([name, vals]) => {
  const row = document.createElement('div');
  row.className = 'flex items-center gap-2';
  row.innerHTML = `
    <span class="text-xs text-gray-500 w-14 font-medium">${name}</span>
    <div class="flex flex-1 gap-0.5 rounded-lg overflow-hidden h-7">
      ${vals.map((v,i) => `<div class="flex-1 cursor-pointer transition-transform hover:scale-y-125 hover:z-10" style="background:${v}" title="${name}-${shades[i]}: ${v}" onclick="navigator.clipboard&&navigator.clipboard.writeText('bg-${name}-${shades[i]}')"></div>`).join('')}
    </div>
  `;
  paletteEl.appendChild(row);
});

// Brand themes
const themes = {
  indigo:  { 50:'239 100% 97%', 600:'239 84% 67%', 700:'239 72% 57%' },
  rose:    { 50:'355 100% 97%', 600:'346 77% 60%', 700:'346 70% 52%' },
  emerald: { 50:'152 81% 96%',  600:'160 84% 39%', 700:'160 76% 32%' },
  violet:  { 50:'250 100% 97%', 600:'262 83% 67%', 700:'263 70% 57%' },
  orange:  { 50:'34 100% 97%',  600:'25 95% 53%',  700:'22 88% 46%' },
};

function applyTheme(name) {
  const t = themes[name];
  document.documentElement.style.setProperty('--p-50',  t[50]);
  document.documentElement.style.setProperty('--p-600', t[600]);
  document.documentElement.style.setProperty('--p-700', t[700]);
}
</script>

</body>
</html>
```

---

## 📝 สรุป Part 32

| Concept | เทคนิค |
|---------|---------|
| Tailwind palette | `{color}-{50-950}` |
| Semantic colors | CSS variables + tailwind.config |
| Multi-brand | `data-brand` attribute + CSS vars |
| Accessibility | Contrast ratio ≥ 4.5:1 (AA) |
| Dynamic theming | `setProperty()` + `hsl(var(...))` |

---

*Part 32 — Color System | Steps 311–320 จาก 1,000 Steps*
