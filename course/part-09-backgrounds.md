# Part 09: Background และ Gradient
## Steps 81–90: พื้นหลังสวยงามทุกแบบ

---

## 🎯 เป้าหมายของ Part นี้

- Background Color, Image, Size, Position
- Gradient ทุกรูปแบบ
- Background Repeat, Attachment
- Background Clip
- Backdrop Filter (Blur)
- Pattern Backgrounds

---

## Step 81: Background Color (ทบทวน)

```html
<!-- เพิ่มเติมจาก Part 04 -->
<div class="bg-white">white</div>
<div class="bg-black">black</div>
<div class="bg-transparent">transparent</div>
<div class="bg-current">current (inherit)</div>

<!-- Opacity modifier -->
<div class="bg-blue-500/20">เบาๆ 20%</div>
<div class="bg-blue-500/50">กึ่งโปร่งใส 50%</div>
<div class="bg-blue-500/80">เกือบทึบ 80%</div>

<!-- Dark mode -->
<div class="bg-white dark:bg-gray-900">
  ขาวใน light mode, เข้มใน dark mode
</div>
```

---

## Step 82: Background Image

```html
<!-- ใช้ URL -->
<div class="bg-[url('/image.jpg')] bg-cover bg-center h-64">
  Background image
</div>

<!-- Built-in gradients (Tailwind treats as bg-image) -->
<div class="bg-gradient-to-r from-blue-500 to-purple-600">
  Linear gradient
</div>

<!-- bg-none: ลบ background image -->
<div class="bg-none">No background image</div>
```

---

## Step 83: Background Size

```html
<!-- bg-auto: ขนาดจริงของรูป -->
<div class="bg-auto bg-[url('/pattern.png')]">auto</div>

<!-- bg-cover: ครอบคลุมทั้ง container -->
<div class="bg-cover bg-[url('/hero.jpg')] h-64">cover</div>

<!-- bg-contain: เห็นรูปทั้งหมด -->
<div class="bg-contain bg-[url('/logo.png')] h-64">contain</div>

<!-- arbitrary -->
<div class="bg-[length:200px_100px]">ขนาดกำหนดเอง</div>
<div class="bg-[length:50%]">50% ของ container</div>
```

---

## Step 84: Background Position

```html
<div class="bg-center">กลาง (default)</div>
<div class="bg-top">บน</div>
<div class="bg-bottom">ล่าง</div>
<div class="bg-left">ซ้าย</div>
<div class="bg-right">ขวา</div>
<div class="bg-left-top">บนซ้าย</div>
<div class="bg-left-bottom">ล่างซ้าย</div>
<div class="bg-right-top">บนขวา</div>
<div class="bg-right-bottom">ล่างขวา</div>

<!-- Arbitrary position -->
<div class="bg-[center_top_1rem]">กลาง ลง 1rem จากบน</div>
<div class="bg-[25%_75%]">25% จากซ้าย, 75% จากบน</div>
```

---

## Step 85: Background Repeat

```html
<div class="bg-repeat">ซ้ำทั้ง X และ Y (default)</div>
<div class="bg-no-repeat">ไม่ซ้ำ</div>
<div class="bg-repeat-x">ซ้ำแนวนอนเท่านั้น</div>
<div class="bg-repeat-y">ซ้ำแนวตั้งเท่านั้น</div>
<div class="bg-repeat-round">ซ้ำแบบ round</div>
<div class="bg-repeat-space">ซ้ำแบบ space</div>
```

---

## Step 86: Background Attachment

```html
<!-- Scroll effect -->
<div class="bg-scroll">เลื่อนตาม scroll (default)</div>
<div class="bg-fixed">ติดอยู่กับ viewport (parallax)</div>
<div class="bg-local">เลื่อนตาม element scroll</div>
```

### Parallax Effect

```html
<div class="bg-[url('/hero.jpg')] bg-fixed bg-cover bg-center h-96 flex items-center justify-center">
  <h1 class="text-white text-5xl font-black drop-shadow-lg">
    Parallax Hero
  </h1>
</div>
```

---

## Step 87: Background Clip

```html
<!-- Text gradient effect -->
<h1 class="bg-gradient-to-r from-purple-500 to-pink-500 bg-clip-text text-transparent text-5xl font-black">
  Gradient Text
</h1>

<!-- bg-clip values -->
<div class="bg-clip-border">border (default)</div>
<div class="bg-clip-padding">padding box</div>
<div class="bg-clip-content">content box</div>
<div class="bg-clip-text text-transparent bg-gradient-to-r from-blue-500 to-green-500">
  Text clip
</div>
```

### Gradient Text Showcase

```html
<!DOCTYPE html>
<html>
<head>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-900 p-12 space-y-6">

  <h1 class="text-6xl font-black bg-gradient-to-r from-blue-400 to-cyan-400 bg-clip-text text-transparent">
    Blue to Cyan
  </h1>
  
  <h1 class="text-6xl font-black bg-gradient-to-r from-purple-400 via-pink-400 to-red-400 bg-clip-text text-transparent">
    Purple to Red
  </h1>
  
  <h1 class="text-6xl font-black bg-gradient-to-r from-yellow-300 to-orange-500 bg-clip-text text-transparent">
    Yellow to Orange
  </h1>
  
  <h1 class="text-6xl font-black bg-gradient-to-br from-emerald-300 to-sky-400 bg-clip-text text-transparent">
    Emerald to Sky
  </h1>
  
  <h1 class="text-6xl font-black bg-[conic-gradient(at_top_right,_var(--tw-gradient-stops))] from-indigo-600 via-purple-600 to-pink-600 bg-clip-text text-transparent">
    Conic Gradient
  </h1>

</body>
</html>
```

---

## Step 88: Backdrop Filter

```html
<!-- Blur background (glass effect) -->
<div class="backdrop-blur-sm">blur(4px)</div>
<div class="backdrop-blur">blur(8px)</div>
<div class="backdrop-blur-md">blur(12px)</div>
<div class="backdrop-blur-lg">blur(16px)</div>
<div class="backdrop-blur-xl">blur(24px)</div>
<div class="backdrop-blur-2xl">blur(40px)</div>
<div class="backdrop-blur-3xl">blur(64px)</div>
<div class="backdrop-blur-none">ไม่มี blur</div>

<!-- Backdrop brightness -->
<div class="backdrop-brightness-50">เข้มขึ้น 50%</div>
<div class="backdrop-brightness-100">ปกติ</div>
<div class="backdrop-brightness-150">สว่างขึ้น</div>

<!-- Backdrop grayscale -->
<div class="backdrop-grayscale">เทา</div>
<div class="backdrop-grayscale-0">สี</div>

<!-- Backdrop saturate -->
<div class="backdrop-saturate-0">ขาวดำ</div>
<div class="backdrop-saturate-100">ปกติ</div>
<div class="backdrop-saturate-200">สีจัด</div>
```

### Glassmorphism Card

```html
<!DOCTYPE html>
<html>
<head>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body>
  <!-- Background image -->
  <div class="min-h-screen bg-gradient-to-br from-purple-600 via-blue-600 to-cyan-600 flex items-center justify-center p-8">
    
    <!-- Floating blobs -->
    <div class="absolute top-20 left-20 size-64 bg-pink-500/30 rounded-full blur-3xl"></div>
    <div class="absolute bottom-20 right-20 size-64 bg-yellow-500/30 rounded-full blur-3xl"></div>
    
    <!-- Glass Card -->
    <div class="relative bg-white/10 backdrop-blur-xl border border-white/20 rounded-3xl p-8 max-w-md w-full shadow-2xl">
      
      <div class="text-center mb-6">
        <div class="size-16 bg-white/20 rounded-2xl flex items-center justify-center text-3xl mx-auto mb-4">
          🔮
        </div>
        <h2 class="text-2xl font-bold text-white">Glassmorphism</h2>
        <p class="text-white/70 text-sm mt-1">Glass effect card</p>
      </div>
      
      <div class="space-y-4">
        <input 
          class="w-full bg-white/10 border border-white/20 rounded-xl px-4 py-3 text-white placeholder-white/50 outline-none focus:border-white/50 transition-colors"
          placeholder="อีเมล"
        >
        <input 
          class="w-full bg-white/10 border border-white/20 rounded-xl px-4 py-3 text-white placeholder-white/50 outline-none focus:border-white/50 transition-colors"
          type="password"
          placeholder="รหัสผ่าน"
        >
        <button class="w-full bg-white text-purple-700 font-bold py-3 rounded-xl hover:bg-white/90 transition-colors">
          เข้าสู่ระบบ
        </button>
      </div>
      
    </div>
    
  </div>
</body>
</html>
```

---

## Step 89: Drop Shadow และ Filter

```html
<!-- Drop Shadow (เงาสำหรับ PNG/SVG) -->
<img class="drop-shadow-sm" src="..." alt="">
<img class="drop-shadow" src="..." alt="">
<img class="drop-shadow-md" src="..." alt="">
<img class="drop-shadow-lg" src="..." alt="">
<img class="drop-shadow-xl" src="..." alt="">
<img class="drop-shadow-2xl" src="..." alt="">
<img class="drop-shadow-none" src="..." alt="">

<!-- Colored drop shadow -->
<img class="drop-shadow-[0_4px_8px_rgba(99,102,241,0.5)]" src="..." alt="">

<!-- Image Filters -->
<img class="blur" src="..." alt="">
<img class="blur-sm" src="..." alt="">
<img class="blur-md" src="..." alt="">
<img class="blur-lg" src="..." alt="">
<img class="blur-none" src="..." alt="">

<img class="brightness-50" src="..." alt="">
<img class="brightness-75" src="..." alt="">
<img class="brightness-100" src="..." alt="">
<img class="brightness-125" src="..." alt="">
<img class="brightness-150" src="..." alt="">

<img class="contrast-50" src="..." alt="">
<img class="contrast-100" src="..." alt="">
<img class="contrast-125" src="..." alt="">
<img class="contrast-150" src="..." alt="">
<img class="contrast-200" src="..." alt="">

<img class="grayscale" src="..." alt="ขาวดำ">
<img class="grayscale-0" src="..." alt="สี">

<img class="hue-rotate-15" src="..." alt="">
<img class="hue-rotate-30" src="..." alt="">
<img class="hue-rotate-90" src="..." alt="">
<img class="hue-rotate-180" src="..." alt="">

<img class="invert" src="..." alt="">
<img class="invert-0" src="..." alt="">

<img class="saturate-0" src="..." alt="">
<img class="saturate-50" src="..." alt="">
<img class="saturate-100" src="..." alt="">
<img class="saturate-150" src="..." alt="">
<img class="saturate-200" src="..." alt="">

<img class="sepia" src="..." alt="sepia tone">
<img class="sepia-0" src="..." alt="">
```

---

## Step 90: Workshop — Hero Section กับ Background ครบรูปแบบ

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Background Workshop</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-950">

  <!-- Hero 1: Solid + Gradient Text -->
  <section class="relative overflow-hidden py-24 px-8">
    <!-- Decorative background elements -->
    <div class="absolute inset-0 bg-[radial-gradient(circle_at_top_right,_#6366f1,_transparent_50%)]"></div>
    <div class="absolute inset-0 bg-[radial-gradient(circle_at_bottom_left,_#a855f7,_transparent_50%)]"></div>
    
    <div class="relative max-w-4xl mx-auto text-center">
      <div class="inline-flex items-center gap-2 bg-white/10 border border-white/20 rounded-full px-4 py-2 text-white text-sm mb-8">
        <span class="size-2 bg-green-400 rounded-full"></span>
        New Release Available
      </div>
      
      <h1 class="text-6xl font-black mb-6 leading-none">
        <span class="text-white">Build Beautiful</span><br>
        <span class="bg-gradient-to-r from-indigo-400 via-purple-400 to-pink-400 bg-clip-text text-transparent">
          Interfaces
        </span>
      </h1>
      
      <p class="text-gray-400 text-xl mb-10 max-w-2xl mx-auto">
        ด้วย Tailwind CSS ที่ทรงพลัง สร้างได้ทุกอย่าง
      </p>
      
      <div class="flex flex-col sm:flex-row gap-4 justify-center">
        <button class="bg-white text-gray-900 font-bold px-8 py-4 rounded-2xl hover:bg-gray-100 transition-colors">
          Get Started →
        </button>
        <button class="bg-white/10 border border-white/20 text-white font-bold px-8 py-4 rounded-2xl hover:bg-white/20 transition-colors backdrop-blur-sm">
          Learn More
        </button>
      </div>
    </div>
  </section>

  <!-- Hero 2: Image Background -->
  <section class="relative min-h-[60vh] flex items-center">
    <div class="absolute inset-0 bg-[url('https://picsum.photos/1600/900?random=1')] bg-cover bg-center"></div>
    <div class="absolute inset-0 bg-black/60 backdrop-blur-[2px]"></div>
    
    <div class="relative max-w-4xl mx-auto px-8 py-16 text-center">
      <h2 class="text-5xl font-black text-white mb-4">
        Beautiful Backgrounds
      </h2>
      <p class="text-white/80 text-lg">
        ด้วย backdrop-blur และ overlay
      </p>
    </div>
  </section>

  <!-- Hero 3: Mesh Gradient -->
  <section class="relative py-24 px-8 bg-gray-950 overflow-hidden">
    <!-- Mesh gradient elements -->
    <div class="absolute top-0 left-1/4 size-96 bg-blue-600/20 rounded-full blur-3xl"></div>
    <div class="absolute bottom-0 right-1/4 size-96 bg-purple-600/20 rounded-full blur-3xl"></div>
    <div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 size-96 bg-pink-600/10 rounded-full blur-3xl"></div>
    
    <div class="relative max-w-4xl mx-auto text-center">
      <h2 class="text-5xl font-black text-white mb-4">Mesh Gradient</h2>
      <p class="text-gray-400 text-lg mb-8">สร้าง ambient light effect ด้วย blur circles</p>
      
      <!-- Glass cards row -->
      <div class="grid grid-cols-3 gap-4">
        <div class="bg-white/5 backdrop-blur-xl border border-white/10 rounded-2xl p-6">
          <div class="text-3xl mb-3">🎨</div>
          <h3 class="text-white font-semibold">Design</h3>
          <p class="text-gray-400 text-sm mt-1">สวยงาม</p>
        </div>
        <div class="bg-white/5 backdrop-blur-xl border border-white/10 rounded-2xl p-6">
          <div class="text-3xl mb-3">⚡</div>
          <h3 class="text-white font-semibold">Speed</h3>
          <p class="text-gray-400 text-sm mt-1">รวดเร็ว</p>
        </div>
        <div class="bg-white/5 backdrop-blur-xl border border-white/10 rounded-2xl p-6">
          <div class="text-3xl mb-3">🔒</div>
          <h3 class="text-white font-semibold">Secure</h3>
          <p class="text-gray-400 text-sm mt-1">ปลอดภัย</p>
        </div>
      </div>
    </div>
  </section>

</body>
</html>
```

---

## 📝 สรุป Part 09

| Category | Utilities |
|----------|-----------|
| Background Color | `bg-{color}` `bg-{color}/{opacity}` |
| Background Image | `bg-[url(...)]` |
| Gradient | `bg-gradient-to-{dir}` `from-*` `via-*` `to-*` |
| BG Size | `bg-auto` `bg-cover` `bg-contain` |
| BG Position | `bg-center` `bg-top` `bg-bottom` etc |
| BG Repeat | `bg-repeat` `bg-no-repeat` `bg-repeat-x/y` |
| BG Attachment | `bg-scroll` `bg-fixed` `bg-local` |
| BG Clip | `bg-clip-text` `bg-clip-border` |
| Backdrop Filter | `backdrop-blur-*` `backdrop-brightness-*` |
| Image Filter | `blur-*` `brightness-*` `grayscale` `sepia` |
| Drop Shadow | `drop-shadow-*` |

---

*Part 09 — จาก 100 Parts | Steps 81–90 จาก 1,000 Steps*
