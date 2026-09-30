# Part 79: Theming System

## เป้าหมาย
- CSS Custom Properties + Tailwind theme tokens
- Dark mode — class strategy + system preference
- Multi-brand theming
- Dynamic theme switcher
- Steps 781–790

---

## Step 781: CSS Custom Properties เป็น Theme Tokens

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Theme Tokens</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      theme: {
        extend: {
          colors: {
            // ใช้ CSS var เป็น token
            primary:   { DEFAULT: 'var(--color-primary)',   foreground: 'var(--color-primary-fg)' },
            surface:   { DEFAULT: 'var(--color-surface)',   border: 'var(--color-surface-border)' },
            muted:     { DEFAULT: 'var(--color-muted)',     foreground: 'var(--color-muted-fg)' },
          },
        },
      },
    }
  </script>
  <style>
    /* Default theme (light) */
    :root {
      --color-primary:    #4f46e5;
      --color-primary-fg: #ffffff;
      --color-surface:    #ffffff;
      --color-surface-border: #e5e7eb;
      --color-muted:      #f9fafb;
      --color-muted-fg:   #6b7280;
    }
    /* Dark theme */
    [data-theme="dark"] {
      --color-primary:    #818cf8;
      --color-primary-fg: #1e1b4b;
      --color-surface:    #1e293b;
      --color-surface-border: #334155;
      --color-muted:      #0f172a;
      --color-muted-fg:   #94a3b8;
    }
  </style>
</head>
<body class="bg-muted min-h-screen p-8">
  <div class="max-w-sm mx-auto bg-surface border border-surface-border rounded-2xl p-6 shadow-sm">
    <h1 class="text-xl font-bold text-gray-900 mb-4">Theme Tokens Demo</h1>
    <button class="bg-primary text-primary-foreground px-4 py-2 rounded-lg text-sm font-semibold">
      Primary Button
    </button>
    <p class="mt-3 text-sm text-muted-foreground">Muted foreground text</p>
    <button onclick="toggleTheme()" class="mt-4 text-xs border border-surface-border px-3 py-1.5 rounded-lg w-full">
      Toggle Dark
    </button>
  </div>
  <script>
    function toggleTheme() {
      document.documentElement.dataset.theme =
        document.documentElement.dataset.theme === 'dark' ? '' : 'dark';
    }
  </script>
</body>
</html>
```

---

## Step 782: Dark Mode — class strategy

```js
// tailwind.config
tailwind.config = {
  darkMode: 'class', // สลับ dark mode ด้วย class="dark" บน <html>
  // darkMode: 'media' — ใช้ prefers-color-scheme แทน class
}
```

```html
<!-- class="dark" บน <html> เปิด dark variant -->
<html class="dark">
  <body class="bg-white dark:bg-slate-900 text-gray-900 dark:text-slate-100">
    <div class="bg-gray-50 dark:bg-slate-800 rounded-2xl p-6">
      <h1 class="font-bold text-gray-900 dark:text-white">Dark Mode Card</h1>
      <p class="text-gray-500 dark:text-slate-400">รองรับ dark mode ด้วย dark: prefix</p>
      <button class="mt-3 bg-indigo-600 dark:bg-indigo-500 text-white px-4 py-2 rounded-lg text-sm">
        Action
      </button>
    </div>
  </body>
</html>
```

---

## Step 783: System Preference Detection

```js
// อ่าน system preference แล้ว apply class
function initTheme() {
  const saved = localStorage.getItem('theme')
  const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches

  if (saved === 'dark' || (!saved && prefersDark)) {
    document.documentElement.classList.add('dark')
  }
}
initTheme()

// Toggle + persist
function toggleDark() {
  const isDark = document.documentElement.classList.toggle('dark')
  localStorage.setItem('theme', isDark ? 'dark' : 'light')
}

// ฟัง system change (ถ้า user ไม่ได้ set manual)
window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', e => {
  if (!localStorage.getItem('theme')) {
    document.documentElement.classList.toggle('dark', e.matches)
  }
})
```

---

## Step 784: Multi-Brand Theming

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Multi-Brand Theme</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    :root, [data-brand="default"] {
      --brand-50:  #eef2ff; --brand-100: #e0e7ff;
      --brand-500: #6366f1; --brand-600: #4f46e5; --brand-700: #4338ca;
      --brand-fg:  #ffffff; --brand-name: "Indigo";
    }
    [data-brand="rose"] {
      --brand-50:  #fff1f2; --brand-100: #ffe4e6;
      --brand-500: #f43f5e; --brand-600: #e11d48; --brand-700: #be123c;
      --brand-fg:  #ffffff; --brand-name: "Rose";
    }
    [data-brand="emerald"] {
      --brand-50:  #ecfdf5; --brand-100: #d1fae5;
      --brand-500: #10b981; --brand-600: #059669; --brand-700: #047857;
      --brand-fg:  #ffffff; --brand-name: "Emerald";
    }
    [data-brand="amber"] {
      --brand-50:  #fffbeb; --brand-100: #fef3c7;
      --brand-500: #f59e0b; --brand-600: #d97706; --brand-700: #b45309;
      --brand-fg:  #1c1917; --brand-name: "Amber";
    }
    .btn-brand { background: var(--brand-600); color: var(--brand-fg); }
    .btn-brand:hover { background: var(--brand-700); }
    .badge-brand { background: var(--brand-100); color: var(--brand-700); }
    .ring-brand { outline: 2px solid var(--brand-500); }
  </style>
</head>
<body class="bg-gray-100 min-h-screen p-8">
  <div class="max-w-xl mx-auto space-y-6">
    <!-- Brand Selector -->
    <div class="bg-white rounded-2xl p-4 flex gap-2 flex-wrap">
      <span class="text-sm font-medium text-gray-600 mr-2 self-center">Brand:</span>
      <button onclick="setBrand('default')" class="px-3 py-1 text-xs rounded-full bg-indigo-100 text-indigo-700 font-medium">Indigo</button>
      <button onclick="setBrand('rose')"    class="px-3 py-1 text-xs rounded-full bg-rose-100 text-rose-700 font-medium">Rose</button>
      <button onclick="setBrand('emerald')" class="px-3 py-1 text-xs rounded-full bg-emerald-100 text-emerald-700 font-medium">Emerald</button>
      <button onclick="setBrand('amber')"   class="px-3 py-1 text-xs rounded-full bg-amber-100 text-amber-700 font-medium">Amber</button>
    </div>
    <!-- Demo Card -->
    <div id="card" class="bg-white rounded-2xl p-6 shadow-sm">
      <div class="flex items-center gap-3 mb-4">
        <div class="w-10 h-10 rounded-xl btn-brand flex items-center justify-center text-lg font-black">A</div>
        <div>
          <h2 class="font-bold text-gray-900">Brand Preview</h2>
          <span class="badge-brand text-xs px-2 py-0.5 rounded-full font-medium">Active</span>
        </div>
      </div>
      <p class="text-gray-500 text-sm mb-4">สลับ brand ด้านบนเพื่อดูการเปลี่ยนสี theme ทั้งหมด</p>
      <div class="flex gap-2">
        <button class="btn-brand px-4 py-2 rounded-lg text-sm font-semibold transition-colors">Primary</button>
        <button class="badge-brand px-4 py-2 rounded-lg text-sm font-semibold">Secondary</button>
      </div>
    </div>
  </div>
  <script>
    function setBrand(name) {
      document.getElementById('card').dataset.brand = name;
    }
  </script>
</body>
</html>
```

---

## Step 785: Theme Switcher UI Component

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Theme Switcher</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script>tailwind.config = { darkMode: 'class' }</script>
</head>
<body class="bg-gray-100 dark:bg-slate-900 min-h-screen flex items-center justify-center transition-colors duration-300">
  <div class="bg-white dark:bg-slate-800 rounded-2xl shadow-lg p-8 w-80">
    <div class="flex items-center justify-between mb-6">
      <h2 class="font-bold text-gray-900 dark:text-white">Theme Switcher</h2>
      <!-- 3-way toggle: light / system / dark -->
      <div class="flex bg-gray-100 dark:bg-slate-700 rounded-xl p-1 gap-1" role="group">
        <button id="btn-light"  onclick="setTheme('light')"  class="theme-btn px-3 py-1 rounded-lg text-xs font-medium transition-all">☀️</button>
        <button id="btn-system" onclick="setTheme('system')" class="theme-btn px-3 py-1 rounded-lg text-xs font-medium transition-all">💻</button>
        <button id="btn-dark"   onclick="setTheme('dark')"   class="theme-btn px-3 py-1 rounded-lg text-xs font-medium transition-all">🌙</button>
      </div>
    </div>
    <p class="text-gray-500 dark:text-slate-400 text-sm" id="theme-label">System default</p>
    <div class="mt-4 grid grid-cols-3 gap-2">
      <div class="h-12 rounded-xl bg-indigo-500"></div>
      <div class="h-12 rounded-xl bg-gray-200 dark:bg-slate-700"></div>
      <div class="h-12 rounded-xl bg-white dark:bg-slate-900 border border-gray-200 dark:border-slate-600"></div>
    </div>
  </div>
  <script>
    const labels = { light: 'Light mode', system: 'System default', dark: 'Dark mode' };
    let current = localStorage.getItem('theme') || 'system';

    function applyTheme(t) {
      if (t === 'dark') document.documentElement.classList.add('dark');
      else if (t === 'light') document.documentElement.classList.remove('dark');
      else {
        const sysDark = matchMedia('(prefers-color-scheme: dark)').matches;
        document.documentElement.classList.toggle('dark', sysDark);
      }
    }

    function setTheme(t) {
      current = t;
      localStorage.setItem('theme', t);
      applyTheme(t);
      document.querySelectorAll('.theme-btn').forEach(b => {
        b.classList.toggle('bg-white', b.id === `btn-${t}`);
        b.classList.toggle('shadow-sm', b.id === `btn-${t}`);
        b.classList.toggle('dark:bg-slate-600', b.id === `btn-${t}`);
      });
      document.getElementById('theme-label').textContent = labels[t];
    }

    applyTheme(current);
    setTheme(current);
  </script>
</body>
</html>
```

---

## Step 786: Color Palette Generator

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Palette Generator</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 min-h-screen p-8">
  <div class="max-w-2xl mx-auto">
    <h1 class="text-2xl font-extrabold text-gray-900 mb-6">Tailwind Palette Explorer</h1>
    <div id="palette" class="space-y-2"></div>
  </div>
  <script>
    const colors = {
      slate: ['#f8fafc','#f1f5f9','#e2e8f0','#cbd5e1','#94a3b8','#64748b','#475569','#334155','#1e293b','#0f172a','#020617'],
      indigo:['#eef2ff','#e0e7ff','#c7d2fe','#a5b4fc','#818cf8','#6366f1','#4f46e5','#4338ca','#3730a3','#312e81','#1e1b4b'],
      rose:  ['#fff1f2','#ffe4e6','#fecdd3','#fda4af','#fb7185','#f43f5e','#e11d48','#be123c','#9f1239','#881337','#4c0519'],
      emerald:['#ecfdf5','#d1fae5','#a7f3d0','#6ee7b7','#34d399','#10b981','#059669','#047857','#065f46','#064e3b','#022c22'],
      amber: ['#fffbeb','#fef3c7','#fde68a','#fcd34d','#fbbf24','#f59e0b','#d97706','#b45309','#92400e','#78350f','#451a03'],
    };
    const shades = [50,100,200,300,400,500,600,700,800,900,950];
    document.getElementById('palette').innerHTML = Object.entries(colors).map(([name,hex]) => `
      <div>
        <p class="text-xs font-semibold text-gray-500 uppercase tracking-wide mb-1">${name}</p>
        <div class="flex rounded-xl overflow-hidden">
          ${hex.map((h,i) => `
            <div class="flex-1 h-10 relative group cursor-pointer" style="background:${h}" title="${name}-${shades[i]}">
              <div class="absolute inset-0 flex items-center justify-center opacity-0 group-hover:opacity-100 bg-black/20 transition-opacity">
                <span class="text-white text-[9px] font-bold">${shades[i]}</span>
              </div>
            </div>
          `).join('')}
        </div>
      </div>
    `).join('');
  </script>
</body>
</html>
```

---

## Step 787: Semantic Color Mapping

```ts
// tailwind.config.ts — semantic names บน design tokens
export default {
  theme: {
    extend: {
      colors: {
        // Semantic — ตั้งชื่อตาม "ทำอะไร" ไม่ใช่ "สีอะไร"
        background: 'hsl(var(--background))',
        foreground:  'hsl(var(--foreground))',
        card:        { DEFAULT: 'hsl(var(--card))', foreground: 'hsl(var(--card-foreground))' },
        popover:     { DEFAULT: 'hsl(var(--popover))', foreground: 'hsl(var(--popover-foreground))' },
        primary:     { DEFAULT: 'hsl(var(--primary))', foreground: 'hsl(var(--primary-foreground))' },
        secondary:   { DEFAULT: 'hsl(var(--secondary))', foreground: 'hsl(var(--secondary-foreground))' },
        muted:       { DEFAULT: 'hsl(var(--muted))', foreground: 'hsl(var(--muted-foreground))' },
        accent:      { DEFAULT: 'hsl(var(--accent))', foreground: 'hsl(var(--accent-foreground))' },
        destructive: { DEFAULT: 'hsl(var(--destructive))', foreground: 'hsl(var(--destructive-foreground))' },
        border:      'hsl(var(--border))',
        input:       'hsl(var(--input))',
        ring:        'hsl(var(--ring))',
      },
      borderRadius: {
        lg: 'var(--radius)',
        md: 'calc(var(--radius) - 2px)',
        sm: 'calc(var(--radius) - 4px)',
      },
    },
  },
}
```

```css
/* globals.css */
:root {
  --background: 0 0% 100%;
  --foreground: 222.2 84% 4.9%;
  --primary: 222.2 47.4% 11.2%;
  --primary-foreground: 210 40% 98%;
  --radius: 0.5rem;
}
.dark {
  --background: 222.2 84% 4.9%;
  --foreground: 210 40% 98%;
  --primary: 210 40% 98%;
  --primary-foreground: 222.2 47.4% 11.2%;
}
```

---

## Step 788: Theme Animation

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Theme Animation</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script>tailwind.config = { darkMode: 'class' }</script>
  <style>
    /* Smooth theme transition ทุก element */
    *, *::before, *::after {
      transition: background-color 0.3s ease, border-color 0.3s ease, color 0.2s ease;
    }
    /* Circular reveal animation */
    @keyframes reveal {
      from { clip-path: circle(0% at var(--cx) var(--cy)); }
      to   { clip-path: circle(150% at var(--cx) var(--cy)); }
    }
    .theme-reveal {
      animation: reveal 0.5s ease forwards;
    }
  </style>
</head>
<body class="bg-white dark:bg-slate-900 min-h-screen transition-colors duration-300">
  <div class="max-w-lg mx-auto pt-16 px-6">
    <div class="flex items-center justify-between mb-8">
      <h1 class="text-2xl font-extrabold text-gray-900 dark:text-white">Theme Animation</h1>
      <button id="toggle" class="relative w-14 h-7 bg-gray-200 dark:bg-indigo-600 rounded-full transition-colors duration-300 focus:outline-none focus-visible:ring-2 focus-visible:ring-indigo-500">
        <span id="knob" class="absolute left-1 top-1 w-5 h-5 bg-white rounded-full shadow transition-transform duration-300 dark:translate-x-7 flex items-center justify-center text-xs">
          <span class="dark:hidden">☀️</span>
          <span class="hidden dark:inline">🌙</span>
        </span>
      </button>
    </div>
    <div class="grid grid-cols-2 gap-4">
      <div class="bg-gray-50 dark:bg-slate-800 rounded-2xl p-4 border border-gray-200 dark:border-slate-700">
        <div class="w-8 h-8 bg-indigo-500 rounded-xl mb-3"></div>
        <p class="font-semibold text-gray-900 dark:text-white text-sm">Card 1</p>
        <p class="text-gray-500 dark:text-slate-400 text-xs mt-1">Dark mode รองรับทุก element</p>
      </div>
      <div class="bg-indigo-600 dark:bg-indigo-500 rounded-2xl p-4">
        <div class="w-8 h-8 bg-white/20 rounded-xl mb-3"></div>
        <p class="font-semibold text-white text-sm">Card 2</p>
        <p class="text-indigo-200 text-xs mt-1">Brand color card</p>
      </div>
    </div>
  </div>
  <script>
    document.getElementById('toggle').addEventListener('click', function(e) {
      const rect = e.currentTarget.getBoundingClientRect();
      const cx = rect.left + rect.width / 2;
      const cy = rect.top + rect.height / 2;
      document.documentElement.style.setProperty('--cx', cx + 'px');
      document.documentElement.style.setProperty('--cy', cy + 'px');
      document.documentElement.classList.toggle('dark');
    });
  </script>
</body>
</html>
```

---

## Step 789: Theme Provider Pattern (React)

```tsx
// theme-context.tsx
import { createContext, useContext, useEffect, useState } from 'react'

type Theme = 'light' | 'dark' | 'system'

interface ThemeCtx {
  theme: Theme
  resolvedTheme: 'light' | 'dark'
  setTheme: (t: Theme) => void
}

const ThemeContext = createContext<ThemeCtx | null>(null)

export function ThemeProvider({ children }: { children: React.ReactNode }) {
  const [theme, setThemeState] = useState<Theme>(() =>
    (localStorage.getItem('theme') as Theme) || 'system'
  )
  const [resolved, setResolved] = useState<'light' | 'dark'>('light')

  useEffect(() => {
    const mq = matchMedia('(prefers-color-scheme: dark)')
    const resolve = (t: Theme) => t === 'system' ? (mq.matches ? 'dark' : 'light') : t

    function apply(t: Theme) {
      const r = resolve(t)
      document.documentElement.classList.toggle('dark', r === 'dark')
      setResolved(r)
    }

    apply(theme)
    const handler = () => theme === 'system' && apply('system')
    mq.addEventListener('change', handler)
    return () => mq.removeEventListener('change', handler)
  }, [theme])

  function setTheme(t: Theme) {
    setThemeState(t)
    localStorage.setItem('theme', t)
  }

  return (
    <ThemeContext.Provider value={{ theme, resolvedTheme: resolved, setTheme }}>
      {children}
    </ThemeContext.Provider>
  )
}

export const useTheme = () => {
  const ctx = useContext(ThemeContext)
  if (!ctx) throw new Error('useTheme must be inside ThemeProvider')
  return ctx
}
```

```tsx
// ThemeToggle.tsx
import { useTheme } from './theme-context'

export function ThemeToggle() {
  const { theme, setTheme } = useTheme()
  const options: Array<{ value: 'light'|'dark'|'system', icon: string }> = [
    { value: 'light',  icon: '☀️' },
    { value: 'system', icon: '💻' },
    { value: 'dark',   icon: '🌙' },
  ]
  return (
    <div className="flex bg-gray-100 dark:bg-slate-800 rounded-xl p-1 gap-1">
      {options.map(o => (
        <button
          key={o.value}
          onClick={() => setTheme(o.value)}
          className={`px-3 py-1.5 rounded-lg text-sm transition-all ${
            theme === o.value
              ? 'bg-white dark:bg-slate-600 shadow-sm'
              : 'text-gray-500 dark:text-slate-400'
          }`}
          aria-pressed={theme === o.value}
        >
          {o.icon}
        </button>
      ))}
    </div>
  )
}
```

---

## Step 790: Workshop — Complete Theming System

```html
<!DOCTYPE html>
<html lang="th" data-theme="light">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Theming Workshop</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    :root, [data-theme="light"] {
      --bg: #f9fafb; --surface: #ffffff; --border: #e5e7eb;
      --text: #111827; --muted: #6b7280;
      --primary: #4f46e5; --primary-fg: #fff;
    }
    [data-theme="dark"] {
      --bg: #0f172a; --surface: #1e293b; --border: #334155;
      --text: #f1f5f9; --muted: #94a3b8;
      --primary: #818cf8; --primary-fg: #1e1b4b;
    }
    [data-theme="sunset"] {
      --bg: #fff7ed; --surface: #ffffff; --border: #fed7aa;
      --text: #431407; --muted: #9a3412;
      --primary: #ea580c; --primary-fg: #fff;
    }
    body { background: var(--bg); color: var(--text); transition: background .3s, color .2s; }
    .card { background: var(--surface); border: 1px solid var(--border); }
    .btn  { background: var(--primary); color: var(--primary-fg); }
    .muted { color: var(--muted); }
  </style>
</head>
<body class="min-h-screen p-8">
  <div class="max-w-lg mx-auto">
    <div class="flex items-center justify-between mb-6">
      <h1 class="text-xl font-extrabold">Theme Workshop</h1>
      <div class="flex gap-2">
        <button onclick="setTheme('light')"  class="px-3 py-1 text-xs rounded-lg btn font-medium opacity-80 hover:opacity-100 transition-opacity">Light</button>
        <button onclick="setTheme('dark')"   class="px-3 py-1 text-xs rounded-lg btn font-medium opacity-80 hover:opacity-100 transition-opacity">Dark</button>
        <button onclick="setTheme('sunset')" class="px-3 py-1 text-xs rounded-lg btn font-medium opacity-80 hover:opacity-100 transition-opacity">Sunset</button>
      </div>
    </div>
    <div class="card rounded-2xl p-6 mb-4">
      <h2 class="font-bold mb-1">Product Card</h2>
      <p class="muted text-sm mb-3">สีทุก element ปรับตาม theme อัตโนมัติ</p>
      <div class="flex gap-2">
        <button class="btn px-4 py-2 rounded-xl text-sm font-semibold transition-all hover:opacity-90">Buy Now</button>
        <button class="px-4 py-2 rounded-xl text-sm border" style="border-color:var(--border)">Learn More</button>
      </div>
    </div>
    <div class="card rounded-2xl p-4">
      <p class="text-xs font-semibold muted uppercase tracking-wide mb-2">Active Theme</p>
      <p class="font-bold text-lg" id="theme-name">Light</p>
    </div>
  </div>
  <script>
    const names = { light: 'Light ☀️', dark: 'Dark 🌙', sunset: 'Sunset 🌅' };
    function setTheme(t) {
      document.documentElement.dataset.theme = t;
      document.getElementById('theme-name').textContent = names[t];
    }
  </script>
</body>
</html>
```

---

## สรุป Part 79

| Step | เนื้อหา |
|------|---------|
| 781 | CSS Custom Properties เป็น theme tokens |
| 782 | Dark mode — class strategy + dark: prefix |
| 783 | System preference detection + persist |
| 784 | Multi-brand theming ด้วย data-brand |
| 785 | Theme switcher UI — 3-way toggle |
| 786 | Color palette explorer |
| 787 | Semantic color mapping (shadcn/ui style) |
| 788 | Theme animation — circular reveal |
| 789 | React ThemeProvider + useTheme hook |
| 790 | Workshop: Complete theming system |

**Part ถัดไป:** Part 80 — Design System Component Library (Steps 791–800)
