# Part 20: Project 1 — Landing Page สมบูรณ์
## Steps 191–200: รวมทุกอย่างที่เรียนมาใน Part 1–19

---

## 🎯 เป้าหมายของ Part นี้

สร้าง Landing Page สมบูรณ์ที่ใช้:
- Responsive design (mobile-first)
- Navbar + Mobile menu
- Hero section with animation
- Features section
- Pricing section
- Testimonials
- FAQ accordion
- CTA section
- Footer

---

## Step 191–200: Full Landing Page Project

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>TailwindPro — เรียน Tailwind CSS อย่างเป็นระบบ</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@400;500;600;700;800;900&display=swap" rel="stylesheet">
  <style>
    body { font-family: 'Sarabun', sans-serif; }
    
    @keyframes fadeUp {
      from { opacity: 0; transform: translateY(24px); }
      to   { opacity: 1; transform: translateY(0); }
    }
    @keyframes float {
      0%, 100% { transform: translateY(0); }
      50% { transform: translateY(-12px); }
    }
    @keyframes slideInLeft {
      from { opacity: 0; transform: translateX(-20px); }
      to   { opacity: 1; transform: translateX(0); }
    }
    
    .animate-fade-up { animation: fadeUp 0.7s ease-out both; }
    .animate-float   { animation: float 4s ease-in-out infinite; }
    .delay-100 { animation-delay: 0.1s; }
    .delay-200 { animation-delay: 0.2s; }
    .delay-300 { animation-delay: 0.3s; }
    .delay-400 { animation-delay: 0.4s; }
    .delay-500 { animation-delay: 0.5s; }
    
    /* Accordion */
    .faq-answer { max-height: 0; overflow: hidden; transition: max-height 0.4s ease; }
    .faq-item.open .faq-answer { max-height: 200px; }
    .faq-item.open .faq-icon { transform: rotate(45deg); }
    .faq-icon { transition: transform 0.3s ease; }
  </style>
</head>

<body class="bg-white antialiased">

<!-- ====== NAVBAR ====== -->
<nav id="navbar" class="fixed top-0 left-0 right-0 z-50 transition-all duration-300 py-4">
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
    <div class="flex items-center justify-between">
      
      <!-- Logo -->
      <a href="#" class="flex items-center gap-2.5">
        <div class="size-9 bg-gradient-to-br from-indigo-500 to-purple-600 rounded-xl flex items-center justify-center text-white font-black text-sm">T</div>
        <span class="font-black text-xl text-white">TailwindPro</span>
      </a>
      
      <!-- Desktop Nav -->
      <div class="hidden md:flex items-center gap-1">
        <a href="#features" class="px-4 py-2 rounded-xl text-sm text-white/80 hover:text-white hover:bg-white/10 transition-all">Features</a>
        <a href="#pricing"  class="px-4 py-2 rounded-xl text-sm text-white/80 hover:text-white hover:bg-white/10 transition-all">ราคา</a>
        <a href="#faq"      class="px-4 py-2 rounded-xl text-sm text-white/80 hover:text-white hover:bg-white/10 transition-all">FAQ</a>
      </div>
      
      <div class="hidden md:flex items-center gap-3">
        <a href="#" class="text-sm text-white/80 hover:text-white transition-colors">เข้าสู่ระบบ</a>
        <a href="#" class="bg-white text-indigo-700 hover:bg-indigo-50 text-sm font-bold px-5 py-2.5 rounded-xl transition-colors shadow-lg shadow-black/10">
          เริ่มเรียนฟรี →
        </a>
      </div>
      
      <!-- Mobile hamburger -->
      <button 
        id="menu-toggle"
        onclick="document.getElementById('mobile-menu').classList.toggle('hidden')"
        class="md:hidden size-10 flex flex-col justify-center items-center gap-1.5 bg-white/10 rounded-xl"
      >
        <span class="w-5 h-0.5 bg-white rounded"></span>
        <span class="w-5 h-0.5 bg-white rounded"></span>
        <span class="w-5 h-0.5 bg-white rounded"></span>
      </button>
    </div>
    
    <!-- Mobile Menu -->
    <div id="mobile-menu" class="hidden md:hidden mt-3 bg-white/10 backdrop-blur-sm rounded-2xl p-3">
      <a href="#features" class="block px-4 py-2.5 text-sm text-white/80 hover:text-white hover:bg-white/10 rounded-xl transition-colors">Features</a>
      <a href="#pricing"  class="block px-4 py-2.5 text-sm text-white/80 hover:text-white hover:bg-white/10 rounded-xl transition-colors">ราคา</a>
      <a href="#faq"      class="block px-4 py-2.5 text-sm text-white/80 hover:text-white hover:bg-white/10 rounded-xl transition-colors">FAQ</a>
      <div class="border-t border-white/20 mt-2 pt-2 flex flex-col gap-2">
        <a href="#" class="block px-4 py-2.5 text-sm text-white/80 hover:text-white hover:bg-white/10 rounded-xl">เข้าสู่ระบบ</a>
        <a href="#" class="block text-center bg-white text-indigo-700 font-bold text-sm px-4 py-2.5 rounded-xl">เริ่มเรียนฟรี →</a>
      </div>
    </div>
  </div>
</nav>

<script>
  // Navbar scroll effect
  const navbar = document.getElementById('navbar');
  window.addEventListener('scroll', () => {
    if (window.scrollY > 50) {
      navbar.classList.add('bg-white/90', 'backdrop-blur-md', 'shadow-sm', 'border-b', 'border-gray-200/50', 'py-2');
      navbar.querySelectorAll('.text-white\\/80, .text-white').forEach(el => {
        el.classList.replace('text-white/80', 'text-gray-600');
        el.classList.replace('text-white', 'text-gray-900');
      });
    } else {
      navbar.classList.remove('bg-white/90', 'backdrop-blur-md', 'shadow-sm', 'border-b', 'border-gray-200/50', 'py-2');
    }
  });
</script>

<!-- ====== HERO ====== -->
<section class="relative min-h-screen bg-gradient-to-br from-slate-900 via-indigo-950 to-purple-950 flex items-center overflow-hidden pt-20">
  
  <!-- Background decoration -->
  <div class="absolute inset-0 pointer-events-none">
    <div class="absolute top-1/4 left-1/4 size-[600px] bg-indigo-500/10 rounded-full blur-3xl"></div>
    <div class="absolute bottom-1/4 right-1/4 size-[500px] bg-purple-500/10 rounded-full blur-3xl"></div>
    <!-- Grid -->
    <div class="absolute inset-0 bg-[url('data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNDAiIGhlaWdodD0iNDAiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+PGRlZnM+PHBhdHRlcm4gaWQ9ImdyaWQiIHdpZHRoPSI0MCIgaGVpZ2h0PSI0MCIgcGF0dGVyblVuaXRzPSJ1c2VyU3BhY2VPblVzZSI+PHBhdGggZD0iTSAwIDEwIEwgNDAgMTAgTSAxMCAwIEwgMTAgNDAgTSAwIDIwIEwgNDAgMjAgTSAyMCAwIEwgMjAgNDAgTSAwIDMwIEwgNDAgMzAgTSAzMCAwIEwgMzAgNDAiIGZpbGw9Im5vbmUiIHN0cm9rZT0iI2ZmZiIgc3Ryb2tlLXdpZHRoPSIwLjIiIG9wYWNpdHk9IjAuMSIvPjwvcGF0dGVybj48L2RlZnM+PHJlY3Qgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSIgZmlsbD0idXJsKCNncmlkKSIvPjwvc3ZnPg==')] opacity-30"></div>
  </div>
  
  <div class="relative max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-20">
    <div class="flex flex-col lg:flex-row items-center gap-12 lg:gap-20">
      
      <!-- Content -->
      <div class="flex-1 text-center lg:text-left">
        
        <div class="opacity-0 animate-fade-up">
          <span class="inline-flex items-center gap-2 text-sm bg-white/10 border border-white/20 text-indigo-300 px-4 py-2 rounded-full mb-6">
            <span class="relative flex h-2 w-2">
              <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-green-400 opacity-75"></span>
              <span class="relative inline-flex rounded-full h-2 w-2 bg-green-400"></span>
            </span>
            อัพเดตใหม่ — เนื้อหาใหม่ทุกสัปดาห์
          </span>
        </div>
        
        <h1 class="opacity-0 animate-fade-up delay-100 text-4xl sm:text-5xl lg:text-6xl xl:text-7xl font-black text-white leading-[1.1] mb-6">
          เรียน<br>
          <span class="bg-gradient-to-r from-indigo-400 via-purple-400 to-pink-400 bg-clip-text text-transparent">
            Tailwind CSS
          </span><br>
          อย่างเป็นระบบ
        </h1>
        
        <p class="opacity-0 animate-fade-up delay-200 text-lg sm:text-xl text-white/60 max-w-xl mx-auto lg:mx-0 leading-relaxed mb-8">
          100+ บทเรียน • 1,000 Steps<br>
          จากพื้นฐานสู่ระดับโลก พร้อม Project จริงทุกบท
        </p>
        
        <div class="opacity-0 animate-fade-up delay-300 flex flex-col sm:flex-row gap-3 justify-center lg:justify-start mb-10">
          <a href="#" class="
            inline-flex items-center justify-center gap-2
            bg-indigo-600 hover:bg-indigo-500 active:scale-95
            text-white font-bold px-8 py-4 rounded-2xl text-base
            shadow-xl shadow-indigo-600/30 hover:shadow-indigo-500/40
            transition-all
          ">
            เริ่มเรียนฟรีเลย
            <span>→</span>
          </a>
          <a href="#" class="
            inline-flex items-center justify-center gap-2
            bg-white/10 hover:bg-white/20 border border-white/20
            text-white font-bold px-8 py-4 rounded-2xl text-base
            transition-all
          ">
            <span>▶</span> ดูตัวอย่าง
          </a>
        </div>
        
        <!-- Social proof -->
        <div class="opacity-0 animate-fade-up delay-400 flex flex-col sm:flex-row items-center gap-4 justify-center lg:justify-start">
          <div class="flex -space-x-3">
            <div class="size-10 rounded-full bg-blue-400 border-2 border-slate-900 flex items-center justify-center text-white font-bold text-sm">A</div>
            <div class="size-10 rounded-full bg-pink-400 border-2 border-slate-900 flex items-center justify-center text-white font-bold text-sm">B</div>
            <div class="size-10 rounded-full bg-green-400 border-2 border-slate-900 flex items-center justify-center text-white font-bold text-sm">C</div>
            <div class="size-10 rounded-full bg-purple-400 border-2 border-slate-900 flex items-center justify-center text-white font-bold text-sm">D</div>
            <div class="size-10 rounded-full bg-yellow-400 border-2 border-slate-900 flex items-center justify-center text-white font-bold text-sm">E</div>
          </div>
          <div>
            <div class="flex items-center gap-0.5 justify-center lg:justify-start mb-0.5">
              <span class="text-yellow-400 text-sm">★★★★★</span>
            </div>
            <p class="text-white/60 text-sm">
              <span class="text-white font-bold">1,248+</span> นักเรียนไว้วางใจ
            </p>
          </div>
        </div>
      </div>
      
      <!-- Hero Visual -->
      <div class="flex-none w-full max-w-md lg:max-w-lg relative">
        <!-- Main card -->
        <div class="bg-white/10 backdrop-blur-sm border border-white/20 rounded-3xl p-6 shadow-2xl animate-float">
          
          <div class="flex items-center justify-between mb-6">
            <div>
              <p class="text-white/60 text-sm">กำลังเรียน</p>
              <h3 class="font-bold text-white">Part 15: Animations</h3>
            </div>
            <div class="size-10 bg-indigo-600 rounded-xl flex items-center justify-center">
              <span class="text-xl">✨</span>
            </div>
          </div>
          
          <!-- Progress -->
          <div class="mb-5">
            <div class="flex justify-between text-sm text-white/60 mb-2">
              <span>Step 147/150</span>
              <span class="text-indigo-300 font-bold">98%</span>
            </div>
            <div class="h-2.5 bg-white/10 rounded-full overflow-hidden">
              <div class="h-full w-[98%] bg-gradient-to-r from-indigo-400 to-purple-400 rounded-full"></div>
            </div>
          </div>
          
          <!-- Code preview -->
          <div class="bg-slate-900 rounded-xl p-4 text-xs font-mono">
            <div class="flex items-center gap-1.5 mb-3">
              <div class="size-2.5 rounded-full bg-red-500"></div>
              <div class="size-2.5 rounded-full bg-yellow-500"></div>
              <div class="size-2.5 rounded-full bg-green-500"></div>
            </div>
            <p class="text-gray-500">{'<'}div class=</p>
            <p class="text-green-400 pl-2">"animate-bounce</p>
            <p class="text-yellow-400 pl-3">hover:scale-110</p>
            <p class="text-blue-400 pl-3">transition-all</p>
            <p class="text-purple-400 pl-3">duration-300"{'>'}</p>
            <p class="text-gray-500 pl-2">🎉</p>
            <p class="text-gray-500">{'<'}/div{'>'}</p>
          </div>
          
          <!-- Stats row -->
          <div class="grid grid-cols-3 gap-3 mt-5">
            <div class="bg-white/5 rounded-xl p-2.5 text-center">
              <p class="font-black text-white text-lg">42</p>
              <p class="text-white/50 text-xs">Parts</p>
            </div>
            <div class="bg-white/5 rounded-xl p-2.5 text-center">
              <p class="font-black text-white text-lg">7🔥</p>
              <p class="text-white/50 text-xs">Streak</p>
            </div>
            <div class="bg-white/5 rounded-xl p-2.5 text-center">
              <p class="font-black text-white text-lg">2.4k</p>
              <p class="text-white/50 text-xs">XP</p>
            </div>
          </div>
        </div>
        
        <!-- Floating badges -->
        <div class="absolute -top-4 -right-4 bg-yellow-400 text-yellow-900 text-xs font-black px-3 py-1.5 rounded-full shadow-lg animate-bounce">
          🎉 Level Up!
        </div>
        <div class="absolute -bottom-5 -left-5 bg-white/10 backdrop-blur-sm border border-white/20 rounded-2xl p-3 shadow-xl animate-float" style="animation-delay:1.5s">
          <div class="flex items-center gap-2">
            <span class="text-xl">🏆</span>
            <div>
              <p class="text-xs font-bold text-white">Certificate</p>
              <p class="text-[10px] text-white/50">สำเร็จการศึกษา</p>
            </div>
          </div>
        </div>
      </div>
    </div>
    
    <!-- Scroll hint -->
    <div class="absolute bottom-8 left-1/2 -translate-x-1/2 animate-bounce text-white/30 text-xs flex flex-col items-center gap-1">
      <span>เลื่อนลง</span>
      <span>↓</span>
    </div>
  </div>
</section>

<!-- ====== LOGOS ====== -->
<section class="bg-gray-50 border-y border-gray-200 py-10 px-4">
  <div class="max-w-5xl mx-auto">
    <p class="text-center text-sm text-gray-400 mb-6">นักเรียนทำงานที่</p>
    <div class="flex flex-wrap items-center justify-center gap-8 opacity-40">
      <span class="font-black text-xl text-gray-600">Accenture</span>
      <span class="font-black text-xl text-gray-600">Agoda</span>
      <span class="font-black text-xl text-gray-600">Grab</span>
      <span class="font-black text-xl text-gray-600">Line</span>
      <span class="font-black text-xl text-gray-600">SCB</span>
      <span class="font-black text-xl text-gray-600">True Corp</span>
    </div>
  </div>
</section>

<!-- ====== FEATURES ====== -->
<section id="features" class="py-20 px-4 sm:px-6 lg:px-8">
  <div class="max-w-7xl mx-auto">
    
    <div class="text-center max-w-2xl mx-auto mb-14">
      <span class="text-xs font-bold tracking-widest text-indigo-600 uppercase">ทำไมต้อง TailwindPro?</span>
      <h2 class="text-3xl sm:text-4xl font-black text-gray-900 mt-3 mb-4">
        เรียนอย่างมีระบบ<br>ได้ผลจริง
      </h2>
      <p class="text-gray-500">ออกแบบมาสำหรับผู้เรียนภาษาไทย ด้วยเนื้อหาที่ใช้งานได้จริง 100%</p>
    </div>
    
    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
      
      <div class="bg-white rounded-2xl p-6 border border-gray-200 hover:border-indigo-200 hover:shadow-lg hover:-translate-y-1 transition-all group">
        <div class="size-12 bg-indigo-100 group-hover:bg-indigo-600 rounded-2xl flex items-center justify-center text-2xl mb-4 transition-colors">📚</div>
        <h3 class="font-bold text-gray-900 mb-2">100+ บทเรียน</h3>
        <p class="text-gray-500 text-sm leading-relaxed">ตั้งแต่ Step 1 ถึง Step 1,000 ครบทุกเนื้อหาตั้งแต่พื้นฐานถึงระดับโลก</p>
      </div>
      
      <div class="bg-white rounded-2xl p-6 border border-gray-200 hover:border-green-200 hover:shadow-lg hover:-translate-y-1 transition-all group">
        <div class="size-12 bg-green-100 group-hover:bg-green-600 rounded-2xl flex items-center justify-center text-2xl mb-4 transition-colors">🚀</div>
        <h3 class="font-bold text-gray-900 mb-2">Project จริงทุกบท</h3>
        <p class="text-gray-500 text-sm leading-relaxed">ไม่ใช่แค่ทฤษฎี ทุก Part มี Workshop และ Project ที่ใช้งานได้จริง</p>
      </div>
      
      <div class="bg-white rounded-2xl p-6 border border-gray-200 hover:border-purple-200 hover:shadow-lg hover:-translate-y-1 transition-all group">
        <div class="size-12 bg-purple-100 group-hover:bg-purple-600 rounded-2xl flex items-center justify-center text-2xl mb-4 transition-colors">🌏</div>
        <h3 class="font-bold text-gray-900 mb-2">ภาษาไทย 100%</h3>
        <p class="text-gray-500 text-sm leading-relaxed">เนื้อหาทั้งหมดเป็นภาษาไทย เข้าใจง่าย ไม่ต้องกังวลเรื่องภาษา</p>
      </div>
      
      <div class="bg-white rounded-2xl p-6 border border-gray-200 hover:border-orange-200 hover:shadow-lg hover:-translate-y-1 transition-all group">
        <div class="size-12 bg-orange-100 group-hover:bg-orange-600 rounded-2xl flex items-center justify-center text-2xl mb-4 transition-colors">🔥</div>
        <h3 class="font-bold text-gray-900 mb-2">อัพเดทสม่ำเสมอ</h3>
        <p class="text-gray-500 text-sm leading-relaxed">เนื้อหาใหม่ทุกสัปดาห์ ทันกับ Tailwind v4 และ ecosystem ล่าสุด</p>
      </div>
      
      <div class="bg-white rounded-2xl p-6 border border-gray-200 hover:border-blue-200 hover:shadow-lg hover:-translate-y-1 transition-all group">
        <div class="size-12 bg-blue-100 group-hover:bg-blue-600 rounded-2xl flex items-center justify-center text-2xl mb-4 transition-colors">🏆</div>
        <h3 class="font-bold text-gray-900 mb-2">Certificate</h3>
        <p class="text-gray-500 text-sm leading-relaxed">ได้รับใบประกาศนียบัตรเมื่อเรียนจบ ใช้ใน Portfolio และ Resume ได้</p>
      </div>
      
      <div class="bg-white rounded-2xl p-6 border border-gray-200 hover:border-pink-200 hover:shadow-lg hover:-translate-y-1 transition-all group">
        <div class="size-12 bg-pink-100 group-hover:bg-pink-600 rounded-2xl flex items-center justify-center text-2xl mb-4 transition-colors">👥</div>
        <h3 class="font-bold text-gray-900 mb-2">ชุมชน</h3>
        <p class="text-gray-500 text-sm leading-relaxed">กลุ่ม Discord สำหรับถาม-ตอบ แชร์ผลงาน และ networking</p>
      </div>
      
    </div>
  </div>
</section>

<!-- ====== PRICING ====== -->
<section id="pricing" class="py-20 px-4 sm:px-6 lg:px-8 bg-gray-50">
  <div class="max-w-5xl mx-auto">
    
    <div class="text-center mb-14">
      <span class="text-xs font-bold tracking-widest text-indigo-600 uppercase">ราคา</span>
      <h2 class="text-3xl sm:text-4xl font-black text-gray-900 mt-3 mb-4">เริ่มต้นฟรี ไม่ต้องใช้บัตรเครดิต</h2>
      <p class="text-gray-500">ลองเรียน 20 Part แรกฟรี แล้วค่อยอัพเกรด</p>
    </div>
    
    <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
      
      <div class="bg-white rounded-2xl border-2 border-gray-200 p-7">
        <h3 class="font-bold text-lg text-gray-900">ฟรี</h3>
        <p class="text-gray-400 text-sm mt-1 mb-6">ลองก่อนตัดสินใจ</p>
        <div class="mb-7">
          <span class="text-4xl font-black text-gray-900">฿0</span>
          <span class="text-gray-400 text-sm ml-1">/เดือน</span>
        </div>
        <ul class="space-y-3 mb-7 text-sm text-gray-600">
          <li class="flex items-center gap-2"><span class="text-green-500">✓</span> 20 บทเรียนแรก</li>
          <li class="flex items-center gap-2"><span class="text-green-500">✓</span> Code examples</li>
          <li class="flex items-center gap-2"><span class="text-green-500">✓</span> Community Discord</li>
          <li class="flex items-center gap-2"><span class="text-gray-300">✗</span> <span class="text-gray-400">โปรเจกต์ขั้นสูง</span></li>
          <li class="flex items-center gap-2"><span class="text-gray-300">✗</span> <span class="text-gray-400">Certificate</span></li>
        </ul>
        <a href="#" class="block text-center border-2 border-gray-200 hover:border-indigo-400 hover:text-indigo-600 text-gray-700 font-semibold py-3 rounded-2xl transition-all">
          เริ่มเลย
        </a>
      </div>
      
      <div class="bg-indigo-600 rounded-2xl p-7 text-white shadow-xl shadow-indigo-600/20 relative">
        <div class="absolute -top-4 left-1/2 -translate-x-1/2 bg-yellow-400 text-yellow-900 text-xs font-black px-4 py-1.5 rounded-full whitespace-nowrap shadow-lg">
          ⭐ ยอดนิยม
        </div>
        <h3 class="font-bold text-lg mt-2">Pro</h3>
        <p class="text-indigo-300 text-sm mt-1 mb-6">สำหรับผู้เรียนจริงจัง</p>
        <div class="mb-7">
          <span class="text-4xl font-black">฿299</span>
          <span class="text-indigo-300 text-sm ml-1">/เดือน</span>
        </div>
        <ul class="space-y-3 mb-7 text-sm">
          <li class="flex items-center gap-2"><span class="text-yellow-300">✓</span> บทเรียนทั้งหมด 100+</li>
          <li class="flex items-center gap-2"><span class="text-yellow-300">✓</span> Project จริง 50+</li>
          <li class="flex items-center gap-2"><span class="text-yellow-300">✓</span> Download source code</li>
          <li class="flex items-center gap-2"><span class="text-yellow-300">✓</span> Certificate</li>
          <li class="flex items-center gap-2"><span class="text-yellow-300">✓</span> Q&A กับผู้สอน</li>
        </ul>
        <a href="#" class="block text-center bg-white text-indigo-700 hover:bg-indigo-50 font-bold py-3 rounded-2xl transition-colors shadow-lg">
          สมัคร Pro ตอนนี้
        </a>
      </div>
      
      <div class="bg-white rounded-2xl border-2 border-gray-200 p-7">
        <h3 class="font-bold text-lg text-gray-900">Team</h3>
        <p class="text-gray-400 text-sm mt-1 mb-6">สำหรับทีม 5+ คน</p>
        <div class="mb-7">
          <span class="text-2xl font-black text-gray-900">ติดต่อ</span>
        </div>
        <ul class="space-y-3 mb-7 text-sm text-gray-600">
          <li class="flex items-center gap-2"><span class="text-green-500">✓</span> ทุกอย่างใน Pro</li>
          <li class="flex items-center gap-2"><span class="text-green-500">✓</span> Team dashboard</li>
          <li class="flex items-center gap-2"><span class="text-green-500">✓</span> Progress tracking</li>
          <li class="flex items-center gap-2"><span class="text-green-500">✓</span> Custom path</li>
          <li class="flex items-center gap-2"><span class="text-green-500">✓</span> Invoice billing</li>
        </ul>
        <a href="#" class="block text-center border-2 border-gray-200 hover:border-indigo-400 hover:text-indigo-600 text-gray-700 font-semibold py-3 rounded-2xl transition-all">
          ติดต่อ Sales
        </a>
      </div>
      
    </div>
  </div>
</section>

<!-- ====== FAQ ====== -->
<section id="faq" class="py-20 px-4 sm:px-6 lg:px-8">
  <div class="max-w-3xl mx-auto">
    
    <div class="text-center mb-12">
      <h2 class="text-3xl sm:text-4xl font-black text-gray-900 mb-4">คำถามที่พบบ่อย</h2>
      <p class="text-gray-500">ถ้าไม่เจอคำตอบ ติดต่อเราได้เลย</p>
    </div>
    
    <div class="space-y-3" id="faq-list">
      
      <div class="faq-item bg-white border border-gray-200 rounded-2xl overflow-hidden">
        <button class="w-full flex items-center justify-between px-6 py-5 text-left" onclick="toggleFaq(this)">
          <span class="font-semibold text-gray-900">ต้องมีพื้นฐาน HTML/CSS ก่อนไหม?</span>
          <span class="faq-icon flex-none size-7 bg-indigo-100 text-indigo-600 rounded-full flex items-center justify-center font-bold">+</span>
        </button>
        <div class="faq-answer px-6 pb-5">
          <p class="text-gray-600 text-sm leading-relaxed">ควรมีพื้นฐาน HTML/CSS พื้นฐานก่อนเล็กน้อย แต่เราจะสอนทบทวนในบทแรกๆ ด้วย</p>
        </div>
      </div>
      
      <div class="faq-item bg-white border border-gray-200 rounded-2xl overflow-hidden">
        <button class="w-full flex items-center justify-between px-6 py-5 text-left" onclick="toggleFaq(this)">
          <span class="font-semibold text-gray-900">เนื้อหาอัพเดทบ่อยแค่ไหน?</span>
          <span class="faq-icon flex-none size-7 bg-indigo-100 text-indigo-600 rounded-full flex items-center justify-center font-bold">+</span>
        </button>
        <div class="faq-answer px-6 pb-5">
          <p class="text-gray-600 text-sm leading-relaxed">เพิ่มเนื้อหาใหม่ทุกสัปดาห์ และอัพเดทตามเวอร์ชั่นใหม่ของ Tailwind CSS เสมอ</p>
        </div>
      </div>
      
      <div class="faq-item bg-white border border-gray-200 rounded-2xl overflow-hidden">
        <button class="w-full flex items-center justify-between px-6 py-5 text-left" onclick="toggleFaq(this)">
          <span class="font-semibold text-gray-900">ยกเลิกได้ตลอดเวลาไหม?</span>
          <span class="faq-icon flex-none size-7 bg-indigo-100 text-indigo-600 rounded-full flex items-center justify-center font-bold">+</span>
        </button>
        <div class="faq-answer px-6 pb-5">
          <p class="text-gray-600 text-sm leading-relaxed">ได้เลย ยกเลิกได้ตลอดเวลา ไม่มีค่าใช้จ่ายแอบแฝงใดๆ</p>
        </div>
      </div>
      
      <div class="faq-item bg-white border border-gray-200 rounded-2xl overflow-hidden">
        <button class="w-full flex items-center justify-between px-6 py-5 text-left" onclick="toggleFaq(this)">
          <span class="font-semibold text-gray-900">รับ Certificate ได้เมื่อไหร่?</span>
          <span class="faq-icon flex-none size-7 bg-indigo-100 text-indigo-600 rounded-full flex items-center justify-center font-bold">+</span>
        </button>
        <div class="faq-answer px-6 pb-5">
          <p class="text-gray-600 text-sm leading-relaxed">รับ Certificate ทันทีเมื่อเรียนครบ 100% และผ่านแบบทดสอบท้ายหลักสูตร สามารถดาวน์โหลด PDF ได้เลย</p>
        </div>
      </div>
      
    </div>
  </div>
</section>

<!-- ====== CTA ====== -->
<section class="bg-gradient-to-br from-indigo-600 to-purple-700 py-20 px-4 text-white text-center">
  <div class="max-w-2xl mx-auto">
    <h2 class="text-3xl sm:text-4xl font-black mb-4">พร้อมเริ่มต้นแล้วหรือยัง?</h2>
    <p class="text-indigo-200 text-lg mb-8">เริ่มเรียน 20 Part แรกฟรี ไม่ต้องใช้บัตรเครดิต</p>
    <a href="#" class="inline-flex items-center gap-2 bg-white text-indigo-700 hover:bg-indigo-50 font-black px-8 py-4 rounded-2xl text-lg transition-colors shadow-xl shadow-black/10">
      เริ่มเรียนฟรีเลย →
    </a>
    <p class="text-indigo-300 text-sm mt-4">1,248+ นักเรียนไว้วางใจแล้ว</p>
  </div>
</section>

<!-- ====== FOOTER ====== -->
<footer class="bg-gray-900 text-white pt-16 pb-8 px-4 sm:px-6 lg:px-8">
  <div class="max-w-7xl mx-auto">
    <div class="grid grid-cols-2 md:grid-cols-4 gap-8 pb-10 border-b border-gray-800">
      <div class="col-span-2 md:col-span-1">
        <div class="flex items-center gap-2.5 mb-4">
          <div class="size-8 bg-indigo-600 rounded-lg flex items-center justify-center text-white font-black text-sm">T</div>
          <span class="font-black text-xl">TailwindPro</span>
        </div>
        <p class="text-gray-400 text-sm leading-relaxed">เรียน Tailwind CSS อย่างเป็นระบบ ตั้งแต่พื้นฐานสู่ระดับโลก</p>
      </div>
      <div>
        <h4 class="font-semibold text-sm uppercase tracking-wider text-gray-400 mb-4">หลักสูตร</h4>
        <ul class="space-y-2.5">
          <li><a href="#" class="text-gray-400 hover:text-white text-sm transition-colors">Level 1: พื้นฐาน</a></li>
          <li><a href="#" class="text-gray-400 hover:text-white text-sm transition-colors">Level 2: กลาง</a></li>
          <li><a href="#" class="text-gray-400 hover:text-white text-sm transition-colors">Level 3: ขั้นสูง</a></li>
          <li><a href="#" class="text-gray-400 hover:text-white text-sm transition-colors">Level 4: โลก</a></li>
        </ul>
      </div>
      <div>
        <h4 class="font-semibold text-sm uppercase tracking-wider text-gray-400 mb-4">บริษัท</h4>
        <ul class="space-y-2.5">
          <li><a href="#" class="text-gray-400 hover:text-white text-sm transition-colors">เกี่ยวกับเรา</a></li>
          <li><a href="#" class="text-gray-400 hover:text-white text-sm transition-colors">บล็อก</a></li>
          <li><a href="#" class="text-gray-400 hover:text-white text-sm transition-colors">ติดต่อ</a></li>
        </ul>
      </div>
      <div>
        <h4 class="font-semibold text-sm uppercase tracking-wider text-gray-400 mb-4">Newsletter</h4>
        <p class="text-gray-400 text-sm mb-3">รับ Tip Tailwind ทุกสัปดาห์</p>
        <div class="flex gap-2">
          <input type="email" placeholder="email@..." class="flex-1 min-w-0 bg-gray-800 border border-gray-700 text-white text-sm rounded-xl px-3 py-2 focus:outline-none focus:border-indigo-500 placeholder:text-gray-600">
          <button class="bg-indigo-600 hover:bg-indigo-700 text-white text-sm px-3 py-2 rounded-xl transition-colors flex-none">→</button>
        </div>
      </div>
    </div>
    <div class="pt-6 flex flex-col sm:flex-row items-center justify-between gap-4">
      <p class="text-gray-500 text-sm">© 2025 TailwindPro. All rights reserved.</p>
      <div class="flex gap-6">
        <a href="#" class="text-gray-500 hover:text-gray-300 text-sm transition-colors">Privacy</a>
        <a href="#" class="text-gray-500 hover:text-gray-300 text-sm transition-colors">Terms</a>
      </div>
    </div>
  </div>
</footer>

<script>
  function toggleFaq(btn) {
    const item = btn.closest('.faq-item');
    const isOpen = item.classList.contains('open');
    document.querySelectorAll('.faq-item').forEach(el => el.classList.remove('open'));
    if (!isOpen) item.classList.add('open');
  }
</script>

</body>
</html>
```

---

## 📝 สรุป Part 20 & Level 1

**คุณสำเร็จ Level 1 แล้ว!** 🎉

ใน 20 Part แรก (200 Steps) คุณเรียนรู้:

| Part | เนื้อหา |
|------|---------|
| 01 | Introduction & Utility-First |
| 02 | Installation & Setup |
| 03 | Typography |
| 04 | Colors & Gradients |
| 05 | Spacing |
| 06 | Sizing |
| 07 | Flexbox |
| 08 | Grid |
| 09 | Backgrounds & Effects |
| 10 | Borders & Shadows |
| 11 | Display & Visibility |
| 12 | Position & Transform |
| 13 | Responsive Design |
| 14 | Hover, Focus, States |
| 15 | Transitions & Animations |
| 16 | Forms & Inputs |
| 17 | Buttons & Components |
| 18 | Navigation & Header |
| 19 | Cards & Content Blocks |
| **20** | **🎯 Project: Landing Page** |

### ➡️ ต่อไป: Level 2 (Parts 21–50)

- Part 21: Tailwind Config ขั้นสูง
- Part 22: Dark Mode
- Part 23: Custom Plugins
- Part 24: Component Patterns
- และอีกมากมาย...

---

*Part 20 — จาก 100 Parts | Steps 191–200 จาก 1,000 Steps*
*🎉 Level 1 Complete!*
