# Part 61: Design Tokens System

## เป้าหมาย
- Design tokens คืออะไรและทำไมต้องใช้
- Token taxonomy: Global → Alias → Component
- CSS custom properties + Tailwind integration
- Style Dictionary สำหรับ multi-platform tokens
- Steps 601–610

---

## Step 601: Design Token Fundamentals

Design tokens คือ **ชื่อที่แทนค่า design decision** เช่น สี, ขนาดตัวอักษร, spacing

```
Raw value → Global token → Alias token → Component token
#6366f1  → color.violet.500 → color.brand.primary → button.background.default
```

ทำไมต้องใช้ tokens:
- **Consistency** — ใช้ค่าเดียวกันทั่วทั้งระบบ
- **Maintainability** — เปลี่ยนที่เดียว propagate ทุกที่
- **Multi-platform** — Web, iOS, Android ใช้ tokens เดียวกัน

---

## Step 602: Token File Structure (JSON)

`tokens/global.json` — ค่าดิบ:
```json
{
  "color": {
    "violet": {
      "50":  { "value": "#f5f3ff" },
      "100": { "value": "#ede9fe" },
      "200": { "value": "#ddd6fe" },
      "300": { "value": "#c4b5fd" },
      "400": { "value": "#a78bfa" },
      "500": { "value": "#8b5cf6" },
      "600": { "value": "#7c3aed" },
      "700": { "value": "#6d28d9" },
      "800": { "value": "#5b21b6" },
      "900": { "value": "#4c1d95" }
    },
    "gray": {
      "50":  { "value": "#f9fafb" },
      "100": { "value": "#f3f4f6" },
      "200": { "value": "#e5e7eb" },
      "300": { "value": "#d1d5db" },
      "400": { "value": "#9ca3af" },
      "500": { "value": "#6b7280" },
      "600": { "value": "#4b5563" },
      "700": { "value": "#374151" },
      "800": { "value": "#1f2937" },
      "900": { "value": "#111827" }
    }
  },
  "space": {
    "0":  { "value": "0" },
    "1":  { "value": "4px" },
    "2":  { "value": "8px" },
    "3":  { "value": "12px" },
    "4":  { "value": "16px" },
    "5":  { "value": "20px" },
    "6":  { "value": "24px" },
    "8":  { "value": "32px" },
    "10": { "value": "40px" },
    "12": { "value": "48px" },
    "16": { "value": "64px" }
  },
  "radius": {
    "none":  { "value": "0" },
    "sm":    { "value": "4px" },
    "md":    { "value": "8px" },
    "lg":    { "value": "12px" },
    "xl":    { "value": "16px" },
    "2xl":   { "value": "20px" },
    "full":  { "value": "9999px" }
  }
}
```

`tokens/semantic.json` — alias tokens:
```json
{
  "color": {
    "brand": {
      "primary":    { "value": "{color.violet.600}" },
      "secondary":  { "value": "{color.violet.100}" },
      "hover":      { "value": "{color.violet.700}" },
      "focus":      { "value": "{color.violet.500}" }
    },
    "surface": {
      "default":    { "value": "{color.gray.50}" },
      "overlay":    { "value": "#ffffff" },
      "subtle":     { "value": "{color.gray.100}" }
    },
    "text": {
      "primary":    { "value": "{color.gray.900}" },
      "secondary":  { "value": "{color.gray.600}" },
      "placeholder":{ "value": "{color.gray.400}" },
      "inverse":    { "value": "#ffffff" },
      "brand":      { "value": "{color.violet.600}" }
    },
    "border": {
      "default":    { "value": "{color.gray.200}" },
      "subtle":     { "value": "{color.gray.100}" },
      "strong":     { "value": "{color.gray.400}" },
      "brand":      { "value": "{color.violet.600}" }
    },
    "status": {
      "success":    { "value": "#16a34a" },
      "warning":    { "value": "#d97706" },
      "danger":     { "value": "#dc2626" },
      "info":       { "value": "#0284c7" }
    }
  }
}
```

`tokens/component.json` — component tokens:
```json
{
  "button": {
    "primary": {
      "background":       { "value": "{color.brand.primary}" },
      "backgroundHover":  { "value": "{color.brand.hover}" },
      "text":             { "value": "{color.text.inverse}" },
      "border":           { "value": "transparent" },
      "focus":            { "value": "{color.brand.focus}" }
    },
    "outline": {
      "background":       { "value": "transparent" },
      "backgroundHover":  { "value": "{color.brand.secondary}" },
      "text":             { "value": "{color.brand.primary}" },
      "border":           { "value": "{color.brand.primary}" }
    }
  },
  "input": {
    "background":    { "value": "#ffffff" },
    "border":        { "value": "{color.border.default}" },
    "borderFocus":   { "value": "{color.brand.focus}" },
    "borderError":   { "value": "{color.status.danger}" },
    "text":          { "value": "{color.text.primary}" },
    "placeholder":   { "value": "{color.text.placeholder}" }
  },
  "card": {
    "background":    { "value": "#ffffff" },
    "border":        { "value": "{color.border.default}" },
    "radius":        { "value": "{radius.2xl}" }
  }
}
```

---

## Step 603: CSS Custom Properties Output

สร้าง `tokens/output/variables.css` ด้วย script:

```css
/* Auto-generated from design tokens — DO NOT EDIT */

:root {
  /* === Global: Colors === */
  --color-violet-50:  #f5f3ff;
  --color-violet-100: #ede9fe;
  --color-violet-500: #8b5cf6;
  --color-violet-600: #7c3aed;
  --color-violet-700: #6d28d9;
  --color-gray-50:    #f9fafb;
  --color-gray-100:   #f3f4f6;
  --color-gray-200:   #e5e7eb;
  --color-gray-900:   #111827;

  /* === Semantic: Brand === */
  --color-brand-primary:   var(--color-violet-600);
  --color-brand-secondary: var(--color-violet-100);
  --color-brand-hover:     var(--color-violet-700);
  --color-brand-focus:     var(--color-violet-500);

  /* === Semantic: Text === */
  --color-text-primary:     var(--color-gray-900);
  --color-text-secondary:   var(--color-gray-600);
  --color-text-placeholder: var(--color-gray-400);
  --color-text-inverse:     #ffffff;
  --color-text-brand:       var(--color-violet-600);

  /* === Semantic: Surface === */
  --color-surface-default:  var(--color-gray-50);
  --color-surface-overlay:  #ffffff;
  --color-surface-subtle:   var(--color-gray-100);

  /* === Semantic: Border === */
  --color-border-default: var(--color-gray-200);
  --color-border-strong:  var(--color-gray-400);
  --color-border-brand:   var(--color-violet-600);

  /* === Semantic: Status === */
  --color-status-success: #16a34a;
  --color-status-warning: #d97706;
  --color-status-danger:  #dc2626;
  --color-status-info:    #0284c7;

  /* === Spacing === */
  --space-1:  4px;
  --space-2:  8px;
  --space-3:  12px;
  --space-4:  16px;
  --space-5:  20px;
  --space-6:  24px;
  --space-8:  32px;

  /* === Border Radius === */
  --radius-sm:   4px;
  --radius-md:   8px;
  --radius-lg:   12px;
  --radius-xl:   16px;
  --radius-2xl:  20px;
  --radius-full: 9999px;
}

/* === Dark Mode Override === */
[data-theme="dark"],
.dark {
  --color-surface-default:  #0f172a;
  --color-surface-overlay:  var(--color-gray-800, #1f2937);
  --color-surface-subtle:   var(--color-gray-800, #1f2937);
  --color-text-primary:     #f9fafb;
  --color-text-secondary:   #9ca3af;
  --color-border-default:   #374151;
  --color-border-strong:    #4b5563;
}
```

---

## Step 604: Tailwind Config from Tokens

`tailwind.config.ts`:
```ts
import type { Config } from 'tailwindcss'

// Token references ที่ใช้ CSS custom properties
const config: Config = {
  content: ['./src/**/*.{ts,tsx}'],
  darkMode: ['class', '[data-theme="dark"]'],
  theme: {
    extend: {
      colors: {
        brand: {
          primary:   'var(--color-brand-primary)',
          secondary: 'var(--color-brand-secondary)',
          hover:     'var(--color-brand-hover)',
          focus:     'var(--color-brand-focus)',
        },
        surface: {
          default: 'var(--color-surface-default)',
          overlay: 'var(--color-surface-overlay)',
          subtle:  'var(--color-surface-subtle)',
        },
        text: {
          primary:     'var(--color-text-primary)',
          secondary:   'var(--color-text-secondary)',
          placeholder: 'var(--color-text-placeholder)',
          inverse:     'var(--color-text-inverse)',
          brand:       'var(--color-text-brand)',
        },
        border: {
          DEFAULT: 'var(--color-border-default)',
          strong:  'var(--color-border-strong)',
          brand:   'var(--color-border-brand)',
        },
        status: {
          success: 'var(--color-status-success)',
          warning: 'var(--color-status-warning)',
          danger:  'var(--color-status-danger)',
          info:    'var(--color-status-info)',
        },
      },
      borderRadius: {
        sm:   'var(--radius-sm)',
        md:   'var(--radius-md)',
        lg:   'var(--radius-lg)',
        xl:   'var(--radius-xl)',
        '2xl':'var(--radius-2xl)',
      },
      spacing: {
        '1': 'var(--space-1)',
        '2': 'var(--space-2)',
        '3': 'var(--space-3)',
        '4': 'var(--space-4)',
        '5': 'var(--space-5)',
        '6': 'var(--space-6)',
        '8': 'var(--space-8)',
      },
    },
  },
  plugins: [],
}

export default config
```

---

## Step 605: Style Dictionary Setup

```bash
npm install -D style-dictionary
```

`sd.config.js`:
```js
const StyleDictionary = require('style-dictionary')

module.exports = {
  source: ['tokens/**/*.json'],
  platforms: {
    // CSS Variables
    css: {
      transformGroup: 'css',
      prefix: 'token',
      buildPath: 'src/styles/tokens/',
      files: [{
        destination: 'variables.css',
        format: 'css/variables',
        selector: ':root',
      }],
    },
    // JavaScript / TypeScript
    js: {
      transformGroup: 'js',
      buildPath: 'src/styles/tokens/',
      files: [{
        destination: 'tokens.ts',
        format: 'javascript/es6',
      }],
    },
    // Tailwind config object
    tailwind: {
      transforms: ['attribute/cti', 'name/cti/camel', 'color/hex'],
      buildPath: 'src/styles/tokens/',
      files: [{
        destination: 'tailwind-tokens.js',
        format: 'javascript/module',
      }],
    },
  },
}
```

```bash
# Generate tokens
npx style-dictionary build
```

---

## Steps 606–610: Token Demo App

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Design Token System Demo - Steps 606-610</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      darkMode: 'class',
    }
  </script>
  <style>
    :root {
      --brand-primary:   #7c3aed;
      --brand-hover:     #6d28d9;
      --brand-secondary: #ede9fe;
      --brand-focus:     #8b5cf6;
      --surface-bg:      #f9fafb;
      --surface-card:    #ffffff;
      --text-primary:    #111827;
      --text-secondary:  #6b7280;
      --border-default:  #e5e7eb;
      --status-success:  #16a34a;
      --status-danger:   #dc2626;
      --status-warning:  #d97706;
      --radius-card:     1.25rem;
      --space-card:      1.25rem;
    }
    .dark {
      --brand-primary:   #8b5cf6;
      --brand-hover:     #7c3aed;
      --brand-secondary: #2e1065;
      --surface-bg:      #0f172a;
      --surface-card:    #1e293b;
      --text-primary:    #f1f5f9;
      --text-secondary:  #94a3b8;
      --border-default:  #334155;
    }

    /* Token-based utilities */
    .t-card   { background: var(--surface-card); border: 1px solid var(--border-default); border-radius: var(--radius-card); padding: var(--space-card); }
    .t-btn    { background: var(--brand-primary); color: #fff; padding: 0.6rem 1rem; border-radius: 0.75rem; font-size: 0.875rem; font-weight: 600; cursor: pointer; transition: background 0.15s; }
    .t-btn:hover { background: var(--brand-hover); }
    .t-h1    { color: var(--text-primary); font-weight: 800; font-size: 1.25rem; }
    .t-body  { color: var(--text-secondary); font-size: 0.875rem; }
    .t-surface { background: var(--surface-bg); }
  </style>
</head>
<body class="t-surface min-h-screen p-8">
  <div class="max-w-4xl mx-auto space-y-8">

    <!-- Header -->
    <div class="flex items-center justify-between">
      <h1 class="t-h1 text-2xl">Design Token System</h1>
      <button id="themeToggle" class="t-btn">🌙 Dark Mode</button>
    </div>

    <!-- Token Inspector -->
    <div class="t-card space-y-4">
      <h2 class="t-h1">Token Inspector</h2>
      <p class="t-body">เปลี่ยน Token เพื่อดูการเปลี่ยนแปลงทั่วทั้งหน้า</p>

      <div class="grid grid-cols-2 md:grid-cols-3 gap-4">
        <div>
          <label class="t-body block mb-1 font-medium">Brand Primary</label>
          <input type="color" value="#7c3aed" id="brandPrimary"
            class="w-full h-10 rounded-xl border cursor-pointer"
            style="border-color: var(--border-default)">
        </div>
        <div>
          <label class="t-body block mb-1 font-medium">Brand Hover</label>
          <input type="color" value="#6d28d9" id="brandHover"
            class="w-full h-10 rounded-xl border cursor-pointer"
            style="border-color: var(--border-default)">
        </div>
        <div>
          <label class="t-body block mb-1 font-medium">Card Background</label>
          <input type="color" value="#ffffff" id="cardBg"
            class="w-full h-10 rounded-xl border cursor-pointer"
            style="border-color: var(--border-default)">
        </div>
      </div>

      <button id="resetTokens" class="text-sm font-medium" style="color: var(--brand-primary)">⟳ Reset Tokens</button>
    </div>

    <!-- Component Preview -->
    <div class="t-card">
      <h2 class="t-h1 mb-4">Component Preview</h2>
      <div class="flex flex-wrap gap-3">
        <button class="t-btn">Primary Button</button>
        <button style="background: transparent; border: 2px solid var(--brand-primary); color: var(--brand-primary); padding: 0.6rem 1rem; border-radius: 0.75rem; font-size: 0.875rem; font-weight: 600; cursor: pointer;">Outline Button</button>
        <button style="background: var(--brand-secondary); color: var(--brand-primary); padding: 0.6rem 1rem; border-radius: 0.75rem; font-size: 0.875rem; font-weight: 600; cursor: pointer;">Subtle Button</button>
      </div>
    </div>

    <!-- Status Colors -->
    <div class="t-card">
      <h2 class="t-h1 mb-4">Status Tokens</h2>
      <div class="grid grid-cols-2 md:grid-cols-4 gap-3">
        <div class="p-3 rounded-xl text-sm font-semibold text-white" style="background: var(--status-success)">✅ Success</div>
        <div class="p-3 rounded-xl text-sm font-semibold text-white" style="background: var(--status-danger)">❌ Danger</div>
        <div class="p-3 rounded-xl text-sm font-semibold text-white" style="background: var(--status-warning)">⚠️ Warning</div>
        <div class="p-3 rounded-xl text-sm font-semibold text-white" style="background: #0284c7">ℹ️ Info</div>
      </div>
    </div>

    <!-- Token Table -->
    <div class="t-card overflow-hidden" style="padding: 0">
      <div class="p-5 border-b" style="border-color: var(--border-default)">
        <h2 class="t-h1">Active Tokens</h2>
      </div>
      <div class="overflow-x-auto">
        <table class="w-full text-sm">
          <thead style="background: var(--surface-bg)">
            <tr>
              <th class="px-5 py-3 text-left t-body font-semibold">Token</th>
              <th class="px-5 py-3 text-left t-body font-semibold">Value</th>
              <th class="px-5 py-3 text-left t-body font-semibold">Preview</th>
            </tr>
          </thead>
          <tbody id="tokenTable" class="divide-y" style="border-color: var(--border-default)">
          </tbody>
        </table>
      </div>
    </div>

  </div>

  <script>
    // Theme toggle
    document.getElementById('themeToggle').addEventListener('click', function() {
      document.documentElement.classList.toggle('dark');
      this.textContent = document.documentElement.classList.contains('dark') ? '☀️ Light Mode' : '🌙 Dark Mode';
    });

    // Token editor
    const defaults = {
      '--brand-primary': '#7c3aed',
      '--brand-hover':   '#6d28d9',
      '--surface-card':  '#ffffff',
    };

    document.getElementById('brandPrimary').addEventListener('input', e => document.documentElement.style.setProperty('--brand-primary', e.target.value));
    document.getElementById('brandHover').addEventListener('input', e => document.documentElement.style.setProperty('--brand-hover', e.target.value));
    document.getElementById('cardBg').addEventListener('input', e => document.documentElement.style.setProperty('--surface-card', e.target.value));

    document.getElementById('resetTokens').addEventListener('click', () => {
      Object.entries(defaults).forEach(([k, v]) => document.documentElement.style.setProperty(k, v));
      document.getElementById('brandPrimary').value = defaults['--brand-primary'];
      document.getElementById('brandHover').value = defaults['--brand-hover'];
      document.getElementById('cardBg').value = defaults['--surface-card'];
    });

    // Token table
    const tokens = [
      ['--brand-primary', 'color'],
      ['--brand-hover', 'color'],
      ['--brand-secondary', 'color'],
      ['--surface-bg', 'color'],
      ['--surface-card', 'color'],
      ['--text-primary', 'color'],
      ['--text-secondary', 'color'],
      ['--border-default', 'color'],
      ['--status-success', 'color'],
      ['--status-danger', 'color'],
    ];

    function renderTable() {
      const tbody = document.getElementById('tokenTable');
      const styles = getComputedStyle(document.documentElement);
      tbody.innerHTML = tokens.map(([name, type]) => {
        const val = styles.getPropertyValue(name).trim();
        return `<tr>
          <td class="px-5 py-3 font-mono text-xs" style="color: var(--text-secondary)">${name}</td>
          <td class="px-5 py-3 font-mono text-xs" style="color: var(--text-primary)">${val}</td>
          <td class="px-5 py-3">
            <div class="w-6 h-6 rounded" style="background: ${val}; border: 1px solid var(--border-default)"></div>
          </td>
        </tr>`;
      }).join('');
    }
    renderTable();
    setInterval(renderTable, 500);
  </script>
</body>
</html>
```

---

## สรุป Part 61

| Step | เนื้อหา |
|------|---------|
| 601 | Token taxonomy: Global → Alias → Component |
| 602 | Token JSON files: global.json, semantic.json, component.json |
| 603 | CSS custom properties output |
| 604 | Tailwind config ที่ reference CSS variables |
| 605 | Style Dictionary setup + platforms: CSS/JS/Tailwind |
| 606 | Token Inspector UI |
| 607 | Component Preview ด้วย tokens |
| 608 | Status color tokens |
| 609 | Dark mode token override |
| 610 | Workshop: Interactive Token System — inspector + editor + live table |

**Part ถัดไป:** Part 62 — Multi-Brand Enterprise System (Steps 611–620)
