# Part 30: Project 2 — Complete Dashboard App
## Steps 291–300: รวมทุกอย่างใน Dashboard จริง

---

## 🎯 เป้าหมายของ Part นี้

Project ที่ 2 รวม Parts 21–29 ทั้งหมด:
- Sidebar navigation + topbar
- Dashboard stats + charts
- Data table with search/filter
- Modal + drawer
- Toast notifications
- Dark mode toggle
- Responsive (mobile-first)

---

## Workshop — Full Dashboard Application (Steps 291–300)

```html
<!DOCTYPE html>
<html lang="th" class="h-full">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>TailwindPro — Dashboard</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: {
        extend: {
          animation: {
            'slide-in': 'slideIn 0.3s ease-out',
            'fade-in': 'fadeIn 0.2s ease-out',
          },
          keyframes: {
            slideIn: { from: { opacity:'0', transform:'translateY(10px)' }, to: { opacity:'1', transform:'translateY(0)' } },
            fadeIn:  { from: { opacity:'0' }, to: { opacity:'1' } },
          }
        }
      }
    }
  </script>
  <style>
    @keyframes tIn  { from { opacity:0; transform:translateX(110%); } to { opacity:1; transform:translateX(0); } }
    @keyframes tOut { from { opacity:1; } to { opacity:0; transform:translateX(110%); } }
    .t-in  { animation: tIn  0.3s ease-out; }
    .t-out { animation: tOut 0.25s ease-in forwards; }
  </style>
</head>
<body class="h-full bg-gray-50 dark:bg-gray-950 font-sans antialiased transition-colors">

<!-- Toast Container -->
<div id="toasts" class="fixed top-4 right-4 z-[100] flex flex-col gap-2 w-80 pointer-events-none"></div>

<!-- Mobile overlay -->
<div id="mobile-overlay" onclick="closeSidebar()" class="hidden fixed inset-0 bg-black/50 z-30 lg:hidden"></div>

<div class="flex h-full overflow-hidden">

  <!-- ========== SIDEBAR ========== -->
  <aside id="sidebar" class="fixed lg:static inset-y-0 left-0 z-40 flex-none w-64 bg-white dark:bg-gray-900 border-r border-gray-200 dark:border-gray-800 flex flex-col -translate-x-full lg:translate-x-0 transition-transform duration-300">
    <!-- Logo -->
    <div class="h-16 flex items-center px-5 border-b border-gray-100 dark:border-gray-800 flex-none">
      <div class="flex items-center gap-2.5">
        <div class="size-9 rounded-xl bg-gradient-to-br from-indigo-600 to-purple-600 flex items-center justify-center flex-none shadow-lg shadow-indigo-500/30">
          <span class="text-white font-black text-base">T</span>
        </div>
        <div>
          <p class="font-black text-gray-900 dark:text-white leading-tight">TailwindPro</p>
          <p class="text-[10px] text-gray-400">Admin v2.0</p>
        </div>
      </div>
    </div>
    
    <!-- Nav -->
    <nav class="flex-1 overflow-y-auto p-3 space-y-0.5">
      <p class="text-[10px] font-bold tracking-widest text-gray-400 uppercase px-3 py-2">Overview</p>
      
      <a href="#" onclick="setPage('dashboard',this)" class="nav-item active flex items-center gap-3 px-3 py-2.5 rounded-xl text-sm font-medium transition-colors">
        <span>🏠</span><span>Dashboard</span>
      </a>
      <a href="#" onclick="setPage('students',this)" class="nav-item flex items-center gap-3 px-3 py-2.5 rounded-xl text-sm font-medium transition-colors text-gray-600 dark:text-gray-400 hover:bg-gray-100 dark:hover:bg-gray-800">
        <span>👥</span><span>นักเรียน</span>
        <span class="ml-auto text-xs bg-indigo-100 dark:bg-indigo-900 text-indigo-700 dark:text-indigo-300 px-1.5 py-0.5 rounded-full font-bold">1.2k</span>
      </a>
      <a href="#" onclick="setPage('courses',this)" class="nav-item flex items-center gap-3 px-3 py-2.5 rounded-xl text-sm font-medium transition-colors text-gray-600 dark:text-gray-400 hover:bg-gray-100 dark:hover:bg-gray-800">
        <span>📚</span><span>คอร์สเรียน</span>
      </a>
      <a href="#" onclick="setPage('reports',this)" class="nav-item flex items-center gap-3 px-3 py-2.5 rounded-xl text-sm font-medium transition-colors text-gray-600 dark:text-gray-400 hover:bg-gray-100 dark:hover:bg-gray-800">
        <span>📊</span><span>รายงาน</span>
      </a>
      
      <p class="text-[10px] font-bold tracking-widest text-gray-400 uppercase px-3 py-2 mt-3">System</p>
      
      <a href="#" class="nav-item flex items-center gap-3 px-3 py-2.5 rounded-xl text-sm font-medium transition-colors text-gray-600 dark:text-gray-400 hover:bg-gray-100 dark:hover:bg-gray-800">
        <span>⚙️</span><span>ตั้งค่า</span>
      </a>
    </nav>
    
    <!-- User -->
    <div class="p-3 border-t border-gray-100 dark:border-gray-800">
      <button onclick="showToast('info','ออกจากระบบแล้ว')" class="w-full flex items-center gap-3 px-3 py-2.5 rounded-xl hover:bg-gray-100 dark:hover:bg-gray-800 transition-colors cursor-pointer text-left">
        <div class="size-9 rounded-full bg-gradient-to-br from-indigo-400 to-purple-500 flex-none"></div>
        <div class="flex-1 min-w-0">
          <p class="font-semibold text-sm text-gray-900 dark:text-white truncate">สมชาย ใจดี</p>
          <p class="text-xs text-gray-400 truncate">admin@tailwindpro.co</p>
        </div>
        <span class="text-gray-300 dark:text-gray-600">→</span>
      </button>
    </div>
  </aside>

  <!-- ========== MAIN ========== -->
  <div class="flex-1 flex flex-col overflow-hidden">
    
    <!-- Topbar -->
    <header class="h-16 bg-white dark:bg-gray-900 border-b border-gray-200 dark:border-gray-800 flex items-center px-4 gap-3 flex-none">
      <button onclick="openSidebar()" class="lg:hidden size-9 flex items-center justify-center rounded-xl hover:bg-gray-100 dark:hover:bg-gray-800 text-gray-500 transition-colors">☰</button>
      
      <div class="flex-1 relative max-w-xs hidden sm:block">
        <span class="absolute left-3 top-1/2 -translate-y-1/2 text-gray-400 text-xs">🔍</span>
        <input class="w-full pl-8 pr-4 py-2 text-sm bg-gray-100 dark:bg-gray-800 rounded-xl border-0 focus:outline-none focus:ring-2 focus:ring-indigo-300 dark:focus:ring-indigo-700 text-gray-900 dark:text-white placeholder-gray-400" placeholder="ค้นหา...">
      </div>
      
      <div class="ml-auto flex items-center gap-2">
        <!-- Dark mode toggle -->
        <button onclick="toggleDark()" id="dark-btn" class="size-9 flex items-center justify-center rounded-xl hover:bg-gray-100 dark:hover:bg-gray-800 text-gray-500 dark:text-gray-400 transition-colors">
          <span id="dark-icon">🌙</span>
        </button>
        
        <!-- Notifications -->
        <div class="relative">
          <button onclick="toggleNotif()" class="relative size-9 flex items-center justify-center rounded-xl hover:bg-gray-100 dark:hover:bg-gray-800 text-gray-500 transition-colors">
            🔔
            <span class="absolute -top-0.5 -right-0.5 size-4 bg-red-500 text-white text-[9px] font-bold rounded-full flex items-center justify-center border-2 border-white dark:border-gray-900">3</span>
          </button>
          <div id="notif-dd" class="hidden absolute right-0 top-11 w-72 bg-white dark:bg-gray-900 rounded-2xl shadow-xl border border-gray-200 dark:border-gray-800 overflow-hidden z-50 animate-slide-in">
            <div class="px-4 py-3 border-b border-gray-100 dark:border-gray-800 flex items-center justify-between">
              <span class="font-bold text-sm text-gray-900 dark:text-white">การแจ้งเตือน</span>
              <button class="text-xs text-indigo-600 dark:text-indigo-400 font-medium">อ่านแล้วทั้งหมด</button>
            </div>
            <div class="divide-y divide-gray-50 dark:divide-gray-800">
              <div class="flex gap-3 px-4 py-3.5 bg-indigo-50/50 dark:bg-indigo-950/30 hover:bg-indigo-50 dark:hover:bg-indigo-950/50 transition-colors cursor-pointer">
                <span class="flex-none text-base">✅</span>
                <div><p class="text-xs font-semibold text-gray-900 dark:text-white">สมชาย จบ Part 25</p><p class="text-[11px] text-gray-400 mt-0.5">2 นาทีที่แล้ว</p></div>
                <div class="size-2 bg-indigo-500 rounded-full mt-1.5 ml-auto flex-none"></div>
              </div>
              <div class="flex gap-3 px-4 py-3.5 hover:bg-gray-50 dark:hover:bg-gray-800 cursor-pointer transition-colors">
                <span class="flex-none text-base">💳</span>
                <div><p class="text-xs font-medium text-gray-700 dark:text-gray-300">ชำระเงิน ฿2,490</p><p class="text-[11px] text-gray-400 mt-0.5">1 ชั่วโมงที่แล้ว</p></div>
              </div>
            </div>
          </div>
        </div>
        
        <div class="size-9 rounded-full bg-gradient-to-br from-indigo-400 to-purple-500 cursor-pointer"></div>
      </div>
    </header>

    <!-- Scrollable content -->
    <main id="page-content" class="flex-1 overflow-y-auto p-4 sm:p-6">
      
      <!-- DASHBOARD PAGE -->
      <div id="page-dashboard">
        <!-- Heading -->
        <div class="mb-6">
          <h1 class="text-2xl font-black text-gray-900 dark:text-white">Dashboard</h1>
          <p class="text-gray-500 dark:text-gray-400 text-sm mt-0.5">สวัสดีคุณสมชาย! วันนี้มีอะไรใหม่บ้าง</p>
        </div>

        <!-- Stats -->
        <div class="grid grid-cols-2 xl:grid-cols-4 gap-3 sm:gap-4 mb-6">
          <div class="bg-white dark:bg-gray-900 rounded-2xl p-4 sm:p-5 border border-gray-200 dark:border-gray-800">
            <p class="text-xs text-gray-400 font-medium">รายได้รวม</p>
            <p class="text-xl sm:text-2xl font-black text-gray-900 dark:text-white mt-1">฿284,500</p>
            <p class="text-xs text-green-600 mt-2 font-semibold">↑ 12.5%</p>
          </div>
          <div class="bg-white dark:bg-gray-900 rounded-2xl p-4 sm:p-5 border border-gray-200 dark:border-gray-800">
            <p class="text-xs text-gray-400 font-medium">นักเรียน</p>
            <p class="text-xl sm:text-2xl font-black text-gray-900 dark:text-white mt-1">1,247</p>
            <p class="text-xs text-green-600 mt-2 font-semibold">↑ 8.3%</p>
          </div>
          <div class="bg-white dark:bg-gray-900 rounded-2xl p-4 sm:p-5 border border-gray-200 dark:border-gray-800">
            <p class="text-xs text-gray-400 font-medium">คอร์ส</p>
            <p class="text-xl sm:text-2xl font-black text-gray-900 dark:text-white mt-1">48</p>
            <p class="text-xs text-indigo-600 mt-2 font-semibold">+3 ใหม่</p>
          </div>
          <div class="bg-white dark:bg-gray-900 rounded-2xl p-4 sm:p-5 border border-gray-200 dark:border-gray-800">
            <p class="text-xs text-gray-400 font-medium">อัตราจบ</p>
            <p class="text-xl sm:text-2xl font-black text-gray-900 dark:text-white mt-1">73%</p>
            <div class="h-1.5 bg-gray-100 dark:bg-gray-800 rounded-full mt-2"><div class="h-full w-[73%] bg-green-500 rounded-full"></div></div>
          </div>
        </div>

        <!-- Charts + Activity -->
        <div class="grid xl:grid-cols-3 gap-4 mb-6">
          <div class="xl:col-span-2 bg-white dark:bg-gray-900 rounded-2xl border border-gray-200 dark:border-gray-800 p-5">
            <h3 class="font-bold text-gray-900 dark:text-white mb-5 text-sm">รายได้รายเดือน</h3>
            <div class="flex items-end gap-2 sm:gap-3 h-28 sm:h-36">
              <div class="flex-1 flex flex-col items-center gap-1"><div class="w-full rounded-t-md bg-indigo-200 dark:bg-indigo-800" style="height:60%"></div><span class="text-[9px] text-gray-400">เม.ย.</span></div>
              <div class="flex-1 flex flex-col items-center gap-1"><div class="w-full rounded-t-md bg-indigo-200 dark:bg-indigo-800" style="height:75%"></div><span class="text-[9px] text-gray-400">พ.ค.</span></div>
              <div class="flex-1 flex flex-col items-center gap-1"><div class="w-full rounded-t-md bg-indigo-200 dark:bg-indigo-800" style="height:50%"></div><span class="text-[9px] text-gray-400">มิ.ย.</span></div>
              <div class="flex-1 flex flex-col items-center gap-1"><div class="w-full rounded-t-md bg-indigo-400 dark:bg-indigo-600" style="height:85%"></div><span class="text-[9px] text-gray-400">ก.ค.</span></div>
              <div class="flex-1 flex flex-col items-center gap-1"><div class="w-full rounded-t-md bg-indigo-500 dark:bg-indigo-500" style="height:70%"></div><span class="text-[9px] text-gray-400">ส.ค.</span></div>
              <div class="flex-1 flex flex-col items-center gap-1"><div class="w-full rounded-t-md bg-indigo-700" style="height:100%"></div><span class="text-[9px] text-gray-400">ก.ย.</span></div>
            </div>
          </div>
          
          <div class="bg-white dark:bg-gray-900 rounded-2xl border border-gray-200 dark:border-gray-800 p-5">
            <h3 class="font-bold text-gray-900 dark:text-white mb-4 text-sm">กิจกรรมล่าสุด</h3>
            <div class="space-y-3.5">
              <div class="flex gap-3 items-start"><span class="flex-none text-base">✅</span><div><p class="text-xs font-semibold text-gray-900 dark:text-white">สมใจ จบ Part 20</p><p class="text-[11px] text-gray-400">2 นาที</p></div></div>
              <div class="flex gap-3 items-start"><span class="flex-none text-base">🆕</span><div><p class="text-xs font-semibold text-gray-900 dark:text-white">ผู้เรียนใหม่ลงทะเบียน</p><p class="text-[11px] text-gray-400">15 นาที</p></div></div>
              <div class="flex gap-3 items-start"><span class="flex-none text-base">⭐</span><div><p class="text-xs font-semibold text-gray-900 dark:text-white">รีวิว 5 ดาวใหม่</p><p class="text-[11px] text-gray-400">1 ชั่วโมง</p></div></div>
              <div class="flex gap-3 items-start"><span class="flex-none text-base">💳</span><div><p class="text-xs font-semibold text-gray-900 dark:text-white">ชำระเงิน ฿2,490</p><p class="text-[11px] text-gray-400">2 ชั่วโมง</p></div></div>
            </div>
          </div>
        </div>

        <!-- Quick actions -->
        <div class="grid grid-cols-2 sm:grid-cols-4 gap-3">
          <button onclick="showToast('success','สร้างคอร์สสำเร็จ!')" class="flex items-center gap-2 px-4 py-3 bg-indigo-600 hover:bg-indigo-700 text-white text-xs font-semibold rounded-xl transition-colors">
            <span>+</span> คอร์สใหม่
          </button>
          <button onclick="showModal()" class="flex items-center gap-2 px-4 py-3 bg-white dark:bg-gray-900 border border-gray-200 dark:border-gray-800 hover:bg-gray-50 text-gray-700 dark:text-gray-300 text-xs font-semibold rounded-xl transition-colors">
            <span>👤</span> เพิ่มนักเรียน
          </button>
          <button onclick="showToast('info','กำลังสร้างรายงาน...')" class="flex items-center gap-2 px-4 py-3 bg-white dark:bg-gray-900 border border-gray-200 dark:border-gray-800 hover:bg-gray-50 text-gray-700 dark:text-gray-300 text-xs font-semibold rounded-xl transition-colors">
            <span>📊</span> Export รายงาน
          </button>
          <button onclick="openDrawer()" class="flex items-center gap-2 px-4 py-3 bg-white dark:bg-gray-900 border border-gray-200 dark:border-gray-800 hover:bg-gray-50 text-gray-700 dark:text-gray-300 text-xs font-semibold rounded-xl transition-colors">
            <span>⚙️</span> ตั้งค่า
          </button>
        </div>
      </div>

      <!-- STUDENTS PAGE -->
      <div id="page-students" class="hidden">
        <div class="flex items-center justify-between mb-6">
          <div><h1 class="text-2xl font-black text-gray-900 dark:text-white">นักเรียน</h1><p class="text-gray-400 text-sm">จัดการบัญชีนักเรียนทั้งหมด</p></div>
          <button onclick="showToast('success','เพิ่มนักเรียนสำเร็จ!')" class="px-5 py-2.5 bg-indigo-600 text-white text-sm font-semibold rounded-xl hover:bg-indigo-700">+ เพิ่มนักเรียน</button>
        </div>
        <div class="bg-white dark:bg-gray-900 rounded-2xl border border-gray-200 dark:border-gray-800 overflow-hidden">
          <div class="overflow-x-auto">
            <table class="w-full text-sm">
              <thead class="bg-gray-50 dark:bg-gray-800 border-b border-gray-200 dark:border-gray-700">
                <tr>
                  <th class="text-left py-3.5 px-4 text-xs font-semibold text-gray-500 dark:text-gray-400 uppercase">ชื่อ</th>
                  <th class="text-left py-3.5 px-4 text-xs font-semibold text-gray-500 dark:text-gray-400 uppercase hidden md:table-cell">อีเมล</th>
                  <th class="text-center py-3.5 px-4 text-xs font-semibold text-gray-500 dark:text-gray-400 uppercase">สถานะ</th>
                  <th class="text-right py-3.5 px-4 text-xs font-semibold text-gray-500 dark:text-gray-400 uppercase">จัดการ</th>
                </tr>
              </thead>
              <tbody class="divide-y divide-gray-100 dark:divide-gray-800">
                <tr class="hover:bg-gray-50 dark:hover:bg-gray-800 transition-colors"><td class="py-4 px-4 font-medium text-gray-900 dark:text-white">สมชาย ใจดี</td><td class="py-4 px-4 text-gray-500 hidden md:table-cell">somchai@co.th</td><td class="py-4 px-4 text-center"><span class="text-xs font-semibold bg-green-100 dark:bg-green-900/30 text-green-700 dark:text-green-400 px-2.5 py-1 rounded-full">Active</span></td><td class="py-4 px-4 text-right"><button onclick="showToast('info','กำลังแก้ไข...')" class="text-indigo-600 text-xs hover:underline">แก้ไข</button></td></tr>
                <tr class="hover:bg-gray-50 dark:hover:bg-gray-800 transition-colors"><td class="py-4 px-4 font-medium text-gray-900 dark:text-white">สมหญิง ใจงาม</td><td class="py-4 px-4 text-gray-500 hidden md:table-cell">somying@co.th</td><td class="py-4 px-4 text-center"><span class="text-xs font-semibold bg-yellow-100 dark:bg-yellow-900/30 text-yellow-700 dark:text-yellow-400 px-2.5 py-1 rounded-full">Pending</span></td><td class="py-4 px-4 text-right"><button onclick="showToast('info','กำลังแก้ไข...')" class="text-indigo-600 text-xs hover:underline">แก้ไข</button></td></tr>
                <tr class="hover:bg-gray-50 dark:hover:bg-gray-800 transition-colors"><td class="py-4 px-4 font-medium text-gray-900 dark:text-white">มานะ ขยัน</td><td class="py-4 px-4 text-gray-500 hidden md:table-cell">mana@co.th</td><td class="py-4 px-4 text-center"><span class="text-xs font-semibold bg-red-100 dark:bg-red-900/30 text-red-700 dark:text-red-400 px-2.5 py-1 rounded-full">Inactive</span></td><td class="py-4 px-4 text-right"><button onclick="showToast('info','กำลังแก้ไข...')" class="text-indigo-600 text-xs hover:underline">แก้ไข</button></td></tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>

      <!-- COURSES PAGE -->
      <div id="page-courses" class="hidden">
        <div class="mb-6"><h1 class="text-2xl font-black text-gray-900 dark:text-white">คอร์สเรียน</h1><p class="text-gray-400 text-sm">จัดการเนื้อหาและคอร์ส</p></div>
        <div class="grid sm:grid-cols-2 lg:grid-cols-3 gap-4">
          <div class="bg-white dark:bg-gray-900 rounded-2xl border border-gray-200 dark:border-gray-800 overflow-hidden hover:shadow-lg transition-shadow">
            <div class="h-32 bg-gradient-to-br from-indigo-400 to-purple-600"></div>
            <div class="p-4"><h3 class="font-bold text-gray-900 dark:text-white text-sm">Tailwind CSS Mastery</h3><p class="text-xs text-gray-400 mt-1">842 นักเรียน • 30 Parts</p><div class="mt-3 flex items-center justify-between"><span class="text-xs font-bold text-green-600 bg-green-50 dark:bg-green-900/30 px-2 py-0.5 rounded-full">เผยแพร่</span><button onclick="showToast('info','กำลังแก้ไขคอร์ส...')" class="text-xs text-indigo-600 font-medium hover:underline">แก้ไข</button></div></div>
          </div>
          <div class="bg-white dark:bg-gray-900 rounded-2xl border border-gray-200 dark:border-gray-800 overflow-hidden hover:shadow-lg transition-shadow">
            <div class="h-32 bg-gradient-to-br from-pink-400 to-rose-600"></div>
            <div class="p-4"><h3 class="font-bold text-gray-900 dark:text-white text-sm">React with Tailwind</h3><p class="text-xs text-gray-400 mt-1">405 นักเรียน • 20 Parts</p><div class="mt-3 flex items-center justify-between"><span class="text-xs font-bold text-green-600 bg-green-50 dark:bg-green-900/30 px-2 py-0.5 rounded-full">เผยแพร่</span><button onclick="showToast('info','กำลังแก้ไขคอร์ส...')" class="text-xs text-indigo-600 font-medium hover:underline">แก้ไข</button></div></div>
          </div>
          <div class="bg-white dark:bg-gray-900 rounded-2xl border border-dashed border-gray-300 dark:border-gray-700 flex items-center justify-center h-[174px] cursor-pointer hover:bg-gray-50 dark:hover:bg-gray-800 transition-colors" onclick="showToast('info','สร้างคอร์สใหม่...')">
            <div class="text-center"><span class="text-3xl text-gray-300">+</span><p class="text-sm text-gray-400 mt-1">สร้างคอร์สใหม่</p></div>
          </div>
        </div>
      </div>

      <!-- REPORTS PAGE -->
      <div id="page-reports" class="hidden">
        <div class="mb-6"><h1 class="text-2xl font-black text-gray-900 dark:text-white">รายงาน</h1><p class="text-gray-400 text-sm">ภาพรวมข้อมูลเชิงลึก</p></div>
        <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
          <div class="bg-white dark:bg-gray-900 rounded-2xl p-5 border border-gray-200 dark:border-gray-800 text-center"><p class="text-3xl font-black text-indigo-600 mb-1">฿284K</p><p class="text-sm text-gray-500">รายได้รวม</p></div>
          <div class="bg-white dark:bg-gray-900 rounded-2xl p-5 border border-gray-200 dark:border-gray-800 text-center"><p class="text-3xl font-black text-purple-600 mb-1">73%</p><p class="text-sm text-gray-500">อัตราจบ</p></div>
          <div class="bg-white dark:bg-gray-900 rounded-2xl p-5 border border-gray-200 dark:border-gray-800 text-center"><p class="text-3xl font-black text-green-600 mb-1">4.9</p><p class="text-sm text-gray-500">คะแนนเฉลี่ย</p></div>
        </div>
      </div>

    </main>
  </div>
</div>

<!-- Modal -->
<div id="modal" class="hidden fixed inset-0 z-50 flex items-center justify-center p-4">
  <div onclick="document.getElementById('modal').classList.add('hidden')" class="absolute inset-0 bg-black/50 backdrop-blur-sm"></div>
  <div class="relative bg-white dark:bg-gray-900 rounded-2xl shadow-2xl w-full max-w-sm p-6">
    <h3 class="font-black text-lg text-gray-900 dark:text-white mb-4">เพิ่มนักเรียนใหม่</h3>
    <div class="space-y-3 mb-5">
      <input placeholder="ชื่อ-นามสกุล" class="w-full px-4 py-3 text-sm rounded-xl border border-gray-200 dark:border-gray-700 bg-white dark:bg-gray-800 text-gray-900 dark:text-white focus:outline-none focus:ring-2 focus:ring-indigo-200">
      <input placeholder="อีเมล" type="email" class="w-full px-4 py-3 text-sm rounded-xl border border-gray-200 dark:border-gray-700 bg-white dark:bg-gray-800 text-gray-900 dark:text-white focus:outline-none focus:ring-2 focus:ring-indigo-200">
    </div>
    <div class="flex gap-3">
      <button onclick="document.getElementById('modal').classList.add('hidden')" class="flex-1 py-2.5 border border-gray-200 dark:border-gray-700 text-gray-600 dark:text-gray-400 text-sm rounded-xl hover:bg-gray-50 dark:hover:bg-gray-800">ยกเลิก</button>
      <button onclick="document.getElementById('modal').classList.add('hidden');showToast('success','เพิ่มนักเรียนสำเร็จ!')" class="flex-1 py-2.5 bg-indigo-600 text-white text-sm font-semibold rounded-xl hover:bg-indigo-700">เพิ่ม</button>
    </div>
  </div>
</div>

<!-- Drawer -->
<div id="drawer-overlay" class="hidden fixed inset-0 z-50 flex justify-end">
  <div onclick="closeDrawer()" class="absolute inset-0 bg-black/40 backdrop-blur-sm"></div>
  <div id="drawer-panel" class="relative bg-white dark:bg-gray-900 w-72 h-full flex flex-col shadow-2xl border-l border-gray-200 dark:border-gray-800" style="transform:translateX(100%); transition:transform 0.3s">
    <div class="p-5 border-b border-gray-100 dark:border-gray-800 flex items-center justify-between">
      <h3 class="font-bold text-gray-900 dark:text-white">การตั้งค่า</h3>
      <button onclick="closeDrawer()" class="text-gray-400 hover:text-gray-700 dark:hover:text-gray-200">✕</button>
    </div>
    <div class="flex-1 p-5 space-y-4">
      <label class="flex items-center justify-between">
        <span class="text-sm text-gray-700 dark:text-gray-300">Dark Mode</span>
        <div class="relative cursor-pointer" onclick="toggleDark()">
          <div id="drawer-dark-bg" class="w-11 h-6 bg-gray-200 rounded-full transition-colors"></div>
          <div id="drawer-dark-dot" class="absolute top-0.5 left-0.5 w-5 h-5 bg-white rounded-full shadow transition-transform"></div>
        </div>
      </label>
    </div>
  </div>
</div>

<script>
// Page navigation
function setPage(page, el) {
  ['dashboard','students','courses','reports'].forEach(p => document.getElementById(`page-${p}`).classList.add('hidden'));
  document.getElementById(`page-${page}`).classList.remove('hidden');
  document.querySelectorAll('.nav-item').forEach(a => {
    a.className = a.className.replace(/\bactive\b/,'').replace('bg-indigo-50 text-indigo-600 dark:bg-indigo-950/50 dark:text-indigo-400 font-semibold','').trim();
    a.classList.add('text-gray-600','dark:text-gray-400','hover:bg-gray-100','dark:hover:bg-gray-800');
  });
  if (el) {
    el.classList.remove('text-gray-600','dark:text-gray-400','hover:bg-gray-100','dark:hover:bg-gray-800');
    el.classList.add('active','bg-indigo-50','text-indigo-600','dark:bg-indigo-950/50','dark:text-indigo-400','font-semibold');
  }
  closeSidebar();
}

// Sidebar mobile
function openSidebar() {
  document.getElementById('sidebar').classList.remove('-translate-x-full');
  document.getElementById('mobile-overlay').classList.remove('hidden');
}
function closeSidebar() {
  document.getElementById('sidebar').classList.add('-translate-x-full');
  document.getElementById('mobile-overlay').classList.add('hidden');
}

// Dark mode
let dark = false;
function toggleDark() {
  dark = !dark;
  document.documentElement.classList.toggle('dark', dark);
  document.getElementById('dark-icon').textContent = dark ? '☀️' : '🌙';
  const bg = document.getElementById('drawer-dark-bg');
  const dot = document.getElementById('drawer-dark-dot');
  if (bg) bg.className = `w-11 h-6 ${dark ? 'bg-indigo-600' : 'bg-gray-200'} rounded-full transition-colors`;
  if (dot) dot.style.transform = dark ? 'translateX(20px)' : 'translateX(0)';
}

// Toast
let tn = 0;
function showToast(type, msg, ms=3500) {
  const id = `t${++tn}`;
  const cfg = {
    success: { ic:'✅', bg:'bg-white dark:bg-gray-800 border-l-4 border-l-green-500' },
    error:   { ic:'❌', bg:'bg-white dark:bg-gray-800 border-l-4 border-l-red-500' },
    warning: { ic:'⚠️', bg:'bg-white dark:bg-gray-800 border-l-4 border-l-yellow-500' },
    info:    { ic:'ℹ️', bg:'bg-white dark:bg-gray-800 border-l-4 border-l-blue-500' },
  }[type] || { ic:'ℹ️', bg:'bg-white border border-gray-200' };
  
  const el = document.createElement('div');
  el.id = id;
  el.className = `pointer-events-auto shadow-lg rounded-xl border border-gray-200 dark:border-gray-700 ${cfg.bg} t-in`;
  el.innerHTML = `<div class="flex items-center gap-3 px-4 py-3"><span class="text-base">${cfg.ic}</span><p class="flex-1 text-sm font-medium text-gray-900 dark:text-white">${msg}</p><button onclick="dismiss('${id}')" class="text-gray-300 hover:text-gray-600 dark:hover:text-gray-200 text-sm">✕</button></div>`;
  document.getElementById('toasts').appendChild(el);
  el._t = setTimeout(() => dismiss(id), ms);
}

function dismiss(id) {
  const el = document.getElementById(id); if(!el) return;
  clearTimeout(el._t);
  el.classList.replace('t-in','t-out');
  setTimeout(() => el.remove(), 300);
}

// Notification dropdown
function toggleNotif() {
  document.getElementById('notif-dd').classList.toggle('hidden');
}
document.addEventListener('click', e => {
  if (!e.target.closest('#notif-dd') && !e.target.closest('button[onclick="toggleNotif()"]')) {
    const dd = document.getElementById('notif-dd');
    if (dd) dd.classList.add('hidden');
  }
});

// Modal
function showModal() { document.getElementById('modal').classList.remove('hidden'); }

// Drawer
function openDrawer() {
  document.getElementById('drawer-overlay').classList.remove('hidden');
  requestAnimationFrame(() => document.getElementById('drawer-panel').style.transform = 'translateX(0)');
}
function closeDrawer() {
  document.getElementById('drawer-panel').style.transform = 'translateX(100%)';
  setTimeout(() => document.getElementById('drawer-overlay').classList.add('hidden'), 300);
}

document.addEventListener('keydown', e => {
  if (e.key === 'Escape') { closeDrawer(); document.getElementById('modal').classList.add('hidden'); }
});
</script>

</body>
</html>
```

---

## 📊 สรุปสิ่งที่สร้างใน Project 2

| Feature | Parts ที่ใช้ |
|---------|------------|
| Sidebar + Topbar | Part 25 |
| KPI Cards + Chart | Part 25 |
| Data Table | Part 26 |
| Modal + Drawer | Part 28 |
| Toast System | Part 29 |
| Dark Mode | Part 22 |
| Custom Config | Part 21 |
| Component patterns | Part 24 |

---

## 🏆 สรุป Level 2 (Parts 21–30)

| Part | หัวข้อ | Steps |
|------|-------|-------|
| 21 | tailwind.config.js | 201–210 |
| 22 | Dark Mode | 211–220 |
| 23 | Custom Plugins | 221–230 |
| 24 | Component Patterns | 231–240 |
| 25 | Dashboard Layout | 241–250 |
| 26 | Data Tables | 251–260 |
| 27 | Multi-step Form | 261–270 |
| 28 | Modal & Dialog | 271–280 |
| 29 | Notification System | 281–290 |
| **30** | **Project 2 — Dashboard** | **291–300** |

**Level 2 Complete! พร้อมเข้า Level 3: Advanced 🚀**

---

*Part 30 — Project 2: Dashboard | Steps 291–300 จาก 1,000 Steps*
