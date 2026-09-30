# Part 74: Form Wizard & Multi-step Forms

## เป้าหมาย
- Multi-step form พร้อม stepper UI
- Validation ต่อ step
- Progress bar + step indicators
- Summary + confirm step
- Steps 731–740

---

## Steps 731–740: Form Wizard Workshop

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Form Wizard - Steps 731-740</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    @keyframes stepIn  { from { opacity:0; transform:translateX(32px); } to { opacity:1; transform:translateX(0); } }
    @keyframes stepOut { from { opacity:1; transform:translateX(0); } to { opacity:0; transform:translateX(-32px); } }
    .step-in  { animation: stepIn  0.3s ease forwards; }
    .step-out { animation: stepOut 0.2s ease forwards; }
    .field-error input, .field-error select, .field-error textarea { border-color: #f87171 !important; }
  </style>
</head>
<body class="min-h-screen bg-gradient-to-br from-indigo-50 via-white to-purple-50 flex items-center justify-center p-4">

  <div class="w-full max-w-xl">
    <!-- Card -->
    <div class="bg-white rounded-3xl shadow-xl overflow-hidden">

      <!-- Progress bar -->
      <div class="h-1.5 bg-gray-100">
        <div id="progressBar" class="h-full bg-indigo-600 transition-all duration-500 ease-out" style="width:20%"></div>
      </div>

      <!-- Header -->
      <div class="px-8 pt-7 pb-3">
        <div class="flex items-center justify-between mb-5">
          <div>
            <p id="stepLabel" class="text-xs font-semibold text-indigo-600 uppercase tracking-wide">ขั้นตอนที่ 1 จาก 5</p>
            <h1 id="stepTitle" class="text-xl font-extrabold text-gray-900 mt-0.5">ข้อมูลส่วนตัว</h1>
          </div>
          <!-- Step dots -->
          <div class="flex gap-1.5">
            <div id="dot1" class="w-2.5 h-2.5 rounded-full bg-indigo-600 transition-colors"></div>
            <div id="dot2" class="w-2.5 h-2.5 rounded-full bg-gray-200 transition-colors"></div>
            <div id="dot3" class="w-2.5 h-2.5 rounded-full bg-gray-200 transition-colors"></div>
            <div id="dot4" class="w-2.5 h-2.5 rounded-full bg-gray-200 transition-colors"></div>
            <div id="dot5" class="w-2.5 h-2.5 rounded-full bg-gray-200 transition-colors"></div>
          </div>
        </div>

        <!-- Stepper line -->
        <div class="flex items-center mb-6">
          <div id="s1" class="flex flex-col items-center">
            <div class="w-8 h-8 rounded-full bg-indigo-600 text-white text-xs flex items-center justify-center font-bold">1</div>
            <p class="text-[10px] text-indigo-600 font-semibold mt-1">ข้อมูล</p>
          </div>
          <div id="line12" class="flex-1 h-0.5 bg-gray-200 mx-1 transition-colors"></div>
          <div id="s2" class="flex flex-col items-center">
            <div class="w-8 h-8 rounded-full bg-gray-200 text-gray-500 text-xs flex items-center justify-center font-bold">2</div>
            <p class="text-[10px] text-gray-400 font-semibold mt-1">ที่อยู่</p>
          </div>
          <div id="line23" class="flex-1 h-0.5 bg-gray-200 mx-1 transition-colors"></div>
          <div id="s3" class="flex flex-col items-center">
            <div class="w-8 h-8 rounded-full bg-gray-200 text-gray-500 text-xs flex items-center justify-center font-bold">3</div>
            <p class="text-[10px] text-gray-400 font-semibold mt-1">แผนการ</p>
          </div>
          <div id="line34" class="flex-1 h-0.5 bg-gray-200 mx-1 transition-colors"></div>
          <div id="s4" class="flex flex-col items-center">
            <div class="w-8 h-8 rounded-full bg-gray-200 text-gray-500 text-xs flex items-center justify-center font-bold">4</div>
            <p class="text-[10px] text-gray-400 font-semibold mt-1">ชำระ</p>
          </div>
          <div id="line45" class="flex-1 h-0.5 bg-gray-200 mx-1 transition-colors"></div>
          <div id="s5" class="flex flex-col items-center">
            <div class="w-8 h-8 rounded-full bg-gray-200 text-gray-500 text-xs flex items-center justify-center font-bold">5</div>
            <p class="text-[10px] text-gray-400 font-semibold mt-1">ยืนยัน</p>
          </div>
        </div>
      </div>

      <!-- Step content -->
      <div id="stepContent" class="px-8 pb-6">

        <!-- Step 1: Personal Info -->
        <div id="step1" class="step-in space-y-4">
          <div class="grid grid-cols-2 gap-3">
            <div id="f-firstname">
              <label class="block text-xs font-semibold text-gray-600 mb-1.5">ชื่อ *</label>
              <input id="firstname" placeholder="ชื่อจริง" class="w-full border border-gray-300 rounded-xl px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-400 transition-colors">
              <p class="err-msg text-xs text-red-500 mt-1 hidden">กรุณากรอกชื่อ</p>
            </div>
            <div id="f-lastname">
              <label class="block text-xs font-semibold text-gray-600 mb-1.5">นามสกุล *</label>
              <input id="lastname" placeholder="นามสกุล" class="w-full border border-gray-300 rounded-xl px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-400 transition-colors">
              <p class="err-msg text-xs text-red-500 mt-1 hidden">กรุณากรอกนามสกุล</p>
            </div>
          </div>
          <div id="f-email">
            <label class="block text-xs font-semibold text-gray-600 mb-1.5">อีเมล *</label>
            <input id="email" type="email" placeholder="you@example.com" class="w-full border border-gray-300 rounded-xl px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-400 transition-colors">
            <p class="err-msg text-xs text-red-500 mt-1 hidden">กรุณากรอกอีเมลที่ถูกต้อง</p>
          </div>
          <div id="f-phone">
            <label class="block text-xs font-semibold text-gray-600 mb-1.5">เบอร์โทร</label>
            <input id="phone" type="tel" placeholder="081-234-5678" class="w-full border border-gray-300 rounded-xl px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-400 transition-colors">
          </div>
          <div id="f-dob">
            <label class="block text-xs font-semibold text-gray-600 mb-1.5">วันเกิด</label>
            <input id="dob" type="date" class="w-full border border-gray-300 rounded-xl px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-400 transition-colors">
          </div>
        </div>

        <!-- Step 2: Address -->
        <div id="step2" class="hidden step-in space-y-4">
          <div id="f-address">
            <label class="block text-xs font-semibold text-gray-600 mb-1.5">ที่อยู่ *</label>
            <textarea id="address" rows="2" placeholder="บ้านเลขที่ ถนน ซอย" class="w-full border border-gray-300 rounded-xl px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-400 transition-colors resize-none"></textarea>
            <p class="err-msg text-xs text-red-500 mt-1 hidden">กรุณากรอกที่อยู่</p>
          </div>
          <div class="grid grid-cols-2 gap-3">
            <div id="f-city">
              <label class="block text-xs font-semibold text-gray-600 mb-1.5">เมือง *</label>
              <input id="city" placeholder="กรุงเทพฯ" class="w-full border border-gray-300 rounded-xl px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-400 transition-colors">
              <p class="err-msg text-xs text-red-500 mt-1 hidden">กรุณากรอกเมือง</p>
            </div>
            <div>
              <label class="block text-xs font-semibold text-gray-600 mb-1.5">รหัสไปรษณีย์</label>
              <input id="postcode" placeholder="10110" class="w-full border border-gray-300 rounded-xl px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-400 transition-colors">
            </div>
          </div>
          <div>
            <label class="block text-xs font-semibold text-gray-600 mb-1.5">จังหวัด</label>
            <select id="province" class="w-full border border-gray-300 rounded-xl px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-400 transition-colors">
              <option value="">เลือกจังหวัด</option>
              <option>กรุงเทพมหานคร</option>
              <option>เชียงใหม่</option>
              <option>ภูเก็ต</option>
              <option>ขอนแก่น</option>
              <option>นครราชสีมา</option>
            </select>
          </div>
        </div>

        <!-- Step 3: Plan -->
        <div id="step3" class="hidden step-in">
          <p class="text-sm text-gray-500 mb-4">เลือกแผนที่เหมาะกับคุณ *</p>
          <div class="space-y-3" id="planGroup">
            <label class="flex items-center gap-3 border-2 border-gray-200 rounded-2xl p-4 cursor-pointer hover:border-indigo-300 transition-colors has-[:checked]:border-indigo-500 has-[:checked]:bg-indigo-50">
              <input type="radio" name="plan" value="starter" class="accent-indigo-600" checked>
              <div class="flex-1">
                <div class="flex justify-between">
                  <p class="font-semibold text-gray-900">Starter</p>
                  <p class="font-extrabold text-indigo-700">฿290<span class="text-xs font-normal text-gray-500">/เดือน</span></p>
                </div>
                <p class="text-xs text-gray-500 mt-0.5">5 projects, 10GB storage</p>
              </div>
            </label>
            <label class="flex items-center gap-3 border-2 border-gray-200 rounded-2xl p-4 cursor-pointer hover:border-indigo-300 transition-colors has-[:checked]:border-indigo-500 has-[:checked]:bg-indigo-50">
              <input type="radio" name="plan" value="pro" class="accent-indigo-600">
              <div class="flex-1">
                <div class="flex justify-between">
                  <p class="font-semibold text-gray-900">Pro <span class="text-xs bg-indigo-600 text-white px-1.5 py-0.5 rounded-full ml-1">Popular</span></p>
                  <p class="font-extrabold text-indigo-700">฿790<span class="text-xs font-normal text-gray-500">/เดือน</span></p>
                </div>
                <p class="text-xs text-gray-500 mt-0.5">Unlimited projects, 100GB, Priority support</p>
              </div>
            </label>
            <label class="flex items-center gap-3 border-2 border-gray-200 rounded-2xl p-4 cursor-pointer hover:border-indigo-300 transition-colors has-[:checked]:border-indigo-500 has-[:checked]:bg-indigo-50">
              <input type="radio" name="plan" value="enterprise" class="accent-indigo-600">
              <div class="flex-1">
                <div class="flex justify-between">
                  <p class="font-semibold text-gray-900">Enterprise</p>
                  <p class="font-extrabold text-indigo-700">฿2,490<span class="text-xs font-normal text-gray-500">/เดือน</span></p>
                </div>
                <p class="text-xs text-gray-500 mt-0.5">Custom limits, SLA 99.99%, Dedicated support</p>
              </div>
            </label>
          </div>
        </div>

        <!-- Step 4: Payment -->
        <div id="step4" class="hidden step-in space-y-4">
          <div id="f-card">
            <label class="block text-xs font-semibold text-gray-600 mb-1.5">หมายเลขบัตร *</label>
            <input id="cardno" placeholder="1234 5678 9012 3456" oninput="formatCard(this)" maxlength="19" class="w-full border border-gray-300 rounded-xl px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-400 font-mono tracking-widest transition-colors">
            <p class="err-msg text-xs text-red-500 mt-1 hidden">กรุณากรอกหมายเลขบัตร 16 หลัก</p>
          </div>
          <div class="grid grid-cols-2 gap-3">
            <div id="f-expiry">
              <label class="block text-xs font-semibold text-gray-600 mb-1.5">วันหมดอายุ *</label>
              <input id="expiry" placeholder="MM/YY" oninput="formatExpiry(this)" maxlength="5" class="w-full border border-gray-300 rounded-xl px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-400 font-mono transition-colors">
              <p class="err-msg text-xs text-red-500 mt-1 hidden">กรุณากรอกวันหมดอายุ</p>
            </div>
            <div id="f-cvv">
              <label class="block text-xs font-semibold text-gray-600 mb-1.5">CVV *</label>
              <input id="cvv" type="password" placeholder="•••" maxlength="4" class="w-full border border-gray-300 rounded-xl px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-400 font-mono transition-colors">
              <p class="err-msg text-xs text-red-500 mt-1 hidden">กรุณากรอก CVV</p>
            </div>
          </div>
          <div>
            <label class="block text-xs font-semibold text-gray-600 mb-1.5">ชื่อบนบัตร</label>
            <input id="cardname" placeholder="SOMCHAI WONGKAM" class="w-full border border-gray-300 rounded-xl px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-400 uppercase transition-colors">
          </div>
          <div class="flex items-center gap-2 bg-gray-50 rounded-xl p-3">
            <svg class="w-4 h-4 text-emerald-600" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z"/></svg>
            <p class="text-xs text-gray-600">ข้อมูลบัตรของคุณถูกเข้ารหัส 256-bit SSL</p>
          </div>
        </div>

        <!-- Step 5: Confirmation -->
        <div id="step5" class="hidden step-in">
          <div id="summaryContent" class="space-y-3"></div>
          <div class="flex items-start gap-2.5 mt-4 bg-emerald-50 border border-emerald-200 rounded-xl p-3">
            <input type="checkbox" id="confirmCheck" class="w-4 h-4 rounded accent-emerald-600 mt-0.5">
            <label for="confirmCheck" class="text-xs text-gray-600">ฉันยืนยันว่าข้อมูลที่กรอกทั้งหมดถูกต้อง และยอมรับ <a href="#" class="text-indigo-600 hover:underline">เงื่อนไขการให้บริการ</a></label>
          </div>
        </div>

      </div>

      <!-- Navigation -->
      <div class="px-8 pb-8 flex justify-between gap-3">
        <button id="backBtn" onclick="prevStep()" class="hidden px-5 py-2.5 border border-gray-300 text-gray-700 rounded-xl text-sm font-semibold hover:bg-gray-50 transition-colors">← ย้อนกลับ</button>
        <div class="flex-1" id="backSpacer"></div>
        <button id="nextBtn" onclick="nextStep()" class="px-6 py-2.5 bg-indigo-600 text-white rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors flex items-center gap-1.5">
          ถัดไป →
        </button>
      </div>
    </div>
  </div>

  <script>
    let currentStep = 1;
    const totalSteps = 5;
    const stepTitles = ['ข้อมูลส่วนตัว','ที่อยู่','เลือกแผน','ชำระเงิน','ยืนยัน'];

    function updateUI() {
      document.getElementById('stepLabel').textContent = `ขั้นตอนที่ ${currentStep} จาก ${totalSteps}`;
      document.getElementById('stepTitle').textContent = stepTitles[currentStep-1];
      document.getElementById('progressBar').style.width = `${(currentStep / totalSteps) * 100}%`;

      // Dots
      for (let i = 1; i <= totalSteps; i++) {
        const dot = document.getElementById('dot' + i);
        dot.className = `w-2.5 h-2.5 rounded-full transition-colors ${i < currentStep ? 'bg-emerald-500' : i === currentStep ? 'bg-indigo-600' : 'bg-gray-200'}`;
      }

      // Stepper
      const stepConfig = [
        { id:'s1', label:'ข้อมูล' }, { id:'s2', label:'ที่อยู่' }, { id:'s3', label:'แผนการ' },
        { id:'s4', label:'ชำระ' }, { id:'s5', label:'ยืนยัน' },
      ];
      stepConfig.forEach((s, i) => {
        const n = i + 1;
        const el = document.getElementById(s.id);
        const done = n < currentStep, active = n === currentStep;
        const circleClass = done ? 'bg-emerald-500 text-white' : active ? 'bg-indigo-600 text-white' : 'bg-gray-200 text-gray-500';
        const textClass = done || active ? 'text-indigo-600' : 'text-gray-400';
        el.innerHTML = `<div class="w-8 h-8 rounded-full ${circleClass} text-xs flex items-center justify-center font-bold">${done ? '✓' : n}</div><p class="text-[10px] ${textClass} font-semibold mt-1">${s.label}</p>`;
      });

      // Lines
      ['12','23','34','45'].forEach((pair, i) => {
        const el = document.getElementById('line' + pair);
        el.className = `flex-1 h-0.5 mx-1 transition-colors ${currentStep > i+1 ? 'bg-emerald-500' : 'bg-gray-200'}`;
      });

      // Back/Next buttons
      const backBtn = document.getElementById('backBtn');
      const backSpacer = document.getElementById('backSpacer');
      const nextBtn = document.getElementById('nextBtn');
      if (currentStep > 1) { backBtn.classList.remove('hidden'); backSpacer.classList.add('hidden'); }
      else { backBtn.classList.add('hidden'); backSpacer.classList.remove('hidden'); }
      nextBtn.textContent = currentStep === totalSteps ? '✅ ยืนยันการสมัคร' : 'ถัดไป →';

      if (currentStep === 5) buildSummary();
    }

    function buildSummary() {
      const plan = document.querySelector('input[name="plan"]:checked')?.value || 'starter';
      const prices = { starter:'฿290', pro:'฿790', enterprise:'฿2,490' };
      document.getElementById('summaryContent').innerHTML = `
        <div class="bg-gray-50 rounded-xl p-4 space-y-2.5">
          <div class="flex justify-between text-sm"><span class="text-gray-500">ชื่อ-นามสกุล</span><span class="font-semibold">${document.getElementById('firstname').value || '—'} ${document.getElementById('lastname').value || ''}</span></div>
          <div class="flex justify-between text-sm"><span class="text-gray-500">อีเมล</span><span class="font-semibold">${document.getElementById('email').value || '—'}</span></div>
          <div class="flex justify-between text-sm"><span class="text-gray-500">ที่อยู่</span><span class="font-semibold text-right max-w-[60%]">${document.getElementById('city').value || '—'}</span></div>
          <hr class="border-gray-200">
          <div class="flex justify-between text-sm"><span class="text-gray-500">แผน</span><span class="font-semibold capitalize">${plan}</span></div>
          <div class="flex justify-between text-sm font-extrabold text-indigo-700"><span>ยอดชำระ</span><span>${prices[plan]}/เดือน</span></div>
        </div>
      `;
    }

    function validate() {
      let ok = true;
      const setErr = (id, msg, show) => {
        const field = document.getElementById('f-' + id);
        if (!field) return;
        const errEl = field.querySelector('.err-msg');
        const input = field.querySelector('input, select, textarea');
        if (show) { input?.classList.add('border-red-400'); if (errEl) { errEl.textContent = msg; errEl.classList.remove('hidden'); } ok = false; }
        else { input?.classList.remove('border-red-400'); if (errEl) errEl.classList.add('hidden'); }
      };

      if (currentStep === 1) {
        setErr('firstname', 'กรุณากรอกชื่อ', !document.getElementById('firstname').value.trim());
        setErr('lastname',  'กรุณากรอกนามสกุล', !document.getElementById('lastname').value.trim());
        const email = document.getElementById('email').value;
        setErr('email', 'กรุณากรอกอีเมลที่ถูกต้อง', !email || !/^[^@]+@[^@]+\.[^@]+$/.test(email));
      }
      if (currentStep === 2) {
        setErr('address', 'กรุณากรอกที่อยู่', !document.getElementById('address').value.trim());
        setErr('city', 'กรุณากรอกเมือง', !document.getElementById('city').value.trim());
      }
      if (currentStep === 4) {
        const card = document.getElementById('cardno').value.replace(/\s/g,'');
        setErr('card', 'กรุณากรอกหมายเลขบัตร 16 หลัก', card.length !== 16);
        setErr('expiry', 'กรุณากรอกวันหมดอายุ', !document.getElementById('expiry').value.match(/^\d{2}\/\d{2}$/));
        setErr('cvv', 'กรุณากรอก CVV', !document.getElementById('cvv').value.match(/^\d{3,4}$/));
      }
      return ok;
    }

    function nextStep() {
      if (!validate()) return;
      if (currentStep === 5) { submitForm(); return; }
      document.getElementById('step' + currentStep).classList.add('hidden');
      currentStep++;
      const el = document.getElementById('step' + currentStep);
      el.classList.remove('hidden');
      el.className = el.className.replace('step-out', '') + ' step-in';
      updateUI();
    }

    function prevStep() {
      document.getElementById('step' + currentStep).classList.add('hidden');
      currentStep--;
      document.getElementById('step' + currentStep).classList.remove('hidden');
      updateUI();
    }

    function submitForm() {
      if (!document.getElementById('confirmCheck').checked) { alert('กรุณายืนยันข้อมูล'); return; }
      const btn = document.getElementById('nextBtn');
      btn.innerHTML = '<svg class="w-4 h-4 animate-spin" fill="none" viewBox="0 0 24 24"><circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle><path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4z"></path></svg> กำลังสมัคร...';
      btn.disabled = true;
      setTimeout(() => {
        document.querySelector('.bg-white.rounded-3xl').innerHTML = `
          <div class="p-12 text-center">
            <div class="w-20 h-20 bg-emerald-100 rounded-full flex items-center justify-center mx-auto mb-5 text-4xl">🎉</div>
            <h2 class="text-2xl font-extrabold text-gray-900 mb-2">สมัครสำเร็จ!</h2>
            <p class="text-gray-500 mb-6">ยินดีต้อนรับ เราส่งอีเมลยืนยันไปแล้ว</p>
            <button onclick="location.reload()" class="bg-indigo-600 text-white px-8 py-3 rounded-xl font-semibold hover:bg-indigo-700 transition-colors">เริ่มใช้งาน →</button>
          </div>`;
      }, 2000);
    }

    function formatCard(el) {
      let val = el.value.replace(/\D/g,'').slice(0,16);
      el.value = val.replace(/(.{4})/g,'$1 ').trim();
    }
    function formatExpiry(el) {
      let val = el.value.replace(/\D/g,'').slice(0,4);
      if (val.length >= 2) val = val.slice(0,2) + '/' + val.slice(2);
      el.value = val;
    }

    updateUI();
  </script>
</body>
</html>
```

---

## สรุป Part 74

| Step | เนื้อหา |
|------|---------|
| 731 | Multi-step progress bar |
| 732 | Step dots + numbered stepper |
| 733 | Step 1 — personal info validation |
| 734 | Step 2 — address form |
| 735 | Step 3 — plan picker (radio cards) |
| 736 | Step 4 — payment form + card formatting |
| 737 | Step 5 — summary review + confirm checkbox |
| 738 | Per-step validation + error messages |
| 739 | Slide-in / slide-out animation |
| 740 | Workshop: Complete Form Wizard — 5 steps, validation, success state |

**Part ถัดไป:** Part 75 — Advanced Animation (Steps 741–750)
