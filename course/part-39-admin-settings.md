# Part 39: Admin Settings Pages

## เป้าหมาย
- สร้าง Settings Pages ระดับ Enterprise
- Profile, Notifications, Billing, Team, API Keys
- Tabbed Navigation, Form Patterns, Danger Zone

---

## Step 381: Settings Layout & Navigation

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Settings Layout - Step 381</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 text-gray-900">

  <div class="max-w-5xl mx-auto px-4 py-8">
    <h1 class="text-2xl font-extrabold mb-6">การตั้งค่า</h1>

    <div class="grid grid-cols-1 lg:grid-cols-[220px_1fr] gap-6">

      <!-- Sidebar Nav -->
      <nav class="bg-white rounded-xl border p-3 h-fit sticky top-6">
        <ul class="space-y-0.5">
          <li>
            <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-lg bg-indigo-50 text-indigo-700 font-medium text-sm">
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z"/></svg>
              โปรไฟล์
            </a>
          </li>
          <li>
            <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-lg text-gray-600 hover:bg-gray-50 text-sm transition-colors">
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z"/></svg>
              ความปลอดภัย
            </a>
          </li>
          <li>
            <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-lg text-gray-600 hover:bg-gray-50 text-sm transition-colors">
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6.002 6.002 0 00-4-5.659V5a2 2 0 10-4 0v.341C7.67 6.165 6 8.388 6 11v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9"/></svg>
              การแจ้งเตือน
            </a>
          </li>
          <li>
            <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-lg text-gray-600 hover:bg-gray-50 text-sm transition-colors">
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 10h18M7 15h1m4 0h1m-7 4h12a3 3 0 003-3V8a3 3 0 00-3-3H6a3 3 0 00-3 3v8a3 3 0 003 3z"/></svg>
              การชำระเงิน
            </a>
          </li>
          <li>
            <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-lg text-gray-600 hover:bg-gray-50 text-sm transition-colors">
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0z"/></svg>
              ทีม
            </a>
          </li>
          <li>
            <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-lg text-gray-600 hover:bg-gray-50 text-sm transition-colors">
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 7a2 2 0 012 2m4 0a6 6 0 01-7.743 5.743L11 17H9v2H7v2H4a1 1 0 01-1-1v-2.586a1 1 0 01.293-.707l5.964-5.964A6 6 0 1121 9z"/></svg>
              API Keys
            </a>
          </li>
          <li class="pt-2 mt-2 border-t">
            <a href="#" class="flex items-center gap-3 px-3 py-2.5 rounded-lg text-red-600 hover:bg-red-50 text-sm transition-colors">
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z"/></svg>
              Danger Zone
            </a>
          </li>
        </ul>
      </nav>

      <!-- Settings Content -->
      <div class="space-y-6">

        <!-- Profile Section -->
        <div class="bg-white rounded-xl border p-6">
          <h2 class="text-lg font-bold mb-5">โปรไฟล์</h2>

          <!-- Avatar -->
          <div class="flex items-center gap-5 mb-6 pb-6 border-b">
            <div class="relative">
              <img src="https://i.pravatar.cc/80?img=1" class="w-16 h-16 rounded-full" alt="">
              <button class="absolute -bottom-1 -right-1 w-7 h-7 bg-indigo-600 rounded-full flex items-center justify-center text-white hover:bg-indigo-700 transition-colors shadow-md">
                <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 9a2 2 0 012-2h.93a2 2 0 001.664-.89l.812-1.22A2 2 0 0110.07 4h3.86a2 2 0 011.664.89l.812 1.22A2 2 0 0018.07 7H19a2 2 0 012 2v9a2 2 0 01-2 2H5a2 2 0 01-2-2V9z"/><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 13a3 3 0 11-6 0 3 3 0 016 0z"/></svg>
              </button>
            </div>
            <div>
              <p class="font-semibold">สมชาย เทคโน</p>
              <p class="text-sm text-gray-500 mb-2">JPG, GIF or PNG · สูงสุด 5MB</p>
              <div class="flex gap-2">
                <button class="text-xs bg-white border border-gray-300 text-gray-700 px-3 py-1.5 rounded-lg hover:bg-gray-50 transition-colors">อัพโหลด</button>
                <button class="text-xs text-red-500 hover:text-red-700 transition-colors">ลบรูป</button>
              </div>
            </div>
          </div>

          <!-- Form -->
          <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
            <div>
              <label class="block text-sm font-medium text-gray-700 mb-1.5">ชื่อ</label>
              <input type="text" value="สมชาย" class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
            </div>
            <div>
              <label class="block text-sm font-medium text-gray-700 mb-1.5">นามสกุล</label>
              <input type="text" value="เทคโน" class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
            </div>
            <div class="md:col-span-2">
              <label class="block text-sm font-medium text-gray-700 mb-1.5">Email</label>
              <input type="email" value="somchai@acme.co.th" class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
            </div>
            <div class="md:col-span-2">
              <label class="block text-sm font-medium text-gray-700 mb-1.5">Bio</label>
              <textarea rows="3" class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 resize-none" placeholder="เกี่ยวกับตัวคุณ...">Senior Frontend Developer ผู้ชื่นชอบ Tailwind CSS และ Clean Code</textarea>
            </div>
            <div>
              <label class="block text-sm font-medium text-gray-700 mb-1.5">บทบาท</label>
              <select class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
                <option selected>Developer</option>
                <option>Designer</option>
                <option>Manager</option>
              </select>
            </div>
            <div>
              <label class="block text-sm font-medium text-gray-700 mb-1.5">ภาษา</label>
              <select class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
                <option selected>ภาษาไทย</option>
                <option>English</option>
              </select>
            </div>
          </div>

          <div class="flex justify-end mt-5 pt-5 border-t gap-3">
            <button class="border border-gray-300 text-gray-700 px-4 py-2 rounded-xl text-sm hover:bg-gray-50 transition-colors">ยกเลิก</button>
            <button class="bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">บันทึกการเปลี่ยนแปลง</button>
          </div>
        </div>

      </div>
    </div>
  </div>

</body>
</html>
```

---

## Step 382: Notification Settings

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Notification Settings - Step 382</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-8">

  <div class="max-w-2xl mx-auto">
    <h2 class="text-xl font-bold mb-6">การแจ้งเตือน</h2>

    <div class="space-y-4">

      <!-- Email Notifications -->
      <div class="bg-white rounded-xl border p-5">
        <div class="flex items-center gap-3 mb-4">
          <div class="w-9 h-9 bg-blue-100 rounded-xl flex items-center justify-center">📧</div>
          <div>
            <p class="font-semibold text-sm">Email Notifications</p>
            <p class="text-xs text-gray-500">รับการแจ้งเตือนผ่าน Email</p>
          </div>
        </div>
        <div class="space-y-3">
          <label class="flex items-center justify-between cursor-pointer">
            <div>
              <p class="text-sm font-medium">บทสรุปรายสัปดาห์</p>
              <p class="text-xs text-gray-500">สรุป activity ของทีมทุกวันจันทร์</p>
            </div>
            <input type="checkbox" checked class="toggle-check w-5 h-5 rounded border-gray-300 text-indigo-600 focus:ring-indigo-500">
          </label>
          <hr>
          <label class="flex items-center justify-between cursor-pointer">
            <div>
              <p class="text-sm font-medium">เมื่อมีการกล่าวถึงคุณ</p>
              <p class="text-xs text-gray-500">แจ้งเมื่อมีคนกล่าวถึงใน comment</p>
            </div>
            <input type="checkbox" checked class="w-5 h-5 rounded border-gray-300 text-indigo-600 focus:ring-indigo-500">
          </label>
          <hr>
          <label class="flex items-center justify-between cursor-pointer">
            <div>
              <p class="text-sm font-medium">โปรโมชัน & ฟีเจอร์ใหม่</p>
              <p class="text-xs text-gray-500">ข่าวสาร product update</p>
            </div>
            <input type="checkbox" class="w-5 h-5 rounded border-gray-300 text-indigo-600 focus:ring-indigo-500">
          </label>
        </div>
      </div>

      <!-- Push Notifications -->
      <div class="bg-white rounded-xl border p-5">
        <div class="flex items-center gap-3 mb-4">
          <div class="w-9 h-9 bg-purple-100 rounded-xl flex items-center justify-center">🔔</div>
          <div>
            <p class="font-semibold text-sm">Push Notifications</p>
            <p class="text-xs text-gray-500">แจ้งเตือนผ่าน Browser</p>
          </div>
          <button class="ml-auto relative bg-indigo-600 rounded-full w-11 h-6 transition-colors">
            <span class="block w-5 h-5 bg-white rounded-full absolute top-0.5 left-5 transition-transform shadow-sm"></span>
          </button>
        </div>
        <div class="space-y-3">
          <label class="flex items-center justify-between cursor-pointer">
            <div>
              <p class="text-sm font-medium">Alert ระดับ Critical</p>
              <p class="text-xs text-gray-500">แจ้งทันทีเมื่อระบบมีปัญหา</p>
            </div>
            <input type="checkbox" checked class="w-5 h-5 rounded border-gray-300 text-indigo-600 focus:ring-indigo-500">
          </label>
          <hr>
          <label class="flex items-center justify-between cursor-pointer">
            <div>
              <p class="text-sm font-medium">Automation สำเร็จ/ล้มเหลว</p>
              <p class="text-xs text-gray-500">ผลลัพธ์ของ automation ที่รัน</p>
            </div>
            <input type="checkbox" checked class="w-5 h-5 rounded border-gray-300 text-indigo-600 focus:ring-indigo-500">
          </label>
        </div>
      </div>

      <!-- Digest Frequency -->
      <div class="bg-white rounded-xl border p-5">
        <p class="font-semibold text-sm mb-3">ความถี่ของ Email Digest</p>
        <div class="space-y-2">
          <label class="flex items-center gap-3 cursor-pointer">
            <input type="radio" name="freq" class="text-indigo-600">
            <span class="text-sm text-gray-700">ทุกวัน</span>
          </label>
          <label class="flex items-center gap-3 cursor-pointer">
            <input type="radio" name="freq" checked class="text-indigo-600">
            <span class="text-sm text-gray-700">ทุกสัปดาห์ (แนะนำ)</span>
          </label>
          <label class="flex items-center gap-3 cursor-pointer">
            <input type="radio" name="freq" class="text-indigo-600">
            <span class="text-sm text-gray-700">ทุกเดือน</span>
          </label>
          <label class="flex items-center gap-3 cursor-pointer">
            <input type="radio" name="freq" class="text-indigo-600">
            <span class="text-sm text-gray-700">ปิดทั้งหมด</span>
          </label>
        </div>
      </div>

      <button class="bg-indigo-600 text-white px-5 py-2.5 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">
        บันทึกการตั้งค่า
      </button>
    </div>
  </div>

</body>
</html>
```

---

## Step 383: Billing Settings

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Billing - Step 383</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-8">

  <div class="max-w-2xl mx-auto space-y-5">
    <h2 class="text-xl font-bold">การชำระเงิน</h2>

    <!-- Current Plan -->
    <div class="bg-white rounded-xl border p-5">
      <div class="flex items-center justify-between mb-4">
        <h3 class="font-semibold">แผนปัจจุบัน</h3>
        <span class="bg-indigo-100 text-indigo-700 text-xs font-bold px-3 py-1 rounded-full">Pro</span>
      </div>
      <div class="flex items-end justify-between">
        <div>
          <p class="text-3xl font-extrabold">฿2,490<span class="text-base font-normal text-gray-500">/เดือน</span></p>
          <p class="text-sm text-gray-500 mt-1">รอบถัดไป: 1 ก.ค. 2024</p>
        </div>
        <div class="flex gap-2">
          <button class="border border-gray-300 text-gray-700 text-sm px-3 py-1.5 rounded-lg hover:bg-gray-50 transition-colors">เปลี่ยนแผน</button>
          <button class="text-red-500 text-sm px-3 py-1.5 rounded-lg hover:bg-red-50 transition-colors">ยกเลิก</button>
        </div>
      </div>
      <!-- Usage Bar -->
      <div class="mt-4 pt-4 border-t">
        <div class="flex justify-between text-xs text-gray-500 mb-1.5">
          <span>ผู้ใช้งาน</span>
          <span>18 / 25 คน</span>
        </div>
        <div class="w-full bg-gray-100 rounded-full h-1.5">
          <div class="bg-indigo-600 h-1.5 rounded-full" style="width: 72%"></div>
        </div>
      </div>
    </div>

    <!-- Payment Method -->
    <div class="bg-white rounded-xl border p-5">
      <div class="flex items-center justify-between mb-4">
        <h3 class="font-semibold">วิธีการชำระเงิน</h3>
        <button class="text-indigo-600 text-sm hover:underline">+ เพิ่มบัตร</button>
      </div>
      <div class="space-y-3">
        <div class="flex items-center justify-between p-3 bg-gray-50 rounded-xl border border-indigo-200">
          <div class="flex items-center gap-3">
            <div class="w-10 h-7 bg-gradient-to-r from-blue-600 to-blue-400 rounded flex items-center justify-center text-white text-xs font-bold">VISA</div>
            <div>
              <p class="text-sm font-medium">•••• •••• •••• 4242</p>
              <p class="text-xs text-gray-500">หมดอายุ 12/2026</p>
            </div>
          </div>
          <div class="flex items-center gap-3">
            <span class="bg-green-100 text-green-700 text-xs px-2 py-0.5 rounded-full font-medium">ค่าเริ่มต้น</span>
            <button class="text-gray-400 hover:text-red-500 transition-colors text-sm">✕</button>
          </div>
        </div>
        <div class="flex items-center justify-between p-3 bg-white rounded-xl border">
          <div class="flex items-center gap-3">
            <div class="w-10 h-7 bg-gradient-to-r from-orange-500 to-red-500 rounded flex items-center justify-center text-white text-xs font-bold">MC</div>
            <div>
              <p class="text-sm font-medium">•••• •••• •••• 8888</p>
              <p class="text-xs text-gray-500">หมดอายุ 8/2025</p>
            </div>
          </div>
          <div class="flex items-center gap-2">
            <button class="text-xs text-indigo-600 hover:underline">ตั้งเป็นค่าเริ่มต้น</button>
            <button class="text-gray-400 hover:text-red-500 transition-colors text-sm">✕</button>
          </div>
        </div>
      </div>
    </div>

    <!-- Invoice History -->
    <div class="bg-white rounded-xl border p-5">
      <h3 class="font-semibold mb-4">ประวัติใบแจ้งหนี้</h3>
      <div class="space-y-0 divide-y">
        <div class="flex items-center justify-between py-3">
          <div>
            <p class="text-sm font-medium">มิ.ย. 2024</p>
            <p class="text-xs text-gray-500">Pro Plan · 1 เดือน</p>
          </div>
          <div class="flex items-center gap-3">
            <span class="text-sm font-semibold">฿2,490</span>
            <span class="bg-green-100 text-green-700 text-xs px-2 py-0.5 rounded-full">จ่ายแล้ว</span>
            <button class="text-indigo-600 text-xs hover:underline">PDF</button>
          </div>
        </div>
        <div class="flex items-center justify-between py-3">
          <div>
            <p class="text-sm font-medium">พ.ค. 2024</p>
            <p class="text-xs text-gray-500">Pro Plan · 1 เดือน</p>
          </div>
          <div class="flex items-center gap-3">
            <span class="text-sm font-semibold">฿2,490</span>
            <span class="bg-green-100 text-green-700 text-xs px-2 py-0.5 rounded-full">จ่ายแล้ว</span>
            <button class="text-indigo-600 text-xs hover:underline">PDF</button>
          </div>
        </div>
        <div class="flex items-center justify-between py-3">
          <div>
            <p class="text-sm font-medium">เม.ย. 2024</p>
            <p class="text-xs text-gray-500">Starter Plan → Pro (Upgrade)</p>
          </div>
          <div class="flex items-center gap-3">
            <span class="text-sm font-semibold">฿1,500</span>
            <span class="bg-green-100 text-green-700 text-xs px-2 py-0.5 rounded-full">จ่ายแล้ว</span>
            <button class="text-indigo-600 text-xs hover:underline">PDF</button>
          </div>
        </div>
      </div>
    </div>

  </div>
</body>
</html>
```

---

## Step 384: Team Management

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Team Settings - Step 384</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-8">

  <div class="max-w-3xl mx-auto space-y-5">
    <div class="flex items-center justify-between">
      <h2 class="text-xl font-bold">จัดการทีม</h2>
      <button onclick="document.getElementById('invite-modal').classList.remove('hidden')"
        class="bg-indigo-600 text-white text-sm px-4 py-2 rounded-lg hover:bg-indigo-700 transition-colors flex items-center gap-2">
        <span>+</span> เชิญสมาชิก
      </button>
    </div>

    <!-- Pending Invites -->
    <div class="bg-white rounded-xl border p-5">
      <h3 class="font-semibold mb-4 text-sm text-gray-500 uppercase tracking-wide">รอการตอบรับ</h3>
      <div class="space-y-2">
        <div class="flex items-center justify-between p-3 bg-yellow-50 border border-yellow-200 rounded-xl">
          <div class="flex items-center gap-3">
            <div class="w-8 h-8 bg-yellow-200 rounded-full flex items-center justify-center text-yellow-700 font-bold text-sm">?</div>
            <div>
              <p class="text-sm font-medium">design@acme.co.th</p>
              <p class="text-xs text-gray-500">เชิญเมื่อ 2 วันที่แล้ว</p>
            </div>
          </div>
          <div class="flex items-center gap-2">
            <span class="text-xs bg-yellow-100 text-yellow-700 px-2 py-0.5 rounded-full font-medium">Editor</span>
            <button class="text-xs text-gray-500 hover:text-red-500 transition-colors">ยกเลิก</button>
          </div>
        </div>
      </div>
    </div>

    <!-- Team Members -->
    <div class="bg-white rounded-xl border p-5">
      <h3 class="font-semibold mb-4 text-sm text-gray-500 uppercase tracking-wide">สมาชิก (18)</h3>
      <div class="space-y-1">
        <!-- Member row -->
        <div class="flex items-center justify-between p-3 hover:bg-gray-50 rounded-xl transition-colors">
          <div class="flex items-center gap-3">
            <img src="https://i.pravatar.cc/36?img=1" class="w-9 h-9 rounded-full" alt="">
            <div>
              <p class="text-sm font-medium">สมชาย เทคโน <span class="text-indigo-600 text-xs">(คุณ)</span></p>
              <p class="text-xs text-gray-500">somchai@acme.co.th</p>
            </div>
          </div>
          <div class="flex items-center gap-3">
            <span class="bg-indigo-100 text-indigo-700 text-xs font-semibold px-2.5 py-1 rounded-full">Owner</span>
          </div>
        </div>

        <div class="flex items-center justify-between p-3 hover:bg-gray-50 rounded-xl transition-colors">
          <div class="flex items-center gap-3">
            <img src="https://i.pravatar.cc/36?img=2" class="w-9 h-9 rounded-full" alt="">
            <div>
              <p class="text-sm font-medium">วิไล สวัสดี</p>
              <p class="text-xs text-gray-500">wilai@acme.co.th</p>
            </div>
          </div>
          <div class="flex items-center gap-3">
            <select class="text-xs border border-gray-200 rounded-lg px-2 py-1 focus:outline-none focus:ring-1 focus:ring-indigo-500">
              <option>Admin</option>
              <option>Editor</option>
              <option>Viewer</option>
            </select>
            <button class="text-gray-400 hover:text-red-500 transition-colors">
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"/></svg>
            </button>
          </div>
        </div>

        <div class="flex items-center justify-between p-3 hover:bg-gray-50 rounded-xl transition-colors">
          <div class="flex items-center gap-3">
            <img src="https://i.pravatar.cc/36?img=3" class="w-9 h-9 rounded-full" alt="">
            <div>
              <p class="text-sm font-medium">ธนพล รัตน์</p>
              <p class="text-xs text-gray-500">thanapol@acme.co.th</p>
            </div>
          </div>
          <div class="flex items-center gap-3">
            <select class="text-xs border border-gray-200 rounded-lg px-2 py-1 focus:outline-none focus:ring-1 focus:ring-indigo-500">
              <option>Admin</option>
              <option selected>Editor</option>
              <option>Viewer</option>
            </select>
            <button class="text-gray-400 hover:text-red-500 transition-colors">
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"/></svg>
            </button>
          </div>
        </div>

        <div class="flex items-center justify-between p-3 hover:bg-gray-50 rounded-xl transition-colors">
          <div class="flex items-center gap-3">
            <img src="https://i.pravatar.cc/36?img=4" class="w-9 h-9 rounded-full" alt="">
            <div>
              <p class="text-sm font-medium">สมหญิง ดี</p>
              <p class="text-xs text-gray-500">somying@acme.co.th</p>
            </div>
          </div>
          <div class="flex items-center gap-3">
            <select class="text-xs border border-gray-200 rounded-lg px-2 py-1 focus:outline-none focus:ring-1 focus:ring-indigo-500">
              <option>Admin</option>
              <option>Editor</option>
              <option selected>Viewer</option>
            </select>
            <button class="text-gray-400 hover:text-red-500 transition-colors">
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"/></svg>
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Roles info -->
    <div class="bg-white rounded-xl border p-5">
      <h3 class="font-semibold mb-4">สิทธิ์การเข้าถึง</h3>
      <div class="overflow-x-auto">
        <table class="w-full text-sm">
          <thead>
            <tr class="border-b">
              <th class="text-left py-2 font-semibold text-gray-500 text-xs">สิทธิ์</th>
              <th class="text-center py-2 font-semibold text-gray-500 text-xs">Owner</th>
              <th class="text-center py-2 font-semibold text-gray-500 text-xs">Admin</th>
              <th class="text-center py-2 font-semibold text-gray-500 text-xs">Editor</th>
              <th class="text-center py-2 font-semibold text-gray-500 text-xs">Viewer</th>
            </tr>
          </thead>
          <tbody class="divide-y text-xs">
            <tr><td class="py-2 text-gray-700">ดูข้อมูล</td><td class="text-center text-green-500">✓</td><td class="text-center text-green-500">✓</td><td class="text-center text-green-500">✓</td><td class="text-center text-green-500">✓</td></tr>
            <tr><td class="py-2 text-gray-700">แก้ไข/สร้าง</td><td class="text-center text-green-500">✓</td><td class="text-center text-green-500">✓</td><td class="text-center text-green-500">✓</td><td class="text-center text-gray-300">✗</td></tr>
            <tr><td class="py-2 text-gray-700">จัดการทีม</td><td class="text-center text-green-500">✓</td><td class="text-center text-green-500">✓</td><td class="text-center text-gray-300">✗</td><td class="text-center text-gray-300">✗</td></tr>
            <tr><td class="py-2 text-gray-700">Billing / API Keys</td><td class="text-center text-green-500">✓</td><td class="text-center text-gray-300">✗</td><td class="text-center text-gray-300">✗</td><td class="text-center text-gray-300">✗</td></tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>

  <!-- Invite Modal -->
  <div id="invite-modal" class="hidden fixed inset-0 bg-black/50 flex items-center justify-center p-4 z-50">
    <div class="bg-white rounded-2xl p-6 w-full max-w-md shadow-xl">
      <div class="flex items-center justify-between mb-5">
        <h3 class="font-extrabold text-lg">เชิญสมาชิกใหม่</h3>
        <button onclick="document.getElementById('invite-modal').classList.add('hidden')" class="text-gray-400 hover:text-gray-600">✕</button>
      </div>
      <div class="space-y-4">
        <div>
          <label class="text-sm font-medium text-gray-700 block mb-1.5">Email</label>
          <input type="email" placeholder="colleague@company.com" class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
        </div>
        <div>
          <label class="text-sm font-medium text-gray-700 block mb-1.5">บทบาท</label>
          <select class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
            <option>Admin</option>
            <option selected>Editor</option>
            <option>Viewer</option>
          </select>
        </div>
        <div>
          <label class="text-sm font-medium text-gray-700 block mb-1.5">ข้อความ (ไม่บังคับ)</label>
          <textarea rows="2" placeholder="ยินดีต้อนรับสู่ทีม!" class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 resize-none"></textarea>
        </div>
        <div class="flex gap-3">
          <button onclick="document.getElementById('invite-modal').classList.add('hidden')" class="flex-1 border border-gray-300 text-gray-700 py-2.5 rounded-xl text-sm hover:bg-gray-50 transition-colors">ยกเลิก</button>
          <button class="flex-1 bg-indigo-600 text-white py-2.5 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">ส่งคำเชิญ</button>
        </div>
      </div>
    </div>
  </div>

</body>
</html>
```

---

## Step 385: API Keys Management

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>API Keys - Step 385</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-8">

  <div class="max-w-2xl mx-auto space-y-5">
    <div class="flex items-center justify-between">
      <h2 class="text-xl font-bold">API Keys</h2>
      <button onclick="createKey()" class="bg-indigo-600 text-white text-sm px-4 py-2 rounded-lg hover:bg-indigo-700 transition-colors">
        + สร้าง API Key
      </button>
    </div>

    <!-- Info banner -->
    <div class="bg-yellow-50 border border-yellow-200 rounded-xl p-4 flex gap-3">
      <span class="text-yellow-500 flex-shrink-0">⚠️</span>
      <p class="text-sm text-yellow-800">เก็บ API Key ไว้ใน secure location เสมอ อย่าเผยแพร่ใน source code หรือ public repo</p>
    </div>

    <!-- API Keys List -->
    <div class="space-y-3" id="keys-list">
      <!-- Populated by JS -->
    </div>

    <!-- Newly created key (shown after creation) -->
    <div id="new-key-banner" class="hidden bg-green-50 border border-green-200 rounded-xl p-4">
      <div class="flex items-center gap-2 mb-2">
        <span class="text-green-600">✓</span>
        <p class="text-sm font-semibold text-green-800">API Key สร้างแล้ว! คัดลอกก่อนปิด</p>
      </div>
      <div class="flex items-center gap-2 bg-white border border-green-200 rounded-lg px-3 py-2">
        <code id="new-key-value" class="flex-1 text-xs font-mono text-gray-800 truncate"></code>
        <button onclick="copyKey()" class="text-xs bg-green-600 text-white px-2 py-1 rounded-md hover:bg-green-700 transition-colors">คัดลอก</button>
      </div>
      <p class="text-xs text-green-700 mt-2">⚠️ Key นี้จะแสดงเพียงครั้งเดียวเท่านั้น</p>
    </div>
  </div>

  <script>
    const keys = [
      { id: 1, name: 'Production API', prefix: 'sk-prod-aBcD...', created: '1 มิ.ย. 2024', lastUsed: '5 นาทีที่แล้ว', perms: ['Read', 'Write'] },
      { id: 2, name: 'Staging API', prefix: 'sk-stg-XyZw...', created: '15 พ.ค. 2024', lastUsed: '2 วันที่แล้ว', perms: ['Read'] },
      { id: 3, name: 'Analytics Dashboard', prefix: 'sk-dash-MnOp...', created: '1 พ.ค. 2024', lastUsed: '1 สัปดาห์ที่แล้ว', perms: ['Read'] },
    ];

    function renderKeys() {
      document.getElementById('keys-list').innerHTML = keys.map(k => `
        <div class="bg-white rounded-xl border p-5">
          <div class="flex items-start justify-between mb-3">
            <div>
              <p class="font-semibold text-sm">${k.name}</p>
              <code class="text-xs text-gray-500 bg-gray-50 px-2 py-0.5 rounded font-mono mt-1 block">${k.prefix}...</code>
            </div>
            <button onclick="revokeKey(${k.id})" class="text-xs text-red-500 hover:text-red-700 transition-colors border border-red-200 px-2.5 py-1 rounded-lg hover:bg-red-50">
              ยกเลิก
            </button>
          </div>
          <div class="flex flex-wrap gap-4 text-xs text-gray-500">
            <span>สร้าง: ${k.created}</span>
            <span>ใช้ล่าสุด: ${k.lastUsed}</span>
            <div class="flex gap-1">
              ${k.perms.map(p => `<span class="bg-blue-100 text-blue-700 px-2 py-0.5 rounded-full font-medium">${p}</span>`).join('')}
            </div>
          </div>
        </div>
      `).join('');
    }

    function createKey() {
      const prefix = 'sk-' + Math.random().toString(36).slice(2,8) + '-';
      const full = prefix + Math.random().toString(36).slice(2,34);
      document.getElementById('new-key-value').textContent = full;
      document.getElementById('new-key-banner').classList.remove('hidden');
      document.getElementById('new-key-banner').scrollIntoView({ behavior: 'smooth' });
    }

    function copyKey() {
      const key = document.getElementById('new-key-value').textContent;
      navigator.clipboard.writeText(key).catch(() => {});
      const btn = document.querySelector('#new-key-banner button');
      btn.textContent = '✓ คัดลอกแล้ว';
      setTimeout(() => btn.textContent = 'คัดลอก', 2000);
    }

    function revokeKey(id) {
      if (confirm('คุณแน่ใจหรือไม่? การยกเลิก Key จะทำให้ Application ที่ใช้งานอยู่หยุดทำงาน')) {
        const idx = keys.findIndex(k => k.id === id);
        keys.splice(idx, 1);
        renderKeys();
      }
    }

    renderKeys();
  </script>

</body>
</html>
```

---

## Step 386: Danger Zone

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Danger Zone - Step 386</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-8">
  <div class="max-w-2xl mx-auto space-y-4">
    <h2 class="text-xl font-bold text-red-600">Danger Zone</h2>
    <p class="text-sm text-gray-500">การกระทำในส่วนนี้ไม่สามารถย้อนกลับได้ กรุณาพิจารณาอย่างระมัดระวัง</p>

    <!-- Export Data -->
    <div class="bg-white border rounded-xl p-5">
      <div class="flex items-center justify-between">
        <div>
          <p class="font-semibold">Export ข้อมูลทั้งหมด</p>
          <p class="text-sm text-gray-500 mt-0.5">ดาวน์โหลดข้อมูลทั้งหมดใน account ของคุณ</p>
        </div>
        <button class="border border-gray-300 text-gray-700 text-sm px-4 py-2 rounded-lg hover:bg-gray-50 transition-colors">
          Export
        </button>
      </div>
    </div>

    <!-- Transfer Ownership -->
    <div class="bg-white border border-orange-200 rounded-xl p-5">
      <div class="flex items-center justify-between">
        <div>
          <p class="font-semibold text-orange-800">โอนความเป็นเจ้าของ</p>
          <p class="text-sm text-gray-500 mt-0.5">โอน Organization ให้สมาชิกคนอื่น</p>
        </div>
        <button onclick="openTransfer()" class="border border-orange-300 text-orange-700 text-sm px-4 py-2 rounded-lg hover:bg-orange-50 transition-colors">
          โอนสิทธิ์
        </button>
      </div>
    </div>

    <!-- Delete Account -->
    <div class="bg-white border border-red-200 rounded-xl p-5">
      <div class="flex items-center justify-between">
        <div>
          <p class="font-semibold text-red-700">ลบ Account</p>
          <p class="text-sm text-gray-500 mt-0.5">ลบ account และข้อมูลทั้งหมดอย่างถาวร</p>
        </div>
        <button onclick="openDelete()" class="bg-red-600 text-white text-sm px-4 py-2 rounded-lg hover:bg-red-700 transition-colors">
          ลบ Account
        </button>
      </div>
    </div>
  </div>

  <!-- Delete Confirm Modal -->
  <div id="delete-modal" class="hidden fixed inset-0 bg-black/50 flex items-center justify-center p-4 z-50">
    <div class="bg-white rounded-2xl p-6 w-full max-w-sm shadow-xl">
      <div class="text-center mb-5">
        <div class="w-14 h-14 bg-red-100 rounded-full flex items-center justify-center mx-auto mb-3">
          <svg class="w-7 h-7 text-red-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"/>
          </svg>
        </div>
        <h3 class="font-extrabold text-lg text-red-700 mb-2">ยืนยันการลบ Account</h3>
        <p class="text-sm text-gray-500">การกระทำนี้ไม่สามารถย้อนกลับได้ ข้อมูลทั้งหมดจะถูกลบอย่างถาวร</p>
      </div>
      <div class="mb-4">
        <label class="text-sm font-medium text-gray-700 block mb-1.5">
          พิมพ์ <strong class="text-red-600">DELETE</strong> เพื่อยืนยัน
        </label>
        <input type="text" id="confirm-text" oninput="checkConfirm(this.value)"
          class="w-full border border-red-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-red-500">
      </div>
      <div class="flex gap-3">
        <button onclick="document.getElementById('delete-modal').classList.add('hidden')"
          class="flex-1 border border-gray-300 py-2.5 rounded-xl text-sm hover:bg-gray-50 transition-colors">
          ยกเลิก
        </button>
        <button id="confirm-delete-btn" disabled
          class="flex-1 bg-red-300 text-white py-2.5 rounded-xl text-sm font-semibold cursor-not-allowed transition-colors">
          ลบ Account
        </button>
      </div>
    </div>
  </div>

  <script>
    function openDelete() { document.getElementById('delete-modal').classList.remove('hidden'); }
    function checkConfirm(val) {
      const btn = document.getElementById('confirm-delete-btn');
      const ok = val === 'DELETE';
      btn.disabled = !ok;
      btn.className = `flex-1 ${ok ? 'bg-red-600 hover:bg-red-700 cursor-pointer' : 'bg-red-300 cursor-not-allowed'} text-white py-2.5 rounded-xl text-sm font-semibold transition-colors`;
    }
  </script>
</body>
</html>
```

---

## Step 387–389: Appearance & Integration Settings

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Appearance Settings - Steps 387-389</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-8">

  <div class="max-w-2xl mx-auto space-y-5">
    <!-- Appearance -->
    <div class="bg-white rounded-xl border p-5">
      <h3 class="font-bold mb-5">Appearance</h3>
      <div class="space-y-4">

        <!-- Theme -->
        <div>
          <p class="text-sm font-medium text-gray-700 mb-3">Theme</p>
          <div class="grid grid-cols-3 gap-3">
            <label class="cursor-pointer">
              <input type="radio" name="theme" value="light" checked class="sr-only peer">
              <div class="border-2 rounded-xl p-3 text-center peer-checked:border-indigo-500 hover:border-gray-300 transition-colors">
                <div class="w-full h-10 bg-white border rounded-lg mb-2"></div>
                <p class="text-xs font-medium">Light</p>
              </div>
            </label>
            <label class="cursor-pointer">
              <input type="radio" name="theme" value="dark" class="sr-only peer">
              <div class="border-2 rounded-xl p-3 text-center peer-checked:border-indigo-500 hover:border-gray-300 transition-colors">
                <div class="w-full h-10 bg-gray-900 border border-gray-700 rounded-lg mb-2"></div>
                <p class="text-xs font-medium">Dark</p>
              </div>
            </label>
            <label class="cursor-pointer">
              <input type="radio" name="theme" value="system" class="sr-only peer">
              <div class="border-2 rounded-xl p-3 text-center peer-checked:border-indigo-500 hover:border-gray-300 transition-colors">
                <div class="w-full h-10 bg-gradient-to-r from-white to-gray-900 border rounded-lg mb-2"></div>
                <p class="text-xs font-medium">System</p>
              </div>
            </label>
          </div>
        </div>

        <!-- Accent Color -->
        <div>
          <p class="text-sm font-medium text-gray-700 mb-3">Accent Color</p>
          <div class="flex gap-2">
            <button class="w-8 h-8 rounded-full bg-indigo-600 ring-2 ring-indigo-600 ring-offset-2 transition-all"></button>
            <button class="w-8 h-8 rounded-full bg-purple-600 hover:ring-2 hover:ring-purple-600 hover:ring-offset-2 transition-all"></button>
            <button class="w-8 h-8 rounded-full bg-blue-600 hover:ring-2 hover:ring-blue-600 hover:ring-offset-2 transition-all"></button>
            <button class="w-8 h-8 rounded-full bg-green-600 hover:ring-2 hover:ring-green-600 hover:ring-offset-2 transition-all"></button>
            <button class="w-8 h-8 rounded-full bg-orange-500 hover:ring-2 hover:ring-orange-500 hover:ring-offset-2 transition-all"></button>
            <button class="w-8 h-8 rounded-full bg-pink-600 hover:ring-2 hover:ring-pink-600 hover:ring-offset-2 transition-all"></button>
          </div>
        </div>

        <!-- Font Size -->
        <div>
          <p class="text-sm font-medium text-gray-700 mb-3">ขนาดตัวอักษร</p>
          <div class="flex items-center gap-3">
            <span class="text-xs text-gray-500">ก</span>
            <input type="range" min="12" max="20" value="16" class="flex-1 accent-indigo-600">
            <span class="text-lg text-gray-500">ก</span>
          </div>
        </div>

        <!-- Compact mode -->
        <label class="flex items-center justify-between cursor-pointer">
          <div>
            <p class="text-sm font-medium">Compact Mode</p>
            <p class="text-xs text-gray-500">แสดงข้อมูลมากขึ้นในพื้นที่น้อยลง</p>
          </div>
          <button class="relative bg-gray-200 rounded-full w-11 h-6 transition-colors duration-200 hover:bg-gray-300" onclick="this.classList.toggle('bg-indigo-600'); this.classList.toggle('bg-gray-200'); this.querySelector('span').classList.toggle('translate-x-5')">
            <span class="block w-5 h-5 bg-white rounded-full absolute top-0.5 left-0.5 transition-transform duration-200 shadow-sm"></span>
          </button>
        </label>
      </div>
    </div>

    <!-- Integrations -->
    <div class="bg-white rounded-xl border p-5">
      <h3 class="font-bold mb-5">Integrations ที่เชื่อมต่อ</h3>
      <div class="space-y-3">
        <div class="flex items-center justify-between p-3 bg-gray-50 rounded-xl">
          <div class="flex items-center gap-3">
            <div class="w-9 h-9 bg-purple-100 rounded-xl flex items-center justify-center font-bold text-purple-700 text-sm">Sl</div>
            <div>
              <p class="text-sm font-medium">Slack</p>
              <p class="text-xs text-green-600">✓ เชื่อมต่อแล้ว · #general</p>
            </div>
          </div>
          <button class="text-xs text-red-500 hover:text-red-700 border border-red-200 px-2.5 py-1 rounded-lg hover:bg-red-50 transition-colors">ยกเลิก</button>
        </div>
        <div class="flex items-center justify-between p-3 bg-gray-50 rounded-xl">
          <div class="flex items-center gap-3">
            <div class="w-9 h-9 bg-green-100 rounded-xl flex items-center justify-center font-bold text-green-700 text-sm">Li</div>
            <div>
              <p class="text-sm font-medium">Line Notify</p>
              <p class="text-xs text-green-600">✓ เชื่อมต่อแล้ว</p>
            </div>
          </div>
          <button class="text-xs text-red-500 hover:text-red-700 border border-red-200 px-2.5 py-1 rounded-lg hover:bg-red-50 transition-colors">ยกเลิก</button>
        </div>
        <div class="flex items-center justify-between p-3 border-dashed border-2 border-gray-200 rounded-xl">
          <div class="flex items-center gap-3">
            <div class="w-9 h-9 bg-gray-100 rounded-xl flex items-center justify-center text-gray-400 text-sm">GH</div>
            <div>
              <p class="text-sm font-medium text-gray-600">GitHub</p>
              <p class="text-xs text-gray-400">ยังไม่ได้เชื่อมต่อ</p>
            </div>
          </div>
          <button class="text-xs text-indigo-600 border border-indigo-200 px-2.5 py-1 rounded-lg hover:bg-indigo-50 transition-colors">เชื่อมต่อ</button>
        </div>
      </div>
    </div>

    <button class="bg-indigo-600 text-white px-5 py-2.5 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">
      บันทึกการตั้งค่า
    </button>
  </div>
</body>
</html>
```

---

## Step 390: Workshop — Full Settings Page

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Full Settings Workshop - Step 390</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 text-gray-900">

  <div class="max-w-5xl mx-auto px-4 py-8">
    <!-- Breadcrumb -->
    <div class="flex items-center gap-2 text-sm text-gray-500 mb-6">
      <a href="#" class="hover:text-indigo-600 transition-colors">Dashboard</a>
      <span>›</span>
      <span class="text-gray-800 font-medium">การตั้งค่า</span>
    </div>

    <div class="grid grid-cols-1 lg:grid-cols-[220px_1fr] gap-6">

      <!-- Sidebar -->
      <nav class="bg-white rounded-xl border p-3 h-fit sticky top-6">
        <p class="text-xs font-semibold text-gray-400 uppercase tracking-wide px-3 mb-2">บัญชี</p>
        <ul class="space-y-0.5 mb-4">
          <li><button onclick="showSection('profile')" class="nav-btn w-full flex items-center gap-3 px-3 py-2.5 rounded-lg bg-indigo-50 text-indigo-700 font-medium text-sm text-left">👤 โปรไฟล์</button></li>
          <li><button onclick="showSection('security')" class="nav-btn w-full flex items-center gap-3 px-3 py-2.5 rounded-lg text-gray-600 hover:bg-gray-50 text-sm text-left transition-colors">🔒 ความปลอดภัย</button></li>
          <li><button onclick="showSection('notifications')" class="nav-btn w-full flex items-center gap-3 px-3 py-2.5 rounded-lg text-gray-600 hover:bg-gray-50 text-sm text-left transition-colors">🔔 การแจ้งเตือน</button></li>
          <li><button onclick="showSection('appearance')" class="nav-btn w-full flex items-center gap-3 px-3 py-2.5 rounded-lg text-gray-600 hover:bg-gray-50 text-sm text-left transition-colors">🎨 Appearance</button></li>
        </ul>
        <p class="text-xs font-semibold text-gray-400 uppercase tracking-wide px-3 mb-2">Organization</p>
        <ul class="space-y-0.5 mb-4">
          <li><button onclick="showSection('billing')" class="nav-btn w-full flex items-center gap-3 px-3 py-2.5 rounded-lg text-gray-600 hover:bg-gray-50 text-sm text-left transition-colors">💳 การชำระเงิน</button></li>
          <li><button onclick="showSection('team')" class="nav-btn w-full flex items-center gap-3 px-3 py-2.5 rounded-lg text-gray-600 hover:bg-gray-50 text-sm text-left transition-colors">👥 ทีม</button></li>
          <li><button onclick="showSection('api')" class="nav-btn w-full flex items-center gap-3 px-3 py-2.5 rounded-lg text-gray-600 hover:bg-gray-50 text-sm text-left transition-colors">🔑 API Keys</button></li>
        </ul>
        <ul class="space-y-0.5 border-t pt-2">
          <li><button onclick="showSection('danger')" class="nav-btn w-full flex items-center gap-3 px-3 py-2.5 rounded-lg text-red-600 hover:bg-red-50 text-sm text-left transition-colors">⚠️ Danger Zone</button></li>
        </ul>
      </nav>

      <!-- Content Area -->
      <div id="section-content">
        <!-- Profile Section (default) -->
        <div id="sec-profile" class="space-y-5">
          <div class="bg-white rounded-xl border p-6">
            <h2 class="text-lg font-bold mb-5">โปรไฟล์</h2>
            <div class="flex items-center gap-4 mb-5 pb-5 border-b">
              <img src="https://i.pravatar.cc/80?img=1" class="w-16 h-16 rounded-full" alt="">
              <div>
                <p class="font-semibold">สมชาย เทคโน</p>
                <p class="text-xs text-gray-500 mt-0.5">JPG, PNG · สูงสุด 5MB</p>
                <button class="text-xs bg-white border border-gray-300 px-3 py-1.5 rounded-lg hover:bg-gray-50 mt-2 transition-colors">อัพโหลด</button>
              </div>
            </div>
            <div class="grid grid-cols-2 gap-4">
              <div>
                <label class="text-sm font-medium text-gray-700 block mb-1.5">ชื่อ</label>
                <input type="text" value="สมชาย" class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
              </div>
              <div>
                <label class="text-sm font-medium text-gray-700 block mb-1.5">นามสกุล</label>
                <input type="text" value="เทคโน" class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
              </div>
              <div class="col-span-2">
                <label class="text-sm font-medium text-gray-700 block mb-1.5">Email</label>
                <input type="email" value="somchai@acme.co.th" class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
              </div>
              <div class="col-span-2">
                <label class="text-sm font-medium text-gray-700 block mb-1.5">Bio</label>
                <textarea rows="2" class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 resize-none">Senior Frontend Developer ผู้ชื่นชอบ Tailwind CSS</textarea>
              </div>
            </div>
            <div class="flex justify-end mt-4 pt-4 border-t gap-3">
              <button class="border border-gray-300 text-gray-700 px-4 py-2 rounded-xl text-sm hover:bg-gray-50 transition-colors">ยกเลิก</button>
              <button onclick="showToast()" class="bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">บันทึก</button>
            </div>
          </div>
        </div>

        <!-- Other sections (hidden) -->
        <div id="sec-security" class="hidden"><div class="bg-white rounded-xl border p-6"><h2 class="text-lg font-bold mb-4">ความปลอดภัย</h2><p class="text-gray-500 text-sm">จัดการรหัสผ่าน, 2FA และ session ที่ active</p></div></div>
        <div id="sec-notifications" class="hidden"><div class="bg-white rounded-xl border p-6"><h2 class="text-lg font-bold mb-4">การแจ้งเตือน</h2><p class="text-gray-500 text-sm">ตั้งค่าการรับ notification ผ่าน Email และ Push</p></div></div>
        <div id="sec-appearance" class="hidden"><div class="bg-white rounded-xl border p-6"><h2 class="text-lg font-bold mb-4">Appearance</h2><p class="text-gray-500 text-sm">ปรับแต่ง Theme, สี และขนาดตัวอักษร</p></div></div>
        <div id="sec-billing" class="hidden"><div class="bg-white rounded-xl border p-6"><h2 class="text-lg font-bold mb-4">การชำระเงิน</h2><p class="text-gray-500 text-sm">จัดการแผน, บัตร และใบแจ้งหนี้</p></div></div>
        <div id="sec-team" class="hidden"><div class="bg-white rounded-xl border p-6"><h2 class="text-lg font-bold mb-4">ทีม</h2><p class="text-gray-500 text-sm">เชิญและจัดการสิทธิ์สมาชิก</p></div></div>
        <div id="sec-api" class="hidden"><div class="bg-white rounded-xl border p-6"><h2 class="text-lg font-bold mb-4">API Keys</h2><p class="text-gray-500 text-sm">สร้างและจัดการ API credentials</p></div></div>
        <div id="sec-danger" class="hidden"><div class="bg-white rounded-xl border border-red-200 p-6"><h2 class="text-lg font-bold mb-4 text-red-700">Danger Zone</h2><p class="text-gray-500 text-sm">ลบ account หรือ export ข้อมูล</p></div></div>
      </div>
    </div>
  </div>

  <!-- Toast -->
  <div id="toast" class="fixed bottom-5 right-5 bg-gray-900 text-white px-4 py-3 rounded-xl shadow-lg flex items-center gap-2 translate-y-16 opacity-0 transition-all duration-300 z-50">
    <span class="text-green-400">✓</span>
    <span class="text-sm">บันทึกการเปลี่ยนแปลงแล้ว</span>
  </div>

  <script>
    const sections = ['profile','security','notifications','appearance','billing','team','api','danger'];

    function showSection(name) {
      sections.forEach(s => document.getElementById('sec-' + s).classList.add('hidden'));
      document.getElementById('sec-' + name).classList.remove('hidden');

      document.querySelectorAll('.nav-btn').forEach(btn => {
        btn.className = 'nav-btn w-full flex items-center gap-3 px-3 py-2.5 rounded-lg text-gray-600 hover:bg-gray-50 text-sm text-left transition-colors';
      });
      event.currentTarget.className = 'nav-btn w-full flex items-center gap-3 px-3 py-2.5 rounded-lg bg-indigo-50 text-indigo-700 font-medium text-sm text-left';
    }

    function showToast() {
      const t = document.getElementById('toast');
      t.classList.remove('translate-y-16', 'opacity-0');
      setTimeout(() => t.classList.add('translate-y-16', 'opacity-0'), 3000);
    }
  </script>

</body>
</html>
```

---

## สรุป Part 39

| Step | เนื้อหา |
|------|---------|
| 381 | Settings Layout: sidebar nav, profile form |
| 382 | Notification Settings: checkbox groups, push toggle |
| 383 | Billing: plan info, payment methods, invoice history |
| 384 | Team Management: invite modal, member list, role selector |
| 385 | API Keys: list, create, revoke, copy |
| 386 | Danger Zone: export, transfer, delete with confirm |
| 387–389 | Appearance: theme picker, accent color, integrations |
| 390 | Workshop: Full Settings Page with tab switching + toast |

**Part ถัดไป:** Part 40 — Project 3: E-Commerce Store (Level 3 Culminating Project)
