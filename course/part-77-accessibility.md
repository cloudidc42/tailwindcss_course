# Part 77: Accessibility Deep Dive

## เป้าหมาย
- ARIA roles, labels, live regions
- Focus management + keyboard traps
- Color contrast + visual indicators
- Screen reader patterns
- Steps 761–770

---

## Steps 761–770: Accessibility Workshop

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Accessibility - Steps 761-770</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    /* Step 761: Focus-visible ring สีชัดเจน */
    :focus-visible { outline: 3px solid #4f46e5; outline-offset: 2px; border-radius: 4px; }
    /* ซ่อน outline ตอน mouse click เท่านั้น */
    :focus:not(:focus-visible) { outline: none; }

    /* Step 762: Skip link */
    .skip-link { position: absolute; left: -9999px; }
    .skip-link:focus { position: fixed; top: 1rem; left: 1rem; z-index: 9999; background: #4f46e5; color: white; padding: 0.5rem 1rem; border-radius: 8px; font-weight: 700; }

    /* High contrast mode support */
    @media (forced-colors: active) {
      .focus-ring { forced-color-adjust: auto; }
    }

    /* Reduced motion */
    @media (prefers-reduced-motion: reduce) {
      *, *::before, *::after { animation-duration: 0.01ms !important; transition-duration: 0.01ms !important; }
    }

    /* Visible focus styles for interactive elements */
    button:focus-visible, a:focus-visible, input:focus-visible, select:focus-visible, textarea:focus-visible {
      outline: 3px solid #4f46e5;
      outline-offset: 2px;
    }
  </style>
</head>
<body class="bg-gray-50">

  <!-- Step 762: Skip Navigation -->
  <a href="#main-content" class="skip-link">ข้ามไปยังเนื้อหาหลัก</a>

  <div class="max-w-3xl mx-auto p-6">
    <h1 id="main-content" class="text-2xl font-extrabold text-gray-900 mb-2 focus:outline-none" tabindex="-1">Accessibility Deep Dive</h1>
    <p class="text-gray-600 mb-8">ตัวอย่าง a11y patterns ครบถ้วน — กด Tab เพื่อทดสอบ keyboard navigation</p>

    <!-- Live Region Demo -->
    <section class="bg-white rounded-2xl border p-6 mb-5" aria-labelledby="live-heading">
      <h2 id="live-heading" class="font-bold text-gray-900 mb-1">Step 763 — ARIA Live Region</h2>
      <p class="text-sm text-gray-500 mb-4">Screen reader จะอ่านเนื้อหาที่เปลี่ยนแปลงใน live region อัตโนมัติ</p>
      <div aria-live="polite" aria-atomic="true" id="liveRegion" class="bg-indigo-50 border border-indigo-200 rounded-xl px-4 py-3 text-sm text-indigo-800 min-h-[2.5rem] mb-4" role="status">
        รอการอัปเดต...
      </div>
      <div class="flex gap-2 flex-wrap">
        <button onclick="updateLive('✅ บันทึกสำเร็จแล้ว')" class="bg-emerald-600 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-emerald-700 transition-colors">บันทึก</button>
        <button onclick="updateLive('❌ เกิดข้อผิดพลาด กรุณาลองใหม่')" class="bg-red-600 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-red-700 transition-colors">เกิดข้อผิดพลาด</button>
        <button onclick="updateLive('⏳ กำลังโหลด...')" class="bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">โหลด</button>
      </div>
    </section>

    <!-- Alert vs Status -->
    <section class="bg-white rounded-2xl border p-6 mb-5">
      <h2 class="font-bold text-gray-900 mb-1">Step 763b — role="alert" vs role="status"</h2>
      <p class="text-sm text-gray-500 mb-4">
        <code class="bg-gray-100 px-1.5 py-0.5 rounded text-xs font-mono">role="alert"</code> = ด่วน (interrupt) /
        <code class="bg-gray-100 px-1.5 py-0.5 rounded text-xs font-mono">role="status"</code> = ไม่ด่วน (polite)
      </p>
      <div id="alertBox" class="hidden bg-red-50 border border-red-300 rounded-xl px-4 py-3 mb-3" role="alert">
        <p class="text-sm font-semibold text-red-700">⚠️ Session กำลังจะหมดอายุ กรุณาบันทึกงาน</p>
      </div>
      <button onclick="document.getElementById('alertBox').classList.toggle('hidden')" class="text-sm border border-gray-300 px-4 py-2 rounded-xl hover:bg-gray-50 transition-colors">Toggle Alert</button>
    </section>

    <!-- Accessible Form -->
    <section class="bg-white rounded-2xl border p-6 mb-5" aria-labelledby="form-heading">
      <h2 id="form-heading" class="font-bold text-gray-900 mb-1">Step 764 — Accessible Form</h2>
      <p class="text-sm text-gray-500 mb-4">ฟอร์มที่ถูกต้องตาม a11y — label, error, describedby</p>
      <form novalidate onsubmit="validateA11yForm(event)" class="space-y-4">
        <div>
          <label for="a11y-name" class="block text-sm font-semibold text-gray-700 mb-1.5">
            ชื่อ-นามสกุล <span aria-label="required" class="text-red-500">*</span>
          </label>
          <input type="text" id="a11y-name" name="name" autocomplete="name"
            aria-required="true" aria-describedby="a11y-name-error"
            class="w-full border border-gray-300 rounded-xl px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
          <p id="a11y-name-error" role="alert" class="text-xs text-red-600 mt-1 hidden">กรุณากรอกชื่อ-นามสกุล</p>
        </div>

        <div>
          <label for="a11y-email" class="block text-sm font-semibold text-gray-700 mb-1.5">
            อีเมล <span aria-label="required" class="text-red-500">*</span>
          </label>
          <input type="email" id="a11y-email" name="email" autocomplete="email"
            aria-required="true" aria-describedby="a11y-email-error a11y-email-hint"
            class="w-full border border-gray-300 rounded-xl px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
          <p id="a11y-email-hint" class="text-xs text-gray-500 mt-1">เช่น name@company.com</p>
          <p id="a11y-email-error" role="alert" class="text-xs text-red-600 mt-1 hidden">กรุณากรอกอีเมลที่ถูกต้อง</p>
        </div>

        <fieldset class="border border-gray-200 rounded-xl p-4">
          <legend class="text-sm font-semibold text-gray-700 px-1">ตำแหน่งงาน</legend>
          <div class="space-y-2 mt-2">
            <label class="flex items-center gap-2.5 text-sm cursor-pointer">
              <input type="radio" name="role" value="developer" class="accent-indigo-600">
              Developer
            </label>
            <label class="flex items-center gap-2.5 text-sm cursor-pointer">
              <input type="radio" name="role" value="designer" class="accent-indigo-600">
              Designer
            </label>
            <label class="flex items-center gap-2.5 text-sm cursor-pointer">
              <input type="radio" name="role" value="pm" class="accent-indigo-600">
              Product Manager
            </label>
          </div>
        </fieldset>

        <div>
          <label for="a11y-bio" class="block text-sm font-semibold text-gray-700 mb-1.5">ประวัติย่อ</label>
          <textarea id="a11y-bio" name="bio" rows="3"
            aria-describedby="a11y-bio-count"
            oninput="updateCharCount(this)" maxlength="200"
            class="w-full border border-gray-300 rounded-xl px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 resize-none"></textarea>
          <p id="a11y-bio-count" class="text-xs text-gray-400 text-right mt-1" aria-live="polite">0/200 ตัวอักษร</p>
        </div>

        <button type="submit" class="bg-indigo-600 text-white px-5 py-2.5 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">บันทึก</button>
      </form>
    </section>

    <!-- Focus Trap Modal -->
    <section class="bg-white rounded-2xl border p-6 mb-5">
      <h2 class="font-bold text-gray-900 mb-1">Step 765 — Focus Trap</h2>
      <p class="text-sm text-gray-500 mb-4">Modal ต้องดัก focus ไว้ภายใน — Tab จะวนในกล่อง modal เท่านั้น</p>
      <button onclick="openTrapModal()" class="bg-indigo-600 text-white px-5 py-2.5 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">เปิด Modal</button>
    </section>

    <!-- Color Contrast -->
    <section class="bg-white rounded-2xl border p-6 mb-5">
      <h2 class="font-bold text-gray-900 mb-1">Step 766 — Color Contrast</h2>
      <p class="text-sm text-gray-500 mb-4">WCAG AA ต้องการ contrast ratio 4.5:1+ สำหรับ normal text, 3:1+ สำหรับ large text</p>
      <div class="grid grid-cols-2 gap-3">
        <div class="rounded-xl p-4 bg-gray-900">
          <p class="text-white font-semibold text-sm">✅ สีขาวบนพื้นดำ</p>
          <p class="text-gray-300 text-xs mt-1">Contrast: 21:1 (Pass AAA)</p>
        </div>
        <div class="rounded-xl p-4 bg-gray-100">
          <p class="text-gray-900 font-semibold text-sm">✅ สีดำบนพื้นเทาอ่อน</p>
          <p class="text-gray-600 text-xs mt-1">Contrast: ~7:1 (Pass AA)</p>
        </div>
        <div class="rounded-xl p-4 bg-yellow-300">
          <p class="text-yellow-900 font-semibold text-sm">✅ สีเข้มบน Yellow</p>
          <p class="text-yellow-800 text-xs mt-1">Contrast: ~5:1 (Pass AA)</p>
        </div>
        <div class="rounded-xl p-4 bg-gray-200">
          <p class="text-gray-400 font-semibold text-sm">❌ สีเทาอ่อนบนพื้นเทา</p>
          <p class="text-gray-400 text-xs mt-1">Contrast: ~2.5:1 (Fail)</p>
        </div>
      </div>
    </section>

    <!-- Screen Reader Patterns -->
    <section class="bg-white rounded-2xl border p-6 mb-5">
      <h2 class="font-bold text-gray-900 mb-1">Steps 767–769 — Screen Reader Patterns</h2>
      <div class="space-y-4">

        <!-- Visually hidden text -->
        <div>
          <h3 class="text-sm font-semibold text-gray-700 mb-2">sr-only label สำหรับ icon buttons</h3>
          <div class="flex gap-2">
            <button aria-label="แก้ไข" class="w-9 h-9 flex items-center justify-center rounded-xl border hover:bg-gray-50 transition-colors" title="แก้ไข">
              ✏️
              <span class="sr-only">แก้ไข</span>
            </button>
            <button aria-label="ลบ" class="w-9 h-9 flex items-center justify-center rounded-xl border hover:bg-gray-50 transition-colors text-red-500" title="ลบ">
              🗑
              <span class="sr-only">ลบ</span>
            </button>
            <button aria-label="แชร์" class="w-9 h-9 flex items-center justify-center rounded-xl border hover:bg-gray-50 transition-colors" title="แชร์">
              📤
              <span class="sr-only">แชร์</span>
            </button>
          </div>
          <p class="text-xs text-gray-400 mt-1">Screen reader อ่านว่า "แก้ไข, ปุ่ม" / "ลบ, ปุ่ม" แม้ไม่มีข้อความบนหน้าจอ</p>
        </div>

        <!-- Decorative image -->
        <div>
          <h3 class="text-sm font-semibold text-gray-700 mb-2">Images — alt text</h3>
          <div class="flex gap-4 items-center flex-wrap">
            <div class="text-center">
              <div class="w-16 h-16 bg-indigo-100 rounded-xl flex items-center justify-center text-2xl">🖼</div>
              <code class="text-xs text-gray-500 block mt-1">alt="ภาพโปรไฟล์"</code>
            </div>
            <div class="text-center">
              <div class="w-16 h-16 bg-gray-100 rounded-xl flex items-center justify-center text-2xl">🎨</div>
              <code class="text-xs text-gray-500 block mt-1">alt="" (decorative)</code>
            </div>
          </div>
        </div>

        <!-- Loading state -->
        <div>
          <h3 class="text-sm font-semibold text-gray-700 mb-2">Loading state ที่ screen reader รับรู้</h3>
          <button onclick="simulateLoad(this)" aria-live="polite"
            class="flex items-center gap-2 bg-indigo-600 text-white px-4 py-2.5 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">
            <span id="loadText">บันทึก</span>
          </button>
        </div>

        <!-- Expanded/Collapsed -->
        <div>
          <h3 class="text-sm font-semibold text-gray-700 mb-2">aria-expanded สำหรับ accordion</h3>
          <div class="border rounded-xl overflow-hidden">
            <button onclick="toggleAccordion(this)" aria-expanded="false" aria-controls="acc-body"
              class="w-full flex justify-between items-center px-4 py-3 text-sm font-semibold text-gray-700 hover:bg-gray-50 transition-colors">
              <span>คำถามที่พบบ่อย</span>
              <span id="acc-icon" class="text-gray-400 transition-transform">▼</span>
            </button>
            <div id="acc-body" hidden class="px-4 py-3 border-t text-sm text-gray-600">
              คำตอบสำหรับคำถามที่พบบ่อย ผู้ใช้ screen reader รับรู้ว่า expanded/collapsed
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Step 770: Complete Checklist -->
    <section class="bg-indigo-50 border border-indigo-200 rounded-2xl p-6 mb-5">
      <h2 class="font-bold text-gray-900 mb-4">Step 770 — Accessibility Checklist</h2>
      <div class="grid grid-cols-1 sm:grid-cols-2 gap-3 text-sm">
        <div class="flex items-start gap-2"><span class="text-emerald-500 flex-shrink-0">✅</span> focus-visible outline ชัดเจน</div>
        <div class="flex items-start gap-2"><span class="text-emerald-500 flex-shrink-0">✅</span> Skip navigation link</div>
        <div class="flex items-start gap-2"><span class="text-emerald-500 flex-shrink-0">✅</span> ARIA labels ครบ</div>
        <div class="flex items-start gap-2"><span class="text-emerald-500 flex-shrink-0">✅</span> role="alert" สำหรับ errors</div>
        <div class="flex items-start gap-2"><span class="text-emerald-500 flex-shrink-0">✅</span> aria-live สำหรับ updates</div>
        <div class="flex items-start gap-2"><span class="text-emerald-500 flex-shrink-0">✅</span> aria-describedby สำหรับ hints</div>
        <div class="flex items-start gap-2"><span class="text-emerald-500 flex-shrink-0">✅</span> fieldset + legend สำหรับ groups</div>
        <div class="flex items-start gap-2"><span class="text-emerald-500 flex-shrink-0">✅</span> Color contrast ≥ 4.5:1</div>
        <div class="flex items-start gap-2"><span class="text-emerald-500 flex-shrink-0">✅</span> Icon buttons มี sr-only label</div>
        <div class="flex items-start gap-2"><span class="text-emerald-500 flex-shrink-0">✅</span> Images มี alt text</div>
        <div class="flex items-start gap-2"><span class="text-emerald-500 flex-shrink-0">✅</span> Focus trap ใน modals</div>
        <div class="flex items-start gap-2"><span class="text-emerald-500 flex-shrink-0">✅</span> aria-expanded สำหรับ toggles</div>
        <div class="flex items-start gap-2"><span class="text-emerald-500 flex-shrink-0">✅</span> prefers-reduced-motion</div>
        <div class="flex items-start gap-2"><span class="text-emerald-500 flex-shrink-0">✅</span> Keyboard navigable ทุก element</div>
      </div>
    </section>
  </div>

  <!-- Focus Trap Modal -->
  <div id="trapOverlay" class="hidden fixed inset-0 bg-black/50 z-50 flex items-center justify-center p-4">
    <div id="trapModal" role="dialog" aria-modal="true" aria-labelledby="trap-title"
         class="bg-white rounded-2xl p-6 w-full max-w-sm shadow-2xl">
      <h3 id="trap-title" class="font-bold text-gray-900 mb-1">Focus Trap Demo</h3>
      <p class="text-sm text-gray-600 mb-4">กด Tab — focus จะวนอยู่ใน modal นี้เท่านั้น</p>
      <div class="space-y-3 mb-5">
        <input placeholder="ชื่อ" class="w-full border border-gray-300 rounded-xl px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
        <input placeholder="อีเมล" class="w-full border border-gray-300 rounded-xl px-3.5 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
      </div>
      <div class="flex gap-2">
        <button onclick="closeTrapModal()" class="flex-1 bg-indigo-600 text-white py-2.5 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">ตกลง</button>
        <button id="cancelTrap" onclick="closeTrapModal()" class="flex-1 border border-gray-300 text-gray-700 py-2.5 rounded-xl text-sm font-semibold hover:bg-gray-50 transition-colors">ยกเลิก</button>
      </div>
    </div>
  </div>

  <script>
    function updateLive(msg) {
      const el = document.getElementById('liveRegion');
      el.textContent = msg;
    }

    function validateA11yForm(e) {
      e.preventDefault();
      let ok = true;
      const name = document.getElementById('a11y-name');
      const nameErr = document.getElementById('a11y-name-error');
      const email = document.getElementById('a11y-email');
      const emailErr = document.getElementById('a11y-email-error');

      if (!name.value.trim()) {
        nameErr.classList.remove('hidden'); name.setAttribute('aria-invalid','true'); ok = false;
      } else { nameErr.classList.add('hidden'); name.removeAttribute('aria-invalid'); }

      if (!email.value || !/^[^@]+@[^@]+\.[^@]+$/.test(email.value)) {
        emailErr.classList.remove('hidden'); email.setAttribute('aria-invalid','true'); ok = false;
      } else { emailErr.classList.add('hidden'); email.removeAttribute('aria-invalid'); }

      if (ok) updateLive('✅ บันทึกสำเร็จแล้ว');
    }

    function updateCharCount(el) {
      document.getElementById('a11y-bio-count').textContent = `${el.value.length}/200 ตัวอักษร`;
    }

    // Focus trap
    function openTrapModal() {
      document.getElementById('trapOverlay').classList.remove('hidden');
      document.getElementById('trapModal').querySelector('input').focus();
      document.addEventListener('keydown', trapKeydown);
    }

    function closeTrapModal() {
      document.getElementById('trapOverlay').classList.add('hidden');
      document.removeEventListener('keydown', trapKeydown);
    }

    function trapKeydown(e) {
      if (e.key === 'Escape') { closeTrapModal(); return; }
      if (e.key !== 'Tab') return;
      const modal = document.getElementById('trapModal');
      const focusable = modal.querySelectorAll('input, button');
      const first = focusable[0], last = focusable[focusable.length - 1];
      if (e.shiftKey) { if (document.activeElement === first) { e.preventDefault(); last.focus(); } }
      else { if (document.activeElement === last) { e.preventDefault(); first.focus(); } }
    }

    function toggleAccordion(btn) {
      const expanded = btn.getAttribute('aria-expanded') === 'true';
      btn.setAttribute('aria-expanded', String(!expanded));
      const body = document.getElementById('acc-body');
      body.hidden = expanded;
      document.getElementById('acc-icon').style.transform = expanded ? 'rotate(0)' : 'rotate(180deg)';
    }

    function simulateLoad(btn) {
      const text = document.getElementById('loadText');
      text.textContent = 'กำลังบันทึก...';
      btn.setAttribute('aria-busy', 'true');
      btn.disabled = true;
      setTimeout(() => {
        text.textContent = '✅ บันทึกสำเร็จ';
        btn.setAttribute('aria-busy', 'false');
        btn.disabled = false;
        setTimeout(() => { text.textContent = 'บันทึก'; }, 2000);
      }, 1500);
    }
  </script>
</body>
</html>
```

---

## สรุป Part 77

| Step | เนื้อหา |
|------|---------|
| 761 | focus-visible — outline ชัดสำหรับ keyboard, ซ่อนสำหรับ mouse |
| 762 | Skip navigation link |
| 763 | ARIA live region — polite/assertive, role=alert, role=status |
| 764 | Accessible form — label, required, aria-describedby, aria-invalid |
| 765 | Focus trap modal — Tab วนภายใน, Esc ปิด |
| 766 | Color contrast — WCAG AA/AAA |
| 767 | sr-only labels สำหรับ icon buttons |
| 768 | Image alt text patterns |
| 769 | aria-expanded accordion, aria-busy loading |
| 770 | Workshop: Accessibility checklist — 14 points ครบถ้วน |

**Part ถัดไป:** Part 78 — Performance Optimization (Steps 771–780)
