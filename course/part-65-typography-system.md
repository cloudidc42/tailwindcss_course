# Part 65: Advanced Typography System

## เป้าหมาย
- Typography scale ระดับ professional
- @tailwindcss/typography plugin (prose)
- Fluid typography ด้วย clamp()
- Custom font pairing + variable fonts
- Steps 641–650

---

## Step 641: Typography Scale Design

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Typography System - Step 641</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&family=Merriweather:wght@300;400;700;900&family=JetBrains+Mono:wght@400;600&display=swap" rel="stylesheet">
  <script>
    tailwind.config = {
      theme: {
        extend: {
          fontFamily: {
            sans:  ['Inter', 'system-ui', 'sans-serif'],
            serif: ['Merriweather', 'Georgia', 'serif'],
            mono:  ['JetBrains Mono', 'Menlo', 'monospace'],
          },
          fontSize: {
            'display-2xl': ['4.5rem', { lineHeight: '1.1', letterSpacing: '-0.02em', fontWeight: '800' }],
            'display-xl':  ['3.75rem', { lineHeight: '1.1', letterSpacing: '-0.02em', fontWeight: '800' }],
            'display-lg':  ['3rem',    { lineHeight: '1.2', letterSpacing: '-0.015em', fontWeight: '700' }],
            'display-md':  ['2.25rem', { lineHeight: '1.25', letterSpacing: '-0.01em', fontWeight: '700' }],
            'display-sm':  ['1.875rem',{ lineHeight: '1.3', fontWeight: '600' }],
            'body-xl':     ['1.25rem', { lineHeight: '1.75' }],
            'body-lg':     ['1.125rem',{ lineHeight: '1.75' }],
            'body-md':     ['1rem',    { lineHeight: '1.75' }],
            'body-sm':     ['0.875rem',{ lineHeight: '1.6' }],
            'body-xs':     ['0.75rem', { lineHeight: '1.5' }],
          },
        },
      },
    }
  </script>
</head>
<body class="bg-white p-8 max-w-4xl mx-auto font-sans">

  <div class="space-y-8">
    <h1 class="text-display-2xl text-gray-900">Typography Scale</h1>

    <!-- Display sizes -->
    <section class="space-y-4 border-b pb-8">
      <h2 class="text-sm font-semibold text-indigo-600 uppercase tracking-widest">Display</h2>
      <div class="space-y-2">
        <div><p class="text-display-2xl text-gray-900 leading-none">Display 2XL — 72px</p><p class="text-xs text-gray-400">4.5rem / 800</p></div>
        <div><p class="text-display-xl text-gray-900 leading-none">Display XL — 60px</p><p class="text-xs text-gray-400">3.75rem / 800</p></div>
        <div><p class="text-display-lg text-gray-900">Display LG — 48px</p><p class="text-xs text-gray-400">3rem / 700</p></div>
        <div><p class="text-display-md text-gray-900">Display MD — 36px</p><p class="text-xs text-gray-400">2.25rem / 700</p></div>
        <div><p class="text-display-sm text-gray-900">Display SM — 30px</p><p class="text-xs text-gray-400">1.875rem / 600</p></div>
      </div>
    </section>

    <!-- Body sizes -->
    <section class="space-y-3 border-b pb-8">
      <h2 class="text-sm font-semibold text-indigo-600 uppercase tracking-widest">Body Text</h2>
      <p class="text-body-xl text-gray-700">Body XL — 20px — เนื้อหาขนาดใหญ่สำหรับ hero sections</p>
      <p class="text-body-lg text-gray-700">Body LG — 18px — บทความหลัก เนื้อหาสำคัญ</p>
      <p class="text-body-md text-gray-700">Body MD — 16px — เนื้อหาทั่วไป default body text</p>
      <p class="text-body-sm text-gray-700">Body SM — 14px — caption ข้อมูลรอง UI labels</p>
      <p class="text-body-xs text-gray-700">Body XS — 12px — overline, fine print</p>
    </section>

    <!-- Font families -->
    <section class="space-y-4">
      <h2 class="text-sm font-semibold text-indigo-600 uppercase tracking-widest">Font Families</h2>
      <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <div>
          <p class="text-xs text-gray-400 mb-2">Sans-serif (Inter)</p>
          <p class="font-sans text-2xl font-bold">The quick brown fox</p>
          <p class="font-sans text-sm text-gray-600 mt-1">ABCDEFGHIJKLMNOPQRSTUVWXYZ<br>abcdefghijklmnopqrstuvwxyz<br>0123456789</p>
        </div>
        <div>
          <p class="text-xs text-gray-400 mb-2">Serif (Merriweather)</p>
          <p class="font-serif text-2xl font-bold">The quick brown fox</p>
          <p class="font-serif text-sm text-gray-600 mt-1">ABCDEFGHIJKLMNOPQRSTUVWXYZ<br>abcdefghijklmnopqrstuvwxyz<br>0123456789</p>
        </div>
        <div>
          <p class="text-xs text-gray-400 mb-2">Mono (JetBrains Mono)</p>
          <p class="font-mono text-xl font-semibold">const hello = 'world'</p>
          <p class="font-mono text-sm text-gray-600 mt-1">if (x &gt; 0) {'{}'}<br>return x * 2;<br>{'}'}</p>
        </div>
      </div>
    </section>
  </div>
</body>
</html>
```

---

## Step 642: Fluid Typography ด้วย clamp()

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Fluid Typography - Step 642</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    /*
     * clamp(min, preferred, max)
     * preferred = slope * 100vw + intercept
     *
     * Formula:
     * slope = (maxSize - minSize) / (maxWidth - minWidth)
     * intercept = minSize - slope * minWidth
     *
     * Example: 16px at 320px → 24px at 1280px
     * slope = (24-16)/(1280-320) = 8/960 = 0.00833
     * intercept = 16 - 0.00833 * 320 = 13.33px
     * → clamp(1rem, 0.833rem + 2.083vw, 1.5rem)
     */

    :root {
      --fluid-base:    clamp(1rem,    0.769rem + 0.962vw,  1.25rem);
      --fluid-md:      clamp(1.125rem, 0.894rem + 0.962vw, 1.375rem);
      --fluid-lg:      clamp(1.25rem,  0.942rem + 1.282vw, 1.625rem);
      --fluid-xl:      clamp(1.5rem,   1.077rem + 1.763vw, 2rem);
      --fluid-2xl:     clamp(1.875rem, 1.202rem + 2.804vw, 2.625rem);
      --fluid-3xl:     clamp(2.25rem,  1.327rem + 3.846vw, 3.25rem);
      --fluid-4xl:     clamp(3rem,     1.731rem + 5.288vw, 4.5rem);
      --fluid-display: clamp(3.5rem,   1.923rem + 6.571vw, 5.5rem);
    }

    .text-fluid-base    { font-size: var(--fluid-base) }
    .text-fluid-md      { font-size: var(--fluid-md) }
    .text-fluid-lg      { font-size: var(--fluid-lg) }
    .text-fluid-xl      { font-size: var(--fluid-xl) }
    .text-fluid-2xl     { font-size: var(--fluid-2xl) }
    .text-fluid-3xl     { font-size: var(--fluid-3xl) }
    .text-fluid-4xl     { font-size: var(--fluid-4xl) }
    .text-fluid-display { font-size: var(--fluid-display) }
  </style>
</head>
<body class="bg-white p-8 max-w-4xl mx-auto">

  <div class="space-y-6">
    <h1 class="text-sm font-semibold text-indigo-600 uppercase tracking-widest">Fluid Typography (ปรับขนาดตามหน้าจอ)</h1>

    <div class="space-y-4">
      <p class="text-fluid-display font-extrabold text-gray-900 leading-tight">Fluid Display</p>
      <p class="text-fluid-4xl font-extrabold text-gray-900 leading-tight">Fluid 4XL</p>
      <p class="text-fluid-3xl font-bold text-gray-800">Fluid 3XL Heading</p>
      <p class="text-fluid-2xl font-bold text-gray-800">Fluid 2XL Section Title</p>
      <p class="text-fluid-xl font-semibold text-gray-700">Fluid XL Subheading</p>
      <p class="text-fluid-lg text-gray-700">Fluid LG — body text สำหรับหน้า landing page</p>
      <p class="text-fluid-md text-gray-600">Fluid MD — เนื้อหาทั่วไปที่ปรับขนาดตาม viewport</p>
      <p class="text-fluid-base text-gray-600">Fluid Base — ขนาดพื้นฐานที่ fluid ระหว่าง 16-20px</p>
    </div>

    <div class="bg-blue-50 rounded-2xl p-4 text-sm text-blue-700">
      💡 ลองปรับขนาดหน้าต่างเบราว์เซอร์เพื่อดูตัวอักษรปรับขนาดอัตโนมัติ
    </div>
  </div>
</body>
</html>
```

---

## Step 643: @tailwindcss/typography (prose)

```bash
npm install -D @tailwindcss/typography
```

`tailwind.config.ts`:
```ts
plugins: [require('@tailwindcss/typography')]
```

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Prose Typography - Step 643</title>
  <script src="https://cdn.tailwindcss.com?plugins=typography"></script>
</head>
<body class="bg-gray-50 p-8">
  <article class="prose prose-lg prose-indigo max-w-3xl mx-auto bg-white rounded-2xl p-8 shadow-sm">

    <h1>สร้าง Blog ด้วย Tailwind CSS Typography</h1>
    <p class="lead">Tailwind Typography plugin (<code>@tailwindcss/typography</code>) ช่วยให้คุณจัดสไตล์ HTML content ที่มาจาก Markdown หรือ CMS ได้อย่างสวยงาม</p>

    <h2>ทำไมต้องใช้ prose?</h2>
    <p>โดยปกติ Tailwind reset default styles ทั้งหมด ทำให้ <code>h1</code>, <code>p</code>, <code>ul</code> ไม่มี style เลย เมื่อ render markdown content จาก API จะต้องเพิ่ม class เอง แต่ prose ทำให้ทุกอย่างสวยงามโดยอัตโนมัติ</p>

    <h3>ตัวอย่างการใช้งาน</h3>
    <pre><code>&lt;article class="prose prose-lg prose-indigo max-w-3xl"&gt;
  {dangerouslySetInnerHTML={{ __html: markdownContent }}}
&lt;/article&gt;</code></pre>

    <h2>Prose Variants</h2>
    <ul>
      <li><strong>prose-sm</strong> — ขนาดเล็ก</li>
      <li><strong>prose-base</strong> — ขนาดปกติ (default)</li>
      <li><strong>prose-lg</strong> — ขนาดใหญ่ (แนะนำสำหรับ blog)</li>
      <li><strong>prose-xl</strong> — ขนาดใหญ่มาก</li>
      <li><strong>prose-2xl</strong> — ใหญ่สุด</li>
    </ul>

    <h2>Color Variants</h2>
    <p>เปลี่ยนสีลิงก์และ accent ด้วย <code>prose-{color}</code>:</p>
    <ul>
      <li>prose-indigo — indigo (default)</li>
      <li>prose-blue — blue</li>
      <li>prose-emerald — emerald</li>
      <li>prose-rose — rose</li>
    </ul>

    <blockquote>
      <p>"Tailwind CSS Typography plugin เป็นหนึ่งใน plugin ที่ขาดไม่ได้สำหรับทุก content-heavy website"</p>
    </blockquote>

    <h2>Dark Mode</h2>
    <p>เพิ่ม <code>prose-invert</code> สำหรับ dark mode:</p>
    <pre><code>&lt;article class="prose dark:prose-invert"&gt;</code></pre>

    <p>Tailwind Typography จัดการทุกอย่างให้ครบ ตั้งแต่ <code>h1-h6</code>, <code>p</code>, <code>ul/ol</code>, <code>blockquote</code>, <code>code</code>, <code>pre</code>, <code>table</code>, <code>img</code>, และ <code>a</code></p>

  </article>
</body>
</html>
```

---

## Steps 644–650: Custom Prose + Variable Fonts + Workshop

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Typography Workshop - Steps 644-650</title>
  <script src="https://cdn.tailwindcss.com?plugins=typography"></script>
  <link href="https://fonts.googleapis.com/css2?family=Inter:ital,opsz,wght@0,14..32,100..900;1,14..32,100..900&family=Lora:ital,wght@0,400..700;1,400..700&display=swap" rel="stylesheet">
  <script>
    tailwind.config = {
      theme: {
        extend: {
          fontFamily: {
            sans:  ['Inter', 'system-ui', 'sans-serif'],
            serif: ['Lora', 'Georgia', 'serif'],
          },
          typography: theme => ({
            custom: {
              css: {
                '--tw-prose-body':      theme('colors.gray[700]'),
                '--tw-prose-headings':  theme('colors.gray[900]'),
                '--tw-prose-links':     theme('colors.indigo[600]'),
                '--tw-prose-bold':      theme('colors.gray[900]'),
                '--tw-prose-code':      theme('colors.indigo[700]'),
                '--tw-prose-pre-bg':    theme('colors.gray[900]'),
                '--tw-prose-invert-body':     theme('colors.gray[300]'),
                '--tw-prose-invert-headings': '#ffffff',
                h1: { fontFamily: 'Lora, serif', fontWeight: '700' },
                h2: { fontFamily: 'Lora, serif', fontWeight: '600' },
                'code::before': { content: '""' },
                'code::after':  { content: '""' },
                code: {
                  backgroundColor: theme('colors.indigo[50]'),
                  borderRadius: '0.375rem',
                  padding: '0.2em 0.4em',
                },
              },
            },
          }),
        },
      },
    }
  </script>
  <style>
    /* Variable font — weight animation */
    .font-morph {
      font-variation-settings: 'wght' var(--font-weight, 400);
      transition: font-variation-settings 0.4s ease;
    }
    .font-morph:hover { --font-weight: 900; }
  </style>
</head>
<body class="bg-gray-50">

  <!-- Page header -->
  <header class="bg-white border-b px-8 py-5 flex items-center justify-between">
    <div>
      <h1 class="font-sans font-bold text-gray-900">Typography Workshop</h1>
      <p class="text-sm text-gray-500">Steps 644–650</p>
    </div>
    <div class="flex gap-2">
      <button onclick="setMode('light')" class="px-3 py-1.5 text-xs font-semibold bg-white border rounded-lg hover:bg-gray-50">Light</button>
      <button onclick="setMode('dark')" class="px-3 py-1.5 text-xs font-semibold bg-gray-900 text-white rounded-lg hover:bg-gray-800">Dark</button>
    </div>
  </header>

  <main id="mainContent" class="max-w-5xl mx-auto p-8 space-y-12">

    <!-- Variable font demo -->
    <section>
      <h2 class="text-sm font-semibold text-indigo-600 uppercase tracking-widest mb-4">Variable Font (hover)</h2>
      <div class="space-y-1">
        <p class="font-morph text-4xl font-sans text-gray-900" style="--font-weight: 100">Hover me — Ultra Thin 100</p>
        <p class="font-morph text-4xl font-sans text-gray-900" style="--font-weight: 300">Hover me — Light 300</p>
        <p class="font-morph text-4xl font-sans text-gray-900" style="--font-weight: 400">Hover me — Regular 400</p>
        <p class="font-morph text-4xl font-sans text-gray-900" style="--font-weight: 600">Hover me — SemiBold 600</p>
        <p class="font-morph text-4xl font-sans text-gray-900" style="--font-weight: 900">Hover me — Black 900</p>
      </div>
    </section>

    <!-- Fluid typography demo -->
    <section>
      <h2 class="text-sm font-semibold text-indigo-600 uppercase tracking-widest mb-4">Fluid Type Scale</h2>
      <div class="space-y-2">
        <p style="font-size: clamp(2.5rem, 5vw + 1rem, 5rem)" class="font-bold text-gray-900 leading-none">Fluid Hero Text</p>
        <p style="font-size: clamp(1.25rem, 2vw + 0.5rem, 2rem)" class="text-gray-600">Fluid Subtitle — ปรับตาม viewport</p>
        <p style="font-size: clamp(1rem, 1vw + 0.5rem, 1.25rem)" class="text-gray-500">Fluid body text ที่อ่านง่ายทุกขนาดหน้าจอ</p>
      </div>
    </section>

    <!-- Prose custom article -->
    <section>
      <h2 class="text-sm font-semibold text-indigo-600 uppercase tracking-widest mb-4">Custom Prose</h2>
      <article id="articleContent" class="prose prose-lg prose-custom max-w-none bg-white rounded-2xl p-8 shadow-sm">
        <h1>บทความตัวอย่างด้วย Custom Prose</h1>
        <p class="lead">Typography ที่ดีช่วยให้ผู้อ่านมีประสบการณ์ที่ดีขึ้น ทั้งในแง่การอ่าน ความเข้าใจ และความรู้สึก</p>
        <h2>หลักการ Typography ที่ดี</h2>
        <ul>
          <li><strong>Line Height</strong> ควรอยู่ที่ 1.5–1.8 สำหรับ body text</li>
          <li><strong>Line Length</strong> ควรอยู่ที่ 60–75 ตัวอักษรต่อบรรทัด</li>
          <li><strong>Contrast</strong> ต้องผ่าน WCAG AA (4.5:1 สำหรับ normal text)</li>
          <li><strong>Font Pairing</strong> — sans-serif สำหรับ UI, serif สำหรับ content</li>
        </ul>
        <h2>Code Example</h2>
        <pre><code>// Tailwind Typography custom config
typography: theme => ({
  custom: {
    css: {
      '--tw-prose-body': theme('colors.gray[700]'),
      h1: { fontFamily: 'Lora, serif' },
    }
  }
})</code></pre>
        <blockquote>
          "Good typography is invisible. When it's working, you don't notice it. You just read."
        </blockquote>
        <p>Typography ที่ดีไม่ใช่แค่เรื่องสวยงาม แต่ยังเกี่ยวกับ accessibility และ readability สำหรับผู้ใช้ทุกคน รวมถึงผู้ที่มีความบกพร่องทางสายตา</p>
      </article>
    </section>

    <!-- Font pairing examples -->
    <section>
      <h2 class="text-sm font-semibold text-indigo-600 uppercase tracking-widest mb-4">Font Pairing Examples</h2>
      <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
        <div class="bg-white rounded-2xl p-6 shadow-sm">
          <p class="text-xs text-gray-400 mb-3">Inter + Lora</p>
          <h3 class="font-serif text-2xl font-bold text-gray-900 mb-2">Brand Story</h3>
          <p class="font-sans text-sm text-gray-600 leading-relaxed">บริษัทของเราก่อตั้งขึ้นด้วยความเชื่อว่าเทคโนโลยีสามารถเปลี่ยนชีวิตคนได้</p>
        </div>
        <div class="bg-gray-900 rounded-2xl p-6">
          <p class="text-xs text-gray-500 mb-3">Inter Bold (Dark)</p>
          <h3 class="font-sans text-2xl font-extrabold text-white mb-2">Performance First</h3>
          <p class="font-sans text-sm text-gray-400 leading-relaxed">Built for speed, designed for scale. Experience the difference.</p>
        </div>
      </div>
    </section>

  </main>

  <script>
    function setMode(mode) {
      const article = document.getElementById('articleContent');
      if (mode === 'dark') {
        document.body.style.background = '#0f172a';
        document.body.style.color = '#f1f5f9';
        article.classList.remove('prose-custom');
        article.classList.add('prose-invert');
        article.style.background = '#1e293b';
      } else {
        document.body.style.background = '#f9fafb';
        document.body.style.color = '#111827';
        article.classList.add('prose-custom');
        article.classList.remove('prose-invert');
        article.style.background = '#ffffff';
      }
    }
  </script>
</body>
</html>
```

---

## สรุป Part 65

| Step | เนื้อหา |
|------|---------|
| 641 | Typography scale — display/body sizes, font families |
| 642 | Fluid typography ด้วย clamp() + CSS custom properties |
| 643 | @tailwindcss/typography plugin — prose variants + colors |
| 644 | Custom prose config — typography theme extension |
| 645 | Variable fonts — font-variation-settings + hover animation |
| 646 | Optical sizing ด้วย Inter opsz axis |
| 647 | Font pairing: sans + serif + mono |
| 648 | Line length + line height best practices |
| 649 | Dark mode prose ด้วย prose-invert |
| 650 | Workshop: Typography showcase — fluid + variable + prose + dark mode |

**Part ถัดไป:** Part 66 — Advanced Layout Patterns (Steps 651–660)
