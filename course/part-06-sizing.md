# Part 06: Sizing — Width, Height, Min/Max
## Steps 51–60: ควบคุมขนาดทุกมิติ

---

## 🎯 เป้าหมายของ Part นี้

- Width และ Height
- Min/Max Width และ Height
- Viewport Units
- Aspect Ratio
- Object Fit สำหรับรูปภาพ
- Responsive Sizing

---

## Step 51: Width

```html
<!-- Fixed width (ใช้ spacing scale) -->
<div class="w-0">0px</div>
<div class="w-px">1px</div>
<div class="w-1">4px</div>
<div class="w-2">8px</div>
<div class="w-4">16px</div>
<div class="w-8">32px</div>
<div class="w-16">64px</div>
<div class="w-32">128px</div>
<div class="w-48">192px</div>
<div class="w-64">256px</div>
<div class="w-80">320px</div>
<div class="w-96">384px</div>

<!-- Percentage -->
<div class="w-1/2">50%</div>
<div class="w-1/3">33.33%</div>
<div class="w-2/3">66.67%</div>
<div class="w-1/4">25%</div>
<div class="w-3/4">75%</div>
<div class="w-1/5">20%</div>
<div class="w-2/5">40%</div>
<div class="w-3/5">60%</div>
<div class="w-4/5">80%</div>
<div class="w-1/6">16.67%</div>
<div class="w-5/6">83.33%</div>
<div class="w-1/12">8.33%</div>

<!-- Special -->
<div class="w-full">100%</div>
<div class="w-screen">100vw</div>
<div class="w-svw">100svw (small viewport)</div>
<div class="w-lvw">100lvw (large viewport)</div>
<div class="w-dvw">100dvw (dynamic viewport)</div>
<div class="w-auto">auto</div>
<div class="w-fit">fit-content</div>
<div class="w-max">max-content</div>
<div class="w-min">min-content</div>
```

---

## Step 52: Max Width

```html
<!-- Named max-width (สำหรับ container) -->
<div class="max-w-none">ไม่จำกัด</div>
<div class="max-w-xs">320px</div>
<div class="max-w-sm">384px</div>
<div class="max-w-md">448px</div>
<div class="max-w-lg">512px</div>
<div class="max-w-xl">576px</div>
<div class="max-w-2xl">672px</div>
<div class="max-w-3xl">768px</div>
<div class="max-w-4xl">896px</div>
<div class="max-w-5xl">1024px</div>
<div class="max-w-6xl">1152px</div>
<div class="max-w-7xl">1280px</div>

<!-- Percentage -->
<div class="max-w-full">100%</div>
<div class="max-w-screen-sm">640px (sm breakpoint)</div>
<div class="max-w-screen-md">768px</div>
<div class="max-w-screen-lg">1024px</div>
<div class="max-w-screen-xl">1280px</div>
<div class="max-w-screen-2xl">1536px</div>

<!-- Fit content -->
<div class="max-w-fit">fit-content</div>
<div class="max-w-max">max-content</div>
<div class="max-w-min">min-content</div>

<!-- Arbitrary -->
<div class="max-w-[600px]">600px</div>
<div class="max-w-[90%]">90%</div>
```

### การใช้ max-w สำหรับ Container

```html
<!-- Standard Layout Pattern -->
<div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
  Full-width layout with max 1280px
</div>

<!-- Article/Blog -->
<article class="max-w-3xl mx-auto px-4">
  Readable article width ~768px
</article>

<!-- Form -->
<form class="max-w-lg mx-auto px-4">
  Form ที่ไม่กว้างเกินไป
</form>

<!-- Modal -->
<div class="max-w-md mx-auto">
  Modal / Dialog
</div>
```

---

## Step 53: Height

```html
<!-- Fixed height -->
<div class="h-0">0px</div>
<div class="h-px">1px</div>
<div class="h-1">4px</div>
<div class="h-4">16px</div>
<div class="h-8">32px</div>
<div class="h-16">64px</div>
<div class="h-32">128px</div>
<div class="h-48">192px</div>
<div class="h-64">256px</div>
<div class="h-96">384px</div>

<!-- Percentage -->
<div class="h-1/2">50%</div>
<div class="h-1/3">33.33%</div>
<div class="h-full">100%</div>

<!-- Viewport -->
<div class="h-screen">100vh</div>
<div class="h-svh">100svh</div>
<div class="h-lvh">100lvh</div>
<div class="h-dvh">100dvh (dynamic — iOS safe area)</div>

<!-- Special -->
<div class="h-auto">auto</div>
<div class="h-fit">fit-content</div>
<div class="h-max">max-content</div>
<div class="h-min">min-content</div>
```

### Min/Max Height

```html
<!-- Min Height -->
<div class="min-h-0">min-height: 0</div>
<div class="min-h-full">min-height: 100%</div>
<div class="min-h-screen">min-height: 100vh</div>
<div class="min-h-svh">min-height: 100svh</div>
<div class="min-h-dvh">min-height: 100dvh</div>

<!-- Max Height -->
<div class="max-h-full">100%</div>
<div class="max-h-screen">100vh</div>
<div class="max-h-96">384px</div>
<div class="max-h-[500px]">500px</div>
```

---

## Step 54: Viewport Units ขั้นสูง

### ปัญหากับ 100vh บน Mobile

```html
<!-- ปัญหา: 100vh รวม address bar บน iOS/Android -->
<div class="h-screen">
  อาจถูก crop เพราะ browser chrome
</div>

<!-- แก้ไข: ใช้ dvh (dynamic viewport height) -->
<div class="h-dvh">
  ปรับตาม viewport จริง รวม browser chrome
</div>

<!-- svh = small viewport height (หน้าที่เล็กที่สุด) -->
<div class="h-svh">Safe ที่สุด ไม่ถูก clip</div>

<!-- lvh = large viewport height (หน้าที่ใหญ่ที่สุด) -->
<div class="h-lvh">ใช้เมื่อ browser chrome ซ่อน</div>
```

### Hero Section ที่ถูกต้อง

```html
<!-- ❌ อาจมีปัญหาบน mobile -->
<section class="h-screen flex items-center justify-center">
  Hero content
</section>

<!-- ✅ ถูกต้อง -->
<section class="min-h-dvh flex items-center justify-center">
  Hero content — รองรับทุก device
</section>
```

---

## Step 55: Aspect Ratio

```html
<!-- Built-in ratios -->
<div class="aspect-auto">auto</div>
<div class="aspect-square">1:1 (square)</div>
<div class="aspect-video">16:9 (video)</div>

<!-- Arbitrary ratio -->
<div class="aspect-[4/3]">4:3</div>
<div class="aspect-[21/9]">21:9 (ultrawide)</div>
<div class="aspect-[3/4]">3:4 (portrait)</div>
```

### ตัวอย่างการใช้ Aspect Ratio

```html
<!-- Video Embed -->
<div class="aspect-video w-full rounded-xl overflow-hidden bg-black">
  <iframe 
    src="https://www.youtube.com/embed/..." 
    class="w-full h-full"
    frameborder="0"
  ></iframe>
</div>

<!-- Product Image -->
<div class="aspect-square w-full rounded-xl overflow-hidden bg-gray-100">
  <img src="product.jpg" alt="Product" class="w-full h-full object-cover">
</div>

<!-- Card Image -->
<div class="aspect-[3/2] w-full rounded-t-xl overflow-hidden">
  <img src="card-image.jpg" alt="" class="w-full h-full object-cover">
</div>

<!-- Avatar -->
<div class="aspect-square w-12 rounded-full overflow-hidden">
  <img src="avatar.jpg" alt="" class="w-full h-full object-cover">
</div>
```

---

## Step 56: Object Fit และ Object Position

```html
<!-- Object Fit -->
<img class="w-full h-48 object-contain">
<!-- contain: รักษา ratio ให้เห็นทั้งรูป -->

<img class="w-full h-48 object-cover">
<!-- cover: เต็มพื้นที่ ตัดส่วนเกิน -->

<img class="w-full h-48 object-fill">
<!-- fill: ยืดให้เต็ม อาจผิดสัดส่วน -->

<img class="w-full h-48 object-none">
<!-- none: ขนาดจริงของรูป -->

<img class="w-full h-48 object-scale-down">
<!-- scale-down: เหมือน contain แต่ไม่ขยาย -->
```

```html
<!-- Object Position -->
<img class="object-cover object-center">center (default)</img>
<img class="object-cover object-top">top</img>
<img class="object-cover object-bottom">bottom</img>
<img class="object-cover object-left">left</img>
<img class="object-cover object-right">right</img>
<img class="object-cover object-left-top">left top</img>
<img class="object-cover object-right-bottom">right bottom</img>
<img class="object-cover object-[center_top_1rem]">custom position</img>
```

### ตัวอย่าง Product Card

```html
<div class="bg-white rounded-xl shadow overflow-hidden group cursor-pointer hover:shadow-lg transition-shadow">
  <!-- Product Image ที่ aspect ratio คงที่ -->
  <div class="aspect-square overflow-hidden bg-gray-100">
    <img 
      src="https://picsum.photos/400/400?random=1" 
      alt="Product"
      class="w-full h-full object-cover group-hover:scale-105 transition-transform duration-300"
    >
  </div>
  
  <!-- Product Info -->
  <div class="p-4">
    <h3 class="font-semibold text-gray-900 mb-1">ชื่อสินค้า</h3>
    <p class="text-gray-500 text-sm mb-3 line-clamp-2">
      รายละเอียดสินค้าสั้นๆ ที่แสดงได้แค่ 2 บรรทัด
    </p>
    <div class="flex items-center justify-between">
      <span class="text-xl font-bold text-gray-900">฿1,290</span>
      <button class="bg-blue-500 hover:bg-blue-600 text-white text-sm px-4 py-2 rounded-lg transition-colors">
        ซื้อเลย
      </button>
    </div>
  </div>
</div>
```

---

## Step 57: Responsive Image Gallery

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Image Gallery</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 p-6">

  <h1 class="text-2xl font-bold text-gray-900 mb-6">Photo Gallery</h1>

  <!-- Masonry-like Grid -->
  <div class="columns-1 sm:columns-2 md:columns-3 lg:columns-4 gap-4 space-y-4">
    
    <div class="break-inside-avoid">
      <img src="https://picsum.photos/400/300?random=1" 
           class="w-full rounded-xl object-cover" alt="">
    </div>
    
    <div class="break-inside-avoid">
      <img src="https://picsum.photos/400/500?random=2" 
           class="w-full rounded-xl object-cover" alt="">
    </div>
    
    <div class="break-inside-avoid">
      <img src="https://picsum.photos/400/250?random=3" 
           class="w-full rounded-xl object-cover" alt="">
    </div>
    
    <div class="break-inside-avoid">
      <img src="https://picsum.photos/400/400?random=4" 
           class="w-full rounded-xl object-cover" alt="">
    </div>
    
    <div class="break-inside-avoid">
      <img src="https://picsum.photos/400/350?random=5" 
           class="w-full rounded-xl object-cover" alt="">
    </div>
    
    <div class="break-inside-avoid">
      <img src="https://picsum.photos/400/600?random=6" 
           class="w-full rounded-xl object-cover" alt="">
    </div>
    
    <div class="break-inside-avoid">
      <img src="https://picsum.photos/400/280?random=7" 
           class="w-full rounded-xl object-cover" alt="">
    </div>
    
    <div class="break-inside-avoid">
      <img src="https://picsum.photos/400/450?random=8" 
           class="w-full rounded-xl object-cover" alt="">
    </div>
    
  </div>

</body>
</html>
```

---

## Step 58: Size Utilities (v3.2+)

```html
<!-- size-* เป็น shorthand สำหรับ w-* h-* พร้อมกัน -->
<div class="size-4">w-4 h-4 (16px x 16px)</div>
<div class="size-8">w-8 h-8 (32px x 32px)</div>
<div class="size-12">w-12 h-12 (48px x 48px)</div>
<div class="size-16">w-16 h-16 (64px x 64px)</div>
<div class="size-24">w-24 h-24 (96px x 96px)</div>
<div class="size-full">w-full h-full</div>
<div class="size-screen">w-screen h-screen</div>
<div class="size-[100px]">100px x 100px</div>
```

### ใช้สร้าง Icon Containers

```html
<!-- Icon sizes standard -->
<div class="size-4 bg-blue-500 rounded">16px icon</div>
<div class="size-5 bg-blue-500 rounded">20px icon</div>
<div class="size-6 bg-blue-500 rounded">24px icon</div>
<div class="size-8 bg-blue-500 rounded">32px icon</div>
<div class="size-10 bg-blue-500 rounded-lg">40px icon button</div>
<div class="size-12 bg-blue-500 rounded-xl">48px icon button</div>

<!-- Avatar sizes -->
<img class="size-8 rounded-full object-cover" src="..." alt=""> <!-- 32px -->
<img class="size-10 rounded-full object-cover" src="..." alt=""> <!-- 40px -->
<img class="size-12 rounded-full object-cover" src="..." alt=""> <!-- 48px -->
<img class="size-16 rounded-full object-cover" src="..." alt=""> <!-- 64px -->
<img class="size-24 rounded-full object-cover" src="..." alt=""> <!-- 96px -->
```

---

## Step 59: Overflow

```html
<!-- Overflow -->
<div class="overflow-auto">scroll เมื่อต้องการ</div>
<div class="overflow-hidden">ซ่อนส่วนเกิน</div>
<div class="overflow-visible">แสดงส่วนเกิน (default)</div>
<div class="overflow-scroll">scroll เสมอ</div>
<div class="overflow-clip">clip อย่างเคร่งครัด</div>

<!-- Overflow X / Y แยก -->
<div class="overflow-x-auto overflow-y-hidden">horizontal scroll only</div>
<div class="overflow-x-hidden overflow-y-auto">vertical scroll only</div>

<!-- Scroll Behavior -->
<div class="scroll-smooth">smooth scroll</div>
<div class="scroll-auto">instant scroll</div>
```

### ตัวอย่าง Scrollable Table

```html
<div class="w-full overflow-x-auto">
  <table class="min-w-full">
    <thead class="bg-gray-50">
      <tr>
        <th class="px-6 py-3 text-left text-xs font-semibold text-gray-500 uppercase whitespace-nowrap">ชื่อ</th>
        <th class="px-6 py-3 text-left text-xs font-semibold text-gray-500 uppercase whitespace-nowrap">อีเมล</th>
        <th class="px-6 py-3 text-left text-xs font-semibold text-gray-500 uppercase whitespace-nowrap">ตำแหน่ง</th>
        <th class="px-6 py-3 text-left text-xs font-semibold text-gray-500 uppercase whitespace-nowrap">สถานะ</th>
      </tr>
    </thead>
    <tbody class="divide-y divide-gray-200 bg-white">
      <tr>
        <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">สมชาย ใจดี</td>
        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">somchai@example.com</td>
        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">Frontend Developer</td>
        <td class="px-6 py-4 whitespace-nowrap">
          <span class="bg-green-100 text-green-700 text-xs px-2 py-1 rounded-full">Active</span>
        </td>
      </tr>
    </tbody>
  </table>
</div>
```

---

## Step 60: Workshop — Responsive Cards Grid

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Responsive Cards</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 min-h-screen py-12">

  <div class="max-w-7xl mx-auto px-4">
    
    <div class="mb-8">
      <h1 class="text-3xl font-bold text-gray-900">สินค้าแนะนำ</h1>
      <p class="text-gray-500 mt-1">เลือกสินค้าที่ชอบได้เลย</p>
    </div>

    <!-- Products Grid -->
    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
      
      <!-- Product Card -->
      <div class="bg-white rounded-2xl overflow-hidden shadow-sm hover:shadow-md transition-shadow group">
        <!-- Image Container with fixed aspect ratio -->
        <div class="aspect-square overflow-hidden bg-gray-50">
          <img 
            src="https://picsum.photos/300/300?random=10" 
            alt="Product"
            class="w-full h-full object-cover group-hover:scale-105 transition-transform duration-500"
          >
        </div>
        
        <!-- Card Body -->
        <div class="p-4">
          <div class="flex items-start justify-between gap-2 mb-2">
            <h3 class="font-semibold text-gray-900 text-sm leading-snug line-clamp-2">
              สินค้าชิ้นที่ดีมากๆ คุณภาพพรีเมียม
            </h3>
          </div>
          
          <div class="flex items-center gap-1 mb-3">
            <span class="text-yellow-400 text-sm">★★★★★</span>
            <span class="text-gray-400 text-xs">(128)</span>
          </div>
          
          <div class="flex items-center justify-between">
            <div>
              <span class="text-lg font-bold text-gray-900">฿1,290</span>
              <span class="text-sm text-gray-400 line-through ml-1">฿1,990</span>
            </div>
            <button class="size-9 bg-blue-500 hover:bg-blue-600 text-white rounded-lg flex items-center justify-center transition-colors text-lg">
              +
            </button>
          </div>
        </div>
      </div>
      
      <!-- Repeat similar cards -->
      <div class="bg-white rounded-2xl overflow-hidden shadow-sm hover:shadow-md transition-shadow group">
        <div class="aspect-square overflow-hidden bg-gray-50">
          <img src="https://picsum.photos/300/300?random=11" alt="" class="w-full h-full object-cover group-hover:scale-105 transition-transform duration-500">
        </div>
        <div class="p-4">
          <h3 class="font-semibold text-gray-900 text-sm mb-2 line-clamp-2">อีกสินค้าหนึ่งที่น่าสนใจ</h3>
          <div class="flex items-center gap-1 mb-3">
            <span class="text-yellow-400 text-sm">★★★★☆</span>
            <span class="text-gray-400 text-xs">(89)</span>
          </div>
          <div class="flex items-center justify-between">
            <span class="text-lg font-bold text-gray-900">฿2,490</span>
            <button class="size-9 bg-blue-500 hover:bg-blue-600 text-white rounded-lg flex items-center justify-center transition-colors text-lg">+</button>
          </div>
        </div>
      </div>

      <div class="bg-white rounded-2xl overflow-hidden shadow-sm hover:shadow-md transition-shadow group">
        <div class="aspect-square overflow-hidden bg-gray-50">
          <img src="https://picsum.photos/300/300?random=12" alt="" class="w-full h-full object-cover group-hover:scale-105 transition-transform duration-500">
        </div>
        <div class="p-4">
          <h3 class="font-semibold text-gray-900 text-sm mb-2 line-clamp-2">สินค้าพรีเมียมสุดๆ</h3>
          <div class="flex items-center gap-1 mb-3">
            <span class="text-yellow-400 text-sm">★★★★★</span>
            <span class="text-gray-400 text-xs">(312)</span>
          </div>
          <div class="flex items-center justify-between">
            <span class="text-lg font-bold text-gray-900">฿3,990</span>
            <button class="size-9 bg-blue-500 hover:bg-blue-600 text-white rounded-lg flex items-center justify-center transition-colors text-lg">+</button>
          </div>
        </div>
      </div>

      <div class="bg-white rounded-2xl overflow-hidden shadow-sm hover:shadow-md transition-shadow group">
        <div class="aspect-square overflow-hidden bg-gray-50">
          <img src="https://picsum.photos/300/300?random=13" alt="" class="w-full h-full object-cover group-hover:scale-105 transition-transform duration-500">
        </div>
        <div class="p-4">
          <h3 class="font-semibold text-gray-900 text-sm mb-2 line-clamp-2">สินค้าใหม่มาแรง</h3>
          <div class="flex items-center gap-1 mb-3">
            <span class="text-yellow-400 text-sm">★★★★☆</span>
            <span class="text-gray-400 text-xs">(45)</span>
          </div>
          <div class="flex items-center justify-between">
            <span class="text-lg font-bold text-gray-900">฿890</span>
            <button class="size-9 bg-blue-500 hover:bg-blue-600 text-white rounded-lg flex items-center justify-center transition-colors text-lg">+</button>
          </div>
        </div>
      </div>
      
    </div>
  </div>

</body>
</html>
```

---

## 📝 สรุป Part 06

| Category | Utilities |
|----------|-----------|
| Width | `w-{n}` `w-{fraction}` `w-full` `w-screen` `w-auto` |
| Height | `h-{n}` `h-{fraction}` `h-full` `h-screen` `h-dvh` |
| Min Width | `min-w-0` `min-w-full` |
| Max Width | `max-w-{size}` `max-w-screen-{bp}` |
| Min/Max Height | `min-h-*` `max-h-*` |
| Aspect Ratio | `aspect-square` `aspect-video` `aspect-[x/y]` |
| Size (shorthand) | `size-{n}` |
| Object Fit | `object-cover` `object-contain` `object-fill` |
| Overflow | `overflow-hidden` `overflow-auto` `overflow-scroll` |

---

*Part 06 — จาก 100 Parts | Steps 51–60 จาก 1,000 Steps*
