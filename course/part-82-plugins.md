# Part 82: Advanced Tailwind Plugins

## เป้าหมาย
- @tailwindcss/typography (@prose)
- @tailwindcss/forms
- @tailwindcss/aspect-ratio
- สร้าง custom plugin
- Steps 811–820

---

## Step 811: @tailwindcss/typography — Prose

```bash
npm install -D @tailwindcss/typography
```

```js
// tailwind.config.js
module.exports = {
  plugins: [require('@tailwindcss/typography')],
}
```

```html
<!-- prose class จัดการ typography ของ HTML content ทั้งหมด -->
<article class="prose prose-indigo lg:prose-lg max-w-none">
  <h1>Article Title</h1>
  <p>Content with <strong>bold</strong> and <em>italic</em> text.</p>
  <ul>
    <li>List item one</li>
    <li>List item two</li>
  </ul>
  <pre><code>const hello = "world";</code></pre>
  <blockquote>A great quote from someone.</blockquote>
  <a href="#">A link</a>
</article>

<!-- Sizes: prose-sm, prose, prose-lg, prose-xl, prose-2xl -->
<!-- Colors: prose-gray, prose-indigo, prose-sky, prose-rose, etc. -->
<!-- Dark: dark:prose-invert (invert สำหรับ dark background) -->
```

---

## Step 812: Prose Customization

```js
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      typography: ({ theme }) => ({
        DEFAULT: {
          css: {
            // Custom heading color
            h1: { color: theme('colors.gray.900'), fontWeight: '900' },
            h2: { color: theme('colors.gray.800'), fontWeight: '800' },
            // Custom link
            a: {
              color: theme('colors.indigo.600'),
              textDecoration: 'none',
              fontWeight: '600',
              '&:hover': { color: theme('colors.indigo.800'), textDecoration: 'underline' },
            },
            // Remove max-width from prose (handle externally)
            maxWidth: 'none',
            // Custom code block
            pre: {
              backgroundColor: theme('colors.gray.900'),
              color: theme('colors.gray.100'),
              borderRadius: theme('borderRadius.2xl'),
            },
          },
        },
        // Dark mode override
        invert: {
          css: {
            h1: { color: theme('colors.gray.100') },
            a: { color: theme('colors.indigo.400') },
          },
        },
      }),
    },
  },
}
```

---

## Step 813: @tailwindcss/forms

```bash
npm install -D @tailwindcss/forms
```

```js
// tailwind.config.js
module.exports = {
  plugins: [
    require('@tailwindcss/forms'),
    // หรือ config class strategy (ไม่ reset global)
    require('@tailwindcss/forms')({ strategy: 'class' }),
  ],
}
```

```html
<!-- plugin reset input styles ให้ consistent cross-browser -->
<input type="text" class="mt-1 block w-full rounded-xl border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm">

<select class="mt-1 block w-full rounded-xl border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm">
  <option>Option A</option>
  <option>Option B</option>
</select>

<!-- class strategy -->
<input type="text" class="form-input mt-1 block w-full rounded-xl">
<select class="form-select mt-1 block w-full rounded-xl">...</select>
<input type="checkbox" class="form-checkbox rounded text-indigo-600">
<input type="radio"    class="form-radio text-indigo-600">
<textarea class="form-textarea mt-1 block w-full rounded-xl"></textarea>
```

---

## Step 814: @tailwindcss/aspect-ratio

```bash
npm install -D @tailwindcss/aspect-ratio
```

```js
module.exports = { plugins: [require('@tailwindcss/aspect-ratio')] }
```

```html
<!-- Note: Tailwind v3 มี aspect-ratio built-in แล้ว -->
<!-- Plugin ช่วย compatibility กับ browser เก่า -->

<!-- Built-in (v3+) -->
<div class="aspect-video bg-gray-200 rounded-2xl overflow-hidden">...</div>
<div class="aspect-square ...">...</div>
<div class="aspect-[4/3] ...">...</div>
<div class="aspect-[16/9] ...">...</div>

<!-- Plugin (legacy) -->
<div class="aspect-w-16 aspect-h-9">
  <iframe src="..." class="w-full h-full"></iframe>
</div>
```

---

## Step 815: สร้าง Custom Plugin — Basic

```js
// tailwind.config.js
const plugin = require('tailwindcss/plugin')

module.exports = {
  plugins: [
    plugin(function({ addUtilities, addComponents, theme, e }) {
      // addUtilities — เพิ่ม utility classes
      addUtilities({
        '.scrollbar-hide': {
          '-ms-overflow-style': 'none',
          'scrollbar-width': 'none',
          '&::-webkit-scrollbar': { display: 'none' },
        },
        '.text-balance': { 'text-wrap': 'balance' },
        '.text-pretty':  { 'text-wrap': 'pretty' },
        '.drag-none':    { '-webkit-user-drag': 'none', 'user-select': 'none' },
      })
    }),
  ],
}
```

```html
<!-- ใช้ custom utility -->
<div class="overflow-x-auto scrollbar-hide flex gap-4 pb-4">
  <!-- horizontal scroll without scrollbar -->
</div>
<h1 class="text-balance">Heading that balances line lengths</h1>
```

---

## Step 816: Custom Plugin — Components + Variants

```js
const plugin = require('tailwindcss/plugin')

module.exports = {
  plugins: [
    plugin(function({ addComponents, addVariant, theme }) {
      // addComponents — เพิ่ม component classes
      addComponents({
        '.btn': {
          display: 'inline-flex',
          alignItems: 'center',
          justifyContent: 'center',
          padding: `${theme('spacing.2')} ${theme('spacing.4')}`,
          fontSize: theme('fontSize.sm'),
          fontWeight: theme('fontWeight.semibold'),
          borderRadius: theme('borderRadius.xl'),
          transition: 'all 150ms',
          cursor: 'pointer',
          '&:disabled': { opacity: '0.5', cursor: 'not-allowed' },
        },
        '.btn-primary': {
          backgroundColor: theme('colors.indigo.600'),
          color: '#fff',
          '&:hover': { backgroundColor: theme('colors.indigo.700') },
        },
        '.btn-secondary': {
          backgroundColor: theme('colors.gray.100'),
          color: theme('colors.gray.800'),
          '&:hover': { backgroundColor: theme('colors.gray.200') },
        },
      })

      // addVariant — เพิ่ม custom variant
      addVariant('hocus', ['&:hover', '&:focus-visible'])
      addVariant('group-hocus', ['.group:hover &', '.group:focus-visible &'])
      addVariant('not-last', '&:not(:last-child)')
      addVariant('supports-grid', '@supports (display: grid)')
    }),
  ],
}
```

```html
<!-- ใช้ custom components + variants -->
<button class="btn btn-primary">Primary</button>
<button class="btn btn-secondary">Secondary</button>
<a class="hocus:text-indigo-600 hocus:underline transition-colors">Link</a>
<li class="not-last:border-b border-gray-100">List item</li>
```

---

## Step 817: Custom Plugin — Dynamic Utilities

```js
const plugin = require('tailwindcss/plugin')

module.exports = {
  theme: {
    extend: {
      // เพิ่ม custom scale
      fluidSize: {
        sm:  ['1rem',  '1.25rem'],
        md:  ['1.25rem', '1.75rem'],
        lg:  ['1.5rem',  '2.5rem'],
        xl:  ['2rem',    '3.5rem'],
      },
    },
  },
  plugins: [
    plugin(function({ matchUtilities, theme }) {
      // matchUtilities — สร้าง utility แบบ dynamic (รองรับ arbitrary values)
      matchUtilities(
        {
          'fluid': (value) => ({
            fontSize: `clamp(${value[0]}, 3vw, ${value[1]})`,
          }),
        },
        { values: theme('fluidSize') }
      )
    }),
  ],
}
```

```html
<!-- ใช้ fluid typography utility -->
<h1 class="fluid-xl">Big Heading</h1>
<h2 class="fluid-lg">Medium Heading</h2>
<p class="fluid-sm">Body text</p>

<!-- หรือ arbitrary value -->
<h1 class="fluid-[1rem,4rem]">Custom fluid</h1>
```

---

## Step 818: Plugin สำหรับ Animation

```js
const plugin = require('tailwindcss/plugin')

module.exports = {
  plugins: [
    plugin(function({ addUtilities, addBase }) {
      addBase({
        '@keyframes fade-up': {
          'from': { opacity: '0', transform: 'translateY(16px)' },
          'to':   { opacity: '1', transform: 'translateY(0)' },
        },
        '@keyframes fade-in': {
          'from': { opacity: '0' },
          'to':   { opacity: '1' },
        },
        '@keyframes scale-in': {
          'from': { opacity: '0', transform: 'scale(0.95)' },
          'to':   { opacity: '1', transform: 'scale(1)' },
        },
        '@keyframes slide-right': {
          'from': { transform: 'translateX(-100%)' },
          'to':   { transform: 'translateX(0)' },
        },
      })

      addUtilities({
        '.animate-fade-up':    { animation: 'fade-up 0.4s ease both' },
        '.animate-fade-in':    { animation: 'fade-in 0.3s ease both' },
        '.animate-scale-in':   { animation: 'scale-in 0.25s ease both' },
        '.animate-slide-right':{ animation: 'slide-right 0.3s ease both' },
        // Delay utilities
        '.animation-delay-100': { 'animation-delay': '100ms' },
        '.animation-delay-200': { 'animation-delay': '200ms' },
        '.animation-delay-300': { 'animation-delay': '300ms' },
        '.animation-delay-500': { 'animation-delay': '500ms' },
      })
    }),
  ],
}
```

```html
<!-- ใช้ animation plugin -->
<div class="animate-fade-up">Fades up on load</div>
<div class="animate-fade-up animation-delay-200">Delayed fade up</div>
<div class="animate-scale-in animation-delay-100">Scale in</div>
```

---

## Step 819: Plugins ยอดนิยมของ Community

```bash
# Scrollbar styling
npm install -D tailwind-scrollbar

# Animate.css integration
npm install -D tailwindcss-animate

# Typography (official)
npm install -D @tailwindcss/typography

# Forms (official)
npm install -D @tailwindcss/forms

# Container queries
npm install -D @tailwindcss/container-queries
```

```js
// tailwind.config.js
module.exports = {
  plugins: [
    require('@tailwindcss/typography'),
    require('@tailwindcss/forms')({ strategy: 'class' }),
    require('@tailwindcss/container-queries'),
    require('tailwindcss-animate'),
    require('tailwind-scrollbar')({ nocompatible: true }),
  ],
}
```

```html
<!-- container queries plugin -->
<div class="@container">
  <div class="@md:flex @md:gap-4">
    <!-- Adapts to container -->
  </div>
</div>

<!-- tailwindcss-animate -->
<div class="animate-in fade-in-0 zoom-in-95 duration-300">Animated element</div>

<!-- tailwind-scrollbar -->
<div class="overflow-y-auto scrollbar scrollbar-thumb-indigo-500 scrollbar-track-gray-100">
  Content
</div>
```

---

## Step 820: Plugin Workshop — All-in-One

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Plugin Workshop</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    /* Custom plugin utilities (simulated via CSS) */
    .scrollbar-hide { -ms-overflow-style:none; scrollbar-width:none; }
    .scrollbar-hide::-webkit-scrollbar { display:none; }
    .text-balance { text-wrap: balance; }
    @keyframes fade-up  { from{opacity:0;transform:translateY(12px)} to{opacity:1;transform:translateY(0)} }
    @keyframes scale-in { from{opacity:0;transform:scale(.95)} to{opacity:1;transform:scale(1)} }
    .animate-fade-up  { animation: fade-up  0.4s ease both; }
    .animate-scale-in { animation: scale-in 0.25s ease both; }
    .delay-100 { animation-delay:100ms; }
    .delay-200 { animation-delay:200ms; }
    .delay-300 { animation-delay:300ms; }
  </style>
</head>
<body class="bg-gray-100 min-h-screen p-6">
  <div class="max-w-3xl mx-auto space-y-8">
    <div class="animate-fade-up">
      <h1 class="text-balance text-2xl font-extrabold text-gray-900">Plugin Workshop — Utilities Demo</h1>
      <p class="text-gray-500 mt-1 text-sm">Showcasing custom plugin utilities</p>
    </div>

    <!-- Scrollbar hide -->
    <div class="animate-fade-up delay-100 bg-white rounded-2xl border p-4">
      <p class="text-xs font-semibold text-gray-400 uppercase tracking-wide mb-3">Scrollbar Hide</p>
      <div class="scrollbar-hide flex gap-3 overflow-x-auto pb-1">
        <div class="flex-shrink-0 w-36 h-24 bg-indigo-100 rounded-xl flex items-center justify-center text-indigo-600 font-bold">Card 1</div>
        <div class="flex-shrink-0 w-36 h-24 bg-purple-100 rounded-xl flex items-center justify-center text-purple-600 font-bold">Card 2</div>
        <div class="flex-shrink-0 w-36 h-24 bg-rose-100 rounded-xl flex items-center justify-center text-rose-600 font-bold">Card 3</div>
        <div class="flex-shrink-0 w-36 h-24 bg-amber-100 rounded-xl flex items-center justify-center text-amber-600 font-bold">Card 4</div>
        <div class="flex-shrink-0 w-36 h-24 bg-emerald-100 rounded-xl flex items-center justify-center text-emerald-600 font-bold">Card 5</div>
      </div>
    </div>

    <!-- Animation stagger -->
    <div class="animate-fade-up delay-200">
      <p class="text-xs font-semibold text-gray-400 uppercase tracking-wide mb-3">Stagger Animation</p>
      <div class="grid grid-cols-3 gap-3">
        <div class="animate-scale-in delay-100 bg-white rounded-2xl border p-4 text-center">
          <div class="text-2xl mb-1">🎨</div><p class="text-xs font-semibold">Design</p>
        </div>
        <div class="animate-scale-in delay-200 bg-white rounded-2xl border p-4 text-center">
          <div class="text-2xl mb-1">⚡</div><p class="text-xs font-semibold">Performance</p>
        </div>
        <div class="animate-scale-in delay-300 bg-white rounded-2xl border p-4 text-center">
          <div class="text-2xl mb-1">♿</div><p class="text-xs font-semibold">Accessibility</p>
        </div>
      </div>
    </div>

    <!-- text-balance -->
    <div class="animate-fade-up delay-300 bg-white rounded-2xl border p-5">
      <p class="text-xs font-semibold text-gray-400 uppercase tracking-wide mb-3">text-balance</p>
      <h2 class="text-balance text-xl font-extrabold text-gray-900">
        Balanced heading text that looks great at any width
      </h2>
    </div>
  </div>
</body>
</html>
```

---

## สรุป Part 82

| Step | เนื้อหา |
|------|---------|
| 811 | @tailwindcss/typography — prose classes |
| 812 | Prose customization ใน tailwind.config |
| 813 | @tailwindcss/forms — form reset |
| 814 | @tailwindcss/aspect-ratio + built-in |
| 815 | Custom plugin — addUtilities |
| 816 | Custom plugin — addComponents + addVariant |
| 817 | Custom plugin — matchUtilities (dynamic) |
| 818 | Animation plugin — custom keyframes + delays |
| 819 | Community plugins — scrollbar, animate, container-queries |
| 820 | Workshop: All plugin utilities demo |

**Part ถัดไป:** Part 83 — Real-World Landing Page (Steps 821–830)
