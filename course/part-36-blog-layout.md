# Part 36: Blog & Content Layout

## เป้าหมาย
- สร้าง Blog Layout ระดับมืออาชีพด้วย Tailwind CSS
- ออกแบบ Article Card, Featured Post, Sidebar
- สร้าง Reading Progress Bar และ Table of Contents
- จัดการ Typography สำหรับเนื้อหา Long-form

---

## Step 351: Blog Homepage Layout

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Blog Layout - Step 351</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 text-gray-900">

  <!-- Header -->
  <header class="bg-white border-b sticky top-0 z-50">
    <div class="max-w-6xl mx-auto px-4 py-4 flex items-center justify-between">
      <a href="#" class="text-2xl font-bold text-indigo-600">DevBlog</a>
      <nav class="hidden md:flex items-center gap-6 text-sm font-medium text-gray-600">
        <a href="#" class="hover:text-indigo-600 transition-colors">หน้าแรก</a>
        <a href="#" class="hover:text-indigo-600 transition-colors">บทความ</a>
        <a href="#" class="hover:text-indigo-600 transition-colors">หมวดหมู่</a>
        <a href="#" class="hover:text-indigo-600 transition-colors">เกี่ยวกับ</a>
      </nav>
      <button class="bg-indigo-600 text-white text-sm px-4 py-2 rounded-lg hover:bg-indigo-700 transition-colors">
        Subscribe
      </button>
    </div>
  </header>

  <!-- Hero Featured Post -->
  <section class="max-w-6xl mx-auto px-4 py-10">
    <div class="relative overflow-hidden rounded-2xl bg-gradient-to-br from-indigo-900 to-purple-900 text-white">
      <div class="absolute inset-0 opacity-20" style="background-image: url('https://picsum.photos/1200/500'); background-size: cover;"></div>
      <div class="relative p-8 md:p-12 max-w-2xl">
        <span class="inline-block bg-indigo-500 text-white text-xs font-semibold px-3 py-1 rounded-full mb-4">
          Featured
        </span>
        <h1 class="text-3xl md:text-4xl font-bold mb-4 leading-tight">
          การออกแบบ Design System ด้วย Tailwind CSS สำหรับองค์กรขนาดใหญ่
        </h1>
        <p class="text-indigo-200 mb-6 leading-relaxed">
          เรียนรู้วิธีสร้าง Design System ที่ scale ได้ดี ตั้งแต่ Color Token, Typography Scale ไปจนถึง Component Library
        </p>
        <div class="flex items-center gap-4">
          <img src="https://i.pravatar.cc/40?img=1" class="w-10 h-10 rounded-full ring-2 ring-white" alt="Author">
          <div>
            <p class="font-semibold text-sm">สมชาย เทคโน</p>
            <p class="text-indigo-300 text-xs">15 มิ.ย. 2024 · 12 นาที</p>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- Main Content + Sidebar -->
  <main class="max-w-6xl mx-auto px-4 pb-16">
    <div class="grid grid-cols-1 lg:grid-cols-[1fr_320px] gap-10">

      <!-- Article Grid -->
      <section>
        <div class="flex items-center justify-between mb-6">
          <h2 class="text-xl font-bold">บทความล่าสุด</h2>
          <a href="#" class="text-sm text-indigo-600 hover:underline">ดูทั้งหมด →</a>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
          <!-- Article Card -->
          <article class="bg-white rounded-xl overflow-hidden shadow-sm hover:shadow-md transition-shadow group">
            <div class="overflow-hidden">
              <img src="https://picsum.photos/400/220?random=1" alt="Article" class="w-full h-48 object-cover group-hover:scale-105 transition-transform duration-300">
            </div>
            <div class="p-5">
              <div class="flex items-center gap-2 mb-3">
                <span class="bg-blue-100 text-blue-700 text-xs font-medium px-2.5 py-1 rounded-full">Tailwind CSS</span>
                <span class="text-gray-400 text-xs">8 นาที</span>
              </div>
              <h3 class="font-bold text-lg mb-2 leading-snug group-hover:text-indigo-600 transition-colors">
                Flexbox vs Grid: เมื่อไหร่ควรใช้อะไร?
              </h3>
              <p class="text-gray-500 text-sm leading-relaxed mb-4 line-clamp-2">
                การเลือกใช้ Flexbox หรือ CSS Grid ให้เหมาะสมกับสถานการณ์ ช่วยให้ Layout แข็งแกร่งและง่ายต่อการ maintain
              </p>
              <div class="flex items-center justify-between">
                <div class="flex items-center gap-2">
                  <img src="https://i.pravatar.cc/32?img=2" class="w-8 h-8 rounded-full" alt="">
                  <span class="text-xs text-gray-500">วิไล สวัสดี</span>
                </div>
                <span class="text-xs text-gray-400">12 มิ.ย. 2024</span>
              </div>
            </div>
          </article>

          <article class="bg-white rounded-xl overflow-hidden shadow-sm hover:shadow-md transition-shadow group">
            <div class="overflow-hidden">
              <img src="https://picsum.photos/400/220?random=2" alt="Article" class="w-full h-48 object-cover group-hover:scale-105 transition-transform duration-300">
            </div>
            <div class="p-5">
              <div class="flex items-center gap-2 mb-3">
                <span class="bg-green-100 text-green-700 text-xs font-medium px-2.5 py-1 rounded-full">Performance</span>
                <span class="text-gray-400 text-xs">6 นาที</span>
              </div>
              <h3 class="font-bold text-lg mb-2 leading-snug group-hover:text-indigo-600 transition-colors">
                Optimize Tailwind CSS Build ให้เล็กที่สุด
              </h3>
              <p class="text-gray-500 text-sm leading-relaxed mb-4 line-clamp-2">
                เทคนิคการ purge CSS, JIT mode และการตั้งค่า content paths ให้ถูกต้องเพื่อลดขนาด bundle
              </p>
              <div class="flex items-center justify-between">
                <div class="flex items-center gap-2">
                  <img src="https://i.pravatar.cc/32?img=3" class="w-8 h-8 rounded-full" alt="">
                  <span class="text-xs text-gray-500">ธนพล รัตน์</span>
                </div>
                <span class="text-xs text-gray-400">10 มิ.ย. 2024</span>
              </div>
            </div>
          </article>

          <article class="bg-white rounded-xl overflow-hidden shadow-sm hover:shadow-md transition-shadow group">
            <div class="overflow-hidden">
              <img src="https://picsum.photos/400/220?random=3" alt="Article" class="w-full h-48 object-cover group-hover:scale-105 transition-transform duration-300">
            </div>
            <div class="p-5">
              <div class="flex items-center gap-2 mb-3">
                <span class="bg-purple-100 text-purple-700 text-xs font-medium px-2.5 py-1 rounded-full">Animation</span>
                <span class="text-gray-400 text-xs">10 นาที</span>
              </div>
              <h3 class="font-bold text-lg mb-2 leading-snug group-hover:text-indigo-600 transition-colors">
                Scroll-Triggered Animations ด้วย Intersection Observer
              </h3>
              <p class="text-gray-500 text-sm leading-relaxed mb-4 line-clamp-2">
                สร้าง animation ที่ trigger เมื่อ element เข้าสู่ viewport โดยไม่ต้องพึ่ง library ใดๆ
              </p>
              <div class="flex items-center justify-between">
                <div class="flex items-center gap-2">
                  <img src="https://i.pravatar.cc/32?img=4" class="w-8 h-8 rounded-full" alt="">
                  <span class="text-xs text-gray-500">สมหญิง ดี</span>
                </div>
                <span class="text-xs text-gray-400">8 มิ.ย. 2024</span>
              </div>
            </div>
          </article>

          <article class="bg-white rounded-xl overflow-hidden shadow-sm hover:shadow-md transition-shadow group">
            <div class="overflow-hidden">
              <img src="https://picsum.photos/400/220?random=4" alt="Article" class="w-full h-48 object-cover group-hover:scale-105 transition-transform duration-300">
            </div>
            <div class="p-5">
              <div class="flex items-center gap-2 mb-3">
                <span class="bg-orange-100 text-orange-700 text-xs font-medium px-2.5 py-1 rounded-full">Dark Mode</span>
                <span class="text-gray-400 text-xs">7 นาที</span>
              </div>
              <h3 class="font-bold text-lg mb-2 leading-snug group-hover:text-indigo-600 transition-colors">
                Dark Mode ด้วย CSS Variables และ Tailwind
              </h3>
              <p class="text-gray-500 text-sm leading-relaxed mb-4 line-clamp-2">
                วิธีสร้าง dark mode ที่ยืดหยุ่นด้วย CSS custom properties ร่วมกับ Tailwind config
              </p>
              <div class="flex items-center justify-between">
                <div class="flex items-center gap-2">
                  <img src="https://i.pravatar.cc/32?img=5" class="w-8 h-8 rounded-full" alt="">
                  <span class="text-xs text-gray-500">ประยุทธ นาค</span>
                </div>
                <span class="text-xs text-gray-400">5 มิ.ย. 2024</span>
              </div>
            </div>
          </article>
        </div>

        <!-- Load More -->
        <div class="text-center mt-8">
          <button class="border border-gray-300 text-gray-700 px-6 py-2.5 rounded-lg hover:bg-gray-50 transition-colors text-sm font-medium">
            โหลดบทความเพิ่ม
          </button>
        </div>
      </section>

      <!-- Sidebar -->
      <aside class="space-y-6">
        <!-- Search -->
        <div class="bg-white rounded-xl p-5 shadow-sm">
          <h3 class="font-bold mb-3">ค้นหา</h3>
          <div class="relative">
            <input type="text" placeholder="ค้นหาบทความ..." class="w-full border border-gray-200 rounded-lg pl-10 pr-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
            <svg class="absolute left-3 top-1/2 -translate-y-1/2 w-4 h-4 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"/>
            </svg>
          </div>
        </div>

        <!-- Categories -->
        <div class="bg-white rounded-xl p-5 shadow-sm">
          <h3 class="font-bold mb-4">หมวดหมู่</h3>
          <ul class="space-y-2">
            <li><a href="#" class="flex items-center justify-between text-sm text-gray-600 hover:text-indigo-600 transition-colors py-1">
              <span>Tailwind CSS</span>
              <span class="bg-gray-100 text-gray-500 text-xs px-2 py-0.5 rounded-full">24</span>
            </a></li>
            <li><a href="#" class="flex items-center justify-between text-sm text-gray-600 hover:text-indigo-600 transition-colors py-1">
              <span>JavaScript</span>
              <span class="bg-gray-100 text-gray-500 text-xs px-2 py-0.5 rounded-full">18</span>
            </a></li>
            <li><a href="#" class="flex items-center justify-between text-sm text-gray-600 hover:text-indigo-600 transition-colors py-1">
              <span>CSS Layout</span>
              <span class="bg-gray-100 text-gray-500 text-xs px-2 py-0.5 rounded-full">15</span>
            </a></li>
            <li><a href="#" class="flex items-center justify-between text-sm text-gray-600 hover:text-indigo-600 transition-colors py-1">
              <span>Performance</span>
              <span class="bg-gray-100 text-gray-500 text-xs px-2 py-0.5 rounded-full">9</span>
            </a></li>
            <li><a href="#" class="flex items-center justify-between text-sm text-gray-600 hover:text-indigo-600 transition-colors py-1">
              <span>Animation</span>
              <span class="bg-gray-100 text-gray-500 text-xs px-2 py-0.5 rounded-full">7</span>
            </a></li>
          </ul>
        </div>

        <!-- Popular Posts -->
        <div class="bg-white rounded-xl p-5 shadow-sm">
          <h3 class="font-bold mb-4">บทความยอดนิยม</h3>
          <div class="space-y-4">
            <a href="#" class="flex gap-3 group">
              <img src="https://picsum.photos/60/60?random=10" class="w-16 h-16 rounded-lg object-cover flex-shrink-0" alt="">
              <div>
                <p class="text-sm font-medium leading-snug group-hover:text-indigo-600 transition-colors line-clamp-2">
                  10 เทคนิค Tailwind ที่ Dev มืออาชีพใช้
                </p>
                <p class="text-xs text-gray-400 mt-1">2.4k views</p>
              </div>
            </a>
            <a href="#" class="flex gap-3 group">
              <img src="https://picsum.photos/60/60?random=11" class="w-16 h-16 rounded-lg object-cover flex-shrink-0" alt="">
              <div>
                <p class="text-sm font-medium leading-snug group-hover:text-indigo-600 transition-colors line-clamp-2">
                  สร้าง Component Library ด้วย Tailwind
                </p>
                <p class="text-xs text-gray-400 mt-1">1.8k views</p>
              </div>
            </a>
            <a href="#" class="flex gap-3 group">
              <img src="https://picsum.photos/60/60?random=12" class="w-16 h-16 rounded-lg object-cover flex-shrink-0" alt="">
              <div>
                <p class="text-sm font-medium leading-snug group-hover:text-indigo-600 transition-colors line-clamp-2">
                  Responsive Design Best Practices 2024
                </p>
                <p class="text-xs text-gray-400 mt-1">1.2k views</p>
              </div>
            </a>
          </div>
        </div>

        <!-- Newsletter -->
        <div class="bg-gradient-to-br from-indigo-600 to-purple-600 rounded-xl p-5 text-white">
          <h3 class="font-bold mb-2">รับบทความใหม่</h3>
          <p class="text-indigo-200 text-sm mb-4">สมัครรับ newsletter รายสัปดาห์ ฟรี!</p>
          <input type="email" placeholder="อีเมลของคุณ" class="w-full bg-white/10 border border-white/20 rounded-lg px-3 py-2 text-sm text-white placeholder-indigo-300 focus:outline-none focus:ring-2 focus:ring-white/50 mb-2">
          <button class="w-full bg-white text-indigo-700 font-semibold text-sm py-2 rounded-lg hover:bg-indigo-50 transition-colors">
            สมัครรับเลย
          </button>
        </div>

        <!-- Tags -->
        <div class="bg-white rounded-xl p-5 shadow-sm">
          <h3 class="font-bold mb-4">แท็ก</h3>
          <div class="flex flex-wrap gap-2">
            <a href="#" class="bg-gray-100 hover:bg-indigo-100 hover:text-indigo-700 text-gray-600 text-xs px-3 py-1.5 rounded-full transition-colors">tailwindcss</a>
            <a href="#" class="bg-gray-100 hover:bg-indigo-100 hover:text-indigo-700 text-gray-600 text-xs px-3 py-1.5 rounded-full transition-colors">css</a>
            <a href="#" class="bg-gray-100 hover:bg-indigo-100 hover:text-indigo-700 text-gray-600 text-xs px-3 py-1.5 rounded-full transition-colors">javascript</a>
            <a href="#" class="bg-gray-100 hover:bg-indigo-100 hover:text-indigo-700 text-gray-600 text-xs px-3 py-1.5 rounded-full transition-colors">webdev</a>
            <a href="#" class="bg-gray-100 hover:bg-indigo-100 hover:text-indigo-700 text-gray-600 text-xs px-3 py-1.5 rounded-full transition-colors">responsive</a>
            <a href="#" class="bg-gray-100 hover:bg-indigo-100 hover:text-indigo-700 text-gray-600 text-xs px-3 py-1.5 rounded-full transition-colors">darkmode</a>
            <a href="#" class="bg-gray-100 hover:bg-indigo-100 hover:text-indigo-700 text-gray-600 text-xs px-3 py-1.5 rounded-full transition-colors">animation</a>
            <a href="#" class="bg-gray-100 hover:bg-indigo-100 hover:text-indigo-700 text-gray-600 text-xs px-3 py-1.5 rounded-full transition-colors">grid</a>
          </div>
        </div>
      </aside>
    </div>
  </main>

</body>
</html>
```

---

## Step 352: Article Page Layout

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Article Page - Step 352</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 text-gray-900">

  <!-- Reading Progress Bar -->
  <div id="progress-bar" class="fixed top-0 left-0 h-1 bg-indigo-600 z-[100] transition-all duration-100" style="width: 0%"></div>

  <!-- Header -->
  <header class="bg-white border-b sticky top-0 z-50">
    <div class="max-w-6xl mx-auto px-4 py-3 flex items-center justify-between">
      <a href="#" class="text-xl font-bold text-indigo-600">DevBlog</a>
      <div class="flex items-center gap-3">
        <button class="flex items-center gap-2 text-sm text-gray-600 hover:text-gray-900">
          <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 5a2 2 0 012-2h10a2 2 0 012 2v16l-7-3.5L5 21V5z"/>
          </svg>
          บันทึก
        </button>
        <button class="flex items-center gap-2 text-sm bg-indigo-600 text-white px-3 py-1.5 rounded-lg hover:bg-indigo-700">
          <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8.684 13.342C8.886 12.938 9 12.482 9 12c0-.482-.114-.938-.316-1.342m0 2.684a3 3 0 110-2.684m0 2.684l6.632 3.316m-6.632-6l6.632-3.316m0 0a3 3 0 105.367-2.684 3 3 0 00-5.367 2.684zm0 9.316a3 3 0 105.368 2.684 3 3 0 00-5.368-2.684z"/>
          </svg>
          แชร์
        </button>
      </div>
    </div>
  </header>

  <div class="max-w-6xl mx-auto px-4 py-10">
    <div class="grid grid-cols-1 lg:grid-cols-[1fr_260px] gap-10">

      <!-- Article Content -->
      <article>
        <!-- Breadcrumb -->
        <nav class="flex items-center gap-2 text-sm text-gray-500 mb-6">
          <a href="#" class="hover:text-indigo-600">หน้าแรก</a>
          <span>›</span>
          <a href="#" class="hover:text-indigo-600">Tailwind CSS</a>
          <span>›</span>
          <span class="text-gray-700">การออกแบบ Design System</span>
        </nav>

        <!-- Category + Time -->
        <div class="flex items-center gap-3 mb-4">
          <span class="bg-indigo-100 text-indigo-700 text-xs font-semibold px-3 py-1 rounded-full">Tailwind CSS</span>
          <span class="text-gray-400 text-sm">12 นาทีอ่าน</span>
        </div>

        <!-- Title -->
        <h1 class="text-3xl md:text-4xl font-extrabold leading-tight mb-4">
          การออกแบบ Design System ด้วย Tailwind CSS สำหรับองค์กรขนาดใหญ่
        </h1>

        <!-- Subtitle -->
        <p class="text-xl text-gray-500 leading-relaxed mb-6">
          เรียนรู้วิธีสร้าง Design System ที่ scale ได้ดี ตั้งแต่ Color Token, Typography Scale ไปจนถึง Component Library ที่ทีมสามารถใช้งานร่วมกันได้
        </p>

        <!-- Author Row -->
        <div class="flex items-center gap-4 py-4 border-y mb-8">
          <img src="https://i.pravatar.cc/50?img=1" class="w-12 h-12 rounded-full" alt="">
          <div class="flex-1">
            <p class="font-semibold">สมชาย เทคโน</p>
            <p class="text-sm text-gray-500">Senior Frontend Developer · 15 มิ.ย. 2024</p>
          </div>
          <div class="flex items-center gap-3 text-sm text-gray-500">
            <span class="flex items-center gap-1">
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z M2.458 12C3.732 7.943 7.523 5 12 5c4.478 0 8.268 2.943 9.542 7-1.274 4.057-5.064 7-9.542 7-4.477 0-8.268-2.943-9.542-7z"/>
              </svg>
              2.4k
            </span>
            <span class="flex items-center gap-1">
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z"/>
              </svg>
              148
            </span>
          </div>
        </div>

        <!-- Cover Image -->
        <img src="https://picsum.photos/800/400?random=20" class="w-full rounded-xl mb-8" alt="Cover">

        <!-- Article Body (prose) -->
        <div class="prose prose-lg max-w-none prose-headings:font-bold prose-a:text-indigo-600">
          <h2>ทำไมต้องมี Design System?</h2>
          <p>ในองค์กรขนาดใหญ่ที่มีทีม Developer หลายทีม การที่แต่ละทีมสร้าง Component ขึ้นมาเองโดยไม่มีมาตรฐานกลาง ทำให้เกิดปัญหา...</p>

          <ul>
            <li>UI ไม่สม่ำเสมอในแต่ละส่วนของ Application</li>
            <li>เสียเวลา duplicate code ซ้ำซ้อน</li>
            <li>ยากต่อการ maintain และ update</li>
            <li>ทีม Design และ Dev ทำงานไม่ sync กัน</li>
          </ul>

          <h2>โครงสร้าง Design Token</h2>
          <p>Design Token คือค่าพื้นฐานของ Design System ได้แก่ สี, typography, spacing, shadow เป็นต้น เราจะกำหนดทั้งหมดนี้ผ่าน CSS Custom Properties...</p>

          <pre class="bg-gray-900 text-green-400 p-4 rounded-lg overflow-x-auto"><code>:root {
  --color-primary-50: #eef2ff;
  --color-primary-500: #6366f1;
  --color-primary-900: #312e81;
  
  --font-size-sm: 0.875rem;
  --font-size-base: 1rem;
  --font-size-lg: 1.125rem;
  
  --spacing-4: 1rem;
  --spacing-8: 2rem;
}</code></pre>

          <h2>การ Extend Tailwind Config</h2>
          <p>เราจะ map Design Token เข้ากับ Tailwind Config เพื่อให้ใช้งานได้ผ่าน utility class...</p>

          <blockquote>
            Design System ที่ดีไม่ใช่แค่ชุด Component — มันคือ "ภาษากลาง" ระหว่าง Designer และ Developer
          </blockquote>

          <h2>Component Architecture</h2>
          <p>เมื่อมี Token พร้อมแล้ว เราจะสร้าง Component ที่ compose จาก Token เหล่านี้...</p>
        </div>

        <!-- Tags -->
        <div class="flex flex-wrap gap-2 mt-8 pt-6 border-t">
          <span class="text-sm text-gray-500 mr-2">แท็ก:</span>
          <a href="#" class="bg-gray-100 text-gray-600 text-sm px-3 py-1 rounded-full hover:bg-indigo-100 hover:text-indigo-600 transition-colors">#tailwindcss</a>
          <a href="#" class="bg-gray-100 text-gray-600 text-sm px-3 py-1 rounded-full hover:bg-indigo-100 hover:text-indigo-600 transition-colors">#design-system</a>
          <a href="#" class="bg-gray-100 text-gray-600 text-sm px-3 py-1 rounded-full hover:bg-indigo-100 hover:text-indigo-600 transition-colors">#css</a>
        </div>

        <!-- Reactions -->
        <div class="flex items-center gap-4 mt-6 p-4 bg-gray-50 rounded-xl">
          <span class="text-sm text-gray-600 font-medium">บทความนี้เป็นประโยชน์ไหม?</span>
          <div class="flex items-center gap-2">
            <button class="flex items-center gap-1.5 bg-white border border-gray-200 text-sm px-3 py-1.5 rounded-lg hover:border-green-400 hover:text-green-600 transition-colors">
              👍 มีประโยชน์ (148)
            </button>
            <button class="flex items-center gap-1.5 bg-white border border-gray-200 text-sm px-3 py-1.5 rounded-lg hover:border-red-400 hover:text-red-600 transition-colors">
              👎 ปรับปรุงได้ (5)
            </button>
          </div>
        </div>

        <!-- Author Card -->
        <div class="mt-8 p-6 bg-white rounded-xl border flex gap-4">
          <img src="https://i.pravatar.cc/80?img=1" class="w-16 h-16 rounded-full flex-shrink-0" alt="">
          <div>
            <p class="font-bold text-lg">สมชาย เทคโน</p>
            <p class="text-gray-500 text-sm mb-3">Senior Frontend Developer ที่ Acme Corp รักการเขียน Blog เกี่ยวกับ Web Development มากกว่า 8 ปี</p>
            <button class="bg-indigo-600 text-white text-sm px-4 py-1.5 rounded-lg hover:bg-indigo-700 transition-colors">
              ติดตาม
            </button>
          </div>
        </div>
      </article>

      <!-- Sticky TOC Sidebar -->
      <aside>
        <div class="sticky top-20 space-y-4">
          <div class="bg-white rounded-xl p-4 shadow-sm">
            <h3 class="font-bold text-sm text-gray-500 uppercase tracking-wide mb-3">สารบัญ</h3>
            <nav class="space-y-1" id="toc">
              <a href="#" class="block text-sm text-indigo-600 bg-indigo-50 px-3 py-1.5 rounded-lg font-medium">ทำไมต้องมี Design System?</a>
              <a href="#" class="block text-sm text-gray-600 hover:text-indigo-600 px-3 py-1.5 rounded-lg transition-colors">โครงสร้าง Design Token</a>
              <a href="#" class="block text-sm text-gray-600 hover:text-indigo-600 px-3 py-1.5 rounded-lg transition-colors">การ Extend Tailwind Config</a>
              <a href="#" class="block text-sm text-gray-600 hover:text-indigo-600 px-3 py-1.5 rounded-lg transition-colors">Component Architecture</a>
              <a href="#" class="block text-sm text-gray-600 hover:text-indigo-600 px-3 py-1.5 rounded-lg transition-colors">Workshop: สร้าง Token</a>
            </nav>
          </div>

          <!-- Share -->
          <div class="bg-white rounded-xl p-4 shadow-sm">
            <h3 class="font-bold text-sm text-gray-500 uppercase tracking-wide mb-3">แชร์บทความ</h3>
            <div class="flex gap-2">
              <button class="flex-1 flex items-center justify-center gap-2 bg-[#1DA1F2] text-white text-xs py-2 rounded-lg hover:opacity-90 transition-opacity">
                Twitter
              </button>
              <button class="flex-1 flex items-center justify-center gap-2 bg-[#4267B2] text-white text-xs py-2 rounded-lg hover:opacity-90 transition-opacity">
                Facebook
              </button>
              <button class="flex-1 flex items-center justify-center gap-2 bg-gray-800 text-white text-xs py-2 rounded-lg hover:opacity-90 transition-opacity">
                Copy
              </button>
            </div>
          </div>
        </div>
      </aside>
    </div>
  </div>

  <script>
    // Reading Progress Bar
    window.addEventListener('scroll', () => {
      const scrolled = window.scrollY;
      const total = document.body.scrollHeight - window.innerHeight;
      const progress = (scrolled / total) * 100;
      document.getElementById('progress-bar').style.width = progress + '%';
    });
  </script>

</body>
</html>
```

---

## Step 353: List-Style Blog (Horizontal Cards)

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Blog List - Step 353</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-white text-gray-900">

  <div class="max-w-3xl mx-auto px-4 py-12">
    <h1 class="text-3xl font-bold mb-2">บทความทั้งหมด</h1>
    <p class="text-gray-500 mb-8">อ่านบทความล่าสุดเกี่ยวกับ Tailwind CSS และ Web Development</p>

    <!-- Filter Tabs -->
    <div class="flex gap-2 mb-8 border-b">
      <button class="pb-3 px-1 text-sm font-semibold text-indigo-600 border-b-2 border-indigo-600">ทั้งหมด</button>
      <button class="pb-3 px-1 text-sm font-medium text-gray-500 hover:text-gray-800 border-b-2 border-transparent">Tailwind CSS</button>
      <button class="pb-3 px-1 text-sm font-medium text-gray-500 hover:text-gray-800 border-b-2 border-transparent">JavaScript</button>
      <button class="pb-3 px-1 text-sm font-medium text-gray-500 hover:text-gray-800 border-b-2 border-transparent">Design</button>
    </div>

    <!-- Horizontal Article Cards -->
    <div class="divide-y">
      <!-- Article Row -->
      <article class="py-6 flex gap-5 group">
        <div class="flex-1 min-w-0">
          <div class="flex items-center gap-2 mb-2">
            <img src="https://i.pravatar.cc/24?img=1" class="w-6 h-6 rounded-full" alt="">
            <span class="text-xs text-gray-500">สมชาย เทคโน</span>
            <span class="text-gray-300">·</span>
            <span class="text-xs text-gray-500">15 มิ.ย.</span>
          </div>
          <h2 class="font-bold text-lg mb-2 leading-snug group-hover:text-indigo-600 transition-colors">
            การออกแบบ Design System ด้วย Tailwind CSS
          </h2>
          <p class="text-gray-500 text-sm leading-relaxed line-clamp-2 mb-3">
            เรียนรู้วิธีสร้าง Design System ที่ scale ได้ดี ตั้งแต่ Color Token ไปจนถึง Component Library
          </p>
          <div class="flex items-center gap-3">
            <span class="bg-blue-100 text-blue-700 text-xs px-2.5 py-1 rounded-full">Design System</span>
            <span class="text-xs text-gray-400 flex items-center gap-1">
              <svg class="w-3 h-3" fill="currentColor" viewBox="0 0 20 20"><path d="M9 4.804A7.968 7.968 0 005.5 4c-1.255 0-2.443.29-3.5.804v10A7.969 7.969 0 015.5 14c1.669 0 3.218.51 4.5 1.385A7.962 7.962 0 0114.5 14c1.255 0 2.443.29 3.5.804v-10A7.968 7.968 0 0014.5 4c-1.255 0-2.443.29-3.5.804V12a1 1 0 11-2 0V4.804z"/></svg>
              12 นาที
            </span>
          </div>
        </div>
        <img src="https://picsum.photos/120/80?random=20" class="w-24 h-20 md:w-32 md:h-24 rounded-lg object-cover flex-shrink-0" alt="">
      </article>

      <article class="py-6 flex gap-5 group">
        <div class="flex-1 min-w-0">
          <div class="flex items-center gap-2 mb-2">
            <img src="https://i.pravatar.cc/24?img=2" class="w-6 h-6 rounded-full" alt="">
            <span class="text-xs text-gray-500">วิไล สวัสดี</span>
            <span class="text-gray-300">·</span>
            <span class="text-xs text-gray-500">12 มิ.ย.</span>
          </div>
          <h2 class="font-bold text-lg mb-2 leading-snug group-hover:text-indigo-600 transition-colors">
            Flexbox vs Grid: เมื่อไหร่ควรใช้อะไร?
          </h2>
          <p class="text-gray-500 text-sm leading-relaxed line-clamp-2 mb-3">
            การเลือกใช้ Flexbox หรือ CSS Grid ให้เหมาะสมกับสถานการณ์ช่วยให้ Layout แข็งแกร่งและง่ายต่อการ maintain
          </p>
          <div class="flex items-center gap-3">
            <span class="bg-purple-100 text-purple-700 text-xs px-2.5 py-1 rounded-full">CSS Layout</span>
            <span class="text-xs text-gray-400">8 นาที</span>
          </div>
        </div>
        <img src="https://picsum.photos/120/80?random=21" class="w-24 h-20 md:w-32 md:h-24 rounded-lg object-cover flex-shrink-0" alt="">
      </article>

      <article class="py-6 flex gap-5 group">
        <div class="flex-1 min-w-0">
          <div class="flex items-center gap-2 mb-2">
            <img src="https://i.pravatar.cc/24?img=3" class="w-6 h-6 rounded-full" alt="">
            <span class="text-xs text-gray-500">ธนพล รัตน์</span>
            <span class="text-gray-300">·</span>
            <span class="text-xs text-gray-500">10 มิ.ย.</span>
          </div>
          <h2 class="font-bold text-lg mb-2 leading-snug group-hover:text-indigo-600 transition-colors">
            Optimize Tailwind CSS Build ให้เล็กที่สุด
          </h2>
          <p class="text-gray-500 text-sm leading-relaxed line-clamp-2 mb-3">
            เทคนิคการ purge CSS, JIT mode และการตั้งค่า content paths ให้ถูกต้องเพื่อลดขนาด bundle
          </p>
          <div class="flex items-center gap-3">
            <span class="bg-green-100 text-green-700 text-xs px-2.5 py-1 rounded-full">Performance</span>
            <span class="text-xs text-gray-400">6 นาที</span>
          </div>
        </div>
        <img src="https://picsum.photos/120/80?random=22" class="w-24 h-20 md:w-32 md:h-24 rounded-lg object-cover flex-shrink-0" alt="">
      </article>
    </div>
  </div>
</body>
</html>
```

---

## Step 354: Category Page with Masonry

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Category Masonry - Step 354</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 text-gray-900 p-8">

  <div class="max-w-6xl mx-auto">
    <h1 class="text-2xl font-bold mb-6">หมวดหมู่: Tailwind CSS</h1>

    <!-- Masonry via CSS columns -->
    <div class="columns-1 sm:columns-2 lg:columns-3 gap-5 space-y-5">

      <!-- Short card -->
      <article class="break-inside-avoid bg-white rounded-xl p-5 shadow-sm hover:shadow-md transition-shadow">
        <span class="bg-indigo-100 text-indigo-700 text-xs font-medium px-2.5 py-1 rounded-full">Quick Tip</span>
        <h3 class="font-bold mt-3 mb-2 hover:text-indigo-600 cursor-pointer">Arbitrary Values ใน Tailwind</h3>
        <p class="text-sm text-gray-500">ใช้ square brackets เพื่อกำหนดค่าที่ไม่มีใน preset เช่น <code class="bg-gray-100 px-1 rounded">w-[250px]</code></p>
        <div class="flex items-center justify-between mt-4">
          <span class="text-xs text-gray-400">3 นาที</span>
          <span class="text-xs text-indigo-600">อ่านต่อ →</span>
        </div>
      </article>

      <!-- Card with image (tall) -->
      <article class="break-inside-avoid bg-white rounded-xl overflow-hidden shadow-sm hover:shadow-md transition-shadow">
        <img src="https://picsum.photos/400/250?random=30" class="w-full object-cover" alt="">
        <div class="p-5">
          <span class="bg-blue-100 text-blue-700 text-xs font-medium px-2.5 py-1 rounded-full">Advanced</span>
          <h3 class="font-bold mt-3 mb-2 hover:text-indigo-600 cursor-pointer">สร้าง Plugin ใน Tailwind CSS</h3>
          <p class="text-sm text-gray-500 line-clamp-3">เรียนรู้การสร้าง custom plugin ด้วย addBase, addComponents, addUtilities และ matchUtilities</p>
          <div class="flex items-center justify-between mt-4">
            <div class="flex items-center gap-2">
              <img src="https://i.pravatar.cc/24?img=5" class="w-6 h-6 rounded-full" alt="">
              <span class="text-xs text-gray-500">สมหญิง</span>
            </div>
            <span class="text-xs text-gray-400">15 นาที</span>
          </div>
        </div>
      </article>

      <!-- Medium card -->
      <article class="break-inside-avoid bg-white rounded-xl p-5 shadow-sm hover:shadow-md transition-shadow">
        <div class="flex items-start gap-3">
          <div class="w-12 h-12 bg-gradient-to-br from-indigo-500 to-purple-500 rounded-xl flex items-center justify-center flex-shrink-0">
            <span class="text-2xl">🎨</span>
          </div>
          <div>
            <span class="bg-purple-100 text-purple-700 text-xs font-medium px-2 py-0.5 rounded-full">Design</span>
            <h3 class="font-bold mt-1 mb-1 text-sm hover:text-indigo-600 cursor-pointer">Color System ด้วย CSS Variables</h3>
            <p class="text-xs text-gray-500">จัดการ color palette ผ่าน custom properties</p>
          </div>
        </div>
        <div class="mt-4 pt-4 border-t flex justify-between text-xs text-gray-400">
          <span>8 มิ.ย. 2024</span>
          <span>7 นาที</span>
        </div>
      </article>

      <!-- Code snippet card -->
      <article class="break-inside-avoid bg-gray-900 rounded-xl p-5 shadow-sm">
        <div class="flex items-center justify-between mb-3">
          <span class="bg-yellow-500 text-black text-xs font-semibold px-2.5 py-1 rounded-full">Snippet</span>
          <button class="text-gray-400 hover:text-white text-xs">Copy</button>
        </div>
        <h3 class="text-white font-bold mb-2">Responsive Grid Snippet</h3>
        <pre class="text-green-400 text-xs overflow-x-auto"><code>grid 
grid-cols-1 
sm:grid-cols-2 
lg:grid-cols-3 
gap-4</code></pre>
        <p class="text-gray-400 text-xs mt-3">3 column responsive grid ที่ใช้บ่อยที่สุด</p>
      </article>

      <!-- Long card with image -->
      <article class="break-inside-avoid bg-white rounded-xl overflow-hidden shadow-sm hover:shadow-md transition-shadow">
        <img src="https://picsum.photos/400/180?random=31" class="w-full object-cover" alt="">
        <div class="p-5">
          <span class="bg-orange-100 text-orange-700 text-xs font-medium px-2.5 py-1 rounded-full">Tutorial</span>
          <h3 class="font-bold mt-3 mb-2 hover:text-indigo-600 cursor-pointer">Dark Mode ที่สมบูรณ์แบบด้วย Tailwind v3</h3>
          <p class="text-sm text-gray-500 leading-relaxed">สร้าง dark mode ที่ใช้ CSS custom properties ร่วมกับ class strategy เพื่อความยืดหยุ่นสูงสุด รองรับทั้ง system preference และ manual toggle</p>
          <div class="flex items-center gap-4 mt-4 text-xs text-gray-400">
            <span>💙 234 likes</span>
            <span>💬 18 comments</span>
            <span>🔖 89 saves</span>
          </div>
        </div>
      </article>

      <!-- Plain text card -->
      <article class="break-inside-avoid bg-white rounded-xl p-5 shadow-sm">
        <p class="text-4xl mb-3">⚡</p>
        <h3 class="font-bold mb-2 hover:text-indigo-600 cursor-pointer">JIT Mode: ทุกอย่างเร็วขึ้น 10x</h3>
        <p class="text-sm text-gray-500">Just-in-Time mode เปลี่ยนวิธีที่ Tailwind generate CSS ทำให้ dev experience ดีขึ้นอย่างมาก</p>
        <div class="mt-4 flex items-center gap-2">
          <span class="bg-yellow-100 text-yellow-700 text-xs px-2.5 py-1 rounded-full">Performance</span>
          <span class="text-xs text-gray-400">4 นาที</span>
        </div>
      </article>

    </div>
  </div>
</body>
</html>
```

---

## Step 355: Comments Section

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Comments - Step 355</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-8">

  <div class="max-w-2xl mx-auto">
    <h2 class="text-xl font-bold mb-6">ความคิดเห็น (12)</h2>

    <!-- Comment Form -->
    <div class="bg-white rounded-xl p-5 mb-6 shadow-sm">
      <div class="flex gap-3">
        <img src="https://i.pravatar.cc/40?img=10" class="w-10 h-10 rounded-full flex-shrink-0" alt="">
        <div class="flex-1">
          <textarea rows="3" placeholder="เขียนความคิดเห็น..." class="w-full border border-gray-200 rounded-xl p-3 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 resize-none"></textarea>
          <div class="flex justify-between items-center mt-2">
            <div class="flex gap-2 text-gray-400">
              <button class="hover:text-gray-600 p-1">😊</button>
              <button class="hover:text-gray-600 p-1">
                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13.828 10.172a4 4 0 00-5.656 0l-4 4a4 4 0 105.656 5.656l1.102-1.101m-.758-4.899a4 4 0 005.656 0l4-4a4 4 0 00-5.656-5.656l-1.1 1.1"/>
                </svg>
              </button>
            </div>
            <button class="bg-indigo-600 text-white text-sm px-4 py-1.5 rounded-lg hover:bg-indigo-700 transition-colors">
              โพสต์
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Comments List -->
    <div class="space-y-4">

      <!-- Comment with Replies -->
      <div class="bg-white rounded-xl p-5 shadow-sm">
        <div class="flex gap-3">
          <img src="https://i.pravatar.cc/40?img=2" class="w-10 h-10 rounded-full flex-shrink-0" alt="">
          <div class="flex-1">
            <div class="flex items-center justify-between mb-1">
              <div class="flex items-center gap-2">
                <span class="font-semibold text-sm">วิไล สวัสดี</span>
                <span class="bg-indigo-100 text-indigo-700 text-xs px-2 py-0.5 rounded-full">Author</span>
              </div>
              <span class="text-xs text-gray-400">2 ชั่วโมงที่แล้ว</span>
            </div>
            <p class="text-sm text-gray-700 leading-relaxed">
              บทความนี้ดีมากครับ! ผมเพิ่งลองทำ Design System ด้วย Tailwind ตามที่อธิบายแล้ว ทีมชอบมากเลย 😊
            </p>
            <div class="flex items-center gap-4 mt-3">
              <button class="flex items-center gap-1.5 text-xs text-gray-500 hover:text-red-500 transition-colors">
                ❤️ 12
              </button>
              <button class="text-xs text-gray-500 hover:text-indigo-600 transition-colors font-medium">
                ตอบกลับ
              </button>
            </div>

            <!-- Reply -->
            <div class="mt-4 pl-4 border-l-2 border-gray-100">
              <div class="flex gap-3">
                <img src="https://i.pravatar.cc/32?img=1" class="w-8 h-8 rounded-full flex-shrink-0" alt="">
                <div class="flex-1">
                  <div class="flex items-center justify-between mb-1">
                    <span class="font-semibold text-sm">สมชาย เทคโน</span>
                    <span class="text-xs text-gray-400">1 ชั่วโมงที่แล้ว</span>
                  </div>
                  <p class="text-sm text-gray-700">
                    ขอบคุณมากครับ ดีใจที่บทความเป็นประโยชน์! ถ้ามีคำถามอะไรเพิ่มเติมถามได้เลยนะครับ 🙏
                  </p>
                  <button class="flex items-center gap-1.5 text-xs text-gray-500 hover:text-red-500 transition-colors mt-2">
                    ❤️ 5
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="bg-white rounded-xl p-5 shadow-sm">
        <div class="flex gap-3">
          <img src="https://i.pravatar.cc/40?img=4" class="w-10 h-10 rounded-full flex-shrink-0" alt="">
          <div class="flex-1">
            <div class="flex items-center justify-between mb-1">
              <span class="font-semibold text-sm">ธนพล รัตน์</span>
              <span class="text-xs text-gray-400">5 ชั่วโมงที่แล้ว</span>
            </div>
            <p class="text-sm text-gray-700 leading-relaxed">
              ขอถามเพิ่มเติมหน่อยครับ ถ้าต้องการ support หลาย brand ใน Design System เดียวกัน มีวิธีที่ดีกว่าการใช้ CSS variables ไหมครับ?
            </p>
            <div class="flex items-center gap-4 mt-3">
              <button class="flex items-center gap-1.5 text-xs text-gray-500 hover:text-red-500 transition-colors">❤️ 3</button>
              <button class="text-xs text-gray-500 hover:text-indigo-600 transition-colors font-medium">ตอบกลับ</button>
            </div>
          </div>
        </div>
      </div>

    </div>

    <button class="w-full mt-4 text-sm text-indigo-600 hover:text-indigo-800 py-3 border border-dashed border-gray-300 rounded-xl hover:bg-gray-50 transition-colors">
      โหลดความคิดเห็นเพิ่ม (10 เหลือ)
    </button>
  </div>
</body>
</html>
```

---

## Step 356: Newsletter Landing Inline

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Newsletter CTA - Step 356</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-white p-8">

  <div class="max-w-2xl mx-auto space-y-6">

    <!-- Inline CTA Banner -->
    <div class="bg-gradient-to-r from-indigo-600 to-purple-600 rounded-2xl p-8 text-white text-center">
      <div class="text-3xl mb-3">📬</div>
      <h2 class="text-2xl font-bold mb-2">รับบทความใหม่ทุกสัปดาห์</h2>
      <p class="text-indigo-200 mb-6">เข้าร่วมกับนักพัฒนา 5,000+ คนที่รับ newsletter สาระ CSS และ Web Dev ทุกวันจันทร์</p>
      <div class="flex gap-2 max-w-sm mx-auto">
        <input type="email" placeholder="your@email.com" class="flex-1 px-4 py-2.5 rounded-xl text-gray-900 text-sm focus:outline-none">
        <button class="bg-white text-indigo-700 font-semibold px-4 py-2.5 rounded-xl hover:bg-indigo-50 transition-colors whitespace-nowrap text-sm">
          สมัครเลย
        </button>
      </div>
      <p class="text-indigo-300 text-xs mt-3">ยกเลิกได้ทุกเมื่อ · ไม่มี spam</p>
    </div>

    <!-- Minimal Inline -->
    <div class="border border-gray-200 rounded-2xl p-6 flex flex-col sm:flex-row items-center gap-4">
      <div class="flex-1 text-center sm:text-left">
        <p class="font-bold text-lg">สมัคร DevBlog Weekly</p>
        <p class="text-gray-500 text-sm mt-1">บทความ Tailwind CSS เลือกสรรทุกสัปดาห์</p>
      </div>
      <div class="flex gap-2 w-full sm:w-auto">
        <input type="email" placeholder="อีเมล..." class="flex-1 sm:w-52 border border-gray-300 rounded-xl px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
        <button class="bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm hover:bg-indigo-700 transition-colors whitespace-nowrap">
          สมัคร
        </button>
      </div>
    </div>

  </div>
</body>
</html>
```

---

## Step 357: Pagination Component

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Pagination - Step 357</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-white p-12 flex flex-col items-center gap-8">

  <!-- Standard Pagination -->
  <nav class="flex items-center gap-1">
    <button class="w-9 h-9 flex items-center justify-center rounded-lg border text-gray-500 hover:bg-gray-50 transition-colors">
      ←
    </button>
    <button class="w-9 h-9 flex items-center justify-center rounded-lg text-sm text-gray-600 hover:bg-gray-50 transition-colors">1</button>
    <button class="w-9 h-9 flex items-center justify-center rounded-lg text-sm text-gray-600 hover:bg-gray-50 transition-colors">2</button>
    <button class="w-9 h-9 flex items-center justify-center rounded-lg text-sm bg-indigo-600 text-white font-semibold">3</button>
    <button class="w-9 h-9 flex items-center justify-center rounded-lg text-sm text-gray-600 hover:bg-gray-50 transition-colors">4</button>
    <span class="w-9 h-9 flex items-center justify-center text-gray-400">...</span>
    <button class="w-9 h-9 flex items-center justify-center rounded-lg text-sm text-gray-600 hover:bg-gray-50 transition-colors">12</button>
    <button class="w-9 h-9 flex items-center justify-center rounded-lg border text-gray-500 hover:bg-gray-50 transition-colors">
      →
    </button>
  </nav>

  <!-- Simple Prev/Next -->
  <div class="flex items-center gap-4">
    <a href="#" class="flex items-center gap-2 text-sm text-gray-600 hover:text-indigo-600 transition-colors group">
      <svg class="w-4 h-4 group-hover:-translate-x-1 transition-transform" fill="none" stroke="currentColor" viewBox="0 0 24 24">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7"/>
      </svg>
      บทความก่อนหน้า
    </a>
    <span class="text-gray-200">|</span>
    <a href="#" class="flex items-center gap-2 text-sm text-gray-600 hover:text-indigo-600 transition-colors group">
      บทความถัดไป
      <svg class="w-4 h-4 group-hover:translate-x-1 transition-transform" fill="none" stroke="currentColor" viewBox="0 0 24 24">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"/>
      </svg>
    </a>
  </div>

  <!-- Article Navigation Cards -->
  <div class="grid grid-cols-2 gap-4 w-full max-w-2xl">
    <a href="#" class="border border-gray-200 rounded-xl p-4 hover:border-indigo-300 hover:bg-indigo-50 transition-all group">
      <p class="text-xs text-gray-400 mb-2">← ก่อนหน้า</p>
      <p class="font-semibold text-sm group-hover:text-indigo-600 transition-colors line-clamp-2">
        Flexbox vs Grid เมื่อไหร่ควรใช้อะไร
      </p>
    </a>
    <a href="#" class="border border-gray-200 rounded-xl p-4 hover:border-indigo-300 hover:bg-indigo-50 transition-all text-right group">
      <p class="text-xs text-gray-400 mb-2">ถัดไป →</p>
      <p class="font-semibold text-sm group-hover:text-indigo-600 transition-colors line-clamp-2">
        Optimize Tailwind Build ให้เล็กที่สุด
      </p>
    </a>
  </div>

</body>
</html>
```

---

## Step 358: Related Articles

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Related Articles - Step 358</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-8">

  <div class="max-w-4xl mx-auto">
    <h2 class="text-xl font-bold mb-6">บทความที่เกี่ยวข้อง</h2>

    <div class="grid grid-cols-1 md:grid-cols-3 gap-5">
      <article class="bg-white rounded-xl overflow-hidden shadow-sm hover:shadow-md transition-shadow group">
        <img src="https://picsum.photos/400/200?random=40" class="w-full h-40 object-cover group-hover:scale-105 transition-transform duration-300" alt="">
        <div class="p-4">
          <span class="text-xs text-indigo-600 font-semibold">Tailwind CSS</span>
          <h3 class="font-bold text-sm mt-1 mb-2 group-hover:text-indigo-600 transition-colors line-clamp-2">
            Custom Plugin ขั้นสูงด้วย matchUtilities
          </h3>
          <p class="text-xs text-gray-400">สมหญิง ดี · 5 นาที</p>
        </div>
      </article>

      <article class="bg-white rounded-xl overflow-hidden shadow-sm hover:shadow-md transition-shadow group">
        <img src="https://picsum.photos/400/200?random=41" class="w-full h-40 object-cover group-hover:scale-105 transition-transform duration-300" alt="">
        <div class="p-4">
          <span class="text-xs text-purple-600 font-semibold">Design System</span>
          <h3 class="font-bold text-sm mt-1 mb-2 group-hover:text-indigo-600 transition-colors line-clamp-2">
            Semantic Color System ที่ทีมเข้าใจได้ง่าย
          </h3>
          <p class="text-xs text-gray-400">ธนพล รัตน์ · 8 นาที</p>
        </div>
      </article>

      <article class="bg-white rounded-xl overflow-hidden shadow-sm hover:shadow-md transition-shadow group">
        <img src="https://picsum.photos/400/200?random=42" class="w-full h-40 object-cover group-hover:scale-105 transition-transform duration-300" alt="">
        <div class="p-4">
          <span class="text-xs text-green-600 font-semibold">Performance</span>
          <h3 class="font-bold text-sm mt-1 mb-2 group-hover:text-indigo-600 transition-colors line-clamp-2">
            Tree-shaking CSS ใน Production Build
          </h3>
          <p class="text-xs text-gray-400">วิไล สวัสดี · 6 นาที</p>
        </div>
      </article>
    </div>
  </div>
</body>
</html>
```

---

## Step 359: Blog Search Results

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Search Results - Step 359</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-8">

  <div class="max-w-2xl mx-auto">
    <!-- Search Bar -->
    <div class="relative mb-6">
      <input type="text" value="design system" class="w-full bg-white border border-gray-200 rounded-xl pl-11 pr-4 py-3 text-sm shadow-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
      <svg class="absolute left-4 top-1/2 -translate-y-1/2 w-4 h-4 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"/>
      </svg>
    </div>

    <p class="text-sm text-gray-500 mb-4">พบ <strong class="text-gray-800">8 ผลลัพธ์</strong> สำหรับ "<strong class="text-indigo-600">design system</strong>"</p>

    <div class="space-y-3">
      <div class="bg-white rounded-xl p-4 shadow-sm hover:shadow-md transition-shadow cursor-pointer group">
        <span class="text-xs text-indigo-500 font-medium">Tailwind CSS › Design</span>
        <h3 class="font-bold mt-1 mb-1 group-hover:text-indigo-600 transition-colors">
          การออกแบบ <mark class="bg-yellow-100 text-yellow-800 px-1 rounded">Design System</mark> ด้วย Tailwind CSS
        </h3>
        <p class="text-sm text-gray-500 line-clamp-2">เรียนรู้วิธีสร้าง <mark class="bg-yellow-100 text-yellow-800 px-0.5 rounded">Design System</mark> ที่ scale ได้ดี ตั้งแต่ Color Token...</p>
        <p class="text-xs text-gray-400 mt-2">15 มิ.ย. 2024 · 12 นาที</p>
      </div>

      <div class="bg-white rounded-xl p-4 shadow-sm hover:shadow-md transition-shadow cursor-pointer group">
        <span class="text-xs text-purple-500 font-medium">Design System › Advanced</span>
        <h3 class="font-bold mt-1 mb-1 group-hover:text-indigo-600 transition-colors">
          Semantic Color ใน <mark class="bg-yellow-100 text-yellow-800 px-1 rounded">Design System</mark>
        </h3>
        <p class="text-sm text-gray-500 line-clamp-2">การจัดการ color palette ผ่าน CSS custom properties สำหรับ <mark class="bg-yellow-100 text-yellow-800 px-0.5 rounded">design system</mark> ที่ยืดหยุ่น</p>
        <p class="text-xs text-gray-400 mt-2">10 มิ.ย. 2024 · 8 นาที</p>
      </div>

      <div class="bg-white rounded-xl p-4 shadow-sm hover:shadow-md transition-shadow cursor-pointer group">
        <span class="text-xs text-green-500 font-medium">Component Library</span>
        <h3 class="font-bold mt-1 mb-1 group-hover:text-indigo-600 transition-colors">
          สร้าง Component Library สำหรับ <mark class="bg-yellow-100 text-yellow-800 px-1 rounded">Design System</mark>
        </h3>
        <p class="text-sm text-gray-500 line-clamp-2">ขั้นตอนการสร้าง reusable component ที่เป็นมาตรฐานด้วย Tailwind และ CVA pattern</p>
        <p class="text-xs text-gray-400 mt-2">5 มิ.ย. 2024 · 10 นาที</p>
      </div>
    </div>
  </div>
</body>
</html>
```

---

## Step 360: Blog Workshop สมบูรณ์

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>DevBlog - Full Workshop Step 360</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    .prose h2 { font-size: 1.5rem; font-weight: 700; margin: 2rem 0 1rem; color: #1e293b; }
    .prose h3 { font-size: 1.125rem; font-weight: 600; margin: 1.5rem 0 0.75rem; color: #334155; }
    .prose p { margin-bottom: 1rem; line-height: 1.8; color: #475569; }
    .prose ul { margin-bottom: 1rem; padding-left: 1.5rem; list-style: disc; color: #475569; }
    .prose li { margin-bottom: 0.5rem; line-height: 1.7; }
    .prose blockquote { border-left: 4px solid #6366f1; padding-left: 1rem; color: #6366f1; font-style: italic; margin: 1.5rem 0; }
    .prose pre { background: #1e293b; color: #a3e635; padding: 1rem; border-radius: 0.75rem; overflow-x: auto; margin: 1rem 0; font-size: 0.875rem; }
    .prose code { background: #f1f5f9; color: #6366f1; padding: 0.1em 0.4em; border-radius: 0.25rem; font-size: 0.875em; }
  </style>
</head>
<body class="bg-gray-50 text-gray-900">

  <!-- Reading Progress -->
  <div id="progress" class="fixed top-0 left-0 h-1 bg-indigo-600 z-[100] transition-[width] duration-75" style="width:0%"></div>

  <!-- Header -->
  <header class="bg-white border-b sticky top-0 z-50 shadow-sm">
    <div class="max-w-6xl mx-auto px-4 py-3 flex items-center justify-between">
      <a href="#" class="text-xl font-extrabold text-indigo-600 tracking-tight">DevBlog</a>
      <nav class="hidden md:flex items-center gap-6 text-sm font-medium text-gray-600">
        <a href="#" class="hover:text-indigo-600 transition-colors">หน้าแรก</a>
        <a href="#" class="hover:text-indigo-600 transition-colors">หมวดหมู่</a>
        <a href="#" class="text-indigo-600 border-b border-indigo-600">บทความ</a>
      </nav>
      <div class="flex items-center gap-2">
        <button onclick="toggleSearch()" class="p-2 text-gray-500 hover:text-gray-900 hover:bg-gray-100 rounded-lg transition-colors">
          <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"/>
          </svg>
        </button>
        <button class="bg-indigo-600 text-white text-sm px-4 py-2 rounded-lg hover:bg-indigo-700 transition-colors">
          Subscribe
        </button>
      </div>
    </div>

    <!-- Search Bar (hidden) -->
    <div id="search-bar" class="hidden border-t px-4 py-3">
      <div class="max-w-2xl mx-auto relative">
        <input type="text" id="search-input" placeholder="ค้นหาบทความ..." oninput="handleSearch(this.value)"
          class="w-full border border-gray-200 rounded-xl pl-10 pr-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
        <svg class="absolute left-3 top-1/2 -translate-y-1/2 w-4 h-4 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"/>
        </svg>
      </div>
      <!-- Live Results -->
      <div id="search-results" class="max-w-2xl mx-auto mt-2 hidden">
        <div class="bg-white rounded-xl shadow-lg border border-gray-100 overflow-hidden">
          <div class="p-3 border-b bg-gray-50 text-xs text-gray-500 font-medium">ผลการค้นหา</div>
          <div id="results-list" class="divide-y max-h-64 overflow-y-auto"></div>
        </div>
      </div>
    </div>
  </header>

  <!-- Homepage or Article view -->
  <div id="homepage" class="block">
    <!-- Hero -->
    <section class="bg-gradient-to-br from-indigo-900 via-purple-900 to-indigo-900 text-white py-16 px-4">
      <div class="max-w-4xl mx-auto text-center">
        <span class="inline-block bg-indigo-500/30 border border-indigo-400/30 text-indigo-300 text-xs font-semibold px-3 py-1 rounded-full mb-6">
          🎉 บทความใหม่ทุกสัปดาห์
        </span>
        <h1 class="text-4xl md:text-5xl font-extrabold mb-4 leading-tight">
          เรียนรู้ Tailwind CSS<br>
          <span class="text-indigo-400">อย่างมืออาชีพ</span>
        </h1>
        <p class="text-indigo-200 text-lg mb-8 max-w-xl mx-auto">
          บทความเชิงปฏิบัติจากนักพัฒนา Senior ที่ใช้งานจริง
        </p>
        <div class="flex flex-col sm:flex-row gap-3 max-w-sm mx-auto">
          <input type="email" placeholder="your@email.com" class="flex-1 px-4 py-2.5 rounded-xl text-gray-900 text-sm focus:outline-none">
          <button class="bg-indigo-500 text-white font-semibold px-5 py-2.5 rounded-xl hover:bg-indigo-400 transition-colors whitespace-nowrap text-sm">
            Subscribe ฟรี
          </button>
        </div>
        <p class="text-indigo-400 text-xs mt-3">เข้าร่วมกับ 5,247 นักพัฒนา</p>
      </div>
    </section>

    <div class="max-w-6xl mx-auto px-4 py-10">
      <div class="grid grid-cols-1 lg:grid-cols-[1fr_300px] gap-10">

        <!-- Articles -->
        <div>
          <!-- Category Filter -->
          <div class="flex gap-2 mb-6 overflow-x-auto pb-2 scrollbar-hide">
            <button onclick="filterArticles('all', this)" class="flex-shrink-0 bg-indigo-600 text-white text-sm px-4 py-1.5 rounded-full font-medium transition-colors">ทั้งหมด</button>
            <button onclick="filterArticles('tailwind', this)" class="flex-shrink-0 bg-gray-100 text-gray-600 text-sm px-4 py-1.5 rounded-full hover:bg-gray-200 transition-colors">Tailwind CSS</button>
            <button onclick="filterArticles('layout', this)" class="flex-shrink-0 bg-gray-100 text-gray-600 text-sm px-4 py-1.5 rounded-full hover:bg-gray-200 transition-colors">Layout</button>
            <button onclick="filterArticles('animation', this)" class="flex-shrink-0 bg-gray-100 text-gray-600 text-sm px-4 py-1.5 rounded-full hover:bg-gray-200 transition-colors">Animation</button>
            <button onclick="filterArticles('design', this)" class="flex-shrink-0 bg-gray-100 text-gray-600 text-sm px-4 py-1.5 rounded-full hover:bg-gray-200 transition-colors">Design</button>
          </div>

          <!-- Featured + Grid -->
          <div class="grid grid-cols-1 md:grid-cols-2 gap-5" id="articles-grid">
            <!-- Articles injected by JS -->
          </div>

          <div class="text-center mt-8">
            <button class="border border-gray-300 text-gray-600 px-6 py-2.5 rounded-xl text-sm hover:bg-gray-50 transition-colors">
              โหลดเพิ่ม
            </button>
          </div>
        </div>

        <!-- Sidebar -->
        <aside class="space-y-5">
          <div class="bg-white rounded-xl p-5 shadow-sm">
            <h3 class="font-bold mb-4 text-sm text-gray-500 uppercase tracking-wide">หมวดหมู่</h3>
            <div class="space-y-2">
              <a href="#" class="flex items-center justify-between text-sm text-gray-600 hover:text-indigo-600 transition-colors py-1">
                <span>Tailwind CSS</span><span class="text-xs bg-gray-100 px-2 py-0.5 rounded-full">24</span>
              </a>
              <a href="#" class="flex items-center justify-between text-sm text-gray-600 hover:text-indigo-600 transition-colors py-1">
                <span>Layout</span><span class="text-xs bg-gray-100 px-2 py-0.5 rounded-full">15</span>
              </a>
              <a href="#" class="flex items-center justify-between text-sm text-gray-600 hover:text-indigo-600 transition-colors py-1">
                <span>Animation</span><span class="text-xs bg-gray-100 px-2 py-0.5 rounded-full">9</span>
              </a>
              <a href="#" class="flex items-center justify-between text-sm text-gray-600 hover:text-indigo-600 transition-colors py-1">
                <span>Design System</span><span class="text-xs bg-gray-100 px-2 py-0.5 rounded-full">7</span>
              </a>
            </div>
          </div>

          <div class="bg-white rounded-xl p-5 shadow-sm">
            <h3 class="font-bold mb-4 text-sm text-gray-500 uppercase tracking-wide">ยอดนิยม</h3>
            <div class="space-y-3">
              <a href="#" class="flex gap-3 group">
                <span class="text-2xl font-black text-gray-100 w-8 text-center flex-shrink-0">01</span>
                <p class="text-sm font-medium group-hover:text-indigo-600 transition-colors line-clamp-2">10 เทคนิค Tailwind ที่ Dev มืออาชีพใช้</p>
              </a>
              <a href="#" class="flex gap-3 group">
                <span class="text-2xl font-black text-gray-100 w-8 text-center flex-shrink-0">02</span>
                <p class="text-sm font-medium group-hover:text-indigo-600 transition-colors line-clamp-2">สร้าง Component Library ด้วย CVA</p>
              </a>
              <a href="#" class="flex gap-3 group">
                <span class="text-2xl font-black text-gray-100 w-8 text-center flex-shrink-0">03</span>
                <p class="text-sm font-medium group-hover:text-indigo-600 transition-colors line-clamp-2">Dark Mode Best Practices 2024</p>
              </a>
            </div>
          </div>

          <div class="bg-gradient-to-br from-indigo-600 to-purple-600 rounded-xl p-5 text-white">
            <p class="font-bold mb-1">DevBlog Weekly</p>
            <p class="text-indigo-200 text-sm mb-4">บทความคัดสรรทุกวันจันทร์</p>
            <input type="email" placeholder="email ของคุณ" class="w-full bg-white/15 border border-white/25 rounded-lg px-3 py-2 text-sm placeholder-indigo-300 focus:outline-none focus:ring-2 focus:ring-white/50 mb-2 text-white">
            <button class="w-full bg-white text-indigo-700 font-semibold text-sm py-2 rounded-lg hover:bg-indigo-50 transition-colors">
              Subscribe ฟรี
            </button>
          </div>
        </aside>
      </div>
    </div>
  </div>

  <script>
    const articles = [
      { id: 1, cat: 'tailwind', tag: 'Tailwind CSS', tagColor: 'indigo', title: 'การออกแบบ Design System ด้วย Tailwind CSS', desc: 'เรียนรู้วิธีสร้าง Design System ที่ scale ได้ดี', img: 1, author: 'สมชาย', date: '15 มิ.ย.', time: '12 นาที' },
      { id: 2, cat: 'layout', tag: 'Layout', tagColor: 'purple', title: 'Flexbox vs Grid เมื่อไหร่ควรใช้อะไร', desc: 'เลือกใช้ Layout technique ให้ถูกกับงาน', img: 2, author: 'วิไล', date: '12 มิ.ย.', time: '8 นาที' },
      { id: 3, cat: 'animation', tag: 'Animation', tagColor: 'pink', title: 'Scroll-Triggered Animations ด้วย JS', desc: 'สร้าง animation ที่ trigger เมื่อ element เข้า viewport', img: 3, author: 'สมหญิง', date: '10 มิ.ย.', time: '10 นาที' },
      { id: 4, cat: 'design', tag: 'Design', tagColor: 'orange', title: 'Dark Mode ด้วย CSS Variables', desc: 'วิธีสร้าง dark mode ที่ยืดหยุ่นด้วย custom properties', img: 4, author: 'ธนพล', date: '8 มิ.ย.', time: '7 นาที' },
      { id: 5, cat: 'tailwind', tag: 'Tailwind CSS', tagColor: 'indigo', title: 'Optimize Tailwind Build ให้เล็กที่สุด', desc: 'เทคนิค purge, JIT mode และ content paths', img: 5, author: 'ประยุทธ', date: '5 มิ.ย.', time: '6 นาที' },
      { id: 6, cat: 'layout', tag: 'Grid', tagColor: 'teal', title: 'Bento Grid Layout ที่สวยงาม', desc: 'สร้าง Bento Grid ด้วย CSS Grid col-span และ row-span', img: 6, author: 'วิไล', date: '3 มิ.ย.', time: '9 นาที' },
    ];

    const colorMap = {
      indigo: 'bg-indigo-100 text-indigo-700',
      purple: 'bg-purple-100 text-purple-700',
      pink: 'bg-pink-100 text-pink-700',
      orange: 'bg-orange-100 text-orange-700',
      teal: 'bg-teal-100 text-teal-700',
    };

    function renderArticles(list) {
      const grid = document.getElementById('articles-grid');
      grid.innerHTML = list.map(a => `
        <article onclick="showArticle(${a.id})" class="bg-white rounded-xl overflow-hidden shadow-sm hover:shadow-md transition-all cursor-pointer group">
          <div class="overflow-hidden">
            <img src="https://picsum.photos/400/200?random=${a.img + 30}" class="w-full h-44 object-cover group-hover:scale-105 transition-transform duration-300" alt="">
          </div>
          <div class="p-5">
            <div class="flex items-center gap-2 mb-3">
              <span class="${colorMap[a.tagColor] || 'bg-gray-100 text-gray-700'} text-xs font-medium px-2.5 py-1 rounded-full">${a.tag}</span>
              <span class="text-gray-400 text-xs">${a.time}</span>
            </div>
            <h3 class="font-bold text-base mb-2 leading-snug group-hover:text-indigo-600 transition-colors">${a.title}</h3>
            <p class="text-gray-500 text-sm line-clamp-2 mb-4">${a.desc}</p>
            <div class="flex items-center justify-between text-xs text-gray-400">
              <span>${a.author}</span>
              <span>${a.date}</span>
            </div>
          </div>
        </article>
      `).join('');
    }

    function filterArticles(cat, btn) {
      document.querySelectorAll('[onclick^="filterArticles"]').forEach(b => {
        b.className = 'flex-shrink-0 bg-gray-100 text-gray-600 text-sm px-4 py-1.5 rounded-full hover:bg-gray-200 transition-colors';
      });
      btn.className = 'flex-shrink-0 bg-indigo-600 text-white text-sm px-4 py-1.5 rounded-full font-medium transition-colors';
      const filtered = cat === 'all' ? articles : articles.filter(a => a.cat === cat);
      renderArticles(filtered);
    }

    function showArticle(id) {
      document.getElementById('homepage').innerHTML = `
        <div class="max-w-3xl mx-auto px-4 py-10">
          <button onclick="location.reload()" class="flex items-center gap-2 text-sm text-gray-500 hover:text-indigo-600 transition-colors mb-6">
            ← กลับหน้าหลัก
          </button>
          <span class="bg-indigo-100 text-indigo-700 text-xs font-semibold px-3 py-1 rounded-full">Tailwind CSS</span>
          <h1 class="text-3xl font-extrabold mt-3 mb-3 leading-tight">การออกแบบ Design System ด้วย Tailwind CSS</h1>
          <p class="text-gray-500 text-lg mb-6">เรียนรู้วิธีสร้าง Design System ที่ scale ได้ดี ตั้งแต่ Color Token ไปจนถึง Component Library</p>
          <div class="flex items-center gap-3 mb-6">
            <img src="https://i.pravatar.cc/40?img=1" class="w-10 h-10 rounded-full" alt="">
            <div>
              <p class="font-semibold text-sm">สมชาย เทคโน</p>
              <p class="text-xs text-gray-500">15 มิ.ย. 2024 · 12 นาที</p>
            </div>
          </div>
          <img src="https://picsum.photos/800/400?random=50" class="w-full rounded-xl mb-8" alt="">
          <div class="prose">
            <h2>ทำไมต้องมี Design System?</h2>
            <p>ในองค์กรขนาดใหญ่ที่มีทีม Developer หลายทีม การที่แต่ละทีมสร้าง Component ขึ้นมาเองโดยไม่มีมาตรฐานกลาง ทำให้เกิดปัญหามากมาย ตั้งแต่ UI ที่ไม่สม่ำเสมอ การ duplicate code และการ maintain ที่ยากลำบาก</p>
            <h2>Design Token คืออะไร?</h2>
            <p>Design Token คือค่าพื้นฐานของ Design System ได้แก่ สี, typography, spacing, shadow เป็นต้น เราจะกำหนดทั้งหมดนี้ผ่าน CSS Custom Properties เพื่อให้แก้ไขได้จากจุดเดียว</p>
            <pre>:root {
  --color-primary: #6366f1;
  --font-size-base: 1rem;
  --spacing-md: 1.5rem;
}</pre>
            <h2>การ Map กับ Tailwind Config</h2>
            <p>จากนั้นเราจะ map token เข้ากับ Tailwind เพื่อให้ใช้งานผ่าน utility class ได้ทันที</p>
            <blockquote>Design System ที่ดีคือ "ภาษากลาง" ระหว่าง Designer และ Developer</blockquote>
          </div>
        </div>
      `;
    }

    let searchVisible = false;
    function toggleSearch() {
      searchVisible = !searchVisible;
      const bar = document.getElementById('search-bar');
      bar.classList.toggle('hidden', !searchVisible);
      if (searchVisible) document.getElementById('search-input').focus();
    }

    function handleSearch(q) {
      const results = document.getElementById('search-results');
      const list = document.getElementById('results-list');
      if (!q.trim()) { results.classList.add('hidden'); return; }
      const matches = articles.filter(a => a.title.toLowerCase().includes(q.toLowerCase()) || a.desc.toLowerCase().includes(q.toLowerCase()));
      if (matches.length === 0) {
        list.innerHTML = '<p class="p-4 text-sm text-gray-500">ไม่พบผลลัพธ์</p>';
      } else {
        list.innerHTML = matches.map(a => `
          <div class="flex items-center gap-3 p-3 hover:bg-gray-50 cursor-pointer transition-colors" onclick="showArticle(${a.id}); toggleSearch()">
            <img src="https://picsum.photos/48/48?random=${a.img}" class="w-10 h-10 rounded-lg object-cover flex-shrink-0" alt="">
            <div>
              <p class="text-sm font-medium text-gray-800">${a.title}</p>
              <p class="text-xs text-gray-400">${a.time} · ${a.author}</p>
            </div>
          </div>
        `).join('');
      }
      results.classList.remove('hidden');
    }

    // Reading progress
    window.addEventListener('scroll', () => {
      const total = document.body.scrollHeight - window.innerHeight;
      document.getElementById('progress').style.width = ((window.scrollY / total) * 100) + '%';
    });

    // Init
    renderArticles(articles);
  </script>

</body>
</html>
```

---

## สรุป Part 36

| Step | เนื้อหา |
|------|---------|
| 351 | Blog Homepage: Featured hero, article grid, sidebar |
| 352 | Article Page: Reading progress, TOC sidebar, author card |
| 353 | List-style blog: horizontal article cards |
| 354 | Category page: Masonry layout ด้วย CSS columns |
| 355 | Comments section: threaded replies |
| 356 | Newsletter CTA banners |
| 357 | Pagination components |
| 358 | Related articles section |
| 359 | Search results page |
| 360 | Workshop: Full Blog App with search, filter, article view |

**Part ถัดไป:** Part 37 — SaaS Landing Page Components
