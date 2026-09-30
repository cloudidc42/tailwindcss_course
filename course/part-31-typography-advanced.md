# Part 31: Advanced Typography System
## Steps 301–310: Typography ระดับมืออาชีพ

---

## 🎯 เป้าหมายของ Part นี้

- Type scale system
- Fluid typography (clamp)
- Prose styles (@tailwindcss/typography)
- Custom font loading
- Text effects (gradient, glow, stroke)
- Truncation & line clamping
- Vertical rhythm

---

## Step 301: Type Scale System

```html
<script src="https://cdn.tailwindcss.com"></script>
<script>
tailwind.config = {
  theme: {
    extend: {
      fontSize: {
        '2xs': ['0.625rem', { lineHeight: '1rem' }],
        'xs':  ['0.75rem',  { lineHeight: '1.25rem' }],
        'sm':  ['0.875rem', { lineHeight: '1.5rem' }],
        'base':['1rem',     { lineHeight: '1.75rem' }],
        'lg':  ['1.125rem', { lineHeight: '1.75rem' }],
        'xl':  ['1.25rem',  { lineHeight: '1.875rem' }],
        '2xl': ['1.5rem',   { lineHeight: '2rem' }],
        '3xl': ['1.875rem', { lineHeight: '2.25rem' }],
        '4xl': ['2.25rem',  { lineHeight: '2.5rem' }],
        '5xl': ['3rem',     { lineHeight: '1.2' }],
        '6xl': ['3.75rem',  { lineHeight: '1.1' }],
        '7xl': ['4.5rem',   { lineHeight: '1.05' }],
        '8xl': ['6rem',     { lineHeight: '1' }],
        '9xl': ['8rem',     { lineHeight: '1' }],
      },
      fontFamily: {
        sans: ['"Inter"', 'system-ui', 'sans-serif'],
        serif: ['"Playfair Display"', 'Georgia', 'serif'],
        mono:  ['"JetBrains Mono"', 'monospace'],
        thai: ['"Sarabun"', 'sans-serif'],
      },
      lineHeight: {
        tight: '1.1',
        snug:  '1.25',
        normal:'1.5',
        relaxed:'1.75',
        loose: '2',
      },
      letterSpacing: {
        tighter: '-0.05em',
        tight:   '-0.025em',
        normal:  '0',
        wide:    '0.025em',
        wider:   '0.05em',
        widest:  '0.1em',
        'super-wide': '0.15em',
      },
    }
  }
}
</script>

<!-- Type scale demo -->
<div class="space-y-3 p-6">
  <p class="text-9xl font-black leading-none">9xl</p>
  <p class="text-7xl font-black">Display</p>
  <p class="text-5xl font-bold">Heading 1</p>
  <p class="text-3xl font-bold">Heading 2</p>
  <p class="text-xl font-semibold">Heading 3</p>
  <p class="text-base leading-relaxed">Body text ระดับ base ที่อ่านง่ายและสบายตา</p>
  <p class="text-sm text-gray-500">Secondary text</p>
  <p class="text-xs text-gray-400">Caption / meta text</p>
  <p class="text-2xs text-gray-300">2xs — tiny label</p>
</div>
```

---

## Step 302: Fluid Typography (CSS clamp)

```html
<!-- Fluid typography ที่ scale ตาม viewport -->
<style>
  /* clamp(min, preferred, max) */
  .text-fluid-sm   { font-size: clamp(0.75rem,  2vw, 0.875rem); }
  .text-fluid-base { font-size: clamp(0.875rem, 2.5vw, 1rem); }
  .text-fluid-lg   { font-size: clamp(1rem,     3vw, 1.25rem); }
  .text-fluid-xl   { font-size: clamp(1.125rem, 4vw, 1.5rem); }
  .text-fluid-2xl  { font-size: clamp(1.5rem,   5vw, 2.25rem); }
  .text-fluid-3xl  { font-size: clamp(1.875rem, 6vw, 3.75rem); }
  .text-fluid-hero { font-size: clamp(2.5rem,   8vw, 6rem); }
  
  /* Fluid line-height */
  .text-fluid-hero { line-height: clamp(1.1, calc(1.1 + 0.5vw), 1.2); }
</style>

<div class="p-8 space-y-4">
  <h1 class="text-fluid-hero font-black text-gray-900">ขนาดใหญ่สวยงาม</h1>
  <h2 class="text-fluid-3xl font-bold text-gray-800">Heading Fluid</h2>
  <p class="text-fluid-base text-gray-600 leading-relaxed max-w-prose">
    ข้อความที่ scale ตาม viewport อย่างนุ่มนวล ไม่กระโดด
  </p>
</div>
```

---

## Step 303: Text Effects

```html
<div class="p-8 space-y-8 bg-gray-900">
  <!-- Gradient text -->
  <h2 class="text-5xl font-black bg-gradient-to-r from-indigo-400 via-purple-400 to-pink-400 bg-clip-text text-transparent">
    Gradient Text
  </h2>
  
  <!-- Animated gradient -->
  <style>
    @keyframes gradientShift { 0%,100%{background-position:0% 50%} 50%{background-position:100% 50%} }
    .text-animated-gradient {
      background: linear-gradient(270deg, #6366f1, #a855f7, #ec4899, #6366f1);
      background-size: 200% 200%;
      -webkit-background-clip: text;
      background-clip: text;
      -webkit-text-fill-color: transparent;
      animation: gradientShift 4s ease infinite;
    }
  </style>
  <h2 class="text-5xl font-black text-animated-gradient">Animated Gradient</h2>
  
  <!-- Glow text -->
  <style>
    .text-glow { text-shadow: 0 0 20px rgba(99,102,241,0.8), 0 0 40px rgba(99,102,241,0.4); }
    .text-glow-green { text-shadow: 0 0 20px rgba(34,197,94,0.8), 0 0 40px rgba(34,197,94,0.4); }
  </style>
  <h2 class="text-5xl font-black text-white text-glow">Glow Effect</h2>
  
  <!-- Outlined text -->
  <style>
    .text-outline { -webkit-text-stroke: 2px white; color: transparent; }
    .text-outline-color { -webkit-text-stroke: 3px #6366f1; color: transparent; }
  </style>
  <h2 class="text-5xl font-black text-outline">Outlined Text</h2>
  
  <!-- Neon text -->
  <style>
    .neon { 
      color: #fff;
      text-shadow: 
        0 0 7px #fff,
        0 0 10px #fff,
        0 0 21px #fff,
        0 0 42px #6366f1,
        0 0 82px #6366f1,
        0 0 92px #6366f1;
    }
  </style>
  <h2 class="text-5xl font-black neon">NEON</h2>
</div>
```

---

## Step 304: Line Clamping & Truncation

```html
<!-- Line clamp (Tailwind built-in) -->
<div class="space-y-4 max-w-sm">
  <!-- 1 line -->
  <p class="truncate text-sm text-gray-700">
    ข้อความยาวๆ ที่จะถูกตัดด้วย ... เมื่อเกิน 1 บรรทัด
  </p>
  
  <!-- 2 lines -->
  <p class="line-clamp-2 text-sm text-gray-700 leading-relaxed">
    ข้อความยาวๆ ที่จะถูกตัดด้วย ... เมื่อเกิน 2 บรรทัด ซึ่งช่วยให้ card UI มีความสม่ำเสมอ ไม่ยาวเกินไป
  </p>
  
  <!-- 3 lines -->
  <p class="line-clamp-3 text-sm text-gray-700 leading-relaxed">
    ข้อความยาวๆ ที่จะถูกตัดด้วย ... เมื่อเกิน 3 บรรทัด เนื้อหาส่วนที่เกินจะซ่อน และแสดง ... แทน ทำให้ UI ดูเรียบร้อย
  </p>
  
  <!-- None — show all -->
  <p class="line-clamp-none text-sm text-gray-700 leading-relaxed">
    แสดงทั้งหมดโดยไม่ตัด ใช้ line-clamp-none เพื่อ override
  </p>
</div>
```

---

## Step 305: Prose (Typography Plugin)

```html
<!-- @tailwindcss/typography plugin -->
<script src="https://cdn.tailwindcss.com?plugins=typography"></script>

<article class="prose prose-lg dark:prose-invert prose-indigo max-w-prose mx-auto p-8">
  <h1>บทนำสู่ Tailwind CSS</h1>
  
  <p>Tailwind CSS เป็น utility-first CSS framework ที่ช่วยให้คุณสร้าง UI ได้อย่างรวดเร็วและ consistent</p>
  
  <h2>ข้อดีของ Tailwind</h2>
  <ul>
    <li>ไม่ต้องตั้งชื่อ class เอง</li>
    <li>สร้าง design system ได้ง่าย</li>
    <li>Production CSS เล็กมากด้วย PurgeCSS</li>
  </ul>
  
  <blockquote>
    "Tailwind CSS ทำให้ฉันเขียน CSS เร็วขึ้น 3 เท่า และ UI สวยขึ้น 10 เท่า"
  </blockquote>
  
  <pre><code>/* ตัวอย่างโค้ด */
&lt;div class="flex items-center gap-4"&gt;
  &lt;span class="text-indigo-600"&gt;Hello&lt;/span&gt;
&lt;/div&gt;</code></pre>
  
  <h3>การติดตั้ง</h3>
  <p>ใช้ npm หรือ CDN ก็ได้ ขึ้นอยู่กับ project</p>
</article>

<!-- Prose modifiers -->
<div class="prose 
  prose-headings:font-black 
  prose-headings:text-gray-900
  prose-p:text-gray-600
  prose-a:text-indigo-600
  prose-a:no-underline
  prose-a:hover:underline
  prose-blockquote:border-indigo-500
  prose-blockquote:bg-indigo-50
  prose-blockquote:p-4
  prose-blockquote:rounded-r-xl
  prose-code:bg-gray-100
  prose-code:px-1.5
  prose-code:py-0.5
  prose-code:rounded-md
  prose-code:text-indigo-700
  max-w-2xl
">
  <h2>Custom Prose Styles</h2>
  <p>ปรับแต่ง prose ด้วย modifier classes ตาม element ที่ต้องการ</p>
</div>
```

---

## Step 306: Reading Comfort & Vertical Rhythm

```html
<!-- Optimal reading width + vertical rhythm -->
<div class="max-w-reading mx-auto">
  <style>
    /* ความกว้างอ่านสบาย: 45–75 characters */
    .max-w-reading { max-width: 65ch; }
    
    /* Vertical rhythm: consistent spacing *)
    .rhythm > * + * { margin-top: 1.5em; }
    .rhythm > h1 + * { margin-top: 0.5em; }
    .rhythm > h2 + * { margin-top: 0.5em; }
    .rhythm h2 { margin-top: 2em; }
  </style>

  <div class="rhythm p-8">
    <h1 class="text-4xl font-black text-gray-900">หัวข้อหลัก</h1>
    <p class="text-gray-600 leading-relaxed">
      ย่อหน้าแนะนำ อ่านง่ายด้วยความกว้างที่เหมาะสม ไม่กว้างเกินหรือแคบเกิน เหมาะสำหรับ blog, article, documentation
    </p>
    <h2 class="text-2xl font-bold text-gray-900">หัวข้อรอง</h2>
    <p class="text-gray-600 leading-relaxed">
      เนื้อหาส่วนที่สอง vertical rhythm ช่วยให้ spacing ระหว่าง elements สม่ำเสมอ ทำให้อ่านง่ายขึ้น
    </p>
  </div>
</div>
```

---

## Step 307–310: Workshop — Typography Showcase

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Typography System</title>
  <script src="https://cdn.tailwindcss.com?plugins=typography"></script>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800;900&family=Sarabun:wght@400;500;600;700&display=swap" rel="stylesheet">
  <style>
    body { font-family: 'Inter', 'Sarabun', sans-serif; }
    .max-w-reading { max-width: 65ch; }
    @keyframes gradShift { 0%,100%{background-position:0% 50%} 50%{background-position:100% 50%} }
    .text-animated {
      background: linear-gradient(270deg, #6366f1, #a855f7, #ec4899);
      background-size: 300% 300%;
      -webkit-background-clip: text;
      background-clip: text;
      -webkit-text-fill-color: transparent;
      animation: gradShift 5s ease infinite;
    }
  </style>
</head>
<body class="bg-white">

  <!-- Hero -->
  <section class="py-24 px-6 text-center border-b border-gray-100">
    <p class="text-sm font-bold tracking-widest text-indigo-500 uppercase mb-4">Typography System</p>
    <h1 class="text-animated font-black leading-tight mb-6" style="font-size: clamp(2.5rem, 6vw, 5rem);">
      Beautiful Type,<br>Beautiful Design
    </h1>
    <p class="text-gray-500 max-w-xl mx-auto leading-relaxed" style="font-size: clamp(1rem, 2vw, 1.25rem);">
      ระบบ typography ที่ดีคือรากฐานของ UI ที่สวยงามและอ่านง่าย
    </p>
  </section>

  <!-- Type scale -->
  <section class="py-16 px-6 max-w-4xl mx-auto border-b border-gray-100">
    <h2 class="text-2xl font-bold text-gray-900 mb-8">Type Scale</h2>
    <div class="space-y-4">
      <div class="flex items-baseline gap-6">
        <span class="text-xs text-gray-400 w-16 flex-none font-mono">9xl</span>
        <span class="text-9xl font-black text-gray-900 leading-none">Aa</span>
      </div>
      <div class="flex items-baseline gap-6">
        <span class="text-xs text-gray-400 w-16 flex-none font-mono">5xl</span>
        <span class="text-5xl font-bold text-gray-900">ข้อความ</span>
      </div>
      <div class="flex items-baseline gap-6">
        <span class="text-xs text-gray-400 w-16 flex-none font-mono">3xl</span>
        <span class="text-3xl font-semibold text-gray-900">Heading Two</span>
      </div>
      <div class="flex items-baseline gap-6">
        <span class="text-xs text-gray-400 w-16 flex-none font-mono">xl</span>
        <span class="text-xl font-medium text-gray-900">Heading Three สาม</span>
      </div>
      <div class="flex items-baseline gap-6">
        <span class="text-xs text-gray-400 w-16 flex-none font-mono">base</span>
        <span class="text-base text-gray-600">Body text อ่านง่ายสบายตา lineHeight relaxed</span>
      </div>
      <div class="flex items-baseline gap-6">
        <span class="text-xs text-gray-400 w-16 flex-none font-mono">sm</span>
        <span class="text-sm text-gray-500">Secondary / helper text ขนาดเล็ก</span>
      </div>
      <div class="flex items-baseline gap-6">
        <span class="text-xs text-gray-400 w-16 flex-none font-mono">xs</span>
        <span class="text-xs text-gray-400">Caption, label, meta information</span>
      </div>
    </div>
  </section>

  <!-- Text effects -->
  <section class="py-16 px-6 bg-gray-950 text-white">
    <div class="max-w-4xl mx-auto">
      <h2 class="text-2xl font-bold mb-8">Text Effects</h2>
      <div class="space-y-8">
        <div>
          <p class="text-xs text-gray-500 mb-2">Gradient</p>
          <h3 class="text-5xl font-black bg-gradient-to-r from-indigo-400 to-pink-400 bg-clip-text text-transparent">Gradient Text</h3>
        </div>
        <div>
          <p class="text-xs text-gray-500 mb-2">Animated Gradient</p>
          <h3 class="text-5xl font-black text-animated">Animated</h3>
        </div>
        <div>
          <p class="text-xs text-gray-500 mb-2">Outlined</p>
          <h3 class="text-5xl font-black" style="-webkit-text-stroke: 2px white; color: transparent;">Outlined</h3>
        </div>
        <div>
          <p class="text-xs text-gray-500 mb-2">Glow</p>
          <h3 class="text-5xl font-black text-white" style="text-shadow: 0 0 20px rgba(99,102,241,0.9), 0 0 50px rgba(99,102,241,0.5);">Glow</h3>
        </div>
      </div>
    </div>
  </section>

  <!-- Prose article -->
  <section class="py-16 px-6">
    <div class="max-w-reading mx-auto">
      <article class="prose prose-lg prose-indigo">
        <h1>บทความตัวอย่าง</h1>
        <p>Lorem ipsum ไทย — ตัวอย่างข้อความในบทความที่ใช้ <code>@tailwindcss/typography</code> plugin สำหรับ style ที่สวยงามและอ่านง่าย</p>
        <h2>ส่วนที่ 1</h2>
        <p>เนื้อหาส่วนแรก มี heading, paragraph, list, blockquote และ code blocks ที่ styled อัตโนมัติ</p>
        <ul>
          <li>ข้อดีที่ 1</li>
          <li>ข้อดีที่ 2</li>
          <li>ข้อดีที่ 3</li>
        </ul>
        <blockquote>
          "Tailwind CSS changes how I write CSS forever"
        </blockquote>
      </article>
    </div>
  </section>

</body>
</html>
```

---

## 📝 สรุป Part 31

| Concept | Class/Technique |
|---------|----------------|
| Type scale | `text-xs` → `text-9xl` |
| Fluid type | `clamp()` ใน CSS |
| Gradient text | `bg-clip-text text-transparent` |
| Line clamp | `line-clamp-2` |
| Prose | `prose prose-lg prose-indigo` |
| Reading width | `max-w-prose` / `65ch` |

---

*Part 31 — Advanced Typography | Steps 301–310 จาก 1,000 Steps*
