# Part 04: Colors — สีและ Color System
## Steps 31–40: ระบบสีที่ครบครันของ Tailwind CSS

---

## 🎯 เป้าหมายของ Part นี้

- เข้าใจ Color System ของ Tailwind
- ใช้งาน Background, Text, Border, Shadow สี
- Color Opacity Modifier
- Custom Colors
- Gradient ขั้นต้น
- Dark Mode Colors

---

## Step 31: Color System Overview

Tailwind ใช้ระบบ **Numeric Color Scale** จาก 50 ถึง 950

```
50   — สีอ่อนมากๆ (แทบขาว)
100  — อ่อนมาก
200  — อ่อน
300  — ค่อนข้างอ่อน
400  — กลางอ่อน
500  — กลาง (มักใช้เป็น "ค่า base")
600  — กลางเข้ม
700  — ค่อนข้างเข้ม
800  — เข้ม
900  — เข้มมาก
950  — เข้มมากๆ (แทบดำ)
```

### สีทั้งหมดใน Tailwind v3

```html
<!-- Slate (เทาเย็น) -->
<div class="bg-slate-50">slate-50</div>
<div class="bg-slate-500">slate-500</div>
<div class="bg-slate-900">slate-900</div>

<!-- Gray -->
<div class="bg-gray-50">gray-50</div>
<div class="bg-gray-500">gray-500</div>
<div class="bg-gray-900">gray-900</div>

<!-- Zinc (เทาเย็นกว่า gray) -->
<div class="bg-zinc-500">zinc-500</div>

<!-- Neutral (เทากลางๆ) -->
<div class="bg-neutral-500">neutral-500</div>

<!-- Stone (เทาอุ่น) -->
<div class="bg-stone-500">stone-500</div>

<!-- Red -->
<div class="bg-red-500">red-500</div>

<!-- Orange -->
<div class="bg-orange-500">orange-500</div>

<!-- Amber -->
<div class="bg-amber-500">amber-500</div>

<!-- Yellow -->
<div class="bg-yellow-500">yellow-500</div>

<!-- Lime -->
<div class="bg-lime-500">lime-500</div>

<!-- Green -->
<div class="bg-green-500">green-500</div>

<!-- Emerald -->
<div class="bg-emerald-500">emerald-500</div>

<!-- Teal -->
<div class="bg-teal-500">teal-500</div>

<!-- Cyan -->
<div class="bg-cyan-500">cyan-500</div>

<!-- Sky -->
<div class="bg-sky-500">sky-500</div>

<!-- Blue -->
<div class="bg-blue-500">blue-500</div>

<!-- Indigo -->
<div class="bg-indigo-500">indigo-500</div>

<!-- Violet -->
<div class="bg-violet-500">violet-500</div>

<!-- Purple -->
<div class="bg-purple-500">purple-500</div>

<!-- Fuchsia -->
<div class="bg-fuchsia-500">fuchsia-500</div>

<!-- Pink -->
<div class="bg-pink-500">pink-500</div>

<!-- Rose -->
<div class="bg-rose-500">rose-500</div>
```

---

## Step 32: Background Color

```html
<!-- Background Colors -->
<div class="bg-white">ขาว</div>
<div class="bg-black">ดำ</div>
<div class="bg-transparent">โปร่งใส</div>
<div class="bg-current">สีปัจจุบัน</div>
<div class="bg-inherit">สืบทอดจาก parent</div>

<!-- Color Scale -->
<div class="bg-blue-50">blue-50 (อ่อนมาก)</div>
<div class="bg-blue-100">blue-100</div>
<div class="bg-blue-200">blue-200</div>
<div class="bg-blue-300">blue-300</div>
<div class="bg-blue-400">blue-400</div>
<div class="bg-blue-500">blue-500 (กลาง)</div>
<div class="bg-blue-600">blue-600</div>
<div class="bg-blue-700">blue-700</div>
<div class="bg-blue-800">blue-800</div>
<div class="bg-blue-900">blue-900 (เข้ม)</div>
<div class="bg-blue-950">blue-950 (เข้มสุด)</div>
```

### Background Opacity

```html
<!-- Opacity Modifier — เพิ่ม /xx หลังชื่อสี -->
<div class="bg-blue-500/100">100% opacity</div>
<div class="bg-blue-500/75">75% opacity</div>
<div class="bg-blue-500/50">50% opacity</div>
<div class="bg-blue-500/25">25% opacity</div>
<div class="bg-blue-500/10">10% opacity</div>
<div class="bg-blue-500/5">5% opacity</div>
<div class="bg-blue-500/0">0% opacity (transparent)</div>

<!-- Arbitrary opacity -->
<div class="bg-blue-500/[.15]">15% opacity</div>
<div class="bg-blue-500/[.33]">33% opacity</div>
```

---

## Step 33: Text Color และ Border Color

```html
<!-- Text Color -->
<p class="text-blue-600">text สีน้ำเงิน</p>
<p class="text-red-500/80">text แดง 80% opacity</p>
<p class="text-gray-700">text เทาเข้ม</p>

<!-- Border Color -->
<div class="border border-blue-500">border น้ำเงิน</div>
<div class="border-2 border-red-400">border แดง 2px</div>
<div class="border border-gray-200">border เทาอ่อน</div>

<!-- Divide Color (เส้นระหว่าง children) -->
<div class="divide-y divide-gray-200">
  <div class="py-2">Item 1</div>
  <div class="py-2">Item 2</div>
  <div class="py-2">Item 3</div>
</div>

<!-- Outline Color -->
<button class="outline outline-2 outline-blue-500">outline button</button>

<!-- Ring Color -->
<button class="ring-2 ring-purple-500">ring button</button>
<button class="focus:ring-2 focus:ring-blue-500 focus:ring-offset-2">focus ring</button>
```

---

## Step 34: Color Combinations ที่ดูดี

### Palette ที่แนะนำ

```html
<!-- Primary Blue Palette -->
<button class="bg-blue-600 hover:bg-blue-700 text-white">Blue Primary</button>

<!-- Subtle Gray Card -->
<div class="bg-gray-50 border border-gray-200 text-gray-700">Subtle Card</div>

<!-- Success Green -->
<div class="bg-green-50 border border-green-200 text-green-800">Success</div>

<!-- Warning Amber -->
<div class="bg-amber-50 border border-amber-200 text-amber-800">Warning</div>

<!-- Error Red -->
<div class="bg-red-50 border border-red-200 text-red-800">Error</div>

<!-- Info Blue -->
<div class="bg-blue-50 border border-blue-200 text-blue-800">Info</div>
```

### ตัวอย่าง Alert Components

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <title>Alert Colors</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 p-8 space-y-4">

  <!-- Success Alert -->
  <div class="flex items-start gap-3 bg-green-50 border border-green-200 text-green-800 rounded-xl p-4">
    <span class="text-green-500 text-xl">✅</span>
    <div>
      <p class="font-semibold">สำเร็จ!</p>
      <p class="text-sm text-green-700">บันทึกข้อมูลเรียบร้อยแล้ว</p>
    </div>
  </div>

  <!-- Warning Alert -->
  <div class="flex items-start gap-3 bg-amber-50 border border-amber-200 text-amber-800 rounded-xl p-4">
    <span class="text-amber-500 text-xl">⚠️</span>
    <div>
      <p class="font-semibold">คำเตือน!</p>
      <p class="text-sm text-amber-700">พื้นที่เก็บข้อมูลเหลือน้อย</p>
    </div>
  </div>

  <!-- Error Alert -->
  <div class="flex items-start gap-3 bg-red-50 border border-red-200 text-red-800 rounded-xl p-4">
    <span class="text-red-500 text-xl">❌</span>
    <div>
      <p class="font-semibold">เกิดข้อผิดพลาด!</p>
      <p class="text-sm text-red-700">ไม่สามารถเชื่อมต่อกับเซิร์ฟเวอร์ได้</p>
    </div>
  </div>

  <!-- Info Alert -->
  <div class="flex items-start gap-3 bg-blue-50 border border-blue-200 text-blue-800 rounded-xl p-4">
    <span class="text-blue-500 text-xl">ℹ️</span>
    <div>
      <p class="font-semibold">ข้อมูล</p>
      <p class="text-sm text-blue-700">มีการอัปเดตระบบในวันพรุ่งนี้</p>
    </div>
  </div>

</body>
</html>
```

---

## Step 35: Gradient Background

```html
<!-- Linear Gradient -->
<div class="bg-gradient-to-r from-blue-500 to-purple-600">ซ้ายไปขวา</div>
<div class="bg-gradient-to-l from-blue-500 to-purple-600">ขวาไปซ้าย</div>
<div class="bg-gradient-to-t from-blue-500 to-purple-600">ล่างขึ้นบน</div>
<div class="bg-gradient-to-b from-blue-500 to-purple-600">บนลงล่าง</div>
<div class="bg-gradient-to-br from-blue-500 to-purple-600">บนซ้ายไปล่างขวา</div>
<div class="bg-gradient-to-tr from-blue-500 to-purple-600">ล่างซ้ายไปบนขวา</div>

<!-- With via (3 colors) -->
<div class="bg-gradient-to-r from-red-500 via-yellow-500 to-green-500">
  3 สี
</div>

<!-- Gradient Opacity -->
<div class="bg-gradient-to-r from-blue-500 to-transparent">
  Fade to transparent
</div>
<div class="bg-gradient-to-b from-black/50 to-transparent">
  Dark overlay
</div>
```

### ตัวอย่าง Hero Section กับ Gradient

```html
<section class="min-h-screen bg-gradient-to-br from-indigo-900 via-purple-900 to-pink-900 flex items-center justify-center p-8">
  <div class="text-center">
    <h1 class="text-5xl font-black text-white mb-4 leading-tight">
      สร้างเว็บที่สวยงาม<br>
      <span class="bg-gradient-to-r from-yellow-300 to-orange-400 bg-clip-text text-transparent">
        ด้วย Tailwind CSS
      </span>
    </h1>
    <p class="text-purple-200 text-lg mb-8">
      Utility-First CSS Framework ที่ทรงพลังที่สุด
    </p>
    <button class="bg-white text-purple-900 font-bold px-8 py-3 rounded-xl hover:bg-purple-50 transition-colors">
      เริ่มต้นวันนี้
    </button>
  </div>
</section>
```

---

## Step 36: Custom Colors ใน Config

```javascript
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      colors: {
        // เพิ่ม brand color
        brand: {
          50: '#eff6ff',
          100: '#dbeafe',
          200: '#bfdbfe',
          300: '#93c5fd',
          400: '#60a5fa',
          500: '#3b82f6',    // main
          600: '#2563eb',
          700: '#1d4ed8',
          800: '#1e40af',
          900: '#1e3a8a',
          950: '#172554',
        },
        
        // Custom สีเดียว
        primary: '#6366f1',
        secondary: '#a855f7',
        
        // CSS Variable (สำหรับ Dark Mode)
        surface: 'var(--color-surface)',
        foreground: 'var(--color-foreground)',
      },
    },
  },
}
```

```html
<!-- ใช้ custom colors -->
<button class="bg-brand-500 hover:bg-brand-600 text-white">Brand Button</button>
<div class="bg-primary text-white">Primary Background</div>
```

---

## Step 37: Arbitrary Color Values

ใส่ค่าสีใดก็ได้โดยตรง

```html
<!-- Hex -->
<div class="bg-[#1a1a2e]">Custom hex color</div>
<p class="text-[#ff6b6b]">Custom text color</p>

<!-- RGB -->
<div class="bg-[rgb(26,26,46)]">RGB color</div>
<div class="bg-[rgba(26,26,46,0.5)]">RGBA color</div>

<!-- HSL -->
<div class="bg-[hsl(220,50%,20%)]">HSL color</div>

<!-- CSS Variable -->
<div class="bg-[var(--primary-color)]">CSS Variable</div>

<!-- Gradient with arbitrary -->
<div class="bg-gradient-to-r from-[#667eea] to-[#764ba2]">
  Custom gradient
</div>
```

---

## Step 38: Color Contrast และ Accessibility

การเลือกสีที่อ่านง่าย (WCAG AA: contrast ratio ≥ 4.5:1)

```html
<!-- ✅ ดี — contrast สูง -->
<div class="bg-white text-gray-900">White bg + Dark text</div>
<div class="bg-blue-600 text-white">Blue bg + White text</div>
<div class="bg-gray-900 text-white">Dark bg + White text</div>
<div class="bg-yellow-400 text-gray-900">Yellow bg + Dark text</div>

<!-- ❌ ไม่ดี — contrast ต่ำ -->
<div class="bg-yellow-200 text-yellow-400">ต่ำเกิน!</div>
<div class="bg-blue-100 text-blue-200">ต่ำเกิน!</div>
<div class="bg-gray-500 text-gray-400">ต่ำเกิน!</div>
```

### Color Combination ที่ปลอดภัย

```
✅ ปลอดภัย:
  - white / black
  - gray-50 / gray-900
  - blue-600 / white
  - blue-50 / blue-900
  - green-600 / white
  - red-600 / white

⚠️ ระวัง (ต้องตรวจ):
  - yellow-* / white
  - lime-* / white
  - cyan-* / white
```

---

## Step 39: Shadow กับ Color

```html
<!-- Drop Shadow -->
<div class="shadow-sm">เงาเล็กน้อย</div>
<div class="shadow">เงาปกติ</div>
<div class="shadow-md">เงากลาง</div>
<div class="shadow-lg">เงาใหญ่</div>
<div class="shadow-xl">เงาใหญ่มาก</div>
<div class="shadow-2xl">เงาใหญ่สุด</div>
<div class="shadow-none">ไม่มีเงา</div>

<!-- Colored Shadow (v3+) -->
<button class="bg-indigo-500 shadow-lg shadow-indigo-500/50 text-white px-6 py-2 rounded-xl">
  Colored Shadow
</button>

<button class="bg-red-500 shadow-lg shadow-red-500/40 text-white px-6 py-2 rounded-xl">
  Red Shadow
</button>

<button class="bg-green-500 shadow-lg shadow-green-500/40 text-white px-6 py-2 rounded-xl">
  Green Shadow
</button>
```

---

## Step 40: Workshop — Color Palette Showcase

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Color Showcase</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-8">

  <h1 class="text-3xl font-bold text-gray-900 mb-8">Color System</h1>
  
  <!-- Color Swatches -->
  <div class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6 mb-12">
    
    <!-- Blue Family -->
    <div class="rounded-xl overflow-hidden shadow-sm border border-gray-100">
      <div class="bg-blue-500 h-20"></div>
      <div class="p-3 bg-white">
        <p class="font-semibold text-gray-800 text-sm">Blue</p>
        <p class="text-gray-400 text-xs">#3b82f6</p>
      </div>
    </div>
    
    <!-- Indigo Family -->
    <div class="rounded-xl overflow-hidden shadow-sm border border-gray-100">
      <div class="bg-indigo-500 h-20"></div>
      <div class="p-3 bg-white">
        <p class="font-semibold text-gray-800 text-sm">Indigo</p>
        <p class="text-gray-400 text-xs">#6366f1</p>
      </div>
    </div>
    
    <!-- Purple Family -->
    <div class="rounded-xl overflow-hidden shadow-sm border border-gray-100">
      <div class="bg-purple-500 h-20"></div>
      <div class="p-3 bg-white">
        <p class="font-semibold text-gray-800 text-sm">Purple</p>
        <p class="text-gray-400 text-xs">#a855f7</p>
      </div>
    </div>
    
    <!-- Pink Family -->
    <div class="rounded-xl overflow-hidden shadow-sm border border-gray-100">
      <div class="bg-pink-500 h-20"></div>
      <div class="p-3 bg-white">
        <p class="font-semibold text-gray-800 text-sm">Pink</p>
        <p class="text-gray-400 text-xs">#ec4899</p>
      </div>
    </div>
    
    <!-- Green Family -->
    <div class="rounded-xl overflow-hidden shadow-sm border border-gray-100">
      <div class="bg-green-500 h-20"></div>
      <div class="p-3 bg-white">
        <p class="font-semibold text-gray-800 text-sm">Green</p>
        <p class="text-gray-400 text-xs">#22c55e</p>
      </div>
    </div>
    
    <!-- Amber Family -->
    <div class="rounded-xl overflow-hidden shadow-sm border border-gray-100">
      <div class="bg-amber-500 h-20"></div>
      <div class="p-3 bg-white">
        <p class="font-semibold text-gray-800 text-sm">Amber</p>
        <p class="text-gray-400 text-xs">#f59e0b</p>
      </div>
    </div>
    
    <!-- Red Family -->
    <div class="rounded-xl overflow-hidden shadow-sm border border-gray-100">
      <div class="bg-red-500 h-20"></div>
      <div class="p-3 bg-white">
        <p class="font-semibold text-gray-800 text-sm">Red</p>
        <p class="text-gray-400 text-xs">#ef4444</p>
      </div>
    </div>
    
    <!-- Gray Family -->
    <div class="rounded-xl overflow-hidden shadow-sm border border-gray-100">
      <div class="bg-gray-500 h-20"></div>
      <div class="p-3 bg-white">
        <p class="font-semibold text-gray-800 text-sm">Gray</p>
        <p class="text-gray-400 text-xs">#6b7280</p>
      </div>
    </div>
    
  </div>

  <!-- Gradient Showcase -->
  <h2 class="text-2xl font-bold text-gray-900 mb-4">Gradients</h2>
  <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-12">
    
    <div class="h-20 rounded-xl bg-gradient-to-r from-blue-600 to-indigo-600 flex items-center px-4">
      <span class="text-white font-semibold">from-blue-600 to-indigo-600</span>
    </div>
    
    <div class="h-20 rounded-xl bg-gradient-to-r from-purple-500 to-pink-500 flex items-center px-4">
      <span class="text-white font-semibold">from-purple-500 to-pink-500</span>
    </div>
    
    <div class="h-20 rounded-xl bg-gradient-to-r from-amber-400 to-orange-500 flex items-center px-4">
      <span class="text-white font-semibold">from-amber-400 to-orange-500</span>
    </div>
    
    <div class="h-20 rounded-xl bg-gradient-to-r from-emerald-400 to-cyan-400 flex items-center px-4">
      <span class="text-white font-semibold">from-emerald-400 to-cyan-400</span>
    </div>
    
    <div class="h-20 rounded-xl bg-gradient-to-br from-indigo-500 via-purple-500 to-pink-500 flex items-center px-4">
      <span class="text-white font-semibold">from-indigo-500 via-purple-500 to-pink-500</span>
    </div>
    
    <div class="h-20 rounded-xl bg-gradient-to-r from-slate-900 to-slate-700 flex items-center px-4">
      <span class="text-white font-semibold">Dark gradient</span>
    </div>
    
  </div>

  <!-- Button Colors -->
  <h2 class="text-2xl font-bold text-gray-900 mb-4">Buttons</h2>
  <div class="flex flex-wrap gap-3">
    <button class="bg-blue-500 hover:bg-blue-600 text-white px-5 py-2.5 rounded-xl font-medium shadow-md shadow-blue-500/25 transition-all hover:shadow-blue-500/40 hover:-translate-y-0.5">Blue</button>
    <button class="bg-indigo-500 hover:bg-indigo-600 text-white px-5 py-2.5 rounded-xl font-medium shadow-md shadow-indigo-500/25 transition-all hover:shadow-indigo-500/40 hover:-translate-y-0.5">Indigo</button>
    <button class="bg-purple-500 hover:bg-purple-600 text-white px-5 py-2.5 rounded-xl font-medium shadow-md shadow-purple-500/25 transition-all hover:shadow-purple-500/40 hover:-translate-y-0.5">Purple</button>
    <button class="bg-pink-500 hover:bg-pink-600 text-white px-5 py-2.5 rounded-xl font-medium shadow-md shadow-pink-500/25 transition-all hover:shadow-pink-500/40 hover:-translate-y-0.5">Pink</button>
    <button class="bg-green-500 hover:bg-green-600 text-white px-5 py-2.5 rounded-xl font-medium shadow-md shadow-green-500/25 transition-all hover:shadow-green-500/40 hover:-translate-y-0.5">Green</button>
    <button class="bg-amber-500 hover:bg-amber-600 text-white px-5 py-2.5 rounded-xl font-medium shadow-md shadow-amber-500/25 transition-all hover:shadow-amber-500/40 hover:-translate-y-0.5">Amber</button>
    <button class="bg-red-500 hover:bg-red-600 text-white px-5 py-2.5 rounded-xl font-medium shadow-md shadow-red-500/25 transition-all hover:shadow-red-500/40 hover:-translate-y-0.5">Red</button>
    <button class="bg-gray-800 hover:bg-gray-900 text-white px-5 py-2.5 rounded-xl font-medium shadow-md shadow-gray-800/25 transition-all hover:shadow-gray-800/40 hover:-translate-y-0.5">Dark</button>
  </div>

</body>
</html>
```

---

## 📝 สรุป Part 04

| Utility | การใช้งาน |
|---------|-----------|
| `bg-{color}-{shade}` | Background color |
| `text-{color}-{shade}` | Text color |
| `border-{color}-{shade}` | Border color |
| `shadow-{color}-{shade}/{opacity}` | Colored shadow |
| `bg-gradient-to-{dir}` | Gradient direction |
| `from-{color}` `via-{color}` `to-{color}` | Gradient stops |
| `{color}/opacity` | Color with opacity |
| `bg-[#hex]` | Arbitrary color |

---

## 🏋️ Exercises

### Exercise 1
สร้าง Status Badge 4 แบบ: Active (เขียว), Pending (เหลือง), Inactive (แดง), Unknown (เทา)

### Exercise 2
สร้าง Pricing Card 3 ระดับ ด้วย color scheme ที่แตกต่าง

### Exercise 3
สร้าง Gradient Hero Banner ที่สวยงามพร้อม text gradient title

---

*Part 04 — จาก 100 Parts | Steps 31–40 จาก 1,000 Steps*
