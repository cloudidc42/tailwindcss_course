# Part 13: Responsive Design พื้นฐาน
## Steps 121–130: Mobile-First Design

---

## 🎯 เป้าหมายของ Part นี้

- Breakpoints ของ Tailwind
- Mobile-First approach
- Responsive utilities ทุก property
- Container queries เบื้องต้น
- Responsive Patterns ที่พบบ่อย

---

## Step 121: Breakpoints

Tailwind ใช้ **Mobile-First** โดย default

```
sm:   min-width: 640px   (Small tablets, large phones)
md:   min-width: 768px   (Tablets)
lg:   min-width: 1024px  (Laptops)
xl:   min-width: 1280px  (Desktops)
2xl:  min-width: 1536px  (Large desktops)
```

### Mobile-First คืออะไร?

```html
<!-- ❌ Desktop-First (Bootstrap default) -->
<p class="text-xl md-down:text-base">ใหญ่ทุกที่ ยกเว้น md -->

<!-- ✅ Mobile-First (Tailwind default) -->
<p class="text-base sm:text-lg md:text-xl lg:text-2xl">
  เล็กบน mobile, ใหญ่ขึ้นเรื่อยๆ
</p>
```

**หลักการ**: เขียน style สำหรับ mobile ก่อน แล้วค่อย override สำหรับ screen ใหญ่

---

## Step 122: Responsive Prefix

```html
<!-- ใช้ prefix:[utility] -->
<div class="text-sm md:text-base lg:text-lg">
  จอเล็ก: sm | จอกลาง: base | จอใหญ่: lg
</div>

<!-- Responsive columns -->
<div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
  Card grid
</div>

<!-- Responsive flex direction -->
<div class="flex flex-col md:flex-row gap-6">
  Stack on mobile, row on tablet+
</div>

<!-- Responsive padding -->
<section class="py-8 md:py-12 lg:py-16 px-4 sm:px-6 lg:px-8">
  Responsive section
</section>

<!-- Responsive display -->
<div class="block md:hidden">Mobile only</div>
<div class="hidden md:block">Tablet+ only</div>
<div class="hidden lg:flex">Desktop flex</div>
```

---

## Step 123: Responsive Typography

```html
<!-- Responsive heading -->
<h1 class="
  text-3xl 
  sm:text-4xl 
  md:text-5xl 
  lg:text-6xl 
  xl:text-7xl
  font-black 
  leading-tight
">
  Responsive Hero Title
</h1>

<!-- Responsive body text -->
<p class="text-sm sm:text-base lg:text-lg text-gray-600 leading-relaxed">
  Responsive paragraph text
</p>

<!-- Responsive text alignment -->
<h2 class="text-center md:text-left text-2xl font-bold">
  กลางบน mobile, ซ้ายบน tablet+
</h2>

<!-- Text clamp (different lines) -->
<p class="line-clamp-3 sm:line-clamp-4 md:line-clamp-none">
  Mobile: 3 lines | Tablet: 4 lines | Desktop: all
</p>
```

---

## Step 124: Responsive Layout Patterns

### Pattern 1: Stacked to Row

```html
<!-- Mobile: Stack | Desktop: Row -->
<div class="flex flex-col md:flex-row gap-6">
  <div class="md:w-1/3 bg-white p-6 rounded-xl">
    Sidebar
  </div>
  <div class="md:flex-1 bg-white p-6 rounded-xl">
    Main Content
  </div>
</div>
```

### Pattern 2: Full Width to Fixed Width

```html
<!-- Mobile: full width | Desktop: max-width centered -->
<div class="w-full md:max-w-md md:mx-auto">
  Form or modal content
</div>
```

### Pattern 3: Simple Grid

```html
<!-- 1 → 2 → 3 → 4 columns -->
<div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
  <div class="bg-white rounded-xl p-4">Card 1</div>
  <div class="bg-white rounded-xl p-4">Card 2</div>
  <div class="bg-white rounded-xl p-4">Card 3</div>
  <div class="bg-white rounded-xl p-4">Card 4</div>
</div>
```

### Pattern 4: Magazine Grid

```html
<!-- Feature article บน mobile = full width, บน desktop = larger -->
<div class="grid grid-cols-1 md:grid-cols-12 gap-6">
  <!-- Feature: full → 7/12 -->
  <article class="md:col-span-7 bg-white rounded-xl overflow-hidden shadow">
    <div class="aspect-video bg-gradient-to-br from-blue-400 to-indigo-600"></div>
    <div class="p-6">
      <h2 class="text-xl font-bold">Featured Article</h2>
    </div>
  </article>
  
  <!-- Sidebar: full → 5/12 -->
  <div class="md:col-span-5 space-y-4">
    <div class="bg-white rounded-xl p-4 shadow flex gap-3">
      <div class="size-16 flex-none bg-gray-100 rounded-lg"></div>
      <div><h3 class="font-bold text-sm">Article 2</h3></div>
    </div>
    <div class="bg-white rounded-xl p-4 shadow flex gap-3">
      <div class="size-16 flex-none bg-gray-100 rounded-lg"></div>
      <div><h3 class="font-bold text-sm">Article 3</h3></div>
    </div>
  </div>
</div>
```

---

## Step 125: Responsive Navigation

```html
<!-- Desktop: horizontal nav | Mobile: hamburger menu -->
<nav class="bg-white border-b">
  <div class="max-w-7xl mx-auto px-4">
    <div class="flex items-center justify-between h-16">
      <div class="font-bold text-xl">Logo</div>
      
      <!-- Desktop Nav -->
      <div class="hidden md:flex items-center gap-6">
        <a href="#" class="text-sm text-gray-600 hover:text-gray-900">Home</a>
        <a href="#" class="text-sm text-gray-600 hover:text-gray-900">About</a>
        <a href="#" class="text-sm text-gray-600 hover:text-gray-900">Services</a>
        <button class="bg-indigo-600 text-white text-sm px-4 py-2 rounded-lg">Contact</button>
      </div>
      
      <!-- Mobile Hamburger -->
      <button class="md:hidden size-10 flex flex-col justify-center items-center gap-1.5">
        <span class="w-6 h-0.5 bg-gray-700"></span>
        <span class="w-6 h-0.5 bg-gray-700"></span>
        <span class="w-6 h-0.5 bg-gray-700"></span>
      </button>
    </div>
  </div>
</nav>
```

---

## Step 126: Responsive Images

```html
<!-- Image ที่ responsive -->
<img 
  class="w-full max-w-none md:max-w-lg lg:max-w-xl mx-auto"
  src="image.jpg" 
  alt="Responsive image"
>

<!-- Responsive aspect ratio -->
<div class="aspect-video md:aspect-square lg:aspect-video overflow-hidden rounded-xl">
  <img class="w-full h-full object-cover" src="..." alt="">
</div>

<!-- Picture element for different sizes -->
<picture>
  <source media="(min-width: 1024px)" srcset="large.jpg">
  <source media="(min-width: 768px)" srcset="medium.jpg">
  <img class="w-full" src="small.jpg" alt="">
</picture>

<!-- Responsive object position -->
<img class="
  w-full h-48 
  object-cover 
  object-top 
  md:object-center
" src="..." alt="">
```

---

## Step 127: Container

```html
<!-- Container class -->
<div class="container mx-auto px-4">
  max-width เปลี่ยนตาม breakpoint
</div>

<!-- Container Breakpoints (default) -->
<!-- sm: 640px, md: 768px, lg: 1024px, xl: 1280px, 2xl: 1536px -->

<!-- Custom Container in config -->
```

```javascript
// tailwind.config.js — customize container
module.exports = {
  theme: {
    container: {
      center: true,           // mx-auto อัตโนมัติ
      padding: '1rem',        // padding default
      screens: {
        sm: '640px',
        md: '768px',
        lg: '1024px',
        xl: '1280px',
        '2xl': '1400px',      // custom max-width
      },
    },
  },
}
```

```html
<!-- หลัง config: container จะ auto-center และมี padding -->
<div class="container">
  Centered container
</div>
```

---

## Step 128: Custom Breakpoints

```javascript
// tailwind.config.js
module.exports = {
  theme: {
    screens: {
      // Override ทั้งหมด
      'xs': '480px',
      'sm': '640px',
      'md': '768px',
      'lg': '1024px',
      'xl': '1280px',
      '2xl': '1536px',
    },
    
    // หรือ extend เพิ่มเฉพาะ
    extend: {
      screens: {
        'xs': '480px',   // เพิ่ม extra small
        '3xl': '1920px', // เพิ่ม extra large
      },
    },
  },
}
```

```html
<!-- ใช้ custom breakpoints -->
<div class="text-sm xs:text-base 3xl:text-xl">
  ขนาด text ที่ responsive มากขึ้น
</div>
```

### Max Width Breakpoints

```javascript
// tailwind.config.js — max-width breakpoints
module.exports = {
  theme: {
    extend: {
      screens: {
        'max-sm': {'max': '639px'},  // max-width 639px
        'max-md': {'max': '767px'},
        'max-lg': {'max': '1023px'},
      },
    },
  },
}
```

```html
<!-- ใช้ max breakpoints -->
<div class="max-sm:text-center max-md:flex-col">
  ตรงกลางบน mobile, stack บน tablet
</div>
```

---

## Step 129: Range Breakpoints

```javascript
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      screens: {
        'md-only': {'min': '768px', 'max': '1023px'}, // tablet only
      },
    },
  },
}
```

```html
<!-- Tablet only -->
<div class="md-only:hidden">ซ่อนเฉพาะ tablet</div>
```

---

## Step 130: Workshop — Full Responsive Page

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Responsive Workshop</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50">

  <!-- Responsive Navbar -->
  <nav class="bg-white border-b border-gray-200 sticky top-0 z-50">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="flex items-center justify-between h-16">
        <div class="flex items-center gap-3">
          <div class="size-8 bg-indigo-600 rounded-lg flex items-center justify-center text-white font-bold text-sm">T</div>
          <span class="font-bold text-gray-900">TailwindCourse</span>
        </div>
        
        <!-- Desktop Navigation -->
        <div class="hidden md:flex items-center gap-8">
          <a href="#" class="text-sm text-gray-600 hover:text-gray-900 transition-colors">หน้าแรก</a>
          <a href="#" class="text-sm text-gray-600 hover:text-gray-900 transition-colors">หลักสูตร</a>
          <a href="#" class="text-sm text-gray-600 hover:text-gray-900 transition-colors">โปรเจกต์</a>
          <a href="#" class="text-sm text-gray-600 hover:text-gray-900 transition-colors">บล็อก</a>
        </div>
        
        <div class="hidden md:flex items-center gap-3">
          <button class="text-sm text-gray-600 hover:text-gray-900 px-3 py-2">เข้าสู่ระบบ</button>
          <button class="bg-indigo-600 hover:bg-indigo-700 text-white text-sm px-4 py-2 rounded-lg transition-colors">
            เริ่มต้น
          </button>
        </div>
        
        <!-- Mobile Menu Button -->
        <button class="md:hidden size-10 flex flex-col justify-center items-center gap-[5px] rounded-lg hover:bg-gray-100 transition-colors">
          <span class="w-5 h-0.5 bg-gray-600 rounded"></span>
          <span class="w-5 h-0.5 bg-gray-600 rounded"></span>
          <span class="w-5 h-0.5 bg-gray-600 rounded"></span>
        </button>
      </div>
    </div>
  </nav>

  <!-- Hero Section -->
  <section class="py-16 sm:py-20 lg:py-28 px-4 sm:px-6 lg:px-8">
    <div class="max-w-7xl mx-auto">
      <div class="flex flex-col lg:flex-row items-center gap-10 lg:gap-16">
        
        <!-- Text Content -->
        <div class="flex-1 text-center lg:text-left">
          <div class="inline-flex items-center gap-2 bg-indigo-50 text-indigo-700 text-sm font-medium px-3 py-1.5 rounded-full mb-5">
            <span class="size-2 bg-indigo-500 rounded-full"></span>
            100+ Part หลักสูตร
          </div>
          
          <h1 class="text-4xl sm:text-5xl lg:text-6xl font-black text-gray-900 leading-tight mb-5">
            เรียน<br>
            <span class="text-indigo-600">Tailwind CSS</span><br>
            อย่างเป็นระบบ
          </h1>
          
          <p class="text-base sm:text-lg text-gray-500 mb-8 max-w-lg mx-auto lg:mx-0 leading-relaxed">
            จาก Step 1 ถึง 1,000 ครบทุกเนื้อหา ตั้งแต่พื้นฐานสู่ระดับโลก
          </p>
          
          <div class="flex flex-col sm:flex-row gap-3 justify-center lg:justify-start">
            <button class="bg-indigo-600 hover:bg-indigo-700 text-white font-semibold px-6 py-3 rounded-xl transition-colors">
              เริ่มเรียนฟรี →
            </button>
            <button class="flex items-center justify-center gap-2 border-2 border-gray-200 hover:border-indigo-300 text-gray-700 font-semibold px-6 py-3 rounded-xl transition-colors">
              <span>▶</span> ดูตัวอย่าง
            </button>
          </div>
          
          <div class="flex items-center gap-4 justify-center lg:justify-start mt-6">
            <div class="flex -space-x-2">
              <div class="size-8 rounded-full bg-blue-400 border-2 border-white"></div>
              <div class="size-8 rounded-full bg-green-400 border-2 border-white"></div>
              <div class="size-8 rounded-full bg-purple-400 border-2 border-white"></div>
              <div class="size-8 rounded-full bg-pink-400 border-2 border-white"></div>
            </div>
            <p class="text-sm text-gray-500"><span class="font-semibold text-gray-700">1,200+</span> นักเรียน</p>
          </div>
        </div>
        
        <!-- Visual -->
        <div class="flex-none w-full max-w-sm lg:max-w-lg">
          <div class="relative">
            <div class="aspect-square rounded-3xl bg-gradient-to-br from-indigo-400 to-purple-600 shadow-2xl shadow-indigo-500/25 flex items-center justify-center text-8xl">
              🎨
            </div>
            <div class="absolute -top-4 -right-4 bg-white rounded-2xl p-3 shadow-xl">
              <p class="text-xs text-gray-500">Progress</p>
              <p class="font-bold text-gray-900">87%</p>
              <div class="mt-1 w-24 h-1.5 bg-gray-100 rounded-full">
                <div class="w-[87%] h-full bg-indigo-500 rounded-full"></div>
              </div>
            </div>
            <div class="absolute -bottom-4 -left-4 bg-white rounded-2xl p-3 shadow-xl flex items-center gap-3">
              <div class="size-8 bg-green-100 rounded-xl flex items-center justify-center">✓</div>
              <div>
                <p class="text-xs text-gray-500">Part 10</p>
                <p class="font-bold text-gray-900 text-sm">Completed!</p>
              </div>
            </div>
          </div>
        </div>
        
      </div>
    </div>
  </section>

  <!-- Stats Section -->
  <section class="bg-white py-12 px-4 border-y border-gray-200">
    <div class="max-w-7xl mx-auto">
      <div class="grid grid-cols-2 lg:grid-cols-4 gap-8">
        <div class="text-center">
          <p class="text-3xl font-black text-indigo-600">100+</p>
          <p class="text-gray-500 text-sm mt-1">บทเรียน</p>
        </div>
        <div class="text-center">
          <p class="text-3xl font-black text-indigo-600">1,000</p>
          <p class="text-gray-500 text-sm mt-1">Steps</p>
        </div>
        <div class="text-center">
          <p class="text-3xl font-black text-indigo-600">50+</p>
          <p class="text-gray-500 text-sm mt-1">โปรเจกต์</p>
        </div>
        <div class="text-center">
          <p class="text-3xl font-black text-indigo-600">4.9★</p>
          <p class="text-gray-500 text-sm mt-1">Rating</p>
        </div>
      </div>
    </div>
  </section>

  <!-- Course Grid -->
  <section class="py-16 px-4 sm:px-6 lg:px-8">
    <div class="max-w-7xl mx-auto">
      <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4 mb-8">
        <div>
          <h2 class="text-2xl sm:text-3xl font-bold text-gray-900">เนื้อหาหลักสูตร</h2>
          <p class="text-gray-500 mt-1">เลือกดูตามระดับ</p>
        </div>
        <div class="flex gap-2">
          <button class="bg-indigo-600 text-white text-sm px-4 py-2 rounded-lg">ทั้งหมด</button>
          <button class="bg-gray-100 text-gray-600 text-sm px-4 py-2 rounded-lg hover:bg-gray-200">พื้นฐาน</button>
          <button class="bg-gray-100 text-gray-600 text-sm px-4 py-2 rounded-lg hover:bg-gray-200">ขั้นสูง</button>
        </div>
      </div>
      
      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
        
        <div class="bg-white rounded-2xl overflow-hidden shadow-sm hover:shadow-md transition-shadow">
          <div class="h-40 bg-gradient-to-br from-blue-500 to-indigo-600 flex items-center justify-center text-4xl">🎨</div>
          <div class="p-5">
            <div class="flex items-center justify-between mb-2">
              <span class="text-xs bg-blue-100 text-blue-700 px-2 py-0.5 rounded-full font-medium">พื้นฐาน</span>
              <span class="text-xs text-gray-400">10 บทเรียน</span>
            </div>
            <h3 class="font-bold text-gray-900 mb-1">Typography & Colors</h3>
            <p class="text-gray-500 text-sm line-clamp-2">ฟอนต์ สี และ spacing</p>
            <div class="mt-3 flex items-center justify-between">
              <span class="text-sm font-semibold text-gray-900">ฟรี</span>
              <button class="text-indigo-600 text-sm font-medium hover:text-indigo-800">เรียน →</button>
            </div>
          </div>
        </div>
        
        <div class="bg-white rounded-2xl overflow-hidden shadow-sm hover:shadow-md transition-shadow">
          <div class="h-40 bg-gradient-to-br from-green-500 to-emerald-600 flex items-center justify-center text-4xl">📐</div>
          <div class="p-5">
            <div class="flex items-center justify-between mb-2">
              <span class="text-xs bg-green-100 text-green-700 px-2 py-0.5 rounded-full font-medium">พื้นฐาน</span>
              <span class="text-xs text-gray-400">12 บทเรียน</span>
            </div>
            <h3 class="font-bold text-gray-900 mb-1">Flexbox & Grid</h3>
            <p class="text-gray-500 text-sm line-clamp-2">Layout ทุกรูปแบบ</p>
            <div class="mt-3 flex items-center justify-between">
              <span class="text-sm font-semibold text-gray-900">ฟรี</span>
              <button class="text-green-600 text-sm font-medium hover:text-green-800">เรียน →</button>
            </div>
          </div>
        </div>
        
        <div class="bg-white rounded-2xl overflow-hidden shadow-sm hover:shadow-md transition-shadow">
          <div class="h-40 bg-gradient-to-br from-purple-500 to-pink-600 flex items-center justify-center text-4xl">✨</div>
          <div class="p-5">
            <div class="flex items-center justify-between mb-2">
              <span class="text-xs bg-purple-100 text-purple-700 px-2 py-0.5 rounded-full font-medium">กลาง</span>
              <span class="text-xs text-gray-400">15 บทเรียน</span>
            </div>
            <h3 class="font-bold text-gray-900 mb-1">Animation & Effects</h3>
            <p class="text-gray-500 text-sm line-clamp-2">Transition, animation, transform</p>
            <div class="mt-3 flex items-center justify-between">
              <span class="text-sm font-semibold text-gray-900">ฟรี</span>
              <button class="text-purple-600 text-sm font-medium hover:text-purple-800">เรียน →</button>
            </div>
          </div>
        </div>
        
      </div>
    </div>
  </section>

</body>
</html>
```

---

## 📝 สรุป Part 13

| Breakpoint | Min Width | Usage |
|------------|-----------|-------|
| (default) | 0px | Mobile styles |
| `sm:` | 640px | Large phones, small tablets |
| `md:` | 768px | Tablets |
| `lg:` | 1024px | Laptops |
| `xl:` | 1280px | Desktops |
| `2xl:` | 1536px | Large desktops |

**Mobile-First Rule**: เขียน default สำหรับ mobile, เพิ่ม breakpoint prefix สำหรับ larger screens

---

*Part 13 — จาก 100 Parts | Steps 121–130 จาก 1,000 Steps*
