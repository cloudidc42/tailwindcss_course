# Part 21: Tailwind Config ขั้นสูง
## Steps 201–210: การปรับแต่ง Tailwind CSS

---

## 🎯 เป้าหมายของ Part นี้

- tailwind.config.js โครงสร้างสมบูรณ์
- theme vs theme.extend
- Custom colors, fonts, spacing, screens
- Design tokens
- Content configuration
- Preset และ multi-config

---

## Step 201: โครงสร้าง tailwind.config.js

```javascript
// tailwind.config.js — full structure
/** @type {import('tailwindcss').Config} */
module.exports = {
  // 1. Content — ไฟล์ที่ Tailwind จะ scan หา class
  content: [
    './index.html',
    './src/**/*.{js,ts,jsx,tsx,vue,svelte}',
    './components/**/*.{js,ts,jsx,tsx}',
    './pages/**/*.{js,ts,jsx,tsx}',
  ],
  
  // 2. Darkmode
  darkMode: 'class',    // 'class' | 'media' | false
  
  // 3. Theme
  theme: {
    // Override ทั้งหมด (ลบ default ทิ้ง)
    colors: { ... },
    
    // Extend (รักษา default ไว้ + เพิ่ม)
    extend: {
      colors: { ... },
      fontFamily: { ... },
      spacing: { ... },
    },
  },
  
  // 4. Plugins
  plugins: [
    require('@tailwindcss/forms'),
    require('@tailwindcss/typography'),
    require('@tailwindcss/aspect-ratio'),
  ],
  
  // 5. Prefix (เพิ่ม prefix เช่น tw-)
  prefix: '',
  
  // 6. Important (เพิ่ม !important ทุก utility)
  important: false,
  // หรือ important: '#app' (scope to #app)
}
```

---

## Step 202: Content Configuration

```javascript
module.exports = {
  content: [
    // HTML
    './**/*.html',
    
    // JS/TS
    './src/**/*.{js,ts}',
    
    // React/Next.js
    './src/**/*.{jsx,tsx}',
    './pages/**/*.{jsx,tsx}',
    './components/**/*.{jsx,tsx}',
    './app/**/*.{jsx,tsx}',      // Next.js App Router
    
    // Vue
    './src/**/*.vue',
    
    // Svelte
    './src/**/*.svelte',
    
    // Templates (Rails, Django)
    './templates/**/*.html',
    
    // Safelist — force include classes
  ],
  
  safelist: [
    // Force include specific classes (เช่น dynamic class จาก API)
    'text-red-500',
    'text-green-500',
    'bg-yellow-100',
    
    // Pattern safelist
    {
      pattern: /bg-(red|green|blue)-(100|500|900)/,
      variants: ['hover', 'focus'],
    },
  ],
  
  blocklist: [
    // Force exclude classes
    'container',
  ],
}
```

---

## Step 203: Custom Colors

```javascript
module.exports = {
  theme: {
    extend: {
      colors: {
        // Simple color
        'brand': '#6366f1',
        
        // Color with shades
        'brand': {
          50:  '#eef2ff',
          100: '#e0e7ff',
          200: '#c7d2fe',
          300: '#a5b4fc',
          400: '#818cf8',
          500: '#6366f1',  // primary
          600: '#4f46e5',
          700: '#4338ca',
          800: '#3730a3',
          900: '#312e81',
          950: '#1e1b4b',
        },
        
        // Using CSS variables (for dynamic theming)
        'primary': {
          DEFAULT: 'hsl(var(--color-primary))',
          foreground: 'hsl(var(--color-primary-foreground))',
        },
        
        // Semantic colors
        'surface': {
          DEFAULT: 'hsl(var(--surface))',
          raised: 'hsl(var(--surface-raised))',
          overlay: 'hsl(var(--surface-overlay))',
        },
      },
    },
  },
}
```

```css
/* globals.css — กำหนด CSS variables */
:root {
  --color-primary: 239 84% 67%;            /* indigo-500 */
  --color-primary-foreground: 0 0% 100%;   /* white */
  --surface: 0 0% 100%;                    /* white */
  --surface-raised: 220 14% 96%;           /* gray-100 */
  --surface-overlay: 220 14% 96%;
}

.dark {
  --color-primary: 239 84% 67%;
  --surface: 222 47% 11%;                  /* dark bg */
  --surface-raised: 215 28% 17%;
}
```

```html
<!-- ใช้งาน -->
<div class="bg-primary text-primary-foreground">Dynamic primary</div>
<div class="bg-surface text-gray-900">Surface card</div>
```

---

## Step 204: Custom Typography

```javascript
module.exports = {
  theme: {
    extend: {
      fontFamily: {
        // Thai fonts
        sans: ['"Sarabun"', '"Noto Sans Thai"', 'sans-serif'],
        heading: ['"Prompt"', '"Kanit"', 'sans-serif'],
        mono: ['"JetBrains Mono"', '"Fira Code"', 'monospace'],
        display: ['"Kanit"', 'sans-serif'],
      },
      
      fontSize: {
        // Custom sizes
        '2xs': ['0.625rem', { lineHeight: '0.875rem' }],  // 10px
        '3xl': ['1.875rem', { lineHeight: '2.25rem' }],   // override
        
        // With letter-spacing and font-weight
        'display-sm': ['1.875rem', {
          lineHeight: '2.25rem',
          letterSpacing: '-0.02em',
          fontWeight: '700',
        }],
        'display-lg': ['3.75rem', {
          lineHeight: '1',
          letterSpacing: '-0.02em',
          fontWeight: '900',
        }],
      },
      
      letterSpacing: {
        tightest: '-0.05em',
        tight: '-0.025em',
        normal: '0em',
        wide: '0.025em',
        wider: '0.05em',
        widest: '0.1em',
        'ultra-wide': '0.2em',
      },
    },
  },
}
```

```html
<h1 class="font-heading text-display-lg tracking-tightest">
  Display Heading
</h1>
<p class="font-sans text-base">Body text in Sarabun</p>
<code class="font-mono text-sm">code snippet</code>
```

---

## Step 205: Custom Spacing

```javascript
module.exports = {
  theme: {
    extend: {
      spacing: {
        // เพิ่ม spacing scale
        '13': '3.25rem',   // 52px
        '15': '3.75rem',   // 60px
        '18': '4.5rem',    // 72px
        '22': '5.5rem',    // 88px
        '26': '6.5rem',    // 104px
        '30': '7.5rem',    // 120px
        
        // Pixel values
        '0.5px': '0.5px',
        '1px': '1px',
        '2px': '2px',
        
        // Named values
        'header': '4rem',     // 64px - navbar height
        'sidebar': '16rem',   // 256px - sidebar width
        'page-top': '5rem',   // 80px - content padding top
      },
    },
  },
}
```

```html
<nav class="h-header sticky top-0">Navigation</nav>
<aside class="w-sidebar">Sidebar</aside>
<main class="pt-page-top">Content</main>
```

---

## Step 206: Custom Border Radius

```javascript
module.exports = {
  theme: {
    extend: {
      borderRadius: {
        // Default scale
        // none: '0', sm: '2px', DEFAULT: '4px', md: '6px',
        // lg: '8px', xl: '12px', 2xl: '16px', 3xl: '24px', full: '9999px'
        
        // Additions
        '4xl': '2rem',   // 32px
        '5xl': '2.5rem', // 40px
        
        // Design system names
        'card': '1rem',       // 16px
        'button': '0.75rem',  // 12px
        'input': '0.625rem',  // 10px
        'badge': '0.375rem',  // 6px
      },
    },
  },
}
```

---

## Step 207: Custom Shadows

```javascript
module.exports = {
  theme: {
    extend: {
      boxShadow: {
        // Colored shadows
        'indigo': '0 4px 30px -4px rgba(99, 102, 241, 0.3)',
        'indigo-xl': '0 20px 40px -8px rgba(99, 102, 241, 0.4)',
        
        // Elevation system
        'elevation-1': '0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24)',
        'elevation-2': '0 3px 6px rgba(0,0,0,0.16), 0 3px 6px rgba(0,0,0,0.23)',
        'elevation-3': '0 10px 20px rgba(0,0,0,0.19), 0 6px 6px rgba(0,0,0,0.23)',
        
        // Inner shadows
        'inner-sm': 'inset 0 1px 2px 0 rgba(0,0,0,0.06)',
        'inner-lg': 'inset 0 4px 8px 0 rgba(0,0,0,0.12)',
        
        // Glow effects
        'glow-indigo': '0 0 30px rgba(99, 102, 241, 0.4)',
        'glow-green':  '0 0 30px rgba(34, 197, 94, 0.4)',
      },
      
      dropShadow: {
        'text-glow': '0 0 20px rgba(99, 102, 241, 0.8)',
      },
    },
  },
}
```

---

## Step 208: Custom Animations & Keyframes

```javascript
module.exports = {
  theme: {
    extend: {
      keyframes: {
        // Slide animations
        'slide-down': {
          '0%': { transform: 'translateY(-10px)', opacity: '0' },
          '100%': { transform: 'translateY(0)', opacity: '1' },
        },
        'slide-up': {
          '0%': { transform: 'translateY(10px)', opacity: '0' },
          '100%': { transform: 'translateY(0)', opacity: '1' },
        },
        'slide-left': {
          '0%': { transform: 'translateX(10px)', opacity: '0' },
          '100%': { transform: 'translateX(0)', opacity: '1' },
        },
        'slide-right': {
          '0%': { transform: 'translateX(-10px)', opacity: '0' },
          '100%': { transform: 'translateX(0)', opacity: '1' },
        },
        
        // Scale animations
        'zoom-in': {
          '0%': { transform: 'scale(0.95)', opacity: '0' },
          '100%': { transform: 'scale(1)', opacity: '1' },
        },
        'zoom-out': {
          '0%': { transform: 'scale(1.05)', opacity: '0' },
          '100%': { transform: 'scale(1)', opacity: '1' },
        },
        
        // Attention seekers
        'wiggle': {
          '0%, 100%': { transform: 'rotate(-3deg)' },
          '50%': { transform: 'rotate(3deg)' },
        },
        'heartbeat': {
          '0%, 100%': { transform: 'scale(1)' },
          '25%': { transform: 'scale(1.1)' },
          '50%': { transform: 'scale(0.95)' },
          '75%': { transform: 'scale(1.05)' },
        },
        
        // Loading
        'skeleton': {
          '0%': { backgroundPosition: '-200% 0' },
          '100%': { backgroundPosition: '200% 0' },
        },
      },
      
      animation: {
        'slide-down': 'slide-down 0.3s ease-out',
        'slide-up': 'slide-up 0.3s ease-out',
        'slide-left': 'slide-left 0.3s ease-out',
        'slide-right': 'slide-right 0.3s ease-out',
        'zoom-in': 'zoom-in 0.2s ease-out',
        'zoom-out': 'zoom-out 0.2s ease-out',
        'wiggle': 'wiggle 0.5s ease-in-out infinite',
        'heartbeat': 'heartbeat 1.5s ease-in-out infinite',
        'skeleton': 'skeleton 1.5s linear infinite',
      },
    },
  },
}
```

---

## Step 209: Plugins

```javascript
// เขียน Plugin เอง
const plugin = require('tailwindcss/plugin')

module.exports = {
  plugins: [
    
    // Plugin จาก npm
    require('@tailwindcss/forms'),
    require('@tailwindcss/typography'),
    require('@tailwindcss/aspect-ratio'),
    require('@tailwindcss/container-queries'),
    
    // Custom plugin
    plugin(function({ addUtilities, addComponents, addBase, theme, e }) {
      
      // addBase — เพิ่ม base styles (เหมือน global CSS)
      addBase({
        'html': { scrollBehavior: 'smooth' },
        'body': { 
          fontFamily: theme('fontFamily.sans'),
          color: theme('colors.gray.900'),
        },
        'h1, h2, h3, h4, h5, h6': {
          fontFamily: theme('fontFamily.heading'),
          fontWeight: '700',
        },
      })
      
      // addUtilities — เพิ่ม utilities
      addUtilities({
        '.text-balance': { textWrap: 'balance' },
        '.text-pretty': { textWrap: 'pretty' },
        '.clip-path-arrow': {
          clipPath: 'polygon(0 0, calc(100% - 1rem) 0, 100% 50%, calc(100% - 1rem) 100%, 0 100%)',
        },
        '.scrollbar-hide': {
          '-ms-overflow-style': 'none',
          'scrollbar-width': 'none',
          '&::-webkit-scrollbar': { display: 'none' },
        },
      })
      
      // addComponents — เพิ่ม component classes
      addComponents({
        '.btn': {
          display: 'inline-flex',
          alignItems: 'center',
          justifyContent: 'center',
          fontWeight: '600',
          borderRadius: theme('borderRadius.xl'),
          transition: 'all 150ms',
          '&:focus-visible': {
            outline: '2px solid ' + theme('colors.indigo.500'),
            outlineOffset: '2px',
          },
        },
        '.btn-primary': {
          backgroundColor: theme('colors.indigo.600'),
          color: 'white',
          padding: '0.625rem 1.25rem',
          '&:hover': {
            backgroundColor: theme('colors.indigo.700'),
          },
          '&:active': {
            transform: 'scale(0.98)',
          },
        },
      })
    }),
  ],
}
```

---

## Step 210: Workshop — Design Token System

```javascript
// tailwind.config.js — Design Token System
const colors = require('tailwindcss/colors')

/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ['./src/**/*.{html,js,ts,jsx,tsx}'],
  darkMode: 'class',
  
  theme: {
    // กำหนด colors ทั้งหมด (override default)
    colors: {
      transparent: 'transparent',
      current: 'currentColor',
      white: '#ffffff',
      black: '#000000',
      
      // Gray scale (ใช้ CSS variables สำหรับ dark mode)
      gray: {
        50:  'hsl(var(--gray-50))',
        100: 'hsl(var(--gray-100))',
        200: 'hsl(var(--gray-200))',
        300: 'hsl(var(--gray-300))',
        400: 'hsl(var(--gray-400))',
        500: 'hsl(var(--gray-500))',
        600: 'hsl(var(--gray-600))',
        700: 'hsl(var(--gray-700))',
        800: 'hsl(var(--gray-800))',
        900: 'hsl(var(--gray-900))',
        950: 'hsl(var(--gray-950))',
      },
      
      // Brand colors
      brand: {
        50:  '#eef2ff',
        100: '#e0e7ff',
        200: '#c7d2fe',
        300: '#a5b4fc',
        400: '#818cf8',
        500: '#6366f1',
        600: '#4f46e5',
        700: '#4338ca',
        800: '#3730a3',
        900: '#312e81',
      },
      
      // Semantic
      primary: 'hsl(var(--primary))',
      secondary: 'hsl(var(--secondary))',
      accent: 'hsl(var(--accent))',
      
      destructive: 'hsl(var(--destructive))',
      success: 'hsl(var(--success))',
      warning: 'hsl(var(--warning))',
      
      background: 'hsl(var(--background))',
      foreground: 'hsl(var(--foreground))',
      
      border: 'hsl(var(--border))',
      ring: 'hsl(var(--ring))',
      
      // Keep useful Tailwind colors
      red: colors.red,
      orange: colors.orange,
      yellow: colors.yellow,
      green: colors.green,
      blue: colors.blue,
      purple: colors.purple,
      pink: colors.pink,
    },
    
    extend: {
      fontFamily: {
        sans: ['"Sarabun"', 'ui-sans-serif', 'system-ui', 'sans-serif'],
        mono: ['"JetBrains Mono"', 'monospace'],
      },
    },
  },
  
  plugins: [
    require('@tailwindcss/forms'),
    require('@tailwindcss/typography'),
  ],
}
```

```css
/* globals.css */
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  :root {
    /* Gray scale — light */
    --gray-50:  220 14% 96%;
    --gray-100: 220 14% 93%;
    --gray-200: 220 13% 91%;
    --gray-300: 216 12% 84%;
    --gray-400: 218 11% 65%;
    --gray-500: 220 9% 46%;
    --gray-600: 215 14% 34%;
    --gray-700: 217 19% 27%;
    --gray-800: 215 28% 17%;
    --gray-900: 221 39% 11%;
    --gray-950: 224 71% 4%;

    /* Semantic */
    --primary:   239 84% 67%;
    --secondary: 220 14% 96%;
    --accent:    239 84% 67%;
    
    --destructive: 0 84% 60%;
    --success:  142 71% 45%;
    --warning:  38 92% 50%;
    
    --background: 0 0% 100%;
    --foreground: 222 47% 11%;
    
    --border: 220 13% 91%;
    --ring:   239 84% 67%;
  }
  
  .dark {
    --gray-50:  224 71% 4%;
    --gray-100: 222 47% 11%;
    --gray-200: 215 28% 17%;
    --gray-300: 217 19% 27%;
    --gray-400: 215 14% 34%;
    --gray-500: 220 9% 46%;
    --gray-600: 218 11% 65%;
    --gray-700: 216 12% 84%;
    --gray-800: 220 13% 91%;
    --gray-900: 220 14% 93%;
    --gray-950: 220 14% 96%;
    
    --background: 222 47% 11%;
    --foreground: 210 40% 98%;
    --border: 217 19% 27%;
  }
}
```

---

## 📝 สรุป Part 21

| Section | หน้าที่ |
|---------|---------|
| `content` | บอก Tailwind ให้ scan ไฟล์ไหน |
| `theme` | Override ทุกค่า default |
| `theme.extend` | เพิ่มค่าใหม่โดยรักษา default ไว้ |
| `plugins` | เพิ่ม utilities/components/base |
| `safelist` | Force include class ที่ dynamic |
| CSS Variables | ทำให้ support dark mode และ theming |

---

*Part 21 — จาก 100 Parts | Steps 201–210 จาก 1,000 Steps*
