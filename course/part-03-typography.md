# Part 03: Typography — ตัวอักษรและข้อความ
## Steps 21–30: ควบคุมการแสดงผลข้อความทุกแง่มุม

---

## 🎯 เป้าหมายของ Part นี้

- Font Family, Size, Weight, Style
- Line Height, Letter Spacing
- Text Color, Alignment, Decoration
- Text Transform, Overflow, Whitespace
- List Styles
- สร้าง Typography ที่สวยงามสำหรับ Web

---

## Step 21: Font Family

```html
<!-- Font Families built-in -->
<p class="font-sans">
  Inter, ui-sans-serif, system-ui, ... (Default)
</p>

<p class="font-serif">
  ui-serif, Georgia, Cambria, "Times New Roman"...
</p>

<p class="font-mono">
  ui-monospace, SFMono-Regular, Menlo, Monaco...
</p>
```

### เพิ่ม Custom Font (Google Fonts)

```html
<!-- index.html — เพิ่มใน <head> -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@300;400;500;600;700&family=Prompt:wght@400;600;700&display=swap" rel="stylesheet">
```

```javascript
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      fontFamily: {
        'sarabun': ['Sarabun', 'sans-serif'],
        'prompt': ['Prompt', 'sans-serif'],
        'thai': ['Sarabun', 'Prompt', 'sans-serif'],
      },
    },
  },
}
```

```html
<!-- ใช้ Custom Font -->
<h1 class="font-prompt font-bold text-3xl">หัวข้อภาษาไทยด้วย Prompt</h1>
<p class="font-sarabun text-gray-700">เนื้อหาภาษาไทยด้วย Sarabun อ่านง่ายสบายตา</p>
```

---

## Step 22: Font Size

Tailwind มี scale ตายตัวที่คำนวณมาดีแล้ว

```html
<!-- Font Size Scale -->
<p class="text-xs">Extra Small — 0.75rem (12px)</p>
<p class="text-sm">Small — 0.875rem (14px)</p>
<p class="text-base">Base — 1rem (16px)</p>
<p class="text-lg">Large — 1.125rem (18px)</p>
<p class="text-xl">Extra Large — 1.25rem (20px)</p>
<p class="text-2xl">2X Large — 1.5rem (24px)</p>
<p class="text-3xl">3X Large — 1.875rem (30px)</p>
<p class="text-4xl">4X Large — 2.25rem (36px)</p>
<p class="text-5xl">5X Large — 3rem (48px)</p>
<p class="text-6xl">6X Large — 3.75rem (60px)</p>
<p class="text-7xl">7X Large — 4.5rem (72px)</p>
<p class="text-8xl">8X Large — 6rem (96px)</p>
<p class="text-9xl">9X Large — 8rem (128px)</p>
```

### Responsive Font Size

```html
<!-- ขนาดเล็กสำหรับมือถือ ใหญ่ขึ้นสำหรับจอใหญ่ -->
<h1 class="text-2xl sm:text-3xl md:text-4xl lg:text-5xl xl:text-6xl font-bold">
  Responsive Heading
</h1>
```

---

## Step 23: Font Weight

```html
<p class="font-thin">Thin — font-weight: 100</p>
<p class="font-extralight">Extra Light — font-weight: 200</p>
<p class="font-light">Light — font-weight: 300</p>
<p class="font-normal">Normal — font-weight: 400</p>
<p class="font-medium">Medium — font-weight: 500</p>
<p class="font-semibold">Semibold — font-weight: 600</p>
<p class="font-bold">Bold — font-weight: 700</p>
<p class="font-extrabold">Extra Bold — font-weight: 800</p>
<p class="font-black">Black — font-weight: 900</p>
```

### การใช้งานจริง

```html
<!-- Heading Hierarchy -->
<article class="space-y-4">
  <h1 class="text-4xl font-black text-gray-900">หัวข้อหลัก</h1>
  <h2 class="text-2xl font-bold text-gray-800">หัวข้อรอง</h2>
  <h3 class="text-xl font-semibold text-gray-700">หัวข้อย่อย</h3>
  <p class="text-base font-normal text-gray-600">เนื้อหาทั่วไป</p>
  <p class="text-sm font-light text-gray-500">คำอธิบายเพิ่มเติม</p>
</article>
```

---

## Step 24: Font Style และ Font Variant

```html
<!-- Font Style -->
<p class="italic">ตัวเอียง / Italic</p>
<p class="not-italic">ปกติ / Normal (ใช้ reset)</p>

<!-- Numeric Font Variant -->
<p class="ordinal">1st, 2nd, 3rd</p>
<p class="slashed-zero">0 — zero มีขีด</p>
<p class="lining-nums">123456789</p>
<p class="oldstyle-nums">123456789</p>
<p class="proportional-nums">1 111 1111</p>
<p class="tabular-nums">1 111 1111</p>

<!-- Combination -->
<table>
  <td class="tabular-nums text-right">1,234.56</td>
  <td class="tabular-nums text-right">98,765.43</td>
</table>
```

---

## Step 25: Line Height (Leading)

Line height ส่งผลมากต่อ readability

```html
<!-- Line Height Values -->
<p class="leading-none">leading-none — 1 (1x font-size)</p>
<p class="leading-tight">leading-tight — 1.25</p>
<p class="leading-snug">leading-snug — 1.375</p>
<p class="leading-normal">leading-normal — 1.5 (default)</p>
<p class="leading-relaxed">leading-relaxed — 1.625</p>
<p class="leading-loose">leading-loose — 2</p>

<!-- Fixed values -->
<p class="leading-3">leading-3 — 0.75rem</p>
<p class="leading-4">leading-4 — 1rem</p>
<p class="leading-5">leading-5 — 1.25rem</p>
<p class="leading-6">leading-6 — 1.5rem</p>
<p class="leading-7">leading-7 — 1.75rem</p>
<p class="leading-8">leading-8 — 2rem</p>
<p class="leading-9">leading-9 — 2.25rem</p>
<p class="leading-10">leading-10 — 2.5rem</p>
```

### ตัวอย่างการใช้งาน

```html
<!-- Heading ควรใช้ leading-tight -->
<h1 class="text-4xl font-bold leading-tight">
  หัวข้อขนาดใหญ่<br>สองบรรทัด
</h1>

<!-- Body text ควรใช้ leading-relaxed -->
<p class="text-base leading-relaxed text-gray-700">
  เนื้อหาบทความที่ต้องอ่านยาวๆ ควรใช้ leading-relaxed หรือ leading-loose
  เพื่อให้อ่านสบายตา ไม่แน่นเกินไป
</p>

<!-- Code ใช้ leading-normal -->
<code class="text-sm leading-normal font-mono">
  const x = 42;
</code>
```

---

## Step 26: Letter Spacing (Tracking)

```html
<p class="tracking-tighter">Tighter — -0.05em</p>
<p class="tracking-tight">Tight — -0.025em</p>
<p class="tracking-normal">Normal — 0em</p>
<p class="tracking-wide">Wide — 0.025em</p>
<p class="tracking-wider">Wider — 0.05em</p>
<p class="tracking-widest">Widest — 0.1em</p>
```

### การใช้งานจริง

```html
<!-- Heading มักใช้ tracking-tight -->
<h1 class="text-5xl font-black tracking-tight">Big Bold Title</h1>

<!-- ALL CAPS labels ใช้ tracking-widest -->
<span class="text-xs font-semibold uppercase tracking-widest text-gray-400">
  CATEGORY LABEL
</span>

<!-- Logo/Brand ใช้ tracking-wide -->
<div class="text-2xl font-bold tracking-wide text-indigo-600">BRAND</div>
```

---

## Step 27: Text Color

Tailwind มี color system ที่ครบครันมาก

```html
<!-- Gray Scale -->
<p class="text-gray-50">gray-50 (แทบขาว)</p>
<p class="text-gray-100">gray-100</p>
<p class="text-gray-200">gray-200</p>
<p class="text-gray-300">gray-300</p>
<p class="text-gray-400">gray-400</p>
<p class="text-gray-500">gray-500 (กลางๆ)</p>
<p class="text-gray-600">gray-600</p>
<p class="text-gray-700">gray-700</p>
<p class="text-gray-800">gray-800</p>
<p class="text-gray-900">gray-900 (เกือบดำ)</p>
<p class="text-gray-950">gray-950 (เข้มที่สุด)</p>

<!-- Colors ที่ใช้บ่อย -->
<p class="text-red-500">Red</p>
<p class="text-orange-500">Orange</p>
<p class="text-yellow-500">Yellow</p>
<p class="text-green-500">Green</p>
<p class="text-blue-500">Blue</p>
<p class="text-indigo-500">Indigo</p>
<p class="text-purple-500">Purple</p>
<p class="text-pink-500">Pink</p>

<!-- Special -->
<p class="text-white">White</p>
<p class="text-black">Black</p>
<p class="text-transparent">Transparent</p>
<p class="text-current">Inherits parent color</p>
<p class="text-inherit">Inherits all text props</p>
```

### Text Color กับ Opacity

```html
<!-- Opacity modifier (v3+) -->
<p class="text-blue-500/100">100% opacity</p>
<p class="text-blue-500/75">75% opacity</p>
<p class="text-blue-500/50">50% opacity</p>
<p class="text-blue-500/25">25% opacity</p>
<p class="text-blue-500/10">10% opacity</p>

<!-- Arbitrary opacity -->
<p class="text-blue-500/[.33]">33% opacity</p>
```

---

## Step 28: Text Alignment

```html
<p class="text-left">จัดชิดซ้าย (default)</p>
<p class="text-center">จัดกลาง</p>
<p class="text-right">จัดชิดขวา</p>
<p class="text-justify">justify — ขยายให้เต็มบรรทัด</p>
<p class="text-start">start (ซ้ายใน LTR, ขวาใน RTL)</p>
<p class="text-end">end (ขวาใน LTR, ซ้ายใน RTL)</p>
```

### การใช้งานจริง

```html
<!-- Responsive alignment -->
<h2 class="text-center md:text-left text-2xl font-bold">
  หัวข้อกลางจอเล็ก ชิดซ้ายจอใหญ่
</h2>

<!-- Card with different alignments -->
<div class="bg-white rounded-xl p-6">
  <div class="text-center mb-4">
    <img class="w-16 h-16 rounded-full mx-auto mb-2" src="..." alt="">
    <h3 class="font-bold">ชื่อ</h3>
  </div>
  <p class="text-justify text-gray-600 text-sm leading-relaxed">
    รายละเอียดที่ justify ดูเป็นระเบียบเรียบร้อย...
  </p>
  <div class="text-right mt-4">
    <a href="#" class="text-blue-500 text-sm">อ่านเพิ่มเติม →</a>
  </div>
</div>
```

---

## Step 29: Text Decoration

```html
<!-- Underline -->
<p class="underline">ขีดเส้นใต้</p>
<p class="overline">ขีดเส้นบน</p>
<p class="line-through">ขีดทับกลาง</p>
<p class="no-underline">ไม่มีเส้น (reset)</p>

<!-- Decoration Color -->
<p class="underline decoration-blue-500">เส้นสีน้ำเงิน</p>
<p class="underline decoration-red-500">เส้นสีแดง</p>

<!-- Decoration Style -->
<p class="underline decoration-solid">solid (default)</p>
<p class="underline decoration-double">double</p>
<p class="underline decoration-dotted">dotted</p>
<p class="underline decoration-dashed">dashed</p>
<p class="underline decoration-wavy">wavy</p>

<!-- Decoration Thickness -->
<p class="underline decoration-1">얇음 1px</p>
<p class="underline decoration-2">2px</p>
<p class="underline decoration-4">4px</p>
<p class="underline decoration-8">8px</p>

<!-- Decoration Offset -->
<p class="underline underline-offset-1">offset 1</p>
<p class="underline underline-offset-2">offset 2</p>
<p class="underline underline-offset-4">offset 4</p>
<p class="underline underline-offset-8">offset 8</p>
```

### ตัวอย่าง Custom Link Style

```html
<a href="#" class="
  text-blue-600 
  underline 
  decoration-blue-300 
  decoration-2
  underline-offset-4 
  hover:decoration-blue-600 
  transition-colors
">
  Link with custom underline
</a>
```

---

## Step 30: Text Transform และ Workshop

### Text Transform

```html
<p class="uppercase">uppercase — ตัวพิมพ์ใหญ่ทั้งหมด</p>
<p class="lowercase">LOWERCASE — ตัวพิมพ์เล็กทั้งหมด</p>
<p class="capitalize">capitalize — ตัวแรกของแต่ละคำใหญ่</p>
<p class="normal-case">Normal Case — ปกติ (reset)</p>
```

### Text Overflow

```html
<!-- Truncate: ตัดข้อความที่ยาวเกิน -->
<p class="truncate w-48">
  ข้อความยาวมากๆ ที่จะถูกตัดด้วย ... 
</p>

<!-- Text Overflow -->
<p class="overflow-hidden text-ellipsis w-48 whitespace-nowrap">
  เทียบเท่ากับ truncate
</p>

<!-- Multi-line clamp (ต้องใช้ line-clamp plugin) -->
<p class="line-clamp-2">
  ข้อความยาวมากๆ ที่จะแสดงแค่ 2 บรรทัดแล้วตัดด้วย ...
</p>
<p class="line-clamp-3">3 บรรทัด</p>
<p class="line-clamp-none">ไม่ตัด</p>
```

### Whitespace

```html
<p class="whitespace-normal">normal — wrap ตามปกติ</p>
<p class="whitespace-nowrap">nowrap — ไม่ขึ้นบรรทัดใหม่</p>
<p class="whitespace-pre">pre — รักษา whitespace</p>
<p class="whitespace-pre-line">pre-line</p>
<p class="whitespace-pre-wrap">pre-wrap</p>
<p class="whitespace-break-spaces">break-spaces</p>
```

### Word Break

```html
<p class="break-normal">break-normal — default</p>
<p class="break-all">break-all — ตัดคำทุกที่</p>
<p class="break-keep">break-keep — รักษาคำ</p>
<p class="break-words">break-words — ตัดเมื่อจำเป็น</p>
```

---

## 🏆 Workshop: สร้าง Article/Blog Page Typography

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Blog Article</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link href="https://fonts.googleapis.com/css2?family=Sarabun:ital,wght@0,300;0,400;0,600;0,700;1,400&display=swap" rel="stylesheet">
  <script>
    tailwind.config = {
      theme: {
        extend: {
          fontFamily: {
            sarabun: ['Sarabun', 'sans-serif'],
          },
        },
      },
    }
  </script>
</head>
<body class="bg-gray-50 font-sarabun">

  <div class="max-w-3xl mx-auto px-4 py-16">
    
    <!-- Article Header -->
    <header class="mb-10">
      <!-- Category Tag -->
      <span class="text-xs font-semibold uppercase tracking-widest text-indigo-500 mb-3 block">
        Frontend Development
      </span>
      
      <!-- Title -->
      <h1 class="text-4xl lg:text-5xl font-black text-gray-900 leading-tight mb-4">
        Tailwind CSS เปลี่ยนโลกการเขียน CSS ได้อย่างไร
      </h1>
      
      <!-- Subtitle -->
      <p class="text-xl text-gray-500 leading-relaxed mb-6">
        ทำความเข้าใจแนวคิด Utility-First และเหตุผลที่ Developer 
        ทั่วโลกหันมาใช้ Tailwind CSS มากขึ้นทุกวัน
      </p>
      
      <!-- Meta -->
      <div class="flex items-center gap-4 text-sm text-gray-400">
        <span>สมชาย ใจดี</span>
        <span>·</span>
        <time>30 กันยายน 2026</time>
        <span>·</span>
        <span>5 นาที</span>
      </div>
    </header>
    
    <!-- Featured Image -->
    <div class="rounded-2xl bg-gradient-to-br from-indigo-500 to-purple-600 h-64 mb-10 flex items-center justify-center">
      <span class="text-white text-5xl">🎨</span>
    </div>
    
    <!-- Article Content -->
    <article class="space-y-6">
      
      <p class="text-lg text-gray-700 leading-relaxed">
        ตั้งแต่ Tailwind CSS เปิดตัวในปี 2019 มันได้กลายเป็น 
        <strong class="font-semibold text-gray-900">หนึ่งในเครื่องมือที่ได้รับความนิยมสูงสุด</strong>
        ในวงการ Frontend Development ด้วยแนวคิดที่แตกต่างอย่างสิ้นเชิง
      </p>
      
      <h2 class="text-2xl font-bold text-gray-900 mt-8">ทำไม Utility-First?</h2>
      
      <p class="text-base text-gray-600 leading-relaxed">
        แนวคิด Utility-First หมายถึงการมี <em>class เล็กๆ</em> ที่แต่ละ class
        ทำหน้าที่เพียงอย่างเดียว แทนที่จะมี component class ที่ทำหลายอย่าง
      </p>
      
      <!-- Quote Block -->
      <blockquote class="border-l-4 border-indigo-500 pl-6 py-2 bg-indigo-50 rounded-r-lg">
        <p class="text-gray-700 italic text-lg leading-relaxed">
          "If you can suppress the urge to try it, I think you'll find 
          that working this way is actually a really pleasant experience."
        </p>
        <cite class="text-sm text-gray-400 mt-2 block not-italic">
          — Adam Wathan, ผู้สร้าง Tailwind CSS
        </cite>
      </blockquote>
      
      <h2 class="text-2xl font-bold text-gray-900 mt-8">ข้อดีที่ได้รับ</h2>
      
      <!-- List -->
      <ul class="space-y-2 text-gray-600">
        <li class="flex items-start gap-2">
          <span class="text-green-500 font-bold mt-0.5">✓</span>
          <span>ไม่ต้องตั้งชื่อ CSS class อีกต่อไป</span>
        </li>
        <li class="flex items-start gap-2">
          <span class="text-green-500 font-bold mt-0.5">✓</span>
          <span>CSS ไม่โตขึ้นตาม codebase</span>
        </li>
        <li class="flex items-start gap-2">
          <span class="text-green-500 font-bold mt-0.5">✓</span>
          <span>Design ที่สม่ำเสมอโดยอัตโนมัติ</span>
        </li>
        <li class="flex items-start gap-2">
          <span class="text-green-500 font-bold mt-0.5">✓</span>
          <span>Responsive design ง่ายขึ้นมาก</span>
        </li>
      </ul>
      
      <!-- Code Block Styling -->
      <div class="bg-gray-900 rounded-xl p-6 overflow-x-auto">
        <p class="text-gray-400 text-xs mb-3 uppercase tracking-widest">ตัวอย่างโค้ด</p>
        <pre class="text-green-400 text-sm font-mono leading-relaxed"><code>&lt;button class="bg-blue-500 hover:bg-blue-700 
  text-white font-bold py-2 px-4 rounded"&gt;
  Click Me
&lt;/button&gt;</code></pre>
      </div>
      
      <!-- Conclusion -->
      <p class="text-base text-gray-600 leading-relaxed">
        ในท้ายที่สุด Tailwind CSS ไม่ได้เหมาะกับทุกคน แต่สำหรับ Developer
        ที่ต้องการ <span class="bg-yellow-100 text-yellow-800 px-1 rounded">
        speed และ flexibility</span> พร้อมกัน มันเป็นตัวเลือกที่ยอดเยี่ยม
      </p>
      
    </article>
    
    <!-- Tags -->
    <footer class="mt-10 pt-6 border-t border-gray-200">
      <div class="flex flex-wrap gap-2">
        <span class="text-xs text-gray-500 mr-2">แท็ก:</span>
        <span class="text-xs bg-gray-100 text-gray-600 px-3 py-1 rounded-full">tailwind css</span>
        <span class="text-xs bg-gray-100 text-gray-600 px-3 py-1 rounded-full">frontend</span>
        <span class="text-xs bg-gray-100 text-gray-600 px-3 py-1 rounded-full">css</span>
        <span class="text-xs bg-gray-100 text-gray-600 px-3 py-1 rounded-full">webdev</span>
      </div>
    </footer>
    
  </div>

</body>
</html>
```

---

## 📋 Reference: Typography Cheatsheet

```
Font Family:    font-sans, font-serif, font-mono
Font Size:      text-xs → text-9xl
Font Weight:    font-thin → font-black
Font Style:     italic, not-italic
Line Height:    leading-none → leading-loose
Letter Spacing: tracking-tighter → tracking-widest
Text Color:     text-{color}-{shade}
Text Align:     text-left, text-center, text-right, text-justify
Text Decoration: underline, overline, line-through, no-underline
Text Transform: uppercase, lowercase, capitalize, normal-case
Text Overflow:  truncate, line-clamp-{n}
Whitespace:     whitespace-normal, whitespace-nowrap, whitespace-pre
Word Break:     break-normal, break-all, break-words
```

---

## 📝 สรุป Part 03

เราเรียนรู้ Typography utilities ทั้งหมดของ Tailwind CSS ตั้งแต่ font family จนถึงการควบคุม overflow รวมถึงการสร้าง Blog article ที่มี typography สวยงาม

---

## 🏋️ Exercises

### Exercise 1
สร้าง Price Tag component ที่มี: ป้ายราคาเก่า (ขีดทับ), ราคาใหม่ (ใหญ่และแดง), badge "ลด 30%"

### Exercise 2
สร้าง Quote Card ที่มี: blockquote text ใหญ่, attribution (ชื่อและตำแหน่ง), design สวยงาม

### Exercise 3
สร้าง Hero Section ที่มี: gradient text, responsive font sizes, subtitle, และ CTA buttons

---

*Part 03 — จาก 100 Parts | Steps 21–30 จาก 1,000 Steps*
