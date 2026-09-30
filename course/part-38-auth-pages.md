# Part 38: Authentication Pages

## เป้าหมาย
- สร้าง Login, Register, Forgot/Reset Password Pages
- Form Validation แบบ Real-time
- Social Auth Buttons, Two-Factor Authentication UI
- Accessible และ Security-focused Design

---

## Step 371: Login Page

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Login - Step 371</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="min-h-screen bg-gray-50 flex">

  <!-- Left Panel (branding) -->
  <div class="hidden lg:flex flex-1 bg-gradient-to-br from-indigo-900 to-purple-900 items-center justify-center p-12">
    <div class="max-w-sm text-white">
      <div class="w-12 h-12 bg-white/20 rounded-2xl flex items-center justify-center mb-6">
        <span class="text-2xl font-extrabold">S</span>
      </div>
      <h1 class="text-3xl font-extrabold mb-4">ยินดีต้อนรับกลับมา!</h1>
      <p class="text-indigo-200 leading-relaxed mb-8">เข้าสู่ระบบเพื่อจัดการธุรกิจของคุณอย่างชาญฉลาดด้วย SaasKit</p>
      <div class="space-y-3">
        <div class="flex items-center gap-3 bg-white/10 rounded-xl p-3">
          <span class="text-2xl">📊</span>
          <div>
            <p class="font-semibold text-sm">Real-time Analytics</p>
            <p class="text-indigo-300 text-xs">วิเคราะห์ข้อมูลทุก metric สำคัญ</p>
          </div>
        </div>
        <div class="flex items-center gap-3 bg-white/10 rounded-xl p-3">
          <span class="text-2xl">🤖</span>
          <div>
            <p class="font-semibold text-sm">AI Automation</p>
            <p class="text-indigo-300 text-xs">ลดงานซ้ำซ้อนด้วย AI อัจฉริยะ</p>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- Right Panel (form) -->
  <div class="flex-1 flex items-center justify-center p-6">
    <div class="w-full max-w-sm">
      <!-- Logo (mobile) -->
      <div class="lg:hidden flex items-center gap-2 mb-8">
        <div class="w-8 h-8 bg-indigo-600 rounded-xl flex items-center justify-center">
          <span class="text-white font-bold text-sm">S</span>
        </div>
        <span class="font-extrabold text-lg">SaasKit</span>
      </div>

      <h2 class="text-2xl font-extrabold mb-1">เข้าสู่ระบบ</h2>
      <p class="text-gray-500 text-sm mb-6">ยังไม่มีบัญชี? <a href="#" class="text-indigo-600 font-semibold hover:underline">สมัครฟรี</a></p>

      <!-- Social Auth -->
      <div class="grid grid-cols-2 gap-3 mb-5">
        <button class="flex items-center justify-center gap-2 border border-gray-300 rounded-xl py-2.5 text-sm font-medium text-gray-700 hover:bg-gray-50 transition-colors">
          <svg class="w-5 h-5" viewBox="0 0 24 24" fill="none">
            <path d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z" fill="#4285F4"/>
            <path d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z" fill="#34A853"/>
            <path d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l2.85-2.22.81-.62z" fill="#FBBC05"/>
            <path d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z" fill="#EA4335"/>
          </svg>
          Google
        </button>
        <button class="flex items-center justify-center gap-2 border border-gray-300 rounded-xl py-2.5 text-sm font-medium text-gray-700 hover:bg-gray-50 transition-colors">
          <svg class="w-5 h-5 text-gray-900" fill="currentColor" viewBox="0 0 24 24">
            <path d="M12 0C5.374 0 0 5.373 0 12c0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23A11.509 11.509 0 0112 5.803c1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576C20.566 21.797 24 17.3 24 12c0-6.627-5.373-12-12-12z"/>
          </svg>
          GitHub
        </button>
      </div>

      <div class="flex items-center gap-3 mb-5">
        <div class="flex-1 h-px bg-gray-200"></div>
        <span class="text-xs text-gray-400 font-medium">หรือเข้าสู่ระบบด้วย Email</span>
        <div class="flex-1 h-px bg-gray-200"></div>
      </div>

      <!-- Login Form -->
      <form onsubmit="handleLogin(event)" class="space-y-4">
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1.5">Email</label>
          <input type="email" id="login-email" placeholder="you@company.com"
            class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 transition-colors"
            required>
        </div>
        <div>
          <div class="flex items-center justify-between mb-1.5">
            <label class="text-sm font-medium text-gray-700">รหัสผ่าน</label>
            <a href="#" class="text-xs text-indigo-600 hover:underline">ลืมรหัสผ่าน?</a>
          </div>
          <div class="relative">
            <input type="password" id="login-pass" placeholder="••••••••"
              class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 pr-10 transition-colors"
              required>
            <button type="button" onclick="togglePass('login-pass', this)" class="absolute right-3 top-1/2 -translate-y-1/2 text-gray-400 hover:text-gray-600">
              <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z M2.458 12C3.732 7.943 7.523 5 12 5c4.478 0 8.268 2.943 9.542 7-1.274 4.057-5.064 7-9.542 7-4.477 0-8.268-2.943-9.542-7z"/>
              </svg>
            </button>
          </div>
        </div>
        <label class="flex items-center gap-2 cursor-pointer">
          <input type="checkbox" class="w-4 h-4 rounded border-gray-300 text-indigo-600 focus:ring-indigo-500">
          <span class="text-sm text-gray-600">จำฉันไว้ในอุปกรณ์นี้</span>
        </label>
        <button type="submit" id="login-btn"
          class="w-full bg-indigo-600 text-white py-2.5 rounded-xl font-semibold text-sm hover:bg-indigo-700 active:scale-[0.98] transition-all">
          เข้าสู่ระบบ
        </button>
      </form>
    </div>
  </div>

  <script>
    function togglePass(id, btn) {
      const input = document.getElementById(id);
      input.type = input.type === 'password' ? 'text' : 'password';
    }
    function handleLogin(e) {
      e.preventDefault();
      const btn = document.getElementById('login-btn');
      btn.textContent = 'กำลังเข้าสู่ระบบ...';
      btn.disabled = true;
      setTimeout(() => { btn.textContent = '✓ เข้าสู่ระบบสำเร็จ'; btn.classList.replace('bg-indigo-600', 'bg-green-600'); }, 1500);
    }
  </script>

</body>
</html>
```

---

## Step 372: Register Page

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Register - Step 372</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="min-h-screen bg-gray-50 flex items-center justify-center p-6">

  <div class="w-full max-w-md bg-white rounded-2xl shadow-lg p-8">
    <!-- Logo -->
    <div class="flex items-center gap-2 mb-6">
      <div class="w-8 h-8 bg-indigo-600 rounded-xl flex items-center justify-center"><span class="text-white font-bold text-sm">S</span></div>
      <span class="font-extrabold text-lg">SaasKit</span>
    </div>

    <h2 class="text-2xl font-extrabold mb-1">สร้างบัญชีใหม่</h2>
    <p class="text-gray-500 text-sm mb-6">มีบัญชีแล้ว? <a href="#" class="text-indigo-600 font-semibold hover:underline">เข้าสู่ระบบ</a></p>

    <form onsubmit="handleRegister(event)" class="space-y-4" novalidate>
      <!-- Name row -->
      <div class="grid grid-cols-2 gap-3">
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1.5">ชื่อ</label>
          <input type="text" id="first-name" placeholder="สมชาย"
            class="w-full border border-gray-300 rounded-xl px-3 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500"
            oninput="validateName(this)" required>
          <p id="first-name-err" class="text-red-500 text-xs mt-1 hidden">กรุณากรอกชื่อ</p>
        </div>
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1.5">นามสกุล</label>
          <input type="text" placeholder="เทคโน"
            class="w-full border border-gray-300 rounded-xl px-3 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
        </div>
      </div>

      <!-- Company -->
      <div>
        <label class="block text-sm font-medium text-gray-700 mb-1.5">ชื่อบริษัท</label>
        <input type="text" placeholder="Acme Corp"
          class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
      </div>

      <!-- Email -->
      <div>
        <label class="block text-sm font-medium text-gray-700 mb-1.5">Email</label>
        <div class="relative">
          <input type="email" id="reg-email" placeholder="you@company.com"
            class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 pr-10"
            oninput="validateEmail(this)" required>
          <span id="email-icon" class="absolute right-3 top-1/2 -translate-y-1/2 text-gray-300 text-sm"></span>
        </div>
        <p id="email-err" class="text-red-500 text-xs mt-1 hidden">กรุณากรอก Email ที่ถูกต้อง</p>
      </div>

      <!-- Password with strength meter -->
      <div>
        <label class="block text-sm font-medium text-gray-700 mb-1.5">รหัสผ่าน</label>
        <div class="relative">
          <input type="password" id="reg-pass" placeholder="••••••••"
            class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 pr-10"
            oninput="checkStrength(this.value)" required minlength="8">
          <button type="button" onclick="togglePass('reg-pass')" class="absolute right-3 top-1/2 -translate-y-1/2 text-gray-400 hover:text-gray-600">👁</button>
        </div>
        <!-- Strength bar -->
        <div class="mt-2">
          <div class="flex gap-1 mb-1">
            <div id="bar-1" class="h-1 flex-1 rounded-full bg-gray-200 transition-colors duration-300"></div>
            <div id="bar-2" class="h-1 flex-1 rounded-full bg-gray-200 transition-colors duration-300"></div>
            <div id="bar-3" class="h-1 flex-1 rounded-full bg-gray-200 transition-colors duration-300"></div>
            <div id="bar-4" class="h-1 flex-1 rounded-full bg-gray-200 transition-colors duration-300"></div>
          </div>
          <p id="strength-label" class="text-xs text-gray-400"></p>
        </div>
        <!-- Requirements -->
        <ul class="mt-2 space-y-1 text-xs text-gray-400">
          <li id="req-len" class="flex items-center gap-1.5"><span>○</span> อย่างน้อย 8 ตัวอักษร</li>
          <li id="req-upper" class="flex items-center gap-1.5"><span>○</span> มีตัวพิมพ์ใหญ่</li>
          <li id="req-num" class="flex items-center gap-1.5"><span>○</span> มีตัวเลข</li>
          <li id="req-sym" class="flex items-center gap-1.5"><span>○</span> มีสัญลักษณ์ (!@#$)</li>
        </ul>
      </div>

      <!-- Terms -->
      <label class="flex items-start gap-2 cursor-pointer">
        <input type="checkbox" id="terms-check" class="w-4 h-4 mt-0.5 rounded border-gray-300 text-indigo-600 focus:ring-indigo-500">
        <span class="text-xs text-gray-500 leading-relaxed">
          ฉันยอมรับ <a href="#" class="text-indigo-600 hover:underline">Terms of Service</a> และ
          <a href="#" class="text-indigo-600 hover:underline">Privacy Policy</a>
        </span>
      </label>

      <button type="submit"
        class="w-full bg-indigo-600 text-white py-2.5 rounded-xl font-semibold text-sm hover:bg-indigo-700 active:scale-[0.98] transition-all">
        สร้างบัญชี
      </button>
    </form>
  </div>

  <script>
    function validateEmail(input) {
      const ok = /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(input.value);
      document.getElementById('email-err').classList.toggle('hidden', ok || !input.value);
      document.getElementById('email-icon').textContent = input.value ? (ok ? '✓' : '✗') : '';
      document.getElementById('email-icon').className = `absolute right-3 top-1/2 -translate-y-1/2 text-sm ${ok ? 'text-green-500' : 'text-red-500'}`;
    }

    function validateName(input) {
      document.getElementById('first-name-err').classList.toggle('hidden', input.value.length > 0);
    }

    function checkStrength(val) {
      const checks = {
        len: val.length >= 8,
        upper: /[A-Z]/.test(val),
        num: /[0-9]/.test(val),
        sym: /[!@#$%^&*]/.test(val),
      };
      const score = Object.values(checks).filter(Boolean).length;

      const colors = ['', 'bg-red-400', 'bg-orange-400', 'bg-yellow-400', 'bg-green-500'];
      const labels = ['', 'อ่อนมาก', 'ปานกลาง', 'ดี', 'แข็งแกร่ง'];
      const textColors = ['', 'text-red-500', 'text-orange-500', 'text-yellow-600', 'text-green-600'];

      for (let i = 1; i <= 4; i++) {
        const bar = document.getElementById('bar-' + i);
        bar.className = `h-1 flex-1 rounded-full transition-colors duration-300 ${i <= score ? colors[score] : 'bg-gray-200'}`;
      }
      const label = document.getElementById('strength-label');
      label.textContent = val ? 'ความปลอดภัย: ' + labels[score] : '';
      label.className = 'text-xs ' + (val ? textColors[score] : 'text-gray-400');

      const update = (id, pass) => {
        const el = document.getElementById(id);
        el.children[0].textContent = pass ? '✓' : '○';
        el.className = `flex items-center gap-1.5 text-xs ${pass ? 'text-green-500' : 'text-gray-400'}`;
      };
      update('req-len', checks.len);
      update('req-upper', checks.upper);
      update('req-num', checks.num);
      update('req-sym', checks.sym);
    }

    function togglePass(id) {
      const input = document.getElementById(id);
      input.type = input.type === 'password' ? 'text' : 'password';
    }

    function handleRegister(e) {
      e.preventDefault();
      if (!document.getElementById('terms-check').checked) {
        alert('กรุณายอมรับ Terms of Service ก่อน');
        return;
      }
      const btn = e.target.querySelector('button[type=submit]');
      btn.textContent = 'กำลังสร้างบัญชี...';
      btn.disabled = true;
      setTimeout(() => { btn.textContent = '✓ สร้างบัญชีสำเร็จ!'; btn.classList.replace('bg-indigo-600', 'bg-green-600'); }, 1500);
    }
  </script>

</body>
</html>
```

---

## Step 373: Forgot Password

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Forgot Password - Step 373</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="min-h-screen bg-gray-50 flex items-center justify-center p-6">

  <div class="w-full max-w-md">

    <!-- Step 1: Request Reset -->
    <div id="step-request" class="bg-white rounded-2xl shadow-sm p-8 border">
      <div class="text-center mb-6">
        <div class="w-14 h-14 bg-indigo-100 rounded-2xl flex items-center justify-center mx-auto mb-4">
          <svg class="w-7 h-7 text-indigo-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z"/>
          </svg>
        </div>
        <h2 class="text-2xl font-extrabold mb-2">ลืมรหัสผ่าน?</h2>
        <p class="text-gray-500 text-sm">กรอก Email ที่ใช้ลงทะเบียน เราจะส่ง link รีเซ็ตรหัสผ่านให้</p>
      </div>

      <form onsubmit="sendReset(event)" class="space-y-4">
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1.5">Email</label>
          <input type="email" placeholder="you@company.com" required
            class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
        </div>
        <button type="submit" class="w-full bg-indigo-600 text-white py-2.5 rounded-xl font-semibold text-sm hover:bg-indigo-700 transition-colors">
          ส่ง Link รีเซ็ต
        </button>
        <a href="#" class="block text-center text-sm text-gray-500 hover:text-gray-700 transition-colors">
          ← กลับไปหน้าเข้าสู่ระบบ
        </a>
      </form>
    </div>

    <!-- Step 2: Email Sent (hidden initially) -->
    <div id="step-sent" class="hidden bg-white rounded-2xl shadow-sm p-8 border text-center">
      <div class="w-16 h-16 bg-green-100 rounded-full flex items-center justify-center mx-auto mb-4">
        <span class="text-3xl">📧</span>
      </div>
      <h2 class="text-xl font-extrabold mb-2">ตรวจสอบ Email ของคุณ!</h2>
      <p class="text-gray-500 text-sm mb-6 leading-relaxed">
        เราส่ง link รีเซ็ตรหัสผ่านไปที่ <strong class="text-gray-800">you@company.com</strong> แล้ว กรุณาตรวจสอบภายใน 15 นาที
      </p>
      <div class="bg-gray-50 rounded-xl p-4 text-left mb-4">
        <p class="text-xs text-gray-500 font-medium mb-2">ไม่ได้รับ Email?</p>
        <ul class="text-xs text-gray-500 space-y-1">
          <li>• ตรวจสอบใน Spam/Junk folder</li>
          <li>• รอสักครู่แล้วลองใหม่</li>
          <li>• ตรวจสอบว่ากรอก Email ถูกต้อง</li>
        </ul>
      </div>
      <button onclick="document.getElementById('step-request').classList.remove('hidden'); document.getElementById('step-sent').classList.add('hidden')"
        class="text-indigo-600 text-sm font-semibold hover:underline">
        ส่งอีกครั้ง
      </button>
    </div>

  </div>

  <script>
    function sendReset(e) {
      e.preventDefault();
      document.getElementById('step-request').classList.add('hidden');
      document.getElementById('step-sent').classList.remove('hidden');
    }
  </script>

</body>
</html>
```

---

## Step 374: Reset Password

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Reset Password - Step 374</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="min-h-screen bg-gray-50 flex items-center justify-center p-6">

  <div class="w-full max-w-md bg-white rounded-2xl shadow-sm border p-8">
    <div class="text-center mb-6">
      <div class="w-14 h-14 bg-indigo-100 rounded-2xl flex items-center justify-center mx-auto mb-4">
        <svg class="w-7 h-7 text-indigo-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 7a2 2 0 012 2m4 0a6 6 0 01-7.743 5.743L11 17H9v2H7v2H4a1 1 0 01-1-1v-2.586a1 1 0 01.293-.707l5.964-5.964A6 6 0 1121 9z"/>
        </svg>
      </div>
      <h2 class="text-2xl font-extrabold mb-2">ตั้งรหัสผ่านใหม่</h2>
      <p class="text-gray-500 text-sm">รหัสผ่านต้องมีความยาวอย่างน้อย 8 ตัวอักษร</p>
    </div>

    <form onsubmit="handleReset(event)" class="space-y-4">
      <div>
        <label class="block text-sm font-medium text-gray-700 mb-1.5">รหัสผ่านใหม่</label>
        <div class="relative">
          <input type="password" id="new-pass" placeholder="••••••••"
            class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 pr-10"
            oninput="updateStrength(this.value)" required>
          <button type="button" onclick="togglePass('new-pass')" class="absolute right-3 top-1/2 -translate-y-1/2 text-gray-400 hover:text-gray-600 text-sm">👁</button>
        </div>
        <div class="flex gap-1 mt-2">
          <div id="s1" class="h-1 flex-1 rounded-full bg-gray-200 transition-colors"></div>
          <div id="s2" class="h-1 flex-1 rounded-full bg-gray-200 transition-colors"></div>
          <div id="s3" class="h-1 flex-1 rounded-full bg-gray-200 transition-colors"></div>
          <div id="s4" class="h-1 flex-1 rounded-full bg-gray-200 transition-colors"></div>
        </div>
      </div>

      <div>
        <label class="block text-sm font-medium text-gray-700 mb-1.5">ยืนยันรหัสผ่าน</label>
        <div class="relative">
          <input type="password" id="confirm-pass" placeholder="••••••••"
            class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 pr-10"
            oninput="checkMatch()" required>
          <span id="match-icon" class="absolute right-3 top-1/2 -translate-y-1/2 text-sm"></span>
        </div>
        <p id="match-err" class="text-red-500 text-xs mt-1 hidden">รหัสผ่านไม่ตรงกัน</p>
      </div>

      <button type="submit" id="reset-btn"
        class="w-full bg-indigo-600 text-white py-2.5 rounded-xl font-semibold text-sm hover:bg-indigo-700 transition-colors">
        ตั้งรหัสผ่านใหม่
      </button>
    </form>
  </div>

  <script>
    function updateStrength(val) {
      const score = [val.length >= 8, /[A-Z]/.test(val), /[0-9]/.test(val), /[!@#$]/.test(val)].filter(Boolean).length;
      const colors = ['bg-red-400', 'bg-orange-400', 'bg-yellow-400', 'bg-green-500'];
      for (let i = 1; i <= 4; i++) {
        document.getElementById('s' + i).className = `h-1 flex-1 rounded-full transition-colors ${i <= score ? colors[score - 1] : 'bg-gray-200'}`;
      }
    }

    function checkMatch() {
      const p1 = document.getElementById('new-pass').value;
      const p2 = document.getElementById('confirm-pass').value;
      const match = p2 && p1 === p2;
      document.getElementById('match-err').classList.toggle('hidden', match || !p2);
      const icon = document.getElementById('match-icon');
      icon.textContent = p2 ? (match ? '✓' : '✗') : '';
      icon.className = `absolute right-3 top-1/2 -translate-y-1/2 text-sm ${match ? 'text-green-500' : 'text-red-500'}`;
    }

    function togglePass(id) {
      const el = document.getElementById(id);
      el.type = el.type === 'password' ? 'text' : 'password';
    }

    function handleReset(e) {
      e.preventDefault();
      const btn = document.getElementById('reset-btn');
      btn.textContent = '✓ เปลี่ยนรหัสผ่านสำเร็จ!';
      btn.classList.replace('bg-indigo-600', 'bg-green-600');
      btn.disabled = true;
    }
  </script>

</body>
</html>
```

---

## Step 375: Two-Factor Authentication

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>2FA - Step 375</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="min-h-screen bg-gray-50 flex items-center justify-center p-6">

  <div class="w-full max-w-sm bg-white rounded-2xl shadow-sm border p-8 text-center">
    <div class="w-14 h-14 bg-indigo-100 rounded-2xl flex items-center justify-center mx-auto mb-4">
      <span class="text-2xl">📱</span>
    </div>
    <h2 class="text-xl font-extrabold mb-2">ยืนยันตัวตน 2 ขั้น</h2>
    <p class="text-gray-500 text-sm mb-6 leading-relaxed">
      กรุณากรอกรหัส 6 หลักจาก Authenticator App ของคุณ
    </p>

    <!-- OTP Input -->
    <div class="flex gap-2 justify-center mb-6">
      <input type="text" maxlength="1" class="otp-input w-12 h-12 border-2 border-gray-300 rounded-xl text-center text-xl font-bold focus:outline-none focus:border-indigo-500 transition-colors" inputmode="numeric">
      <input type="text" maxlength="1" class="otp-input w-12 h-12 border-2 border-gray-300 rounded-xl text-center text-xl font-bold focus:outline-none focus:border-indigo-500 transition-colors" inputmode="numeric">
      <input type="text" maxlength="1" class="otp-input w-12 h-12 border-2 border-gray-300 rounded-xl text-center text-xl font-bold focus:outline-none focus:border-indigo-500 transition-colors" inputmode="numeric">
      <span class="flex items-center text-gray-300 text-xl">–</span>
      <input type="text" maxlength="1" class="otp-input w-12 h-12 border-2 border-gray-300 rounded-xl text-center text-xl font-bold focus:outline-none focus:border-indigo-500 transition-colors" inputmode="numeric">
      <input type="text" maxlength="1" class="otp-input w-12 h-12 border-2 border-gray-300 rounded-xl text-center text-xl font-bold focus:outline-none focus:border-indigo-500 transition-colors" inputmode="numeric">
      <input type="text" maxlength="1" class="otp-input w-12 h-12 border-2 border-gray-300 rounded-xl text-center text-xl font-bold focus:outline-none focus:border-indigo-500 transition-colors" inputmode="numeric">
    </div>

    <!-- Timer -->
    <p class="text-sm text-gray-500 mb-4">
      รหัสหมดอายุใน <span id="timer" class="font-bold text-indigo-600">02:00</span>
    </p>

    <button id="verify-btn" onclick="verifyOTP()"
      class="w-full bg-indigo-600 text-white py-2.5 rounded-xl font-semibold text-sm hover:bg-indigo-700 transition-colors mb-4">
      ยืนยัน
    </button>

    <div class="flex flex-col gap-2 text-sm text-gray-500">
      <button class="hover:text-indigo-600 transition-colors">ไม่ได้รับรหัส? ส่งใหม่</button>
      <button class="hover:text-indigo-600 transition-colors">ใช้รหัสสำรอง</button>
    </div>
  </div>

  <script>
    // OTP input auto-focus
    const inputs = document.querySelectorAll('.otp-input');
    inputs.forEach((input, i) => {
      input.addEventListener('input', () => {
        if (input.value && i < inputs.length - 1) {
          inputs[i + 1].focus();
        }
        checkComplete();
      });
      input.addEventListener('keydown', (e) => {
        if (e.key === 'Backspace' && !input.value && i > 0) {
          inputs[i - 1].focus();
        }
      });
    });

    function checkComplete() {
      const values = [...inputs].map(i => i.value).join('');
      if (values.length === 6) {
        document.getElementById('verify-btn').className = 'w-full bg-indigo-600 text-white py-2.5 rounded-xl font-semibold text-sm hover:bg-indigo-700 transition-colors mb-4 shadow-lg shadow-indigo-200';
      }
    }

    // Countdown timer
    let seconds = 120;
    const timer = setInterval(() => {
      seconds--;
      const m = Math.floor(seconds / 60).toString().padStart(2, '0');
      const s = (seconds % 60).toString().padStart(2, '0');
      document.getElementById('timer').textContent = `${m}:${s}`;
      if (seconds <= 0) {
        clearInterval(timer);
        document.getElementById('timer').textContent = 'หมดอายุ';
        document.getElementById('timer').className = 'font-bold text-red-500';
      }
    }, 1000);

    function verifyOTP() {
      const btn = document.getElementById('verify-btn');
      btn.textContent = 'กำลังยืนยัน...';
      setTimeout(() => {
        btn.textContent = '✓ ยืนยันสำเร็จ!';
        btn.classList.replace('bg-indigo-600', 'bg-green-600');
        inputs.forEach(i => { i.classList.add('border-green-500', 'bg-green-50'); });
      }, 1000);
    }
  </script>

</body>
</html>
```

---

## Step 376: Magic Link Login

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Magic Link - Step 376</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="min-h-screen bg-gray-50 flex items-center justify-center p-6">

  <div class="w-full max-w-sm">
    <div id="form-view" class="bg-white rounded-2xl shadow-sm border p-8 text-center">
      <div class="text-4xl mb-4">✨</div>
      <h2 class="text-xl font-extrabold mb-2">Magic Link Login</h2>
      <p class="text-gray-500 text-sm mb-6">ไม่ต้องจำรหัสผ่าน! กรอก Email แล้วเราจะส่ง link เข้าสู่ระบบให้</p>
      <form onsubmit="sendMagic(event)" class="space-y-4">
        <input type="email" placeholder="you@email.com" required
          class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 text-center">
        <button type="submit" class="w-full bg-indigo-600 text-white py-2.5 rounded-xl font-semibold text-sm hover:bg-indigo-700 transition-colors">
          ส่ง Magic Link ✨
        </button>
        <a href="#" class="block text-sm text-gray-500 hover:text-indigo-600">เข้าสู่ระบบด้วยรหัสผ่านแทน</a>
      </form>
    </div>

    <div id="sent-view" class="hidden bg-white rounded-2xl shadow-sm border p-8 text-center">
      <div class="text-5xl mb-4 animate-bounce">📬</div>
      <h2 class="text-xl font-extrabold mb-2">ตรวจสอบ Email!</h2>
      <p class="text-gray-500 text-sm leading-relaxed mb-4">เราส่ง magic link ไปที่ Email ของคุณแล้ว คลิก link เพื่อเข้าสู่ระบบได้เลย</p>
      <p class="text-xs text-gray-400">Link มีอายุ 15 นาที · เปิดจากอุปกรณ์เดิม</p>
    </div>
  </div>

  <script>
    function sendMagic(e) {
      e.preventDefault();
      document.getElementById('form-view').classList.add('hidden');
      document.getElementById('sent-view').classList.remove('hidden');
    }
  </script>

</body>
</html>
```

---

## Step 377: Security Settings (Account)

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Security Settings - Step 377</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-8">
  <div class="max-w-xl mx-auto space-y-4">
    <h2 class="text-xl font-bold">ความปลอดภัยของบัญชี</h2>

    <!-- 2FA Toggle -->
    <div class="bg-white rounded-xl p-5 border">
      <div class="flex items-center justify-between">
        <div>
          <p class="font-semibold">การยืนยันตัวตน 2 ขั้น (2FA)</p>
          <p class="text-sm text-gray-500 mt-0.5">เพิ่มความปลอดภัยด้วย Authenticator App</p>
        </div>
        <button onclick="this.classList.toggle('bg-indigo-600'); this.classList.toggle('bg-gray-200'); this.querySelector('span').classList.toggle('translate-x-5')"
          class="relative bg-gray-200 rounded-full w-11 h-6 transition-colors duration-200">
          <span class="block w-5 h-5 bg-white rounded-full absolute top-0.5 left-0.5 transition-transform duration-200 shadow-sm"></span>
        </button>
      </div>
    </div>

    <!-- Active Sessions -->
    <div class="bg-white rounded-xl p-5 border">
      <h3 class="font-semibold mb-4">Session ที่ Active อยู่</h3>
      <div class="space-y-3">
        <div class="flex items-center justify-between">
          <div class="flex items-center gap-3">
            <div class="w-9 h-9 bg-indigo-100 rounded-xl flex items-center justify-center">💻</div>
            <div>
              <p class="text-sm font-medium">MacBook Pro · Chrome</p>
              <p class="text-xs text-gray-400">กรุงเทพ, ไทย · ตอนนี้</p>
            </div>
          </div>
          <span class="flex items-center gap-1 text-xs text-green-600 bg-green-50 px-2 py-1 rounded-full font-medium">
            <span class="w-1.5 h-1.5 bg-green-500 rounded-full"></span>อุปกรณ์นี้
          </span>
        </div>
        <div class="flex items-center justify-between">
          <div class="flex items-center gap-3">
            <div class="w-9 h-9 bg-gray-100 rounded-xl flex items-center justify-center">📱</div>
            <div>
              <p class="text-sm font-medium">iPhone 15 · Safari</p>
              <p class="text-xs text-gray-400">กรุงเทพ, ไทย · 2 ชั่วโมงที่แล้ว</p>
            </div>
          </div>
          <button class="text-xs text-red-500 hover:text-red-700 transition-colors">ยกเลิก</button>
        </div>
        <div class="flex items-center justify-between">
          <div class="flex items-center gap-3">
            <div class="w-9 h-9 bg-gray-100 rounded-xl flex items-center justify-center">🪟</div>
            <div>
              <p class="text-sm font-medium">Windows PC · Firefox</p>
              <p class="text-xs text-gray-400">เชียงใหม่, ไทย · เมื่อวาน</p>
            </div>
          </div>
          <button class="text-xs text-red-500 hover:text-red-700 transition-colors">ยกเลิก</button>
        </div>
      </div>
      <button class="w-full mt-4 text-sm text-red-500 hover:text-red-700 border border-red-200 rounded-lg py-2 hover:bg-red-50 transition-colors">
        ยกเลิก Session อื่นทั้งหมด
      </button>
    </div>

    <!-- Login History -->
    <div class="bg-white rounded-xl p-5 border">
      <h3 class="font-semibold mb-4">ประวัติการเข้าสู่ระบบ</h3>
      <div class="space-y-2 text-sm">
        <div class="flex items-center justify-between py-2 border-b">
          <div class="flex items-center gap-2">
            <span class="text-green-500">✓</span>
            <span class="text-gray-700">เข้าสู่ระบบสำเร็จ</span>
          </div>
          <span class="text-gray-400 text-xs">ตอนนี้</span>
        </div>
        <div class="flex items-center justify-between py-2 border-b">
          <div class="flex items-center gap-2">
            <span class="text-green-500">✓</span>
            <span class="text-gray-700">เข้าสู่ระบบสำเร็จ · iPhone</span>
          </div>
          <span class="text-gray-400 text-xs">2 ชม. ที่แล้ว</span>
        </div>
        <div class="flex items-center justify-between py-2">
          <div class="flex items-center gap-2">
            <span class="text-red-500">✗</span>
            <span class="text-gray-700">ล้มเหลว · รหัสผ่านผิด</span>
          </div>
          <span class="text-gray-400 text-xs">เมื่อวาน</span>
        </div>
      </div>
    </div>
  </div>
</body>
</html>
```

---

## Step 378–379: Email Verification & Profile Setup

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Onboarding - Steps 378-379</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="min-h-screen bg-gray-50 flex items-center justify-center p-6">

  <!-- Onboarding Steps -->
  <div class="w-full max-w-md">

    <!-- Progress indicator -->
    <div class="flex items-center justify-between mb-8 px-2">
      <div class="flex items-center gap-2">
        <div class="w-8 h-8 bg-indigo-600 rounded-full flex items-center justify-center text-white text-sm font-bold">✓</div>
        <span class="text-xs text-indigo-600 font-medium">สมัครสมาชิก</span>
      </div>
      <div class="flex-1 h-px bg-indigo-300 mx-2"></div>
      <div class="flex items-center gap-2">
        <div class="w-8 h-8 bg-indigo-600 rounded-full flex items-center justify-center text-white text-sm font-bold">2</div>
        <span class="text-xs text-indigo-600 font-medium">ยืนยัน Email</span>
      </div>
      <div class="flex-1 h-px bg-gray-200 mx-2"></div>
      <div class="flex items-center gap-2">
        <div class="w-8 h-8 bg-gray-200 rounded-full flex items-center justify-center text-gray-400 text-sm font-bold">3</div>
        <span class="text-xs text-gray-400 font-medium">ตั้งค่าโปรไฟล์</span>
      </div>
    </div>

    <!-- Email Verify Card -->
    <div class="bg-white rounded-2xl border p-8 text-center">
      <div class="text-4xl mb-4">📬</div>
      <h2 class="text-xl font-extrabold mb-2">ยืนยัน Email ของคุณ</h2>
      <p class="text-gray-500 text-sm leading-relaxed mb-6">
        เราส่งรหัสยืนยัน 6 หลักไปที่<br>
        <strong class="text-gray-800">somchai@gmail.com</strong>
      </p>

      <div class="flex gap-2 justify-center mb-5">
        <input type="text" maxlength="1" class="w-11 h-11 border-2 border-gray-300 rounded-xl text-center font-bold text-lg focus:outline-none focus:border-indigo-500" inputmode="numeric">
        <input type="text" maxlength="1" class="w-11 h-11 border-2 border-gray-300 rounded-xl text-center font-bold text-lg focus:outline-none focus:border-indigo-500" inputmode="numeric">
        <input type="text" maxlength="1" class="w-11 h-11 border-2 border-gray-300 rounded-xl text-center font-bold text-lg focus:outline-none focus:border-indigo-500" inputmode="numeric">
        <span class="flex items-center text-gray-300">–</span>
        <input type="text" maxlength="1" class="w-11 h-11 border-2 border-gray-300 rounded-xl text-center font-bold text-lg focus:outline-none focus:border-indigo-500" inputmode="numeric">
        <input type="text" maxlength="1" class="w-11 h-11 border-2 border-gray-300 rounded-xl text-center font-bold text-lg focus:outline-none focus:border-indigo-500" inputmode="numeric">
        <input type="text" maxlength="1" class="w-11 h-11 border-2 border-gray-300 rounded-xl text-center font-bold text-lg focus:outline-none focus:border-indigo-500" inputmode="numeric">
      </div>

      <button onclick="nextStep()" class="w-full bg-indigo-600 text-white py-2.5 rounded-xl font-semibold text-sm hover:bg-indigo-700 transition-colors mb-3">
        ยืนยันรหัส
      </button>
      <button class="text-sm text-gray-500 hover:text-indigo-600 transition-colors">ส่งรหัสใหม่</button>
    </div>

    <!-- Profile Setup (hidden) -->
    <div id="profile-step" class="hidden bg-white rounded-2xl border p-8 mt-4">
      <h2 class="text-xl font-extrabold mb-5">ตั้งค่าโปรไฟล์</h2>

      <!-- Avatar Upload -->
      <div class="flex items-center gap-4 mb-5">
        <div class="relative">
          <div class="w-16 h-16 bg-indigo-100 rounded-full flex items-center justify-center text-2xl">👤</div>
          <button class="absolute -bottom-1 -right-1 w-6 h-6 bg-indigo-600 rounded-full flex items-center justify-center text-white text-xs hover:bg-indigo-700">+</button>
        </div>
        <div>
          <p class="text-sm font-medium">อัพโหลดรูปโปรไฟล์</p>
          <p class="text-xs text-gray-400">JPG, PNG · ไม่เกิน 5MB</p>
        </div>
      </div>

      <div class="space-y-3">
        <div>
          <label class="text-sm font-medium text-gray-700">ชื่อที่แสดง</label>
          <input type="text" value="สมชาย เทคโน" class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 mt-1.5">
        </div>
        <div>
          <label class="text-sm font-medium text-gray-700">บทบาท</label>
          <select class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 mt-1.5">
            <option>Developer</option>
            <option>Designer</option>
            <option>Product Manager</option>
            <option>Business Owner</option>
          </select>
        </div>
        <button class="w-full bg-indigo-600 text-white py-2.5 rounded-xl font-semibold text-sm hover:bg-indigo-700 transition-colors">
          บันทึกและเริ่มใช้งาน 🚀
        </button>
      </div>
    </div>
  </div>

  <script>
    function nextStep() {
      document.querySelector('.bg-white.rounded-2xl').classList.add('hidden');
      document.getElementById('profile-step').classList.remove('hidden');
    }
  </script>
</body>
</html>
```

---

## Step 380: Workshop — Complete Auth Flow

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Auth Flow Workshop - Step 380</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="min-h-screen bg-gray-100 flex items-center justify-center p-4">

  <!-- Auth Container -->
  <div class="w-full max-w-sm bg-white rounded-2xl shadow-lg overflow-hidden">

    <!-- Tabs -->
    <div class="flex border-b">
      <button id="tab-login" onclick="switchTab('login')" class="flex-1 py-3.5 text-sm font-semibold text-indigo-600 border-b-2 border-indigo-600 transition-colors">
        เข้าสู่ระบบ
      </button>
      <button id="tab-register" onclick="switchTab('register')" class="flex-1 py-3.5 text-sm font-medium text-gray-500 border-b-2 border-transparent hover:text-gray-800 transition-colors">
        สมัครสมาชิก
      </button>
    </div>

    <div class="p-7">

      <!-- Login Form -->
      <div id="view-login">
        <!-- Social -->
        <div class="grid grid-cols-2 gap-3 mb-5">
          <button class="flex items-center justify-center gap-2 border rounded-xl py-2.5 text-sm text-gray-700 hover:bg-gray-50 transition-colors">
            <svg class="w-4 h-4" viewBox="0 0 24 24" fill="none"><path d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z" fill="#4285F4"/><path d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z" fill="#34A853"/><path d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l2.85-2.22.81-.62z" fill="#FBBC05"/><path d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z" fill="#EA4335"/></svg>
            Google
          </button>
          <button class="flex items-center justify-center gap-2 border rounded-xl py-2.5 text-sm text-gray-700 hover:bg-gray-50 transition-colors">
            <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 24 24"><path d="M12 0C5.374 0 0 5.373 0 12c0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23A11.509 11.509 0 0112 5.803c1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576C20.566 21.797 24 17.3 24 12c0-6.627-5.373-12-12-12z"/></svg>
            GitHub
          </button>
        </div>

        <div class="flex items-center gap-3 mb-5">
          <div class="flex-1 h-px bg-gray-200"></div>
          <span class="text-xs text-gray-400">หรือ</span>
          <div class="flex-1 h-px bg-gray-200"></div>
        </div>

        <form onsubmit="handleAuth(event,'login')" class="space-y-4">
          <div>
            <label class="text-sm font-medium text-gray-700 block mb-1.5">Email</label>
            <input type="email" placeholder="you@email.com"
              class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500" required>
          </div>
          <div>
            <div class="flex justify-between mb-1.5">
              <label class="text-sm font-medium text-gray-700">รหัสผ่าน</label>
              <button type="button" onclick="switchTab('forgot')" class="text-xs text-indigo-600 hover:underline">ลืมรหัสผ่าน?</button>
            </div>
            <input type="password" placeholder="••••••••"
              class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500" required>
          </div>
          <button type="submit" class="w-full bg-indigo-600 text-white py-2.5 rounded-xl font-semibold text-sm hover:bg-indigo-700 transition-all active:scale-[0.98]">
            เข้าสู่ระบบ
          </button>
        </form>
      </div>

      <!-- Register Form -->
      <div id="view-register" class="hidden">
        <form onsubmit="handleAuth(event,'register')" class="space-y-4">
          <div class="grid grid-cols-2 gap-3">
            <div>
              <label class="text-sm font-medium text-gray-700 block mb-1.5">ชื่อ</label>
              <input type="text" placeholder="สมชาย" class="w-full border border-gray-300 rounded-xl px-3 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500" required>
            </div>
            <div>
              <label class="text-sm font-medium text-gray-700 block mb-1.5">นามสกุล</label>
              <input type="text" placeholder="เทคโน" class="w-full border border-gray-300 rounded-xl px-3 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500" required>
            </div>
          </div>
          <div>
            <label class="text-sm font-medium text-gray-700 block mb-1.5">Email</label>
            <input type="email" placeholder="you@email.com" class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500" required>
          </div>
          <div>
            <label class="text-sm font-medium text-gray-700 block mb-1.5">รหัสผ่าน</label>
            <input type="password" id="reg-pw" placeholder="••••••••" oninput="updateBars(this.value)"
              class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500" required>
            <div class="flex gap-1 mt-2">
              <div id="b1" class="h-1 flex-1 rounded-full bg-gray-200 transition-colors"></div>
              <div id="b2" class="h-1 flex-1 rounded-full bg-gray-200 transition-colors"></div>
              <div id="b3" class="h-1 flex-1 rounded-full bg-gray-200 transition-colors"></div>
              <div id="b4" class="h-1 flex-1 rounded-full bg-gray-200 transition-colors"></div>
            </div>
          </div>
          <label class="flex items-start gap-2 cursor-pointer">
            <input type="checkbox" class="w-4 h-4 mt-0.5 rounded border-gray-300 text-indigo-600" required>
            <span class="text-xs text-gray-500">ยอมรับ <a href="#" class="text-indigo-600 hover:underline">Terms</a> และ <a href="#" class="text-indigo-600 hover:underline">Privacy Policy</a></span>
          </label>
          <button type="submit" class="w-full bg-indigo-600 text-white py-2.5 rounded-xl font-semibold text-sm hover:bg-indigo-700 transition-all active:scale-[0.98]">
            สร้างบัญชี
          </button>
        </form>
      </div>

      <!-- Forgot View -->
      <div id="view-forgot" class="hidden text-center">
        <div class="text-4xl mb-4">🔐</div>
        <h3 class="font-extrabold mb-2">รีเซ็ตรหัสผ่าน</h3>
        <p class="text-gray-500 text-sm mb-5">กรอก Email และเราจะส่ง link ให้</p>
        <form onsubmit="handleForgot(event)" class="space-y-3">
          <input type="email" placeholder="you@email.com" class="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500" required>
          <button type="submit" class="w-full bg-indigo-600 text-white py-2.5 rounded-xl font-semibold text-sm hover:bg-indigo-700 transition-colors">
            ส่ง Link รีเซ็ต
          </button>
          <button type="button" onclick="switchTab('login')" class="w-full text-sm text-gray-500 hover:text-gray-700">← กลับ</button>
        </form>
      </div>

      <!-- Success View -->
      <div id="view-success" class="hidden text-center py-4">
        <div class="w-16 h-16 bg-green-100 rounded-full flex items-center justify-center mx-auto mb-4">
          <svg class="w-8 h-8 text-green-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M5 13l4 4L19 7"/>
          </svg>
        </div>
        <h3 class="font-extrabold text-xl mb-2" id="success-title">เข้าสู่ระบบสำเร็จ!</h3>
        <p class="text-gray-500 text-sm mb-5" id="success-msg">กำลังพาคุณไปยัง Dashboard...</p>
        <button onclick="switchTab('login')" class="text-sm text-indigo-600 hover:underline">กลับหน้าแรก</button>
      </div>

    </div>
  </div>

  <script>
    const views = ['login','register','forgot','success'];

    function switchTab(tab) {
      views.forEach(v => document.getElementById('view-' + v).classList.add('hidden'));
      document.getElementById('view-' + tab).classList.remove('hidden');

      // Update tab styles
      ['login','register'].forEach(t => {
        const el = document.getElementById('tab-' + t);
        if (!el) return;
        if (t === tab) {
          el.className = 'flex-1 py-3.5 text-sm font-semibold text-indigo-600 border-b-2 border-indigo-600 transition-colors';
        } else {
          el.className = 'flex-1 py-3.5 text-sm font-medium text-gray-500 border-b-2 border-transparent hover:text-gray-800 transition-colors';
        }
      });
    }

    function handleAuth(e, type) {
      e.preventDefault();
      document.getElementById('success-title').textContent = type === 'login' ? 'เข้าสู่ระบบสำเร็จ!' : 'สมัครสมาชิกสำเร็จ!';
      document.getElementById('success-msg').textContent = type === 'login' ? 'กำลังพาคุณไปยัง Dashboard...' : 'ตรวจสอบ Email เพื่อยืนยันบัญชีของคุณ';
      switchTab('success');
    }

    function handleForgot(e) {
      e.preventDefault();
      document.getElementById('success-title').textContent = 'ส่ง Link สำเร็จ!';
      document.getElementById('success-msg').textContent = 'ตรวจสอบ Email ของคุณ';
      switchTab('success');
    }

    function updateBars(val) {
      const score = [val.length >= 8, /[A-Z]/.test(val), /[0-9]/.test(val), /[!@#$]/.test(val)].filter(Boolean).length;
      const colors = ['bg-red-400', 'bg-orange-400', 'bg-yellow-400', 'bg-green-500'];
      for (let i = 1; i <= 4; i++) {
        document.getElementById('b' + i).className = `h-1 flex-1 rounded-full transition-colors ${i <= score ? colors[score - 1] : 'bg-gray-200'}`;
      }
    }
  </script>

</body>
</html>
```

---

## สรุป Part 38

| Step | เนื้อหา |
|------|---------|
| 371 | Login Page: split-panel, social auth, show/hide password |
| 372 | Register Page: real-time validation, password strength |
| 373 | Forgot Password: request form → email sent state |
| 374 | Reset Password: confirm match, strength meter |
| 375 | Two-Factor Auth: OTP input auto-focus, countdown timer |
| 376 | Magic Link Login: passwordless flow |
| 377 | Security Settings: 2FA toggle, active sessions, login history |
| 378-379 | Email Verification + Profile Setup onboarding flow |
| 380 | Workshop: Complete Auth Flow with tab switching |

**Part ถัดไป:** Part 39 — Admin Settings Pages
