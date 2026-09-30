# Part 23: Custom Plugins
## Steps 221–230: เขียน Tailwind Plugin เอง

---

## 🎯 เป้าหมายของ Part นี้

- Plugin API: addBase, addComponents, addUtilities
- Dynamic utilities ด้วย matchUtilities
- Plugin options
- Extracting reusable plugin
- Publishing plugin

---

## Step 221: Plugin Basics

```javascript
// tailwind.config.js
const plugin = require('tailwindcss/plugin')

module.exports = {
  plugins: [
    plugin(function({ addBase, addComponents, addUtilities, addVariant, theme, e, config }) {
      // addBase    — global/reset styles
      // addComponents — component classes (override-able by utilities)
      // addUtilities  — utility classes
      // addVariant — custom variants เช่น .dark .group-hover ฯลฯ
      // theme()    — access theme values
      // e()        — escape string สำหรับ CSS class names
      // config()   — access Tailwind config
    }),
  ],
}
```

---

## Step 222: addBase — Global Styles

```javascript
plugin(function({ addBase, theme }) {
  addBase({
    // Reset/normalize
    '*, *::before, *::after': {
      boxSizing: 'border-box',
    },
    
    // Typography defaults
    'html': {
      scrollBehavior: 'smooth',
      textSizeAdjust: '100%',
    },
    'body': {
      fontFamily: theme('fontFamily.sans'),
      color: theme('colors.gray.900'),
      lineHeight: theme('lineHeight.relaxed'),
    },
    
    // Heading sizes
    'h1': { fontSize: theme('fontSize.4xl'), fontWeight: '800', lineHeight: '1.1' },
    'h2': { fontSize: theme('fontSize.3xl'), fontWeight: '700', lineHeight: '1.2' },
    'h3': { fontSize: theme('fontSize.2xl'), fontWeight: '700', lineHeight: '1.3' },
    'h4': { fontSize: theme('fontSize.xl'),  fontWeight: '600' },
    
    // Links
    'a': {
      color: theme('colors.indigo.600'),
      textDecoration: 'underline',
      textUnderlineOffset: '2px',
      '&:hover': { color: theme('colors.indigo.800') },
    },
    
    // Code
    'code': {
      fontFamily: theme('fontFamily.mono'),
      fontSize: '0.875em',
      backgroundColor: theme('colors.gray.100'),
      padding: '0.125rem 0.25rem',
      borderRadius: '0.25rem',
    },
    'pre': {
      backgroundColor: theme('colors.gray.950'),
      color: theme('colors.gray.100'),
      borderRadius: theme('borderRadius.xl'),
      padding: theme('spacing.4'),
      overflow: 'auto',
      '& code': {
        backgroundColor: 'transparent',
        padding: '0',
        fontSize: 'inherit',
      },
    },
    
    // Form resets
    'input, textarea, select': {
      outline: 'none',
    },
    'button': {
      cursor: 'pointer',
    },
  })
})
```

---

## Step 223: addUtilities — Custom Utilities

```javascript
plugin(function({ addUtilities, theme }) {
  addUtilities({
    // Text utilities
    '.text-balance': { textWrap: 'balance' },
    '.text-pretty': { textWrap: 'pretty' },
    
    // Scrollbar
    '.scrollbar-hide': {
      '&::-webkit-scrollbar': { display: 'none' },
      '-ms-overflow-style': 'none',
      'scrollbar-width': 'none',
    },
    '.scrollbar-thin': {
      'scrollbar-width': 'thin',
      '&::-webkit-scrollbar': { width: '6px', height: '6px' },
      '&::-webkit-scrollbar-track': { background: 'transparent' },
      '&::-webkit-scrollbar-thumb': {
        background: theme('colors.gray.300'),
        borderRadius: '9999px',
      },
    },
    
    // Clip paths
    '.clip-path-none':   { clipPath: 'none' },
    '.clip-path-chevron': {
      clipPath: 'polygon(0 0, 95% 0, 100% 50%, 95% 100%, 0 100%)',
    },
    '.clip-path-arrow': {
      clipPath: 'polygon(0 0, calc(100% - 1rem) 0, 100% 50%, calc(100% - 1rem) 100%, 0 100%, 1rem 50%)',
    },
    
    // Aspect ratios (for older browsers)
    '.aspect-golden': { aspectRatio: '1.618' },
    
    // Line clamp
    '.line-clamp-1': { display: '-webkit-box', '-webkit-line-clamp': '1', '-webkit-box-orient': 'vertical', overflow: 'hidden' },
    '.line-clamp-2': { display: '-webkit-box', '-webkit-line-clamp': '2', '-webkit-box-orient': 'vertical', overflow: 'hidden' },
    
    // Glass effect
    '.glass': {
      background: 'rgba(255, 255, 255, 0.1)',
      backdropFilter: 'blur(12px)',
      border: '1px solid rgba(255, 255, 255, 0.2)',
    },
    '.glass-dark': {
      background: 'rgba(0, 0, 0, 0.2)',
      backdropFilter: 'blur(12px)',
      border: '1px solid rgba(255, 255, 255, 0.1)',
    },
    
    // Gradient text
    '.gradient-text': {
      background: 'linear-gradient(135deg, #6366f1, #a855f7)',
      '-webkit-background-clip': 'text',
      '-webkit-text-fill-color': 'transparent',
      backgroundClip: 'text',
    },
    
    // Hide scrollbar
    '.no-scrollbar': {
      '-ms-overflow-style': 'none',
      'scrollbar-width': 'none',
      '&::-webkit-scrollbar': { display: 'none' },
    },
    
    // Full bleed
    '.full-bleed': {
      width: '100vw',
      marginLeft: 'calc(50% - 50vw)',
    },
  })
})
```

---

## Step 224: addComponents — Reusable Components

```javascript
plugin(function({ addComponents, theme }) {
  addComponents({
    // Button system
    '.btn': {
      display: 'inline-flex',
      alignItems: 'center',
      justifyContent: 'center',
      gap: theme('spacing.2'),
      fontFamily: theme('fontFamily.sans'),
      fontWeight: '600',
      fontSize: theme('fontSize.sm'),
      lineHeight: '1',
      borderRadius: theme('borderRadius.xl'),
      transitionProperty: 'all',
      transitionDuration: '150ms',
      cursor: 'pointer',
      userSelect: 'none',
      whiteSpace: 'nowrap',
      '&:focus-visible': {
        outline: '2px solid',
        outlineOffset: '2px',
      },
      '&:disabled': {
        opacity: '0.5',
        cursor: 'not-allowed',
        pointerEvents: 'none',
      },
    },
    
    '.btn-sm':  { padding: '0.375rem 0.75rem',  fontSize: theme('fontSize.xs') },
    '.btn-md':  { padding: '0.625rem 1.25rem'  },
    '.btn-lg':  { padding: '0.75rem 1.5rem',    fontSize: theme('fontSize.base') },
    '.btn-xl':  { padding: '1rem 2rem',         fontSize: theme('fontSize.lg'), borderRadius: theme('borderRadius.2xl') },
    
    '.btn-primary': {
      backgroundColor: theme('colors.indigo.600'),
      color: 'white',
      '&:hover': { backgroundColor: theme('colors.indigo.700') },
      '&:active': { transform: 'scale(0.98)', backgroundColor: theme('colors.indigo.800') },
      '&:focus-visible': { outlineColor: theme('colors.indigo.500') },
    },
    '.btn-secondary': {
      backgroundColor: theme('colors.gray.100'),
      color: theme('colors.gray.900'),
      '&:hover': { backgroundColor: theme('colors.gray.200') },
      '&:active': { transform: 'scale(0.98)' },
    },
    '.btn-outline': {
      border: '2px solid ' + theme('colors.indigo.600'),
      color: theme('colors.indigo.600'),
      backgroundColor: 'transparent',
      '&:hover': {
        backgroundColor: theme('colors.indigo.600'),
        color: 'white',
      },
    },
    '.btn-ghost': {
      backgroundColor: 'transparent',
      color: theme('colors.indigo.600'),
      '&:hover': { backgroundColor: theme('colors.indigo.50') },
    },
    '.btn-danger': {
      backgroundColor: theme('colors.red.600'),
      color: 'white',
      '&:hover': { backgroundColor: theme('colors.red.700') },
      '&:focus-visible': { outlineColor: theme('colors.red.500') },
    },
    
    // Input system
    '.input': {
      display: 'block',
      width: '100%',
      padding: `${theme('spacing.3')} ${theme('spacing.4')}`,
      borderRadius: theme('borderRadius.xl'),
      border: `1px solid ${theme('colors.gray.300')}`,
      backgroundColor: 'white',
      fontSize: theme('fontSize.sm'),
      color: theme('colors.gray.900'),
      lineHeight: theme('lineHeight.normal'),
      transition: 'all 150ms',
      '&::placeholder': { color: theme('colors.gray.400') },
      '&:hover': { borderColor: theme('colors.gray.400') },
      '&:focus': {
        outline: 'none',
        borderColor: theme('colors.indigo.500'),
        boxShadow: `0 0 0 3px ${theme('colors.indigo.100')}`,
      },
      '&:disabled': {
        backgroundColor: theme('colors.gray.100'),
        cursor: 'not-allowed',
        opacity: '0.7',
      },
      '&.input-error': {
        borderColor: theme('colors.red.400'),
        '&:focus': {
          borderColor: theme('colors.red.500'),
          boxShadow: `0 0 0 3px ${theme('colors.red.100')}`,
        },
      },
    },
    
    // Card system
    '.card': {
      backgroundColor: 'white',
      borderRadius: theme('borderRadius.2xl'),
      border: `1px solid ${theme('colors.gray.200')}`,
      boxShadow: theme('boxShadow.sm'),
      overflow: 'hidden',
    },
    '.card-body': {
      padding: theme('spacing.6'),
    },
    '.card-hover': {
      transition: 'all 300ms',
      '&:hover': {
        boxShadow: theme('boxShadow.xl'),
        transform: 'translateY(-2px)',
      },
    },
    
    // Badge system
    '.badge': {
      display: 'inline-flex',
      alignItems: 'center',
      gap: theme('spacing.1'),
      fontWeight: '600',
      fontSize: theme('fontSize.xs'),
      padding: '0.25rem 0.625rem',
      borderRadius: '9999px',
    },
    '.badge-primary': {
      backgroundColor: theme('colors.indigo.100'),
      color: theme('colors.indigo.700'),
    },
    '.badge-success': {
      backgroundColor: theme('colors.green.100'),
      color: theme('colors.green.700'),
    },
    '.badge-warning': {
      backgroundColor: theme('colors.yellow.100'),
      color: theme('colors.yellow.700'),
    },
    '.badge-danger': {
      backgroundColor: theme('colors.red.100'),
      color: theme('colors.red.700'),
    },
  })
})
```

---

## Step 225: matchUtilities — Dynamic Utilities

```javascript
plugin(function({ matchUtilities, theme }) {
  
  // สร้าง glow-{color} utility
  matchUtilities(
    {
      'glow': (value) => ({
        boxShadow: `0 0 30px ${value}`,
      }),
    },
    { values: theme('colors'), type: 'color' }
  )
  
  // สร้าง text-stroke-{width} utility
  matchUtilities(
    {
      'text-stroke': (value) => ({
        '-webkit-text-stroke-width': value,
        'text-stroke-width': value,
      }),
    },
    { values: { DEFAULT: '1px', '2': '2px', '4': '4px' } }
  )
  
  // สร้าง gradient-{direction} utility
  matchUtilities(
    {
      'gradient': (value) => ({
        background: `linear-gradient(${value}, var(--tw-gradient-from), var(--tw-gradient-to))`,
      }),
    },
    {
      values: {
        'to-t': '0deg',
        'to-r': '90deg',
        'to-b': '180deg',
        'to-l': '270deg',
        'to-tr': '45deg',
        'to-br': '135deg',
        'to-bl': '225deg',
        'to-tl': '315deg',
      },
    }
  )
})
```

---

## Step 226: addVariant — Custom Variants

```javascript
plugin(function({ addVariant }) {
  
  // Mobile variant (<=640px)
  addVariant('mobile', '@media (max-width: 640px)')
  
  // Theme variants
  addVariant('theme-dark',  '[data-theme="dark"] &')
  addVariant('theme-light', '[data-theme="light"] &')
  
  // RTL/LTR
  addVariant('rtl', '[dir="rtl"] &')
  addVariant('ltr', '[dir="ltr"] &')
  
  // Print
  addVariant('print', '@media print')
  
  // Scrolled (เมื่อ scroll ลงมา)
  addVariant('scrolled', '.scrolled &')
  
  // Data attribute variants
  addVariant('data-open',   '&[data-state="open"]')
  addVariant('data-closed', '&[data-state="closed"]')
  addVariant('data-active', '&[data-active]')
  
  // Nesting variants
  addVariant('group-open',   ':merge(.group)[data-state="open"] &')
})
```

```html
<!-- ใช้ custom variants -->
<div class="block mobile:hidden">Desktop only</div>
<div class="hidden mobile:block">Mobile only</div>

<div class="hidden print:block">Print only</div>

<div class="scrolled:bg-white/90 scrolled:shadow-sm fixed top-0">
  Scrolled navbar
</div>

<div class="data-open:opacity-100 data-closed:opacity-0 transition-opacity">
  Animated content
</div>
```

---

## Step 227: Plugin with Options

```javascript
// plugins/my-plugin.js
const plugin = require('tailwindcss/plugin')

module.exports = plugin.withOptions(
  // Plugin factory (returns plugin function)
  function(options = {}) {
    return function({ addComponents, theme }) {
      const {
        prefix = '',
        defaultRadius = 'xl',
        enableAnimations = true,
      } = options
      
      addComponents({
        [`.${prefix}card`]: {
          borderRadius: theme(`borderRadius.${defaultRadius}`),
          ...(enableAnimations ? {
            transition: 'all 300ms',
            '&:hover': { transform: 'translateY(-2px)' },
          } : {}),
        },
      })
    }
  },
  
  // Config extension factory
  function(options = {}) {
    return {
      theme: {
        extend: {
          borderRadius: {
            card: '1rem',
          },
        },
      },
    }
  }
)
```

```javascript
// tailwind.config.js
module.exports = {
  plugins: [
    require('./plugins/my-plugin')({
      prefix: 'custom-',
      defaultRadius: '2xl',
      enableAnimations: true,
    }),
  ],
}
```

---

## Step 228: Complete Plugin Example

```javascript
// plugins/ui.js — Complete UI Plugin
const plugin = require('tailwindcss/plugin')

module.exports = plugin(function({ addBase, addComponents, addUtilities, theme }) {
  
  // Base
  addBase({
    ':root': {
      '--radius': '0.75rem',
    },
    'html': { scrollBehavior: 'smooth' },
  })
  
  // Utilities
  addUtilities({
    '.glass': {
      background: 'rgba(255,255,255,0.1)',
      backdropFilter: 'blur(12px)',
      border: '1px solid rgba(255,255,255,0.2)',
    },
    '.scrollbar-hide': {
      '&::-webkit-scrollbar': { display: 'none' },
      '-ms-overflow-style': 'none',
      'scrollbar-width': 'none',
    },
    '.text-balance': { textWrap: 'balance' },
    '.gradient-text': {
      background: 'linear-gradient(135deg, #6366f1, #a855f7)',
      '-webkit-background-clip': 'text',
      '-webkit-text-fill-color': 'transparent',
      backgroundClip: 'text',
    },
  })
  
  // Components
  addComponents({
    '.btn': {
      display: 'inline-flex',
      alignItems: 'center',
      justifyContent: 'center',
      gap: '0.5rem',
      fontWeight: '600',
      fontSize: theme('fontSize.sm'),
      padding: '0.625rem 1.25rem',
      borderRadius: 'var(--radius)',
      transition: 'all 150ms',
      cursor: 'pointer',
      '&:active': { transform: 'scale(0.98)' },
      '&:disabled': { opacity: '0.5', cursor: 'not-allowed' },
    },
    '.btn-primary': {
      backgroundColor: theme('colors.indigo.600'),
      color: '#fff',
      '&:hover': { backgroundColor: theme('colors.indigo.700') },
    },
    '.btn-secondary': {
      backgroundColor: theme('colors.gray.100'),
      color: theme('colors.gray.900'),
      '&:hover': { backgroundColor: theme('colors.gray.200') },
    },
    '.input': {
      display: 'block',
      width: '100%',
      padding: '0.75rem 1rem',
      borderRadius: 'var(--radius)',
      border: `1px solid ${theme('colors.gray.300')}`,
      backgroundColor: '#fff',
      fontSize: theme('fontSize.sm'),
      '&:focus': {
        outline: 'none',
        borderColor: theme('colors.indigo.500'),
        boxShadow: `0 0 0 3px ${theme('colors.indigo.100')}`,
      },
    },
    '.card': {
      backgroundColor: '#fff',
      borderRadius: 'calc(var(--radius) * 1.5)',
      border: `1px solid ${theme('colors.gray.200')}`,
      padding: theme('spacing.6'),
    },
  })
})
```

```html
<!-- ใช้ plugin classes -->
<button class="btn btn-primary">Primary Button</button>
<button class="btn btn-secondary btn-sm">Small Secondary</button>
<input class="input" placeholder="Enter text...">
<div class="card">Card content</div>
<div class="glass rounded-xl p-6">Glass card</div>
<h1 class="gradient-text text-4xl font-black">Gradient Heading</h1>
```

---

## Step 229–230: Workshop — Plugin-Powered Design System

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Plugin Design System</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      plugins: [
        tailwind.plugin(function({ addComponents, addUtilities, theme }) {
          addUtilities({
            '.glass': {
              background: 'rgba(255,255,255,0.1)',
              backdropFilter: 'blur(12px)',
              border: '1px solid rgba(255,255,255,0.2)',
            },
            '.gradient-text': {
              background: 'linear-gradient(135deg, #6366f1, #a855f7)',
              '-webkit-background-clip': 'text',
              '-webkit-text-fill-color': 'transparent',
              backgroundClip: 'text',
            },
            '.text-balance': { textWrap: 'balance' },
          })
          addComponents({
            '.btn': {
              display: 'inline-flex',
              alignItems: 'center',
              justifyContent: 'center',
              gap: '0.5rem',
              fontWeight: '600',
              fontSize: '0.875rem',
              padding: '0.625rem 1.25rem',
              borderRadius: '0.75rem',
              transition: 'all 150ms',
              cursor: 'pointer',
              '&:active': { transform: 'scale(0.98)' },
            },
            '.btn-primary': {
              backgroundColor: '#4f46e5',
              color: '#fff',
              '&:hover': { backgroundColor: '#4338ca' },
            },
            '.btn-outline': {
              border: '2px solid #4f46e5',
              color: '#4f46e5',
              backgroundColor: 'transparent',
              '&:hover': { backgroundColor: '#4f46e5', color: '#fff' },
            },
            '.card': {
              backgroundColor: '#fff',
              borderRadius: '1rem',
              border: '1px solid #e5e7eb',
              padding: '1.5rem',
              boxShadow: '0 1px 3px rgba(0,0,0,0.1)',
            },
          })
        }),
      ],
    }
  </script>
</head>
<body class="bg-gradient-to-br from-slate-900 to-indigo-950 min-h-screen p-8">

  <div class="max-w-4xl mx-auto">
    <h1 class="text-4xl font-black text-white mb-2 text-balance">
      Plugin <span class="gradient-text">Design System</span>
    </h1>
    <p class="text-white/60 mb-8">Component classes ที่มาจาก Tailwind Plugin</p>
    
    <!-- Buttons -->
    <div class="card mb-6">
      <h2 class="font-bold text-gray-900 mb-4">Buttons (.btn)</h2>
      <div class="flex flex-wrap gap-3">
        <button class="btn btn-primary">Primary</button>
        <button class="btn btn-outline">Outline</button>
        <button class="btn btn-primary" disabled>Disabled</button>
      </div>
    </div>
    
    <!-- Glass Cards -->
    <div class="grid grid-cols-1 sm:grid-cols-3 gap-4 mb-6">
      <div class="glass rounded-2xl p-5 text-white">
        <p class="text-3xl font-black">100+</p>
        <p class="text-white/60 text-sm">บทเรียน</p>
      </div>
      <div class="glass rounded-2xl p-5 text-white">
        <p class="text-3xl font-black">1,000</p>
        <p class="text-white/60 text-sm">Steps</p>
      </div>
      <div class="glass rounded-2xl p-5 text-white">
        <p class="text-3xl font-black">4.9★</p>
        <p class="text-white/60 text-sm">Rating</p>
      </div>
    </div>
    
    <!-- Regular cards -->
    <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
      <div class="card">
        <h3 class="font-bold text-gray-900 mb-2">Card Component</h3>
        <p class="text-gray-500 text-sm text-balance">Component class จาก plugin ใช้ง่ายไม่ต้องเขียน utility class ซ้ำ</p>
        <button class="btn btn-primary mt-4">Action</button>
      </div>
      <div class="card">
        <h3 class="font-bold text-gray-900 mb-2">Design System</h3>
        <p class="text-gray-500 text-sm text-balance">ใช้ plugin สร้าง component library ที่ consistent ทั้ง project</p>
        <button class="btn btn-outline mt-4">Details</button>
      </div>
    </div>
  </div>

</body>
</html>
```

---

## 📝 สรุป Part 23

| Function | ใช้สำหรับ |
|----------|---------|
| `addBase()` | Global/reset styles |
| `addComponents()` | Reusable component classes |
| `addUtilities()` | Single-purpose utilities |
| `addVariant()` | Custom pseudo-classes/states |
| `matchUtilities()` | Dynamic utilities ที่รับ value |
| `plugin.withOptions()` | Plugin ที่รับ config options |

---

*Part 23 — จาก 100 Parts | Steps 221–230 จาก 1,000 Steps*
