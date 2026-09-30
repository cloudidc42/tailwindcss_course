# Part 27: Multi-Step Form & Validation
## Steps 261–270: Form ซับซ้อนระดับมืออาชีพ

---

## 🎯 เป้าหมายของ Part นี้

- Multi-step wizard UI
- Step progress indicator
- Form validation (real-time)
- Error messages
- Review & Submit step
- Success screen

---

## Step 261: Step Progress Indicator

```html
<!-- Progress steps indicator -->
<div class="flex items-center gap-0">
  <!-- Step 1 — Completed -->
  <div class="flex items-center">
    <div class="size-9 rounded-full bg-indigo-600 flex items-center justify-center text-white text-sm font-bold flex-none">✓</div>
    <div class="h-0.5 w-16 sm:w-24 bg-indigo-600"></div>
  </div>
  
  <!-- Step 2 — Active -->
  <div class="flex items-center">
    <div class="size-9 rounded-full bg-indigo-600 ring-4 ring-indigo-100 flex items-center justify-center text-white text-sm font-bold flex-none">2</div>
    <div class="h-0.5 w-16 sm:w-24 bg-gray-200"></div>
  </div>
  
  <!-- Step 3 — Pending -->
  <div class="flex items-center">
    <div class="size-9 rounded-full bg-gray-200 flex items-center justify-center text-gray-500 text-sm font-bold flex-none">3</div>
    <div class="h-0.5 w-16 sm:w-24 bg-gray-200"></div>
  </div>
  
  <!-- Step 4 — Pending -->
  <div class="flex items-center">
    <div class="size-9 rounded-full bg-gray-200 flex items-center justify-center text-gray-500 text-sm font-bold flex-none">4</div>
  </div>
</div>

<!-- Step labels below (optional) -->
<div class="flex text-xs text-gray-500 mt-2 gap-0">
  <span class="w-9 text-center font-medium text-indigo-600">ข้อมูล</span>
  <span class="w-16 sm:w-24"></span>
  <span class="w-9 text-center font-medium text-indigo-600">ที่อยู่</span>
  <span class="w-16 sm:w-24"></span>
  <span class="w-9 text-center">ยืนยัน</span>
  <span class="w-16 sm:w-24"></span>
  <span class="w-9 text-center">เสร็จ</span>
</div>
```

---

## Step 262: Form Field with Real-Time Validation

```html
<!-- Validated input field -->
<div class="space-y-1.5">
  <label class="text-sm font-medium text-gray-700" for="email">อีเมล <span class="text-red-500">*</span></label>
  
  <div class="relative">
    <input
      id="email"
      type="email"
      class="w-full px-4 py-3 rounded-xl border border-gray-300 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-200 focus:border-indigo-400 transition-all peer"
      placeholder="you@example.com"
      oninput="validateEmail(this)"
    >
    <!-- Valid icon -->
    <span id="email-valid-icon" class="hidden absolute right-3 top-1/2 -translate-y-1/2 text-green-500 text-sm">✓</span>
    <!-- Invalid icon -->
    <span id="email-invalid-icon" class="hidden absolute right-3 top-1/2 -translate-y-1/2 text-red-500 text-sm">✕</span>
  </div>
  
  <!-- Error message -->
  <p id="email-error" class="hidden text-xs text-red-500 flex items-center gap-1">
    ⚠ กรุณากรอกอีเมลให้ถูกต้อง
  </p>
  <!-- Success hint -->
  <p id="email-success" class="hidden text-xs text-green-600">อีเมลถูกต้อง</p>
</div>

<script>
function validateEmail(input) {
  const val = input.value;
  const valid = /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(val);
  const hasValue = val.length > 0;
  
  input.className = input.className
    .replace(/border-\w+-\d+/g, '')
    .replace(/ring-\w+-\d+/g, '');
  
  document.getElementById('email-valid-icon').classList.toggle('hidden', !(hasValue && valid));
  document.getElementById('email-invalid-icon').classList.toggle('hidden', !(hasValue && !valid));
  document.getElementById('email-error').classList.toggle('hidden', !(hasValue && !valid));
  document.getElementById('email-success').classList.toggle('hidden', !(hasValue && valid));
  
  if (hasValue && !valid) {
    input.classList.add('border-red-400', 'focus:ring-red-200', 'focus:border-red-400');
  } else if (hasValue && valid) {
    input.classList.add('border-green-400', 'focus:ring-green-200', 'focus:border-green-400');
  } else {
    input.classList.add('border-gray-300', 'focus:ring-indigo-200', 'focus:border-indigo-400');
  }
}
</script>
```

---

## Step 263: Password Strength Meter

```html
<div class="space-y-2">
  <label class="text-sm font-medium text-gray-700">รหัสผ่าน <span class="text-red-500">*</span></label>
  
  <div class="relative">
    <input
      id="password"
      type="password"
      oninput="checkStrength(this.value)"
      class="w-full px-4 py-3 pr-10 rounded-xl border border-gray-300 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-200 transition-all"
      placeholder="อย่างน้อย 8 ตัวอักษร"
    >
    <button type="button" onclick="togglePwd()" class="absolute right-3 top-1/2 -translate-y-1/2 text-gray-400 hover:text-gray-700 text-sm">👁</button>
  </div>
  
  <!-- Strength bars -->
  <div class="flex gap-1.5 mt-1">
    <div id="bar-1" class="h-1.5 flex-1 rounded-full bg-gray-200 transition-all"></div>
    <div id="bar-2" class="h-1.5 flex-1 rounded-full bg-gray-200 transition-all"></div>
    <div id="bar-3" class="h-1.5 flex-1 rounded-full bg-gray-200 transition-all"></div>
    <div id="bar-4" class="h-1.5 flex-1 rounded-full bg-gray-200 transition-all"></div>
  </div>
  <p id="strength-label" class="text-xs text-gray-400"></p>
  
  <!-- Requirements checklist -->
  <ul class="space-y-1 mt-2">
    <li id="req-len" class="flex items-center gap-1.5 text-xs text-gray-400"><span>○</span> อย่างน้อย 8 ตัวอักษร</li>
    <li id="req-upper" class="flex items-center gap-1.5 text-xs text-gray-400"><span>○</span> มีตัวพิมพ์ใหญ่</li>
    <li id="req-num" class="flex items-center gap-1.5 text-xs text-gray-400"><span>○</span> มีตัวเลข</li>
    <li id="req-special" class="flex items-center gap-1.5 text-xs text-gray-400"><span>○</span> มีอักขระพิเศษ</li>
  </ul>
</div>

<script>
function togglePwd() {
  const input = document.getElementById('password');
  input.type = input.type === 'password' ? 'text' : 'password';
}

function checkStrength(val) {
  const checks = {
    len:     val.length >= 8,
    upper:   /[A-Z]/.test(val),
    num:     /[0-9]/.test(val),
    special: /[^A-Za-z0-9]/.test(val),
  };
  
  Object.entries(checks).forEach(([key, ok]) => {
    const el = document.getElementById(`req-${key}`);
    el.className = `flex items-center gap-1.5 text-xs ${ok ? 'text-green-600' : 'text-gray-400'}`;
    el.querySelector('span').textContent = ok ? '✓' : '○';
  });
  
  const score = Object.values(checks).filter(Boolean).length;
  const colors = ['', 'bg-red-500', 'bg-orange-400', 'bg-yellow-400', 'bg-green-500'];
  const labels = ['', 'อ่อนมาก', 'อ่อน', 'ปานกลาง', 'แข็งแกร่ง'];
  const textColors = ['', 'text-red-500', 'text-orange-500', 'text-yellow-600', 'text-green-600'];
  
  for (let i = 1; i <= 4; i++) {
    const bar = document.getElementById(`bar-${i}`);
    bar.className = `h-1.5 flex-1 rounded-full transition-all ${i <= score ? colors[score] : 'bg-gray-200'}`;
  }
  document.getElementById('strength-label').className = `text-xs ${textColors[score] || 'text-gray-400'}`;
  document.getElementById('strength-label').textContent = val ? labels[score] || '' : '';
}
</script>
```

---

## Step 264–270: Workshop — Complete Multi-Step Registration Form

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Multi-Step Form</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gradient-to-br from-indigo-50 via-white to-purple-50 min-h-screen flex items-center justify-center p-4">

<div class="w-full max-w-lg">
  <!-- Card -->
  <div class="bg-white rounded-3xl shadow-xl border border-gray-100 overflow-hidden">
    <!-- Header -->
    <div class="bg-gradient-to-r from-indigo-600 to-purple-600 p-6 text-white">
      <h1 class="text-xl font-black">สมัครสมาชิก</h1>
      <p class="text-indigo-200 text-sm mt-1">กรอกข้อมูลเพื่อเริ่มเรียน</p>
      
      <!-- Steps -->
      <div class="flex items-center gap-1 mt-5">
        <div id="s1" class="size-8 rounded-full bg-white text-indigo-600 font-bold text-sm flex items-center justify-center flex-none">1</div>
        <div id="l1" class="flex-1 h-0.5 bg-indigo-400"></div>
        <div id="s2" class="size-8 rounded-full bg-indigo-400 text-white font-bold text-sm flex items-center justify-center flex-none">2</div>
        <div id="l2" class="flex-1 h-0.5 bg-indigo-400"></div>
        <div id="s3" class="size-8 rounded-full bg-indigo-400 text-white font-bold text-sm flex items-center justify-center flex-none">3</div>
      </div>
      <div class="flex text-[10px] text-indigo-200 mt-1.5 font-medium">
        <span class="flex-none w-8 text-center text-white">ข้อมูล</span>
        <span class="flex-1"></span>
        <span class="flex-none w-8 text-center">รหัสผ่าน</span>
        <span class="flex-1"></span>
        <span class="flex-none w-8 text-center">ยืนยัน</span>
      </div>
    </div>
    
    <!-- Form body -->
    <div class="p-6">
      <!-- STEP 1 -->
      <div id="step-1">
        <h2 class="font-bold text-gray-900 mb-5">ข้อมูลส่วนตัว</h2>
        <div class="space-y-4">
          <div class="grid grid-cols-2 gap-3">
            <div>
              <label class="block text-xs font-semibold text-gray-700 mb-1.5">ชื่อ <span class="text-red-500">*</span></label>
              <input id="fname" type="text" placeholder="สมชาย" class="w-full px-3 py-2.5 text-sm rounded-xl border border-gray-200 focus:outline-none focus:ring-2 focus:ring-indigo-200">
            </div>
            <div>
              <label class="block text-xs font-semibold text-gray-700 mb-1.5">นามสกุล <span class="text-red-500">*</span></label>
              <input id="lname" type="text" placeholder="ใจดี" class="w-full px-3 py-2.5 text-sm rounded-xl border border-gray-200 focus:outline-none focus:ring-2 focus:ring-indigo-200">
            </div>
          </div>
          <div>
            <label class="block text-xs font-semibold text-gray-700 mb-1.5">อีเมล <span class="text-red-500">*</span></label>
            <input id="email" type="email" placeholder="somchai@example.com" class="w-full px-3 py-2.5 text-sm rounded-xl border border-gray-200 focus:outline-none focus:ring-2 focus:ring-indigo-200">
            <p id="email-err" class="hidden text-xs text-red-500 mt-1">⚠ อีเมลไม่ถูกต้อง</p>
          </div>
          <div>
            <label class="block text-xs font-semibold text-gray-700 mb-1.5">เบอร์โทร</label>
            <input type="tel" placeholder="08X-XXX-XXXX" class="w-full px-3 py-2.5 text-sm rounded-xl border border-gray-200 focus:outline-none focus:ring-2 focus:ring-indigo-200">
          </div>
        </div>
      </div>

      <!-- STEP 2 -->
      <div id="step-2" class="hidden">
        <h2 class="font-bold text-gray-900 mb-5">ตั้งรหัสผ่าน</h2>
        <div class="space-y-4">
          <div>
            <label class="block text-xs font-semibold text-gray-700 mb-1.5">รหัสผ่าน <span class="text-red-500">*</span></label>
            <input id="pwd" type="password" oninput="checkPwd(this.value)" placeholder="อย่างน้อย 8 ตัวอักษร" class="w-full px-3 py-2.5 text-sm rounded-xl border border-gray-200 focus:outline-none focus:ring-2 focus:ring-indigo-200">
            <div class="flex gap-1 mt-2">
              <div id="pb1" class="h-1.5 flex-1 rounded-full bg-gray-200 transition-all"></div>
              <div id="pb2" class="h-1.5 flex-1 rounded-full bg-gray-200 transition-all"></div>
              <div id="pb3" class="h-1.5 flex-1 rounded-full bg-gray-200 transition-all"></div>
              <div id="pb4" class="h-1.5 flex-1 rounded-full bg-gray-200 transition-all"></div>
            </div>
          </div>
          <div>
            <label class="block text-xs font-semibold text-gray-700 mb-1.5">ยืนยันรหัสผ่าน <span class="text-red-500">*</span></label>
            <input id="pwd2" type="password" placeholder="กรอกรหัสผ่านอีกครั้ง" class="w-full px-3 py-2.5 text-sm rounded-xl border border-gray-200 focus:outline-none focus:ring-2 focus:ring-indigo-200">
            <p id="pwd-err" class="hidden text-xs text-red-500 mt-1">⚠ รหัสผ่านไม่ตรงกัน</p>
          </div>
        </div>
      </div>

      <!-- STEP 3: Review -->
      <div id="step-3" class="hidden">
        <h2 class="font-bold text-gray-900 mb-5">ตรวจสอบข้อมูล</h2>
        <div class="bg-gray-50 rounded-xl p-4 space-y-3 text-sm">
          <div class="flex justify-between"><span class="text-gray-500">ชื่อ</span><span id="rev-name" class="font-medium text-gray-900">—</span></div>
          <div class="flex justify-between"><span class="text-gray-500">อีเมล</span><span id="rev-email" class="font-medium text-gray-900">—</span></div>
          <div class="flex justify-between"><span class="text-gray-500">รหัสผ่าน</span><span class="font-medium text-gray-900">••••••••</span></div>
        </div>
        <label class="flex items-start gap-3 mt-5 cursor-pointer">
          <input type="checkbox" id="agree" class="mt-0.5 rounded border-gray-300 text-indigo-600">
          <span class="text-xs text-gray-500">ฉันยอมรับ <a href="#" class="text-indigo-600 underline">เงื่อนไขการใช้งาน</a> และ <a href="#" class="text-indigo-600 underline">นโยบายความเป็นส่วนตัว</a></span>
        </label>
      </div>

      <!-- STEP 4: Success -->
      <div id="step-4" class="hidden text-center py-4">
        <div class="size-20 rounded-full bg-green-100 flex items-center justify-center text-4xl mx-auto mb-5">🎉</div>
        <h2 class="text-xl font-black text-gray-900">สมัครสำเร็จ!</h2>
        <p class="text-gray-500 text-sm mt-2">ยินดีต้อนรับสู่ TailwindPro</p>
        <button class="mt-6 w-full py-3 bg-indigo-600 text-white font-semibold rounded-xl hover:bg-indigo-700 transition-colors">เริ่มเรียนเลย →</button>
      </div>

      <!-- Navigation -->
      <div id="nav-buttons" class="flex gap-3 mt-6">
        <button id="btn-back" onclick="goBack()" class="hidden flex-1 py-3 border border-gray-200 text-gray-600 font-semibold rounded-xl hover:bg-gray-50 transition-colors text-sm">← ย้อนกลับ</button>
        <button id="btn-next" onclick="goNext()" class="flex-1 py-3 bg-indigo-600 text-white font-semibold rounded-xl hover:bg-indigo-700 transition-colors text-sm">ถัดไป →</button>
      </div>
    </div>
  </div>
</div>

<script>
let currentStep = 1;

function checkPwd(val) {
  const score = [val.length >= 8, /[A-Z]/.test(val), /[0-9]/.test(val), /[^A-Za-z0-9]/.test(val)].filter(Boolean).length;
  const colors = ['', 'bg-red-500', 'bg-orange-400', 'bg-yellow-400', 'bg-green-500'];
  for (let i = 1; i <= 4; i++) {
    document.getElementById(`pb${i}`).className = `h-1.5 flex-1 rounded-full transition-all ${i <= score ? colors[score] : 'bg-gray-200'}`;
  }
}

function goNext() {
  if (currentStep === 1) {
    const email = document.getElementById('email').value;
    const fname = document.getElementById('fname').value;
    if (!fname || !email || !/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) {
      document.getElementById('email-err').classList.remove('hidden');
      return;
    }
    document.getElementById('email-err').classList.add('hidden');
  }
  if (currentStep === 2) {
    const p1 = document.getElementById('pwd').value;
    const p2 = document.getElementById('pwd2').value;
    if (!p1 || p1 !== p2) {
      document.getElementById('pwd-err').classList.remove('hidden');
      return;
    }
    document.getElementById('pwd-err').classList.add('hidden');
    // populate review
    document.getElementById('rev-name').textContent = document.getElementById('fname').value + ' ' + document.getElementById('lname').value;
    document.getElementById('rev-email').textContent = document.getElementById('email').value;
  }
  if (currentStep === 3) {
    if (!document.getElementById('agree').checked) {
      alert('กรุณายอมรับเงื่อนไข');
      return;
    }
    document.getElementById('step-3').classList.add('hidden');
    document.getElementById('step-4').classList.remove('hidden');
    document.getElementById('nav-buttons').classList.add('hidden');
    updateStepUI(4);
    return;
  }
  
  document.getElementById(`step-${currentStep}`).classList.add('hidden');
  currentStep++;
  document.getElementById(`step-${currentStep}`).classList.remove('hidden');
  updateStepUI(currentStep);
}

function goBack() {
  document.getElementById(`step-${currentStep}`).classList.add('hidden');
  currentStep--;
  document.getElementById(`step-${currentStep}`).classList.remove('hidden');
  updateStepUI(currentStep);
}

function updateStepUI(step) {
  document.getElementById('btn-back').classList.toggle('hidden', step <= 1);
  document.getElementById('btn-next').textContent = step === 3 ? 'สมัครสมาชิก ✓' : 'ถัดไป →';
  
  ['s1','s2','s3'].forEach((id, i) => {
    const el = document.getElementById(id);
    const n = i + 1;
    if (n < step) {
      el.className = 'size-8 rounded-full bg-green-400 text-white font-bold text-sm flex items-center justify-center flex-none';
      el.textContent = '✓';
    } else if (n === step) {
      el.className = 'size-8 rounded-full bg-white text-indigo-600 font-bold text-sm flex items-center justify-center flex-none';
      el.textContent = n;
    } else {
      el.className = 'size-8 rounded-full bg-indigo-400 text-white font-bold text-sm flex items-center justify-center flex-none';
      el.textContent = n;
    }
  });
}
</script>

</body>
</html>
```

---

## 📝 สรุป Part 27

| Feature | เทคนิค |
|---------|---------|
| Step indicator | flex + conditional ring/color |
| Real-time validation | oninput + classList toggle |
| Password strength | score bars + checklist |
| Multi-step JS | show/hide div + step counter |

---

*Part 27 — Multi-Step Form | Steps 261–270 จาก 1,000 Steps*
