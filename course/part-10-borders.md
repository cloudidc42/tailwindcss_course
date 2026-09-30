# Part 10: Border, Radius, Shadow
## Steps 91–100: ขอบและเงา

---

## 🎯 เป้าหมายของ Part นี้

- Border Width, Color, Style
- Border Radius (rounded)
- Box Shadow
- Outline
- Ring (focus indicator)
- Divide (border ระหว่าง children)

---

## Step 91: Border Width

```html
<!-- All sides -->
<div class="border">1px border (default)</div>
<div class="border-0">0px (ลบ border)</div>
<div class="border-2">2px border</div>
<div class="border-4">4px border</div>
<div class="border-8">8px border</div>

<!-- Sides -->
<div class="border-t">top</div>
<div class="border-r">right</div>
<div class="border-b">bottom</div>
<div class="border-l">left</div>
<div class="border-x">left + right</div>
<div class="border-y">top + bottom</div>

<!-- Individual + width -->
<div class="border-t-2">top 2px</div>
<div class="border-b-4">bottom 4px</div>
<div class="border-l-8">left 8px</div>

<!-- Logical -->
<div class="border-s">inline-start</div>
<div class="border-e">inline-end</div>
```

---

## Step 92: Border Color

```html
<div class="border border-gray-200">เทาอ่อน</div>
<div class="border border-gray-300">เทากลาง</div>
<div class="border border-blue-500">น้ำเงิน</div>
<div class="border border-red-500">แดง</div>
<div class="border border-green-500">เขียว</div>

<!-- Opacity -->
<div class="border border-blue-500/50">50% opacity</div>

<!-- Sides -->
<div class="border-t-2 border-blue-500">Top border blue</div>
<div class="border-b-2 border-red-500">Bottom border red</div>

<!-- Transparent -->
<div class="border border-transparent">transparent border</div>
```

---

## Step 93: Border Style

```html
<div class="border border-solid">solid (default)</div>
<div class="border border-dashed">dashed</div>
<div class="border border-dotted">dotted</div>
<div class="border border-double">double</div>
<div class="border border-hidden">hidden</div>
<div class="border border-none">none</div>
```

### ตัวอย่าง Decorative Border

```html
<!-- Dashed card border -->
<div class="border-2 border-dashed border-gray-300 rounded-xl p-6 text-center text-gray-500 hover:border-blue-400 hover:text-blue-500 cursor-pointer transition-colors">
  <span class="text-2xl">+</span>
  <p class="mt-2 text-sm">เพิ่มรายการ</p>
</div>

<!-- Gradient border (ใช้ pseudo-element concept) -->
<div class="relative p-[2px] rounded-xl bg-gradient-to-r from-indigo-500 to-purple-500">
  <div class="bg-white rounded-xl p-4">
    <p>Content inside gradient border</p>
  </div>
</div>

<!-- Accent border left -->
<div class="border-l-4 border-l-blue-500 pl-4 py-2 bg-blue-50">
  <p class="font-semibold text-blue-800">หมายเหตุ</p>
  <p class="text-blue-700 text-sm mt-1">เนื้อหาสำคัญ</p>
</div>
```

---

## Step 94: Border Radius

```html
<!-- Size scale -->
<div class="rounded-none">0px</div>
<div class="rounded-sm">2px</div>
<div class="rounded">4px</div>
<div class="rounded-md">6px</div>
<div class="rounded-lg">8px</div>
<div class="rounded-xl">12px</div>
<div class="rounded-2xl">16px</div>
<div class="rounded-3xl">24px</div>
<div class="rounded-full">9999px (วงกลม)</div>

<!-- Sides -->
<div class="rounded-t">top-left + top-right</div>
<div class="rounded-r">top-right + bottom-right</div>
<div class="rounded-b">bottom-left + bottom-right</div>
<div class="rounded-l">top-left + bottom-left</div>

<!-- Corners -->
<div class="rounded-tl-xl">top-left</div>
<div class="rounded-tr-xl">top-right</div>
<div class="rounded-bl-xl">bottom-left</div>
<div class="rounded-br-xl">bottom-right</div>

<!-- Logical -->
<div class="rounded-ss">start-start</div>
<div class="rounded-se">start-end</div>
<div class="rounded-es">end-start</div>
<div class="rounded-ee">end-end</div>
```

### ตัวอย่าง Rounded Patterns

```html
<!-- Avatar shapes -->
<img class="size-12 rounded-full">
<img class="size-12 rounded-2xl">
<img class="size-12 rounded-lg">

<!-- Tag/Badge shapes -->
<span class="bg-blue-100 text-blue-700 px-3 py-1 rounded-full text-sm">Full radius</span>
<span class="bg-blue-100 text-blue-700 px-3 py-1 rounded-lg text-sm">Rounded lg</span>
<span class="bg-blue-100 text-blue-700 px-3 py-1 rounded text-sm">Rounded sm</span>

<!-- Mixed corners -->
<div class="rounded-tl-2xl rounded-tr-2xl rounded-bl rounded-br bg-blue-500 p-4">
  มุมบนโค้ง มุมล่างเล็ก
</div>
```

---

## Step 95: Box Shadow

```html
<!-- Shadow scale -->
<div class="shadow-sm">เล็กน้อย — 0 1px 2px</div>
<div class="shadow">เล็ก — 0 1px 3px</div>
<div class="shadow-md">กลาง — 0 4px 6px</div>
<div class="shadow-lg">ใหญ่ — 0 10px 15px</div>
<div class="shadow-xl">ใหญ่มาก — 0 20px 25px</div>
<div class="shadow-2xl">ใหญ่สุด — 0 25px 50px</div>
<div class="shadow-inner">เงาข้างใน</div>
<div class="shadow-none">ไม่มีเงา</div>

<!-- Colored Shadows (v3) -->
<button class="bg-blue-500 shadow-lg shadow-blue-500/50 text-white px-6 py-3 rounded-xl">
  Blue Shadow
</button>
<button class="bg-violet-500 shadow-lg shadow-violet-500/50 text-white px-6 py-3 rounded-xl">
  Violet Shadow
</button>
<button class="bg-pink-500 shadow-lg shadow-pink-500/50 text-white px-6 py-3 rounded-xl">
  Pink Shadow
</button>
```

### Hover Shadow Effect

```html
<div class="bg-white rounded-xl p-6 shadow-sm hover:shadow-lg transition-shadow duration-300 cursor-pointer">
  Hover to see shadow grow
</div>

<!-- Card with interactive shadow -->
<div class="group bg-white rounded-2xl p-6 shadow-sm hover:shadow-xl hover:-translate-y-1 transition-all duration-300 cursor-pointer">
  <h3 class="font-bold text-gray-900">Card Title</h3>
  <p class="text-gray-500 text-sm mt-1">Hover me!</p>
</div>
```

---

## Step 96: Outline

```html
<!-- Outline width -->
<div class="outline">1px</div>
<div class="outline-0">0px (ลบ)</div>
<div class="outline-1">1px</div>
<div class="outline-2">2px</div>
<div class="outline-4">4px</div>
<div class="outline-8">8px</div>

<!-- Outline color -->
<div class="outline outline-blue-500">blue outline</div>
<div class="outline outline-red-500">red outline</div>

<!-- Outline style -->
<div class="outline outline-solid">solid</div>
<div class="outline outline-dashed">dashed</div>
<div class="outline outline-dotted">dotted</div>
<div class="outline outline-double">double</div>
<div class="outline outline-none">none (ลบ native outline)</div>

<!-- Outline offset -->
<div class="outline outline-2 outline-blue-500 outline-offset-0">offset 0</div>
<div class="outline outline-2 outline-blue-500 outline-offset-2">offset 2</div>
<div class="outline outline-2 outline-blue-500 outline-offset-4">offset 4</div>
<div class="outline outline-2 outline-blue-500 outline-offset-8">offset 8</div>
```

---

## Step 97: Ring (Focus Indicator)

Ring ใช้ทำ focus indicator สำหรับ Accessibility

```html
<!-- Ring width -->
<button class="ring-0">0</button>
<button class="ring-1">1px</button>
<button class="ring-2">2px</button>
<button class="ring-4">4px</button>
<button class="ring-8">8px</button>

<!-- Ring color -->
<button class="ring-2 ring-blue-500">blue ring</button>
<button class="ring-2 ring-red-500">red ring</button>
<button class="ring-2 ring-green-500">green ring</button>

<!-- Ring opacity -->
<button class="ring-2 ring-blue-500/50">50% opacity</button>

<!-- Ring offset -->
<button class="ring-2 ring-blue-500 ring-offset-2">offset 2</button>
<button class="ring-2 ring-blue-500 ring-offset-4">offset 4</button>
<button class="ring-2 ring-blue-500 ring-offset-8">offset 8</button>

<!-- Focus ring (standard pattern) -->
<button class="
  bg-blue-500 text-white px-4 py-2 rounded-lg
  focus:outline-none 
  focus:ring-2 
  focus:ring-blue-500 
  focus:ring-offset-2
  transition-all
">
  Accessible Button
</button>

<!-- Focus visible (keyboard only) -->
<button class="
  bg-blue-500 text-white px-4 py-2 rounded-lg
  focus-visible:outline-none 
  focus-visible:ring-2 
  focus-visible:ring-blue-500 
  focus-visible:ring-offset-2
">
  Keyboard accessible (no mouse ring)
</button>
```

---

## Step 98: Divide

Divide เพิ่ม border ระหว่าง children อัตโนมัติ

```html
<!-- Divide Y (เส้นระหว่างแถว) -->
<div class="divide-y divide-gray-200">
  <div class="py-3">Item 1</div>
  <div class="py-3">Item 2</div>
  <div class="py-3">Item 3</div>
</div>

<!-- Divide X (เส้นระหว่างคอลัมน์) -->
<div class="flex divide-x divide-gray-200">
  <div class="px-4">Column 1</div>
  <div class="px-4">Column 2</div>
  <div class="px-4">Column 3</div>
</div>

<!-- Divide width -->
<div class="divide-y divide-y-2 divide-gray-300">
  2px divider
</div>

<!-- Divide color -->
<div class="divide-y divide-blue-200">
  Blue divider
</div>

<!-- Divide style -->
<div class="divide-y divide-dashed">dashed</div>
<div class="divide-y divide-dotted">dotted</div>
```

### ตัวอย่าง List กับ Divide

```html
<div class="bg-white rounded-xl shadow overflow-hidden">
  <div class="divide-y divide-gray-100">
    
    <div class="flex items-center justify-between px-6 py-4 hover:bg-gray-50 transition-colors cursor-pointer">
      <div class="flex items-center gap-3">
        <div class="size-10 bg-blue-100 text-blue-600 rounded-xl flex items-center justify-center">📧</div>
        <div>
          <p class="font-medium text-gray-900 text-sm">อีเมล</p>
          <p class="text-gray-400 text-xs">example@email.com</p>
        </div>
      </div>
      <span class="text-gray-300 text-lg">›</span>
    </div>
    
    <div class="flex items-center justify-between px-6 py-4 hover:bg-gray-50 transition-colors cursor-pointer">
      <div class="flex items-center gap-3">
        <div class="size-10 bg-green-100 text-green-600 rounded-xl flex items-center justify-center">📱</div>
        <div>
          <p class="font-medium text-gray-900 text-sm">โทรศัพท์</p>
          <p class="text-gray-400 text-xs">+66 81 234 5678</p>
        </div>
      </div>
      <span class="text-gray-300 text-lg">›</span>
    </div>
    
    <div class="flex items-center justify-between px-6 py-4 hover:bg-gray-50 transition-colors cursor-pointer">
      <div class="flex items-center gap-3">
        <div class="size-10 bg-purple-100 text-purple-600 rounded-xl flex items-center justify-center">📍</div>
        <div>
          <p class="font-medium text-gray-900 text-sm">ที่อยู่</p>
          <p class="text-gray-400 text-xs">กรุงเทพฯ ประเทศไทย</p>
        </div>
      </div>
      <span class="text-gray-300 text-lg">›</span>
    </div>
    
  </div>
</div>
```

---

## Step 99: Advanced Border Patterns

### Pattern 1: Gradient Border

```html
<!-- วิธีที่ 1: box-shadow trick -->
<button class="relative px-6 py-3 rounded-xl bg-white text-gray-900 font-semibold shadow-[inset_0_0_0_2px_transparent] hover:shadow-[inset_0_0_0_2px_#6366f1] transition-shadow">
  Hover for border
</button>

<!-- วิธีที่ 2: pseudo-element trick -->
<div class="relative p-[2px] rounded-2xl overflow-hidden">
  <div class="absolute inset-0 bg-gradient-to-r from-blue-500 to-purple-500"></div>
  <div class="relative bg-white rounded-2xl p-6">
    <p>Content with gradient border</p>
  </div>
</div>
```

### Pattern 2: Notch / Cut Corner

```html
<!-- Cut corner effect ด้วย clip-path -->
<div class="bg-blue-500 text-white p-6 [clip-path:polygon(0_0,calc(100%-20px)_0,100%_20px,100%_100%,0_100%)]">
  Cut corner card
</div>
```

### Pattern 3: Badge Position

```html
<!-- Badge ที่ติดขอบการ์ด -->
<div class="relative bg-white rounded-xl shadow p-6">
  <div class="absolute -top-2 -right-2 bg-red-500 text-white text-xs font-bold w-6 h-6 rounded-full flex items-center justify-center">
    3
  </div>
  <p>Card with notification badge</p>
</div>
```

---

## Step 100: Workshop — Complete Component Library

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Border & Shadow Workshop</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 p-8 space-y-8">

  <h1 class="text-3xl font-black text-gray-900">Border & Shadow Components</h1>

  <!-- Section 1: Cards -->
  <section>
    <h2 class="text-lg font-semibold text-gray-700 mb-3">Cards</h2>
    <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
      
      <!-- Flat -->
      <div class="bg-white border border-gray-200 rounded-xl p-5">
        <h3 class="font-semibold text-gray-900">Flat Card</h3>
        <p class="text-gray-500 text-sm mt-1">Border only, no shadow</p>
      </div>
      
      <!-- Shadow -->
      <div class="bg-white rounded-xl shadow-md p-5">
        <h3 class="font-semibold text-gray-900">Shadow Card</h3>
        <p class="text-gray-500 text-sm mt-1">Shadow only, no border</p>
      </div>
      
      <!-- Both -->
      <div class="bg-white border border-gray-100 rounded-xl shadow-sm p-5">
        <h3 class="font-semibold text-gray-900">Combined</h3>
        <p class="text-gray-500 text-sm mt-1">Subtle border + shadow</p>
      </div>
      
    </div>
  </section>

  <!-- Section 2: Buttons -->
  <section>
    <h2 class="text-lg font-semibold text-gray-700 mb-3">Buttons</h2>
    <div class="flex flex-wrap gap-3">
      
      <button class="bg-indigo-600 hover:bg-indigo-700 text-white font-semibold px-5 py-2.5 rounded-xl shadow-md shadow-indigo-500/30 hover:shadow-lg hover:shadow-indigo-500/40 transition-all hover:-translate-y-0.5">
        Primary
      </button>
      
      <button class="bg-white hover:bg-gray-50 text-gray-700 font-semibold px-5 py-2.5 rounded-xl border border-gray-200 shadow-sm hover:shadow-md transition-all hover:-translate-y-0.5">
        Secondary
      </button>
      
      <button class="text-indigo-600 font-semibold px-5 py-2.5 rounded-xl border-2 border-indigo-200 hover:border-indigo-400 hover:bg-indigo-50 transition-all">
        Outlined
      </button>
      
      <button class="bg-gradient-to-r from-indigo-500 to-purple-600 hover:from-indigo-600 hover:to-purple-700 text-white font-semibold px-5 py-2.5 rounded-xl shadow-md shadow-purple-500/30 transition-all hover:-translate-y-0.5">
        Gradient
      </button>
      
      <button class="bg-white/10 backdrop-blur-sm border border-white/20 text-gray-900 font-semibold px-5 py-2.5 rounded-xl hover:bg-white/20 transition-all">
        Glass
      </button>
      
    </div>
  </section>

  <!-- Section 3: Badges -->
  <section>
    <h2 class="text-lg font-semibold text-gray-700 mb-3">Badges</h2>
    <div class="flex flex-wrap gap-2">
      <span class="bg-blue-100 text-blue-700 text-xs font-semibold px-2.5 py-1 rounded-full">Default</span>
      <span class="bg-green-100 text-green-700 text-xs font-semibold px-2.5 py-1 rounded-full">Success</span>
      <span class="bg-amber-100 text-amber-700 text-xs font-semibold px-2.5 py-1 rounded-full">Warning</span>
      <span class="bg-red-100 text-red-700 text-xs font-semibold px-2.5 py-1 rounded-full">Error</span>
      <span class="bg-purple-100 text-purple-700 text-xs font-semibold px-2.5 py-1 rounded-full">Purple</span>
      
      <span class="border border-blue-300 text-blue-700 text-xs font-semibold px-2.5 py-1 rounded-full">Outlined</span>
      
      <span class="bg-gradient-to-r from-indigo-500 to-purple-500 text-white text-xs font-semibold px-2.5 py-1 rounded-full">Gradient</span>
    </div>
  </section>

  <!-- Section 4: Dividers -->
  <section>
    <h2 class="text-lg font-semibold text-gray-700 mb-3">Dividers</h2>
    
    <!-- Simple -->
    <div class="border-t border-gray-200 my-4"></div>
    
    <!-- With text -->
    <div class="relative my-4">
      <div class="absolute inset-0 flex items-center">
        <div class="w-full border-t border-gray-200"></div>
      </div>
      <div class="relative flex justify-center">
        <span class="bg-gray-100 px-4 text-sm text-gray-500">หรือ</span>
      </div>
    </div>
    
    <!-- Colored -->
    <div class="border-t-2 border-indigo-500 my-4"></div>
    
    <!-- Dashed -->
    <div class="border-t-2 border-dashed border-gray-300 my-4"></div>
  </section>

</body>
</html>
```

---

## 📝 สรุป Part 10

| Category | Utilities |
|----------|-----------|
| Border Width | `border-{0,2,4,8}` `border-{side}-{size}` |
| Border Color | `border-{color}-{shade}` |
| Border Style | `border-solid/dashed/dotted/double/none` |
| Border Radius | `rounded-{none,sm,md,lg,xl,2xl,3xl,full}` |
| Box Shadow | `shadow-{sm,md,lg,xl,2xl,inner,none}` |
| Colored Shadow | `shadow-{color}/{opacity}` |
| Outline | `outline-{width}` `outline-{color}` `outline-offset-*` |
| Ring | `ring-{width}` `ring-{color}` `ring-offset-*` |
| Divide | `divide-y` `divide-x` `divide-{color}` |

---

## 🔜 จาก Part 11 เป็นต้นไป

เราจะเข้าสู่หัวข้อขั้นสูงขึ้น:
- Part 11: Display, Visibility, Overflow
- Part 12: Position
- Part 13: Responsive Design
- Part 14–15: States, Transitions, Animations

---

*Part 10 — จาก 100 Parts | Steps 91–100 จาก 1,000 Steps*
