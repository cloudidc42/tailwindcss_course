# Part 69: Authentication UI

## เป้าหมาย
- Login / Register forms
- Password strength indicator
- Multi-step onboarding
- Social login buttons
- Steps 681–690

---

## Steps 681–690: Auth UI Workshop

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Auth UI - Steps 681-690</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    @keyframes slideIn { from { opacity:0; transform:translateX(24px); } to { opacity:1; transform:translateX(0); } }
    @keyframes fadeUp  { from { opacity:0; transform:translateY(16px); } to { opacity:1; transform:translateY(0); } }
    .slide-in  { animation: slideIn 0.35s ease forwards; }
    .fade-up   { animation: fadeUp  0.4s ease forwards; }
    .strength-bar { transition: width 0.4s ease, background 0.4s ease; }
    input:focus ~ label, input:not(:placeholder-shown) ~ label {
      transform: translateY(-1.4rem) scale(0.8);
      color: #6366f1;
    }
  </style>
</head>
<body class="min-h-screen bg-gradient-to-br from-indigo-50 via-white to-purple-50 flex items-center justify-center p-4">

  <!-- Tab selector -->
  <div class="w-full max-w-md">
    <div class="flex bg-gray-100 rounded-2xl p-1 mb-6">
      <button onclick="showTab('login')" id="tab-login" class="flex-1 py-2 text-sm font-semibold rounded-xl bg-white shadow-sm text-gray-900 transition-all">เข้าสู่ระบบ</button>
      <button onclick="showTab('register')" id="tab-register" class="flex-1 py-2 text-sm font-semibold rounded-xl text-gray-500 transition-all hover:text-gray-700">สมัครสมาชิก</button>
      <button onclick="showTab('forgot')" id="tab-forgot" class="flex-1 py-2 text-sm font-semibold rounded-xl text-gray-500 transition-all hover:text-gray-700">ลืมรหัสผ่าน</button>
    </div>

    <!-- Login Form -->
    <div id="panel-login" class="fade-up bg-white rounded-3xl shadow-xl p-8">
      <div class="text-center mb-7">
        <div class="w-14 h-14 bg-indigo-600 rounded-2xl flex items-center justify-center mx-auto mb-3 text-2xl">🔐</div>
        <h1 class="text-xl font-extrabold text-gray-900">ยินดีต้อนรับกลับ</h1>
        <p class="text-sm text-gray-500 mt-1">เข้าสู่ระบบเพื่อดำเนินการต่อ</p>
      </div>

      <!-- Social Login -->
      <div class="flex gap-3 mb-5">
        <button class="flex-1 flex items-center justify-center gap-2 border border-gray-300 rounded-xl py-2.5 text-sm font-medium text-gray-700 hover:bg-gray-50 transition-colors">
          <svg class="w-4 h-4" viewBox="0 0 24 24"><path fill="#4285F4" d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z"/><path fill="#34A853" d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z"/><path fill="#FBBC05" d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l2.85-2.22.81-.62z"/><path fill="#EA4335" d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z"/></svg>
          Google
        </button>
        <button class="flex-1 flex items-center justify-center gap-2 border border-gray-300 rounded-xl py-2.5 text-sm font-medium text-gray-700 hover:bg-gray-50 transition-colors">
          <svg class="w-4 h-4" fill="#1877F2" viewBox="0 0 24 24"><path d="M24 12.073c0-6.627-5.373-12-12-12s-12 5.373-12 12c0 5.99 4.388 10.954 10.125 11.854v-8.385H7.078v-3.47h3.047V9.43c0-3.007 1.792-4.669 4.533-4.669 1.312 0 2.686.235 2.686.235v2.953H15.83c-1.491 0-1.956.925-1.956 1.874v2.25h3.328l-.532 3.47h-2.796v8.385C19.612 23.027 24 18.062 24 12.073z"/></svg>
          Facebook
        </button>
      </div>
      <div class="flex items-center gap-3 mb-5">
        <div class="flex-1 h-px bg-gray-200"></div>
        <span class="text-xs text-gray-400">หรือใช้อีเมล</span>
        <div class="flex-1 h-px bg-gray-200"></div>
      </div>

      <form onsubmit="handleLogin(event)" class="space-y-4">
        <div>
          <label class="block text-xs font-semibold text-gray-600 mb-1.5">อีเมล</label>
          <input type="email" id="loginEmail" placeholder="you@example.com" required
            class="w-full border border-gray-300 rounded-xl px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-400 transition-colors placeholder-gray-400">
        </div>
        <div>
          <div class="flex justify-between mb-1.5">
            <label class="text-xs font-semibold text-gray-600">รหัสผ่าน</label>
            <a href="#" onclick="showTab('forgot')" class="text-xs text-indigo-600 hover:underline">ลืมรหัสผ่าน?</a>
          </div>
          <div class="relative">
            <input type="password" id="loginPwd" placeholder="กรอกรหัสผ่าน" required
              class="w-full border border-gray-300 rounded-xl px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-400 pr-10 transition-colors placeholder-gray-400">
            <button type="button" onclick="togglePwd('loginPwd', this)" class="absolute right-3 top-1/2 -translate-y-1/2 text-gray-400 hover:text-gray-600">👁</button>
          </div>
        </div>
        <div class="flex items-center gap-2">
          <input type="checkbox" id="remember" class="w-4 h-4 rounded accent-indigo-600">
          <label for="remember" class="text-sm text-gray-600">จำฉันไว้</label>
        </div>
        <button type="submit" id="loginBtn" class="w-full bg-indigo-600 text-white py-2.5 rounded-xl font-semibold hover:bg-indigo-700 transition-colors flex items-center justify-center gap-2">
          เข้าสู่ระบบ
        </button>
      </form>
      <p class="text-center text-sm text-gray-500 mt-5">ยังไม่มีบัญชี? <a href="#" onclick="showTab('register')" class="text-indigo-600 font-semibold hover:underline">สมัครสมาชิก</a></p>
    </div>

    <!-- Register Form -->
    <div id="panel-register" class="hidden fade-up bg-white rounded-3xl shadow-xl p-8">
      <!-- Stepper -->
      <div class="flex items-center justify-between mb-7">
        <div class="flex-1">
          <div id="step1dot" class="w-7 h-7 rounded-full bg-indigo-600 text-white text-xs flex items-center justify-center font-bold mx-auto mb-1">1</div>
          <p class="text-[10px] text-center font-semibold text-indigo-600">บัญชี</p>
        </div>
        <div class="flex-1 h-px bg-gray-200 mx-1"></div>
        <div class="flex-1">
          <div id="step2dot" class="w-7 h-7 rounded-full bg-gray-200 text-gray-500 text-xs flex items-center justify-center font-bold mx-auto mb-1">2</div>
          <p class="text-[10px] text-center font-semibold text-gray-400">ข้อมูล</p>
        </div>
        <div class="flex-1 h-px bg-gray-200 mx-1"></div>
        <div class="flex-1">
          <div id="step3dot" class="w-7 h-7 rounded-full bg-gray-200 text-gray-500 text-xs flex items-center justify-center font-bold mx-auto mb-1">3</div>
          <p class="text-[10px] text-center font-semibold text-gray-400">เสร็จ</p>
        </div>
      </div>

      <!-- Step 1 -->
      <div id="regStep1">
        <h2 class="font-extrabold text-lg text-gray-900 mb-5">สร้างบัญชีใหม่</h2>
        <div class="space-y-4">
          <div>
            <label class="block text-xs font-semibold text-gray-600 mb-1.5">อีเมล</label>
            <input type="email" id="regEmail" placeholder="you@example.com"
              class="w-full border border-gray-300 rounded-xl px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-400">
          </div>
          <div>
            <label class="block text-xs font-semibold text-gray-600 mb-1.5">รหัสผ่าน</label>
            <div class="relative">
              <input type="password" id="regPwd" placeholder="อย่างน้อย 8 ตัวอักษร" oninput="checkStrength(this.value)"
                class="w-full border border-gray-300 rounded-xl px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-400 pr-10">
              <button type="button" onclick="togglePwd('regPwd', this)" class="absolute right-3 top-1/2 -translate-y-1/2 text-gray-400 hover:text-gray-600">👁</button>
            </div>
            <!-- Strength bar -->
            <div class="mt-2">
              <div class="flex gap-1 mb-1">
                <div id="sb1" class="strength-bar h-1 flex-1 rounded-full bg-gray-200"></div>
                <div id="sb2" class="strength-bar h-1 flex-1 rounded-full bg-gray-200"></div>
                <div id="sb3" class="strength-bar h-1 flex-1 rounded-full bg-gray-200"></div>
                <div id="sb4" class="strength-bar h-1 flex-1 rounded-full bg-gray-200"></div>
              </div>
              <p id="strengthLabel" class="text-xs text-gray-400">กรุณากรอกรหัสผ่าน</p>
            </div>
          </div>
          <div>
            <label class="block text-xs font-semibold text-gray-600 mb-1.5">ยืนยันรหัสผ่าน</label>
            <input type="password" id="regPwdConfirm" placeholder="กรอกรหัสผ่านอีกครั้ง"
              class="w-full border border-gray-300 rounded-xl px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-400">
          </div>
          <button onclick="goStep(2)" class="w-full bg-indigo-600 text-white py-2.5 rounded-xl font-semibold hover:bg-indigo-700 transition-colors">ถัดไป →</button>
        </div>
      </div>

      <!-- Step 2 -->
      <div id="regStep2" class="hidden">
        <h2 class="font-extrabold text-lg text-gray-900 mb-5">ข้อมูลส่วนตัว</h2>
        <div class="space-y-4">
          <div class="grid grid-cols-2 gap-3">
            <div>
              <label class="block text-xs font-semibold text-gray-600 mb-1.5">ชื่อ</label>
              <input type="text" placeholder="ชื่อจริง"
                class="w-full border border-gray-300 rounded-xl px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-400">
            </div>
            <div>
              <label class="block text-xs font-semibold text-gray-600 mb-1.5">นามสกุล</label>
              <input type="text" placeholder="นามสกุล"
                class="w-full border border-gray-300 rounded-xl px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-400">
            </div>
          </div>
          <div>
            <label class="block text-xs font-semibold text-gray-600 mb-1.5">เบอร์โทรศัพท์</label>
            <div class="flex gap-2">
              <select class="border border-gray-300 rounded-xl px-2.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-400">
                <option>🇹🇭 +66</option>
                <option>🇺🇸 +1</option>
                <option>🇬🇧 +44</option>
              </select>
              <input type="tel" placeholder="081-234-5678"
                class="flex-1 border border-gray-300 rounded-xl px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-400">
            </div>
          </div>
          <div>
            <label class="block text-xs font-semibold text-gray-600 mb-2">ฉันเป็น...</label>
            <div class="grid grid-cols-2 gap-2">
              <label class="flex items-center gap-2.5 border-2 border-gray-200 rounded-xl p-3 cursor-pointer has-[:checked]:border-indigo-500 has-[:checked]:bg-indigo-50 transition-colors">
                <input type="radio" name="role" value="customer" class="accent-indigo-600" checked>
                <div>
                  <p class="text-sm font-semibold text-gray-800">ลูกค้า</p>
                  <p class="text-xs text-gray-500">ช้อปปิ้ง</p>
                </div>
              </label>
              <label class="flex items-center gap-2.5 border-2 border-gray-200 rounded-xl p-3 cursor-pointer has-[:checked]:border-indigo-500 has-[:checked]:bg-indigo-50 transition-colors">
                <input type="radio" name="role" value="seller" class="accent-indigo-600">
                <div>
                  <p class="text-sm font-semibold text-gray-800">ผู้ขาย</p>
                  <p class="text-xs text-gray-500">เปิดร้าน</p>
                </div>
              </label>
            </div>
          </div>
          <div>
            <label class="flex items-start gap-2.5 cursor-pointer">
              <input type="checkbox" class="w-4 h-4 rounded accent-indigo-600 mt-0.5">
              <span class="text-xs text-gray-600">ฉันยอมรับ <a href="#" class="text-indigo-600 hover:underline">เงื่อนไขการใช้บริการ</a> และ <a href="#" class="text-indigo-600 hover:underline">นโยบายความเป็นส่วนตัว</a></span>
            </label>
          </div>
          <div class="flex gap-2">
            <button onclick="goStep(1)" class="flex-1 border border-gray-300 text-gray-700 py-2.5 rounded-xl font-semibold hover:bg-gray-50 transition-colors">← ย้อนกลับ</button>
            <button onclick="goStep(3)" class="flex-1 bg-indigo-600 text-white py-2.5 rounded-xl font-semibold hover:bg-indigo-700 transition-colors">สมัครสมาชิก</button>
          </div>
        </div>
      </div>

      <!-- Step 3: Success -->
      <div id="regStep3" class="hidden text-center py-4">
        <div class="w-16 h-16 bg-emerald-100 rounded-full flex items-center justify-center mx-auto mb-4 text-3xl">✅</div>
        <h2 class="font-extrabold text-xl text-gray-900 mb-2">สมัครสำเร็จ!</h2>
        <p class="text-sm text-gray-500 mb-6">เราส่งอีเมลยืนยันไปแล้ว<br>กรุณาตรวจสอบกล่องอีเมลของคุณ</p>
        <button onclick="showTab('login')" class="bg-indigo-600 text-white px-8 py-2.5 rounded-xl font-semibold hover:bg-indigo-700 transition-colors">เข้าสู่ระบบ</button>
      </div>
    </div>

    <!-- Forgot Password -->
    <div id="panel-forgot" class="hidden fade-up bg-white rounded-3xl shadow-xl p-8">
      <div class="text-center mb-7">
        <div class="w-14 h-14 bg-orange-100 rounded-2xl flex items-center justify-center mx-auto mb-3 text-2xl">🔑</div>
        <h1 class="text-xl font-extrabold text-gray-900">ลืมรหัสผ่าน?</h1>
        <p class="text-sm text-gray-500 mt-1">กรอกอีเมลเพื่อรีเซ็ตรหัสผ่าน</p>
      </div>
      <div id="forgotForm">
        <div class="mb-4">
          <label class="block text-xs font-semibold text-gray-600 mb-1.5">อีเมล</label>
          <input type="email" id="forgotEmail" placeholder="you@example.com"
            class="w-full border border-gray-300 rounded-xl px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-400">
        </div>
        <button onclick="sendReset()" class="w-full bg-orange-500 text-white py-2.5 rounded-xl font-semibold hover:bg-orange-600 transition-colors">ส่งลิงก์รีเซ็ต</button>
        <button onclick="showTab('login')" class="w-full mt-2 text-sm text-gray-500 hover:text-gray-700 transition-colors py-1">← กลับไปเข้าสู่ระบบ</button>
      </div>
      <div id="forgotSent" class="hidden text-center py-4">
        <div class="text-4xl mb-3">📧</div>
        <p class="font-semibold text-gray-800 mb-1">อีเมลถูกส่งแล้ว!</p>
        <p class="text-sm text-gray-500 mb-4">ตรวจสอบกล่องอีเมล แล้วคลิกลิงก์ที่ส่งไป</p>
        <button onclick="showTab('login')" class="text-sm text-indigo-600 hover:underline">กลับสู่หน้าเข้าสู่ระบบ</button>
      </div>
    </div>
  </div>

  <script>
    function showTab(tab) {
      ['login','register','forgot'].forEach(t => {
        document.getElementById('panel-' + t).classList.add('hidden');
        const btn = document.getElementById('tab-' + t);
        if (btn) btn.className = 'flex-1 py-2 text-sm font-semibold rounded-xl ' +
          (t === tab ? 'bg-white shadow-sm text-gray-900' : 'text-gray-500 hover:text-gray-700') + ' transition-all';
      });
      document.getElementById('panel-' + tab).classList.remove('hidden');
    }

    function togglePwd(id, btn) {
      const input = document.getElementById(id);
      input.type = input.type === 'password' ? 'text' : 'password';
      btn.textContent = input.type === 'password' ? '👁' : '🙈';
    }

    function checkStrength(val) {
      let score = 0;
      if (val.length >= 8) score++;
      if (/[A-Z]/.test(val)) score++;
      if (/[0-9]/.test(val)) score++;
      if (/[^A-Za-z0-9]/.test(val)) score++;

      const colors = ['bg-red-400', 'bg-orange-400', 'bg-yellow-400', 'bg-emerald-500'];
      const labels = ['อ่อนมาก', 'อ่อน', 'ปานกลาง', 'แข็งแรง'];
      const textColors = ['text-red-500', 'text-orange-500', 'text-yellow-600', 'text-emerald-600'];

      for (let i = 1; i <= 4; i++) {
        const el = document.getElementById('sb' + i);
        el.className = `strength-bar h-1 flex-1 rounded-full ${i <= score ? colors[score-1] : 'bg-gray-200'}`;
      }
      const label = document.getElementById('strengthLabel');
      if (score === 0) { label.textContent = 'กรุณากรอกรหัสผ่าน'; label.className = 'text-xs text-gray-400'; }
      else { label.textContent = `รหัสผ่าน: ${labels[score-1]}`; label.className = `text-xs ${textColors[score-1]}`; }
    }

    function goStep(n) {
      [1,2,3].forEach(i => {
        document.getElementById('regStep' + i).classList.add('hidden');
        const dot = document.getElementById('step' + i + 'dot');
        dot.className = `w-7 h-7 rounded-full text-xs flex items-center justify-center font-bold mx-auto mb-1 ${i < n ? 'bg-emerald-500 text-white' : i === n ? 'bg-indigo-600 text-white' : 'bg-gray-200 text-gray-500'}`;
      });
      document.getElementById('regStep' + n).classList.remove('hidden');
    }

    function handleLogin(e) {
      e.preventDefault();
      const btn = document.getElementById('loginBtn');
      btn.innerHTML = '<svg class="w-4 h-4 animate-spin" fill="none" viewBox="0 0 24 24"><circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle><path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4z"></path></svg> กำลังเข้าสู่ระบบ...';
      btn.disabled = true;
      setTimeout(() => {
        btn.innerHTML = '✅ เข้าสู่ระบบสำเร็จ';
        btn.className = btn.className.replace('bg-indigo-600 hover:bg-indigo-700', 'bg-emerald-600');
      }, 1500);
    }

    function sendReset() {
      const email = document.getElementById('forgotEmail').value;
      if (!email) return;
      document.getElementById('forgotForm').classList.add('hidden');
      document.getElementById('forgotSent').classList.remove('hidden');
    }
  </script>
</body>
</html>
```

---

## สรุป Part 69

| Step | เนื้อหา |
|------|---------|
| 681 | Login form — email/password, validation |
| 682 | Social login buttons — Google, Facebook |
| 683 | Show/hide password toggle |
| 684 | Register multi-step form — stepper indicator |
| 685 | Password strength indicator — 4 bars |
| 686 | Role picker (customer/seller) ด้วย radio cards |
| 687 | Forgot password form + sent state |
| 688 | Tab switcher — login/register/forgot |
| 689 | Loading state บน submit button |
| 690 | Workshop: Complete Auth UI — login/register/forgot สมบูรณ์ |

**Part ถัดไป:** Part 70 — Command Palette & Keyboard Navigation (Steps 691–700)
