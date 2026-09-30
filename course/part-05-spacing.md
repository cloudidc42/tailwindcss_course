# Part 05: Spacing — Padding, Margin, Gap
## Steps 41–50: ควบคุม Space ทุกมิติ

---

## 🎯 เป้าหมายของ Part นี้

- Padding ทุกทิศทาง
- Margin ทุกทิศทาง
- Space Between (สำหรับ flex/grid)
- Gap ใน Grid และ Flex
- Negative Values
- Spacing Scale

---

## Step 41: Spacing Scale ของ Tailwind

Tailwind ใช้ **8px base unit** โดยค่าตัวเลขคูณ 4px:

```
0   → 0px
px  → 1px
0.5 → 2px
1   → 4px
1.5 → 6px
2   → 8px
2.5 → 10px
3   → 12px
3.5 → 14px
4   → 16px
5   → 20px
6   → 24px
7   → 28px
8   → 32px
9   → 36px
10  → 40px
11  → 44px
12  → 48px
14  → 56px
16  → 64px
20  → 80px
24  → 96px
28  → 112px
32  → 128px
36  → 144px
40  → 160px
44  → 176px
48  → 192px
52  → 208px
56  → 224px
60  → 240px
64  → 256px
72  → 288px
80  → 320px
96  → 384px
```

---

## Step 42: Padding

```html
<!-- All sides -->
<div class="p-0">0px</div>
<div class="p-1">4px</div>
<div class="p-2">8px</div>
<div class="p-3">12px</div>
<div class="p-4">16px</div>
<div class="p-5">20px</div>
<div class="p-6">24px</div>
<div class="p-8">32px</div>
<div class="p-10">40px</div>
<div class="p-12">48px</div>

<!-- Horizontal (x = left + right) -->
<div class="px-2">padding-left: 8px; padding-right: 8px;</div>
<div class="px-4">left: 16px; right: 16px</div>
<div class="px-6">left: 24px; right: 24px</div>
<div class="px-8">left: 32px; right: 32px</div>

<!-- Vertical (y = top + bottom) -->
<div class="py-2">padding-top: 8px; padding-bottom: 8px;</div>
<div class="py-4">top: 16px; bottom: 16px</div>

<!-- Individual sides -->
<div class="pt-4">padding-top: 16px</div>
<div class="pr-4">padding-right: 16px</div>
<div class="pb-4">padding-bottom: 16px</div>
<div class="pl-4">padding-left: 16px</div>

<!-- Logical properties (LTR/RTL) -->
<div class="ps-4">padding-inline-start: 16px</div>
<div class="pe-4">padding-inline-end: 16px</div>
```

### ตัวอย่างการใช้ Padding

```html
<!-- Card Padding -->
<div class="bg-white rounded-xl shadow p-6">
  เนื้อหา card ที่มี padding 24px รอบด้าน
</div>

<!-- Button Padding -->
<button class="bg-blue-500 text-white px-6 py-3 rounded-lg">
  Button กว้างแนวนอน บางแนวตั้ง
</button>

<!-- Tight Component -->
<span class="bg-gray-100 text-gray-700 text-xs px-2 py-0.5 rounded">
  Badge เล็กๆ
</span>

<!-- Section Padding -->
<section class="py-16 px-4 md:px-8 lg:px-16">
  Section padding responsive
</section>
```

---

## Step 43: Margin

```html
<!-- All sides -->
<div class="m-0">0px</div>
<div class="m-4">16px ทุกด้าน</div>
<div class="m-auto">auto (จัดกลาง)</div>

<!-- Horizontal / Vertical -->
<div class="mx-4">left: 16px, right: 16px</div>
<div class="my-4">top: 16px, bottom: 16px</div>
<div class="mx-auto">จัดกลางแนวนอน</div>

<!-- Individual -->
<div class="mt-4">margin-top: 16px</div>
<div class="mr-4">margin-right: 16px</div>
<div class="mb-4">margin-bottom: 16px</div>
<div class="ml-4">margin-left: 16px</div>

<!-- Negative margin -->
<div class="mt-4">ปกติ</div>
<div class="-mt-4">บนขึ้นไป 16px (negative!)</div>
<div class="-mx-4">ขยายออกซ้ายขวา 16px</div>
```

### Negative Margin ใช้งานจริง

```html
<!-- Card ที่มี image เกินขอบ -->
<div class="bg-white rounded-xl shadow overflow-visible">
  <!-- รูปยื่นออกมาจาก card -->
  <img class="-mt-8 mx-auto w-16 h-16 rounded-full border-4 border-white" 
       src="avatar.jpg" alt="">
  <div class="p-4 pt-2 text-center">
    <h3 class="font-bold">Profile Name</h3>
  </div>
</div>

<!-- Pull content into overlap -->
<section class="bg-gray-900 py-16">
  <div class="bg-white rounded-xl -mb-8 mx-4 md:mx-8 p-6 shadow-xl">
    CTA section ที่ overlap ส่วนถัดไป
  </div>
</section>
<section class="bg-gray-100 pt-16">
  เนื้อหาถัดมา
</section>
```

---

## Step 44: Space Between

`space-x-*` และ `space-y-*` เพิ่ม margin ระหว่าง children

```html
<!-- Space X (horizontal) -->
<div class="flex space-x-4">
  <div>Item 1</div>
  <div>Item 2</div>
  <div>Item 3</div>
</div>
<!-- เหมือนกับ margin-left ให้ทุก element ยกเว้นตัวแรก -->

<!-- Space Y (vertical) -->
<div class="flex flex-col space-y-4">
  <div>Row 1</div>
  <div>Row 2</div>
  <div>Row 3</div>
</div>

<!-- Reverse (สำหรับ flex-row-reverse) -->
<div class="flex flex-row-reverse space-x-4 space-x-reverse">
  <div>Item 1</div>
  <div>Item 2</div>
</div>
```

### space-y ในรูปแบบต่างๆ

```html
<!-- Navigation items -->
<nav class="flex space-x-6">
  <a href="#">หน้าแรก</a>
  <a href="#">เกี่ยวกับ</a>
  <a href="#">บริการ</a>
  <a href="#">ติดต่อ</a>
</nav>

<!-- Form fields -->
<form class="flex flex-col space-y-4">
  <input class="border rounded px-4 py-2" placeholder="ชื่อ">
  <input class="border rounded px-4 py-2" placeholder="อีเมล">
  <input class="border rounded px-4 py-2" placeholder="ข้อความ">
  <button class="bg-blue-500 text-white py-2 rounded">ส่ง</button>
</form>

<!-- Card list -->
<div class="flex flex-col space-y-3">
  <div class="bg-white rounded-lg p-4 shadow-sm">Card 1</div>
  <div class="bg-white rounded-lg p-4 shadow-sm">Card 2</div>
  <div class="bg-white rounded-lg p-4 shadow-sm">Card 3</div>
</div>
```

---

## Step 45: Gap (ใน Grid และ Flex)

`gap` ดีกว่า `space-*` เพราะทำงานทั้ง row และ column

```html
<!-- Gap ใน Grid -->
<div class="grid grid-cols-3 gap-4">
  <div>1</div><div>2</div><div>3</div>
  <div>4</div><div>5</div><div>6</div>
</div>

<!-- Gap X และ Y แยก -->
<div class="grid grid-cols-3 gap-x-6 gap-y-4">
  ช่องว่างแนวนอน 24px, แนวตั้ง 16px
</div>

<!-- Gap ใน Flex -->
<div class="flex flex-wrap gap-3">
  <span class="badge">Tag 1</span>
  <span class="badge">Tag 2</span>
  <span class="badge">Tag 3</span>
  <span class="badge">Tag 4</span>
</div>
```

### เปรียบเทียบ gap vs space-*

```html
<!-- space-x: ใช้ margin เฉพาะ flex row -->
<div class="flex space-x-4">
  <div>1</div>
  <div>2</div>
</div>

<!-- gap: ใช้ได้ทั้ง flex และ grid, ทำงานทุกทิศทาง -->
<div class="flex gap-4">
  <div>1</div>
  <div>2</div>
</div>

<!-- สำหรับ flex wrap → gap ดีกว่า space-x มาก -->
<div class="flex flex-wrap gap-3">
  <!-- gap ทำงานถูกต้องเมื่อ wrap -->
</div>
```

> 💡 **แนะนำ**: ใช้ `gap` แทน `space-*` เสมอ ยกเว้นมีเหตุผลพิเศษ

---

## Step 46: Arbitrary Spacing

```html
<!-- ใส่ค่าใดก็ได้ -->
<div class="p-[13px]">13px padding</div>
<div class="mt-[10vh]">10vh margin top</div>
<div class="gap-[17px]">17px gap</div>
<div class="p-[2.5rem]">2.5rem padding</div>

<!-- CSS Variable -->
<div class="p-[var(--spacing)]">CSS variable spacing</div>
```

---

## Step 47: Container กับ Padding

```html
<!-- Max Width Container ที่ centered -->
<div class="container mx-auto px-4">
  เนื้อหาที่ไม่กว้างเกินไปและจัดกลาง
</div>

<!-- Max Width Breakpoints ของ container -->
<!-- sm:  640px  md: 768px  lg: 1024px  xl: 1280px  2xl: 1536px -->

<!-- Custom max-width -->
<div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
  Bootstrap-style container responsive padding
</div>

<div class="max-w-5xl mx-auto px-4">Large container</div>
<div class="max-w-4xl mx-auto px-4">Medium container</div>
<div class="max-w-3xl mx-auto px-4">Small container (article)</div>
<div class="max-w-2xl mx-auto px-4">Narrow (form)</div>
<div class="max-w-lg mx-auto px-4">Very narrow (modal)</div>
<div class="max-w-sm mx-auto px-4">Card width</div>
```

---

## Step 48: Responsive Spacing

```html
<!-- Padding เปลี่ยนตาม screen size -->
<section class="py-8 md:py-12 lg:py-16 xl:py-20 px-4 md:px-6 lg:px-8">
  Responsive section padding
</section>

<!-- Gap เปลี่ยนตาม screen size -->
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 md:gap-6 lg:gap-8">
  Responsive grid gap
</div>

<!-- Margin responsive -->
<h2 class="mb-4 md:mb-6 lg:mb-8">Responsive margin bottom</h2>
```

---

## Step 49: Spacing Patterns ที่พบบ่อย

### 1. Section Spacing

```html
<!-- Standard section spacing -->
<section class="py-16 lg:py-24">
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
    Section content
  </div>
</section>
```

### 2. Card Content Spacing

```html
<!-- Card padding standard -->
<div class="bg-white rounded-xl shadow-md p-6 md:p-8">
  Card content
</div>

<!-- Card with header/body/footer -->
<div class="bg-white rounded-xl shadow overflow-hidden">
  <div class="px-6 py-4 border-b border-gray-100">
    Header
  </div>
  <div class="p-6">
    Body
  </div>
  <div class="px-6 py-4 bg-gray-50 border-t border-gray-100">
    Footer
  </div>
</div>
```

### 3. Form Spacing

```html
<form class="space-y-6">
  <div class="space-y-2">
    <label class="block text-sm font-medium text-gray-700">ชื่อ</label>
    <input class="w-full border border-gray-300 rounded-lg px-4 py-2.5 focus:ring-2 focus:ring-blue-500 focus:border-transparent outline-none">
  </div>
  
  <div class="space-y-2">
    <label class="block text-sm font-medium text-gray-700">อีเมล</label>
    <input class="w-full border border-gray-300 rounded-lg px-4 py-2.5 focus:ring-2 focus:ring-blue-500 focus:border-transparent outline-none">
  </div>
  
  <button class="w-full bg-blue-600 text-white py-3 rounded-lg font-medium">
    ส่งข้อมูล
  </button>
</form>
```

### 4. List Spacing

```html
<!-- Vertical list -->
<ul class="space-y-3">
  <li class="flex items-center gap-3 p-4 bg-white rounded-lg shadow-sm">
    <span class="w-2 h-2 bg-blue-500 rounded-full"></span>
    <span>รายการที่ 1</span>
  </li>
  <li class="flex items-center gap-3 p-4 bg-white rounded-lg shadow-sm">
    <span class="w-2 h-2 bg-blue-500 rounded-full"></span>
    <span>รายการที่ 2</span>
  </li>
</ul>
```

---

## Step 50: Workshop — Page Layout with Perfect Spacing

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Spacing Workshop</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50">

  <!-- Header -->
  <header class="bg-white border-b border-gray-200 sticky top-0 z-50">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="flex items-center justify-between h-16">
        <div class="font-bold text-xl text-indigo-600">Logo</div>
        <nav class="hidden md:flex items-center gap-6">
          <a href="#" class="text-sm text-gray-600 hover:text-gray-900">หน้าแรก</a>
          <a href="#" class="text-sm text-gray-600 hover:text-gray-900">สินค้า</a>
          <a href="#" class="text-sm text-gray-600 hover:text-gray-900">เกี่ยวกับ</a>
        </nav>
        <button class="bg-indigo-600 text-white text-sm px-4 py-2 rounded-lg">
          เริ่มต้น
        </button>
      </div>
    </div>
  </header>

  <!-- Hero Section -->
  <section class="py-20 lg:py-28">
    <div class="max-w-4xl mx-auto px-4 text-center">
      <p class="text-indigo-500 font-semibold text-sm uppercase tracking-widest mb-3">
        New Release
      </p>
      <h1 class="text-4xl lg:text-6xl font-black text-gray-900 mb-6 leading-tight">
        สร้างเว็บสวยๆ<br>ได้เลยวันนี้
      </h1>
      <p class="text-xl text-gray-500 mb-10 max-w-2xl mx-auto leading-relaxed">
        หลักสูตร Tailwind CSS ที่ครบครันที่สุด สอนตั้งแต่ศูนย์ถึงระดับมืออาชีพ
      </p>
      <div class="flex flex-col sm:flex-row gap-4 justify-center">
        <button class="bg-indigo-600 hover:bg-indigo-700 text-white font-semibold px-8 py-4 rounded-xl transition-colors">
          เริ่มเรียนฟรี →
        </button>
        <button class="border-2 border-gray-300 hover:border-indigo-300 text-gray-700 font-semibold px-8 py-4 rounded-xl transition-colors">
          ดูหลักสูตร
        </button>
      </div>
    </div>
  </section>

  <!-- Features Grid -->
  <section class="py-16 bg-white">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="text-center mb-12">
        <h2 class="text-3xl font-bold text-gray-900 mb-4">ทำไมต้องเลือกเรา</h2>
        <p class="text-gray-500 max-w-2xl mx-auto">เนื้อหาครบครัน อัปเดตล่าสุด ใช้งานได้จริง</p>
      </div>
      
      <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
        <div class="text-center p-6">
          <div class="text-4xl mb-4">🎯</div>
          <h3 class="font-bold text-lg text-gray-900 mb-2">100+ Part</h3>
          <p class="text-gray-500 text-sm leading-relaxed">เนื้อหาครบตั้งแต่ต้นจนจบ แบบ step-by-step</p>
        </div>
        <div class="text-center p-6">
          <div class="text-4xl mb-4">⚡</div>
          <h3 class="font-bold text-lg text-gray-900 mb-2">ใช้งานได้จริง</h3>
          <p class="text-gray-500 text-sm leading-relaxed">โค้ดทุกอย่างทดสอบแล้ว copy ไปใช้ได้เลย</p>
        </div>
        <div class="text-center p-6">
          <div class="text-4xl mb-4">🏆</div>
          <h3 class="font-bold text-lg text-gray-900 mb-2">ระดับโลก</h3>
          <p class="text-gray-500 text-sm leading-relaxed">เรียนรู้แนวคิดและ pattern ระดับ professional</p>
        </div>
      </div>
    </div>
  </section>

  <!-- CTA Section -->
  <section class="py-16 bg-indigo-600">
    <div class="max-w-3xl mx-auto px-4 text-center">
      <h2 class="text-3xl font-bold text-white mb-4">พร้อมเริ่มแล้วหรือยัง?</h2>
      <p class="text-indigo-200 mb-8">เริ่มต้นเรียน Tailwind CSS วันนี้ ฟรี!</p>
      <button class="bg-white text-indigo-600 font-bold px-8 py-3 rounded-xl hover:bg-indigo-50 transition-colors">
        เริ่มเรียนเลย →
      </button>
    </div>
  </section>

  <!-- Footer -->
  <footer class="bg-gray-900 py-8">
    <div class="max-w-7xl mx-auto px-4 text-center">
      <p class="text-gray-400 text-sm">© 2026 Tailwind Course. All rights reserved.</p>
    </div>
  </footer>

</body>
</html>
```

---

## 📝 สรุป Part 05

| Utility | คำอธิบาย |
|---------|---------|
| `p-{n}` `px-{n}` `py-{n}` `pt/r/b/l-{n}` | Padding |
| `m-{n}` `mx-{n}` `my-{n}` `mt/r/b/l-{n}` | Margin |
| `-m-{n}` `-mt-{n}` | Negative margin |
| `mx-auto` | Center horizontally |
| `space-x-{n}` `space-y-{n}` | Space between children |
| `gap-{n}` `gap-x-{n}` `gap-y-{n}` | Grid/Flex gap |
| `p-[value]` | Arbitrary spacing |

---

## 🏋️ Exercises

### Exercise 1
สร้าง Navbar ที่มี logo, navigation links, และ CTA button ด้วย spacing ที่ถูกต้อง

### Exercise 2
สร้าง Form ที่มี 4 fields (ชื่อ, นามสกุล, อีเมล, ข้อความ) ด้วย spacing สวยงาม

### Exercise 3
สร้าง Feature Grid 4 columns ที่มี icon, title, description ด้วย gap ที่เหมาะสม

---

*Part 05 — จาก 100 Parts | Steps 41–50 จาก 1,000 Steps*
