# Part 22: Dark Mode
## Steps 211–220: การทำ Dark Mode อย่างถูกต้อง

---

## 🎯 เป้าหมายของ Part นี้

- Dark mode strategies
- dark: prefix
- System preference vs manual toggle
- Dark mode + CSS variables
- Dark mode components

---

## Step 211: Dark Mode Strategies

```javascript
// tailwind.config.js
module.exports = {
  // Strategy 1: System preference (prefers-color-scheme)
  darkMode: 'media',
  
  // Strategy 2: Class-based (manual toggle) — แนะนำ
  darkMode: 'class',
  
  // Strategy 3: Selector-based (Tailwind v3.4+)
  darkMode: ['selector', '[data-theme="dark"]'],
  
  // Strategy 4: Class with custom selector
  darkMode: ['class', '[data-mode="dark"]'],
}
```

```html
<!-- Class strategy: เพิ่ม/ลบ class 'dark' ที่ <html> -->
<html class="dark">
  ...
</html>

<!-- System strategy: อัตโนมัติตาม OS setting -->
<!-- ไม่ต้องทำอะไรเพิ่ม -->

<!-- Selector strategy -->
<html data-theme="dark">
  ...
</html>
```

---

## Step 212: dark: Prefix

```html
<!-- dark: prefix ทำงานกับ utility ทุกตัว -->
<div class="bg-white dark:bg-gray-900 text-gray-900 dark:text-gray-100">
  Content adapts to dark mode
</div>

<!-- Typography -->
<h1 class="text-gray-900 dark:text-white font-bold">Heading</h1>
<p class="text-gray-600 dark:text-gray-400">Body text</p>
<span class="text-indigo-600 dark:text-indigo-400">Link color</span>

<!-- Borders -->
<div class="border border-gray-200 dark:border-gray-700 rounded-xl p-4">
  Card border
</div>

<!-- Backgrounds -->
<div class="bg-gray-50 dark:bg-gray-800">Section</div>
<div class="bg-white dark:bg-gray-900">Card</div>
<div class="bg-gray-100 dark:bg-gray-800">Input bg</div>

<!-- Shadows -->
<div class="shadow-sm dark:shadow-gray-900/50">With dark shadow</div>

<!-- Opacity/Color modifier -->
<div class="bg-black/5 dark:bg-white/5">Semi-transparent</div>
```

---

## Step 213: Complete Dark Mode Components

```html
<!-- Navbar -->
<nav class="
  bg-white dark:bg-gray-900 
  border-b border-gray-200 dark:border-gray-800
">
  <div class="flex items-center px-6 h-16">
    <span class="font-bold text-gray-900 dark:text-white">Logo</span>
    <div class="ml-auto flex items-center gap-4">
      <a href="#" class="text-gray-600 dark:text-gray-400 hover:text-gray-900 dark:hover:text-white text-sm transition-colors">
        Nav Link
      </a>
      <button class="bg-indigo-600 hover:bg-indigo-700 text-white text-sm px-4 py-2 rounded-lg">
        CTA
      </button>
    </div>
  </div>
</nav>

<!-- Card -->
<div class="
  bg-white dark:bg-gray-800 
  border border-gray-200 dark:border-gray-700 
  rounded-2xl p-6 
  shadow-sm dark:shadow-gray-900/30
">
  <h3 class="font-bold text-gray-900 dark:text-white mb-2">Card Title</h3>
  <p class="text-gray-500 dark:text-gray-400 text-sm">Card description text</p>
  <button class="
    mt-4 bg-indigo-600 hover:bg-indigo-700 
    dark:bg-indigo-500 dark:hover:bg-indigo-400
    text-white text-sm font-medium px-4 py-2 rounded-lg transition-colors
  ">
    Action
  </button>
</div>

<!-- Input -->
<input 
  type="text"
  class="
    w-full px-4 py-3 rounded-xl
    bg-white dark:bg-gray-800
    border border-gray-300 dark:border-gray-600
    text-gray-900 dark:text-gray-100
    placeholder:text-gray-400 dark:placeholder:text-gray-500
    focus:outline-none
    focus:border-indigo-500 dark:focus:border-indigo-400
    focus:ring-2 focus:ring-indigo-200 dark:focus:ring-indigo-800
    transition-all
  "
  placeholder="Type here..."
>

<!-- Badge -->
<span class="
  text-xs font-semibold px-2.5 py-1 rounded-full
  bg-indigo-100 dark:bg-indigo-900/50
  text-indigo-700 dark:text-indigo-300
">
  Badge
</span>

<!-- Divider -->
<hr class="border-gray-200 dark:border-gray-700">

<!-- Code block -->
<pre class="
  bg-gray-950 dark:bg-black 
  text-gray-100 
  p-4 rounded-xl overflow-x-auto text-sm font-mono
">code here</pre>
```

---

## Step 214: Dark Mode Toggle Button

```html
<!-- Toggle button -->
<button
  id="theme-toggle"
  onclick="toggleDark()"
  class="
    relative size-10 rounded-xl 
    bg-gray-100 dark:bg-gray-800 
    hover:bg-gray-200 dark:hover:bg-gray-700
    flex items-center justify-center
    text-gray-600 dark:text-gray-400
    transition-all
  "
  aria-label="Toggle dark mode"
>
  <!-- Sun icon (shown in dark mode) -->
  <svg class="hidden dark:block size-5" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
    <path stroke-linecap="round" stroke-linejoin="round" d="M12 3v2.25m6.364.386-1.591 1.591M21 12h-2.25m-.386 6.364-1.591-1.591M12 18.75V21m-4.773-4.227-1.591 1.591M5.25 12H3m4.227-4.773L5.636 5.636M15.75 12a3.75 3.75 0 1 1-7.5 0 3.75 3.75 0 0 1 7.5 0Z"/>
  </svg>
  <!-- Moon icon (shown in light mode) -->
  <svg class="block dark:hidden size-5" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
    <path stroke-linecap="round" stroke-linejoin="round" d="M21.752 15.002A9.72 9.72 0 0 1 18 15.75c-5.385 0-9.75-4.365-9.75-9.75 0-1.33.266-2.597.748-3.752A9.753 9.753 0 0 0 3 11.25C3 16.635 7.365 21 12.75 21a9.753 9.753 0 0 0 9.002-5.998Z"/>
  </svg>
</button>

<script>
  function toggleDark() {
    document.documentElement.classList.toggle('dark');
    localStorage.setItem('theme', 
      document.documentElement.classList.contains('dark') ? 'dark' : 'light'
    );
  }
  
  // Load saved preference
  const savedTheme = localStorage.getItem('theme');
  const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
  
  if (savedTheme === 'dark' || (!savedTheme && prefersDark)) {
    document.documentElement.classList.add('dark');
  }
</script>
```

---

## Step 215: Dark Mode + CSS Variables

```css
/* globals.css */
@layer base {
  :root {
    --bg: 0 0% 100%;
    --bg-subtle: 220 14% 96%;
    --bg-muted:  220 14% 93%;
    
    --text: 222 47% 11%;
    --text-muted: 220 9% 46%;
    --text-subtle: 218 11% 65%;
    
    --border: 220 13% 91%;
    --border-strong: 218 11% 65%;
    
    --primary: 239 84% 67%;
    --primary-dark: 238 83% 60%;
  }
  
  .dark {
    --bg: 222 47% 11%;
    --bg-subtle: 215 28% 17%;
    --bg-muted:  217 19% 27%;
    
    --text: 210 40% 98%;
    --text-muted: 216 12% 84%;
    --text-subtle: 215 14% 34%;
    
    --border: 217 19% 27%;
    --border-strong: 215 14% 34%;
  }
}
```

```javascript
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      colors: {
        // ใช้ CSS variables
        bg: {
          DEFAULT: 'hsl(var(--bg))',
          subtle: 'hsl(var(--bg-subtle))',
          muted: 'hsl(var(--bg-muted))',
        },
        'app-text': {
          DEFAULT: 'hsl(var(--text))',
          muted: 'hsl(var(--text-muted))',
          subtle: 'hsl(var(--text-subtle))',
        },
        'app-border': {
          DEFAULT: 'hsl(var(--border))',
          strong: 'hsl(var(--border-strong))',
        },
      },
    },
  },
}
```

```html
<!-- ใช้งานง่ายขึ้น ไม่ต้องเขียน dark: ทุกที่ -->
<div class="bg-bg text-app-text border border-app-border rounded-xl p-4">
  Automatically adapts to dark mode
</div>
```

---

## Step 216–220: Workshop — Full Dark Mode Page

```html
<!DOCTYPE html>
<html lang="th" class="">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Dark Mode Workshop</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@400;500;600;700;800&display=swap" rel="stylesheet">
  <style>
    body { font-family: 'Sarabun', sans-serif; }
  </style>
  <script>
    // Apply saved theme before first render (prevent flash)
    const t = localStorage.getItem('theme');
    const d = window.matchMedia('(prefers-color-scheme: dark)').matches;
    if (t === 'dark' || (!t && d)) {
      document.documentElement.classList.add('dark');
    }
  </script>
</head>
<body class="bg-gray-50 dark:bg-gray-950 min-h-screen transition-colors duration-300">

<!-- Navbar -->
<nav class="sticky top-0 z-50 bg-white/80 dark:bg-gray-900/80 backdrop-blur-md border-b border-gray-200 dark:border-gray-800 transition-colors">
  <div class="max-w-6xl mx-auto px-4 sm:px-6 flex items-center justify-between h-16">
    <div class="flex items-center gap-2">
      <div class="size-8 bg-indigo-600 rounded-lg flex items-center justify-center text-white font-black text-sm">T</div>
      <span class="font-black text-gray-900 dark:text-white">TailwindPro</span>
    </div>
    <div class="flex items-center gap-3">
      <div class="hidden sm:flex items-center gap-1">
        <a href="#" class="px-3 py-2 rounded-lg text-sm text-gray-600 dark:text-gray-400 hover:bg-gray-100 dark:hover:bg-gray-800 hover:text-gray-900 dark:hover:text-white transition-all">หลักสูตร</a>
        <a href="#" class="px-3 py-2 rounded-lg text-sm text-gray-600 dark:text-gray-400 hover:bg-gray-100 dark:hover:bg-gray-800 hover:text-gray-900 dark:hover:text-white transition-all">บล็อก</a>
      </div>
      <!-- Dark Mode Toggle -->
      <button 
        onclick="toggleDark()" 
        class="size-9 rounded-lg bg-gray-100 dark:bg-gray-800 hover:bg-gray-200 dark:hover:bg-gray-700 flex items-center justify-center text-gray-600 dark:text-yellow-400 transition-all"
        aria-label="Toggle dark mode"
      >
        <span class="dark:hidden text-sm">🌙</span>
        <span class="hidden dark:block text-sm">☀️</span>
      </button>
      <button class="bg-indigo-600 hover:bg-indigo-700 text-white text-sm font-medium px-4 py-2 rounded-lg transition-colors">
        เริ่มเรียน
      </button>
    </div>
  </div>
</nav>

<!-- Hero -->
<section class="max-w-6xl mx-auto px-4 sm:px-6 py-16 sm:py-24">
  <div class="text-center max-w-2xl mx-auto mb-14">
    <span class="inline-flex items-center gap-2 text-xs font-bold tracking-widest text-indigo-600 dark:text-indigo-400 uppercase bg-indigo-50 dark:bg-indigo-900/30 px-3 py-1.5 rounded-full mb-4">
      Dark Mode Ready
    </span>
    <h1 class="text-4xl sm:text-5xl font-black text-gray-900 dark:text-white leading-tight mb-4">
      ดีทั้ง Light<br>
      <span class="text-indigo-600 dark:text-indigo-400">และ Dark Mode</span>
    </h1>
    <p class="text-gray-500 dark:text-gray-400 text-lg leading-relaxed">
      ออกแบบมาสวยงามในทุก theme กด toggle ที่มุมขวาบนเพื่อลองเปลี่ยน
    </p>
  </div>
  
  <!-- Cards grid -->
  <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-5">
    
    <!-- Feature card -->
    <div class="bg-white dark:bg-gray-900 border border-gray-200 dark:border-gray-800 rounded-2xl p-6 hover:shadow-lg dark:hover:shadow-gray-900/50 hover:-translate-y-0.5 transition-all group">
      <div class="size-12 bg-indigo-100 dark:bg-indigo-900/50 rounded-2xl flex items-center justify-center text-2xl mb-4 group-hover:scale-110 transition-transform">📚</div>
      <h3 class="font-bold text-gray-900 dark:text-white mb-2">100+ บทเรียน</h3>
      <p class="text-gray-500 dark:text-gray-400 text-sm leading-relaxed">เนื้อหาครบ ตั้งแต่พื้นฐานถึงระดับโลก</p>
      <div class="mt-4 flex items-center gap-1 text-indigo-600 dark:text-indigo-400 text-sm font-medium">
        <span>เรียนเลย</span>
        <span class="group-hover:translate-x-1 transition-transform">→</span>
      </div>
    </div>
    
    <div class="bg-white dark:bg-gray-900 border border-gray-200 dark:border-gray-800 rounded-2xl p-6 hover:shadow-lg dark:hover:shadow-gray-900/50 hover:-translate-y-0.5 transition-all group">
      <div class="size-12 bg-green-100 dark:bg-green-900/50 rounded-2xl flex items-center justify-center text-2xl mb-4 group-hover:scale-110 transition-transform">🚀</div>
      <h3 class="font-bold text-gray-900 dark:text-white mb-2">Project จริง</h3>
      <p class="text-gray-500 dark:text-gray-400 text-sm leading-relaxed">ทุกบทมี Workshop และ Project ใช้งานได้จริง</p>
      <div class="mt-4 flex items-center gap-1 text-green-600 dark:text-green-400 text-sm font-medium">
        <span>ดูตัวอย่าง</span>
        <span class="group-hover:translate-x-1 transition-transform">→</span>
      </div>
    </div>
    
    <div class="bg-white dark:bg-gray-900 border border-gray-200 dark:border-gray-800 rounded-2xl p-6 hover:shadow-lg dark:hover:shadow-gray-900/50 hover:-translate-y-0.5 transition-all group">
      <div class="size-12 bg-purple-100 dark:bg-purple-900/50 rounded-2xl flex items-center justify-center text-2xl mb-4 group-hover:scale-110 transition-transform">🌙</div>
      <h3 class="font-bold text-gray-900 dark:text-white mb-2">Dark Mode</h3>
      <p class="text-gray-500 dark:text-gray-400 text-sm leading-relaxed">ทุก component รองรับ Dark Mode อย่างสมบูรณ์</p>
      <div class="mt-4 flex items-center gap-1 text-purple-600 dark:text-purple-400 text-sm font-medium">
        <span>ลองดู</span>
        <span class="group-hover:translate-x-1 transition-transform">→</span>
      </div>
    </div>
    
  </div>
</section>

<!-- Form section -->
<section class="max-w-md mx-auto px-4 pb-16">
  <div class="bg-white dark:bg-gray-900 border border-gray-200 dark:border-gray-800 rounded-2xl p-8">
    <h2 class="font-bold text-xl text-gray-900 dark:text-white mb-6">สมัครสมาชิก</h2>
    <div class="space-y-4">
      <div>
        <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1.5">ชื่อ</label>
        <input type="text" placeholder="สมชาย ใจดี"
          class="w-full px-4 py-3 rounded-xl border border-gray-300 dark:border-gray-700 bg-white dark:bg-gray-800 text-gray-900 dark:text-white placeholder:text-gray-400 dark:placeholder:text-gray-600 focus:outline-none focus:border-indigo-500 dark:focus:border-indigo-400 focus:ring-2 focus:ring-indigo-200 dark:focus:ring-indigo-900 transition-all">
      </div>
      <div>
        <label class="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1.5">อีเมล</label>
        <input type="email" placeholder="your@email.com"
          class="w-full px-4 py-3 rounded-xl border border-gray-300 dark:border-gray-700 bg-white dark:bg-gray-800 text-gray-900 dark:text-white placeholder:text-gray-400 dark:placeholder:text-gray-600 focus:outline-none focus:border-indigo-500 dark:focus:border-indigo-400 focus:ring-2 focus:ring-indigo-200 dark:focus:ring-indigo-900 transition-all">
      </div>
      <div class="flex items-center gap-3 pt-1">
        <div class="relative size-5 rounded border-2 border-gray-300 dark:border-gray-600 bg-white dark:bg-gray-800 group-has-[:checked]:bg-indigo-600 group-has-[:checked]:border-indigo-600 transition-all flex items-center justify-center">
        </div>
        <label class="text-sm text-gray-600 dark:text-gray-400">
          ยอมรับ <a href="#" class="text-indigo-600 dark:text-indigo-400 hover:underline">เงื่อนไข</a>
        </label>
      </div>
      <button class="w-full bg-indigo-600 hover:bg-indigo-700 dark:bg-indigo-500 dark:hover:bg-indigo-400 text-white font-semibold py-3 rounded-xl transition-colors mt-2">
        สมัครเลย →
      </button>
    </div>
  </div>
</section>

<script>
  function toggleDark() {
    const html = document.documentElement;
    html.classList.toggle('dark');
    localStorage.setItem('theme', html.classList.contains('dark') ? 'dark' : 'light');
  }
</script>

</body>
</html>
```

---

## 📝 สรุป Part 22

| Strategy | เมื่อใช้ |
|----------|---------|
| `darkMode: 'media'` | ตาม OS อัตโนมัติ |
| `darkMode: 'class'` | ควบคุมด้วย JS (แนะนำ) |
| CSS variables | ทำ theming ที่ยืดหยุ่นกว่า |

**Dark Mode Checklist:**
- `bg-white dark:bg-gray-900` — พื้นหลัง
- `text-gray-900 dark:text-white` — ตัวอักษร
- `border-gray-200 dark:border-gray-700` — เส้นขอบ
- `placeholder:text-gray-400 dark:placeholder:text-gray-600`
- `focus:ring-indigo-200 dark:focus:ring-indigo-800`

---

*Part 22 — จาก 100 Parts | Steps 211–220 จาก 1,000 Steps*
