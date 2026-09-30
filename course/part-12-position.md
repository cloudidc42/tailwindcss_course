# Part 12: Position — Static, Relative, Absolute, Fixed, Sticky
## Steps 111–120: ควบคุมตำแหน่ง Element

---

## 🎯 เป้าหมายของ Part นี้

- Position types ทุกแบบ
- Top, Right, Bottom, Left
- Inset (shorthand)
- Transform เบื้องต้น (translate)
- Practical Positioning Patterns

---

## Step 111: Position Types

```html
<!-- Position types -->
<div class="static">static — default, อยู่ใน normal flow</div>
<div class="relative">relative — อ้างอิงจากตำแหน่งเดิม</div>
<div class="absolute">absolute — ออกจาก flow, อ้างอิง parent relative</div>
<div class="fixed">fixed — ติดกับ viewport</div>
<div class="sticky">sticky — relative จนถึงขอบ viewport</div>
```

### เข้าใจแต่ละ Position

```
static:   ปกติ, top/right/bottom/left ไม่ทำงาน
relative: เหมือน static แต่ top/right/... ทำงาน
          และเป็น "containing block" ให้ absolute children
absolute: ออกจาก normal flow, วางตามแห่ parent relative ที่ใกล้สุด
fixed:    ติด viewport, ไม่เลื่อนตาม scroll
sticky:   เหมือน relative จนกว่าจะถึงขอบ viewport แล้วค้างอยู่
```

---

## Step 112: Top, Right, Bottom, Left (TRBL)

```html
<!-- Inset shorthand (all sides) -->
<div class="absolute inset-0">ครอบคลุมทั้ง parent (top: 0, right: 0, bottom: 0, left: 0)</div>
<div class="absolute inset-4">ห่าง 16px ทุกด้าน</div>
<div class="absolute inset-x-0">ซ้าย-ขวา 0 (full width)</div>
<div class="absolute inset-y-0">บน-ล่าง 0 (full height)</div>

<!-- Individual -->
<div class="absolute top-0">top: 0</div>
<div class="absolute top-4">top: 16px</div>
<div class="absolute top-1/2">top: 50%</div>
<div class="absolute top-full">top: 100%</div>

<div class="absolute right-0">right: 0</div>
<div class="absolute right-4">right: 16px</div>

<div class="absolute bottom-0">bottom: 0</div>
<div class="absolute bottom-4">bottom: 16px</div>

<div class="absolute left-0">left: 0</div>
<div class="absolute left-4">left: 16px</div>

<!-- Negative values -->
<div class="absolute -top-4">top: -16px</div>
<div class="absolute -right-2">right: -8px</div>

<!-- Auto -->
<div class="absolute top-auto">top: auto</div>

<!-- Arbitrary -->
<div class="absolute top-[10%]">top: 10%</div>
<div class="absolute left-[calc(50%-100px)]">calc</div>
```

---

## Step 113: Centering with Position

```html
<!-- Center absolutely positioned element -->

<!-- ❌ เก่า (ยุ่งยาก) -->
<div class="absolute top-1/2 left-1/2" style="transform: translate(-50%, -50%)">
  Old way
</div>

<!-- ✅ Tailwind วิธีใหม่ -->
<div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2">
  Centered!
</div>

<!-- Center horizontally only -->
<div class="absolute left-1/2 -translate-x-1/2">
  Horizontal center
</div>

<!-- Center vertically only -->
<div class="absolute top-1/2 -translate-y-1/2">
  Vertical center
</div>
```

---

## Step 114: Relative + Absolute Pattern

```html
<!-- Parent: relative, Child: absolute -->
<div class="relative bg-white rounded-xl p-6">
  <!-- Badge ที่ corner -->
  <div class="absolute top-3 right-3 bg-red-500 text-white text-xs font-bold px-2 py-0.5 rounded-full">
    NEW
  </div>
  
  <h3 class="font-bold text-gray-900">Card Title</h3>
  <p class="text-gray-500 text-sm mt-1">เนื้อหา card</p>
</div>

<!-- Image Overlay -->
<div class="relative overflow-hidden rounded-2xl">
  <img src="image.jpg" class="w-full h-48 object-cover" alt="">
  
  <!-- Gradient overlay from bottom -->
  <div class="absolute inset-0 bg-gradient-to-t from-black/70 to-transparent"></div>
  
  <!-- Text over image -->
  <div class="absolute bottom-0 left-0 right-0 p-4">
    <h3 class="text-white font-bold">Image Title</h3>
    <p class="text-white/80 text-sm">Subtitle here</p>
  </div>
</div>

<!-- Loading Spinner Overlay -->
<div class="relative">
  <div class="bg-white rounded-xl p-6">
    Content ข้างใน
  </div>
  
  <!-- Loading overlay -->
  <div class="absolute inset-0 bg-white/70 backdrop-blur-sm rounded-xl flex items-center justify-center">
    <div class="size-8 border-3 border-blue-500 border-t-transparent rounded-full animate-spin"></div>
  </div>
</div>
```

---

## Step 115: Fixed Position

```html
<!-- Fixed Header -->
<header class="fixed top-0 left-0 right-0 z-50 bg-white/90 backdrop-blur-md border-b border-gray-200">
  <div class="max-w-7xl mx-auto px-4 h-16 flex items-center justify-between">
    <div class="font-bold text-xl">Logo</div>
    <nav class="flex gap-6">
      <a href="#" class="text-gray-600 hover:text-gray-900 text-sm">Home</a>
    </nav>
  </div>
</header>

<!-- Content ต้องมี padding-top เท่ากับ header height -->
<main class="pt-16">
  Page content
</main>

<!-- Fixed Sidebar -->
<aside class="fixed left-0 top-0 h-screen w-64 bg-gray-900 z-40">
  Sidebar
</aside>

<!-- Content offset -->
<main class="ml-64">
  Content ถัดจาก sidebar
</main>

<!-- Fixed FAB (Floating Action Button) -->
<button class="fixed bottom-6 right-6 size-14 bg-blue-600 hover:bg-blue-700 text-white rounded-full shadow-lg hover:shadow-xl transition-all hover:-translate-y-1 flex items-center justify-center z-50 text-xl">
  +
</button>

<!-- Fixed Toast/Notification -->
<div class="fixed bottom-4 right-4 bg-gray-900 text-white text-sm px-4 py-3 rounded-xl shadow-lg z-[100] flex items-center gap-3">
  <span class="text-green-400">✓</span>
  บันทึกสำเร็จแล้ว!
  <button class="text-gray-400 hover:text-white ml-2">×</button>
</div>
```

---

## Step 116: Sticky Position

```html
<!-- Sticky Header -->
<header class="sticky top-0 z-50 bg-white shadow">
  เลื่อนลงแล้ว header จะติดบนสุด
</header>

<!-- Sticky Table Header -->
<div class="overflow-auto max-h-96">
  <table class="w-full">
    <thead class="sticky top-0 bg-white shadow-sm">
      <tr>
        <th class="px-4 py-3 text-left">ชื่อ</th>
        <th class="px-4 py-3 text-left">อีเมล</th>
      </tr>
    </thead>
    <tbody>
      <!-- Rows -->
    </tbody>
  </table>
</div>

<!-- Sticky Sidebar -->
<div class="flex gap-8">
  <aside class="w-64 flex-none">
    <div class="sticky top-20">
      Sidebar content ที่ sticky
    </div>
  </aside>
  <main class="flex-1">
    Long content...
  </main>
</div>

<!-- Sticky Section Headers -->
<div>
  <div class="sticky top-16 bg-gray-100 px-4 py-2 text-xs font-semibold text-gray-500 uppercase z-10">
    A
  </div>
  <div class="py-3 px-4 border-b">Anya</div>
  <div class="py-3 px-4 border-b">Aaron</div>
  
  <div class="sticky top-16 bg-gray-100 px-4 py-2 text-xs font-semibold text-gray-500 uppercase z-10">
    B
  </div>
  <div class="py-3 px-4 border-b">Bob</div>
  <div class="py-3 px-4 border-b">Bella</div>
</div>
```

---

## Step 117: Transform (เบื้องต้น)

```html
<!-- Translate -->
<div class="translate-x-4">ขยับขวา 16px</div>
<div class="-translate-x-4">ขยับซ้าย 16px</div>
<div class="translate-y-4">ขยับลง 16px</div>
<div class="-translate-y-4">ขยับขึ้น 16px</div>
<div class="translate-x-1/2">ขยับขวา 50%</div>
<div class="-translate-x-1/2">ขยับซ้าย 50%</div>
<div class="translate-x-full">ขยับขวา 100%</div>
<div class="-translate-x-full">ขยับซ้าย 100%</div>

<!-- Scale -->
<div class="scale-75">75%</div>
<div class="scale-90">90%</div>
<div class="scale-100">100% (normal)</div>
<div class="scale-110">110%</div>
<div class="scale-125">125%</div>
<div class="scale-150">150%</div>
<div class="-scale-x-100">flip horizontal</div>

<!-- Rotate -->
<div class="rotate-0">0°</div>
<div class="rotate-45">45°</div>
<div class="rotate-90">90°</div>
<div class="rotate-180">180°</div>
<div class="-rotate-45">-45°</div>
<div class="rotate-[30deg]">30°</div>

<!-- Skew -->
<div class="skew-x-6">skew X 6°</div>
<div class="skew-y-3">skew Y 3°</div>
<div class="-skew-x-6">-6°</div>

<!-- Transform Origin -->
<div class="origin-center">center (default)</div>
<div class="origin-top">top</div>
<div class="origin-top-right">top right</div>
<div class="origin-right">right</div>
<div class="origin-bottom-right">bottom right</div>
<div class="origin-bottom">bottom</div>
<div class="origin-bottom-left">bottom left</div>
<div class="origin-left">left</div>
<div class="origin-top-left">top left</div>
```

---

## Step 118: Positioning Patterns Workshop

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Position Workshop</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100">

  <!-- Fixed Header -->
  <header class="fixed top-0 left-0 right-0 z-50 bg-white/95 backdrop-blur-md border-b border-gray-200">
    <div class="max-w-7xl mx-auto px-4 h-16 flex items-center justify-between">
      <div class="font-bold text-xl text-indigo-600">Brand</div>
      <nav class="hidden md:flex items-center gap-6">
        <a href="#section1" class="text-sm text-gray-600 hover:text-gray-900 transition-colors">Section 1</a>
        <a href="#section2" class="text-sm text-gray-600 hover:text-gray-900 transition-colors">Section 2</a>
        <a href="#section3" class="text-sm text-gray-600 hover:text-gray-900 transition-colors">Section 3</a>
      </nav>
    </div>
  </header>

  <!-- Main Content -->
  <main class="pt-16">
    
    <!-- Section 1 -->
    <section id="section1" class="scroll-mt-20 py-20 px-4 bg-white">
      <div class="max-w-4xl mx-auto">
        <h2 class="text-3xl font-bold text-gray-900 mb-6">Position: Relative + Absolute</h2>
        
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
          
          <!-- Card with badge -->
          <div class="relative bg-white border border-gray-200 rounded-2xl p-6 shadow-sm">
            <span class="absolute -top-3 left-6 bg-indigo-500 text-white text-xs font-bold px-3 py-1 rounded-full">
              NEW
            </span>
            <h3 class="font-bold text-gray-900 mb-2">Card with Badge</h3>
            <p class="text-gray-500 text-sm">Badge ที่ยื่นออกมาจาก card</p>
          </div>
          
          <!-- Image card -->
          <div class="relative overflow-hidden rounded-2xl h-48 bg-gray-900">
            <div class="absolute inset-0 bg-gradient-to-br from-indigo-500 to-purple-600"></div>
            <div class="absolute inset-0 bg-gradient-to-t from-black/60 to-transparent"></div>
            <div class="absolute bottom-4 left-4 right-4">
              <h3 class="text-white font-bold">Image Overlay</h3>
              <p class="text-white/70 text-sm">Text ทับบน image</p>
            </div>
            <div class="absolute top-4 right-4">
              <span class="bg-white/20 backdrop-blur-sm text-white text-xs px-2 py-1 rounded-lg">
                🔥 Hot
              </span>
            </div>
          </div>
          
        </div>
      </div>
    </section>

    <!-- Section 2 -->
    <section id="section2" class="scroll-mt-20 py-20 px-4 bg-gray-50">
      <div class="max-w-4xl mx-auto">
        <h2 class="text-3xl font-bold text-gray-900 mb-6">Sticky Sidebar</h2>
        
        <div class="flex gap-6">
          <!-- Sticky Sidebar -->
          <aside class="hidden md:block w-56 flex-none">
            <div class="sticky top-24 space-y-2">
              <a href="#" class="block text-sm text-gray-600 hover:text-indigo-600 py-2 border-l-2 border-transparent hover:border-indigo-500 pl-3 transition-all">
                Introduction
              </a>
              <a href="#" class="block text-sm text-indigo-600 font-medium py-2 border-l-2 border-indigo-500 pl-3">
                Getting Started
              </a>
              <a href="#" class="block text-sm text-gray-600 hover:text-indigo-600 py-2 border-l-2 border-transparent hover:border-indigo-500 pl-3 transition-all">
                Configuration
              </a>
              <a href="#" class="block text-sm text-gray-600 hover:text-indigo-600 py-2 border-l-2 border-transparent hover:border-indigo-500 pl-3 transition-all">
                Advanced Usage
              </a>
            </div>
          </aside>
          
          <!-- Main Content -->
          <div class="flex-1 space-y-4">
            <div class="bg-white rounded-xl p-6 shadow-sm">
              <h3 class="font-bold text-gray-900 mb-3">Getting Started</h3>
              <p class="text-gray-600 text-sm leading-relaxed">
                Lorem ipsum dolor sit amet, consectetur adipiscing elit. 
                Sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
              </p>
            </div>
            <div class="bg-white rounded-xl p-6 shadow-sm">
              <h3 class="font-bold text-gray-900 mb-3">Configuration</h3>
              <p class="text-gray-600 text-sm leading-relaxed">
                Ut enim ad minim veniam, quis nostrud exercitation ullamco 
                laboris nisi ut aliquip ex ea commodo consequat.
              </p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Section 3 -->
    <section id="section3" class="scroll-mt-20 py-20 px-4 bg-white">
      <div class="max-w-4xl mx-auto">
        <h2 class="text-3xl font-bold text-gray-900 mb-6">Transform Effects</h2>
        
        <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
          <div class="bg-indigo-100 rounded-xl h-24 flex items-center justify-center text-sm text-indigo-700 hover:scale-110 transition-transform cursor-pointer">
            hover:scale-110
          </div>
          <div class="bg-purple-100 rounded-xl h-24 flex items-center justify-center text-sm text-purple-700 hover:rotate-6 transition-transform cursor-pointer">
            hover:rotate-6
          </div>
          <div class="bg-pink-100 rounded-xl h-24 flex items-center justify-center text-sm text-pink-700 hover:-translate-y-2 transition-transform cursor-pointer">
            hover:-translate-y-2
          </div>
          <div class="bg-blue-100 rounded-xl h-24 flex items-center justify-center text-sm text-blue-700 hover:skew-x-3 transition-transform cursor-pointer">
            hover:skew-x-3
          </div>
        </div>
      </div>
    </section>

  </main>

  <!-- FAB -->
  <button class="fixed bottom-6 right-6 size-14 bg-indigo-600 hover:bg-indigo-700 text-white rounded-2xl shadow-lg hover:shadow-xl transition-all hover:-translate-y-1 flex items-center justify-center z-50 text-2xl">
    ↑
  </button>

</body>
</html>
```

---

## Step 119: Float

```html
<!-- Float (ใช้น้อยแล้วในยุค Flexbox/Grid) -->
<img class="float-left mr-4 mb-2 w-48 rounded-xl" src="..." alt="">
<img class="float-right ml-4 mb-2 w-48 rounded-xl" src="..." alt="">
<div class="float-none">no float</div>

<!-- Clear float -->
<div class="clear-left">clear left</div>
<div class="clear-right">clear right</div>
<div class="clear-both">clear both</div>
<div class="clear-none">no clear</div>
```

---

## Step 120: สรุป Position Patterns

```html
<!-- Pattern 1: Overlay Card (ใช้บ่อยมาก) -->
<div class="relative group overflow-hidden rounded-2xl">
  <img class="w-full h-64 object-cover transition-transform duration-500 group-hover:scale-105" src="..." alt="">
  <div class="absolute inset-0 bg-gradient-to-t from-black/80 via-transparent to-transparent"></div>
  <div class="absolute bottom-0 left-0 right-0 p-6 translate-y-4 group-hover:translate-y-0 transition-transform duration-300">
    <h3 class="text-white font-bold text-xl">Title</h3>
    <p class="text-white/0 group-hover:text-white/80 text-sm transition-all duration-300">Subtitle ที่ซ่อนอยู่</p>
  </div>
</div>

<!-- Pattern 2: Dropdown Menu -->
<div class="relative group">
  <button class="flex items-center gap-1 text-gray-700 hover:text-gray-900">
    Menu ▾
  </button>
  <div class="absolute top-full left-0 mt-1 bg-white border border-gray-200 rounded-xl shadow-lg p-1 w-48 invisible opacity-0 group-hover:visible group-hover:opacity-100 transition-all duration-150 z-50">
    <a href="#" class="block px-3 py-2 text-sm text-gray-700 hover:bg-gray-50 rounded-lg">Option 1</a>
    <a href="#" class="block px-3 py-2 text-sm text-gray-700 hover:bg-gray-50 rounded-lg">Option 2</a>
    <a href="#" class="block px-3 py-2 text-sm text-gray-700 hover:bg-gray-50 rounded-lg">Option 3</a>
  </div>
</div>

<!-- Pattern 3: Modal Dialog -->
<div class="fixed inset-0 bg-black/50 backdrop-blur-sm flex items-center justify-center z-[100] p-4">
  <div class="bg-white rounded-2xl shadow-2xl max-w-md w-full p-6 relative">
    <button class="absolute top-4 right-4 text-gray-400 hover:text-gray-600 text-xl leading-none">×</button>
    <h2 class="text-xl font-bold text-gray-900 mb-2">Modal Title</h2>
    <p class="text-gray-600 text-sm">Modal content...</p>
  </div>
</div>
```

---

## 📝 สรุป Part 12

| Property | Classes |
|----------|---------|
| Position | `static` `relative` `absolute` `fixed` `sticky` |
| Inset | `inset-{n}` `inset-x-{n}` `inset-y-{n}` |
| Individual | `top-{n}` `right-{n}` `bottom-{n}` `left-{n}` |
| Translate | `translate-x-{n}` `translate-y-{n}` `-translate-*` |
| Scale | `scale-{75,90,100,110,125,150}` `scale-x-*` `scale-y-*` |
| Rotate | `rotate-{0,45,90,180}` `-rotate-*` |
| Skew | `skew-x-{n}` `skew-y-{n}` |
| Origin | `origin-{center,top,right,bottom,left,corner}` |
| Float | `float-left` `float-right` `float-none` |
| Clear | `clear-left` `clear-right` `clear-both` |

---

*Part 12 — จาก 100 Parts | Steps 111–120 จาก 1,000 Steps*
