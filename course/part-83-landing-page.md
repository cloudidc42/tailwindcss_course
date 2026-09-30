# Part 83: Real-World Landing Page

## เป้าหมาย
- SaaS landing page ครบสมบูรณ์
- Hero, Features, Pricing, Testimonials, CTA, Footer
- Scroll animations, responsive, dark mode ready
- Steps 821–830

---

## Step 821: Landing Page Structure

```
Layout:
  <nav>           — Sticky navigation
  <hero>          — Hero section + CTA
  <logos>         — Trust / social proof logos
  <features>      — 3-column feature grid
  <how-it-works>  — Step-by-step flow
  <pricing>       — 3-tier pricing table
  <testimonials>  — Testimonial cards
  <faq>           — Accordion FAQ
  <cta>           — Bottom CTA banner
  <footer>        — Links + copyright
```

---

## Step 822: Hero Section

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Hero</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    @keyframes float { 0%,100%{transform:translateY(0)} 50%{transform:translateY(-12px)} }
    .float { animation: float 4s ease-in-out infinite; }
  </style>
</head>
<body class="bg-white">
  <!-- Nav -->
  <nav class="sticky top-0 z-40 bg-white/90 backdrop-blur border-b">
    <div class="max-w-6xl mx-auto px-6 h-16 flex items-center justify-between">
      <div class="font-extrabold text-xl text-indigo-600">Streamly</div>
      <div class="hidden md:flex items-center gap-6 text-sm font-medium text-gray-600">
        <a href="#features" class="hover:text-gray-900">Features</a>
        <a href="#pricing"  class="hover:text-gray-900">Pricing</a>
        <a href="#faq"      class="hover:text-gray-900">FAQ</a>
      </div>
      <div class="flex items-center gap-3">
        <button class="hidden md:block text-sm font-medium text-gray-600 hover:text-gray-900">Log in</button>
        <button class="px-4 py-2 bg-indigo-600 text-white text-sm font-semibold rounded-xl hover:bg-indigo-700 transition-colors">Start free</button>
      </div>
    </div>
  </nav>

  <!-- Hero -->
  <section class="max-w-6xl mx-auto px-6 py-20 md:py-32">
    <div class="max-w-3xl mx-auto text-center">
      <!-- Badge -->
      <div class="inline-flex items-center gap-2 px-3 py-1 bg-indigo-50 border border-indigo-200 rounded-full text-sm text-indigo-700 font-medium mb-6">
        <span class="w-1.5 h-1.5 rounded-full bg-indigo-500 animate-pulse"></span>
        New: AI-powered analytics →
      </div>
      <!-- Heading -->
      <h1 class="text-4xl md:text-5xl lg:text-6xl font-extrabold text-gray-900 leading-[1.1] mb-6">
        Ship faster with<br>
        <span class="bg-gradient-to-r from-indigo-600 to-purple-600 bg-clip-text text-transparent">
          beautiful workflows
        </span>
      </h1>
      <p class="text-lg md:text-xl text-gray-500 mb-8 max-w-xl mx-auto">
        Streamly ช่วยทีมของคุณ collaborate, track และ ship projects ด้วยความเร็วสูงสุด ลดเวลา setup ลง 80%
      </p>
      <!-- CTA buttons -->
      <div class="flex flex-col sm:flex-row gap-3 justify-center mb-8">
        <button class="px-7 py-3.5 bg-indigo-600 text-white font-semibold rounded-2xl hover:bg-indigo-700 transition-all shadow-lg shadow-indigo-200 hover:shadow-indigo-300 hover:-translate-y-0.5">
          Get started free →
        </button>
        <button class="px-7 py-3.5 bg-gray-50 border text-gray-700 font-semibold rounded-2xl hover:bg-gray-100 transition-colors flex items-center gap-2 justify-center">
          ▶ Watch demo <span class="text-sm text-gray-400">3 min</span>
        </button>
      </div>
      <!-- Social proof -->
      <p class="text-sm text-gray-400">ไม่ต้องใส่บัตรเครดิต · ฟรี 14 วัน · ยกเลิกได้ทุกเมื่อ</p>
    </div>
    <!-- Hero visual -->
    <div class="mt-14 relative float">
      <div class="bg-gradient-to-br from-indigo-600 to-purple-700 rounded-3xl p-1 shadow-2xl shadow-indigo-200 max-w-4xl mx-auto">
        <div class="bg-gray-900 rounded-[calc(1.5rem-4px)] h-64 md:h-96 flex items-center justify-center">
          <p class="text-gray-500 text-lg">App screenshot</p>
        </div>
      </div>
    </div>
  </section>
</body>
</html>
```

---

## Step 823: Social Proof + Feature Grid

```html
<!-- Trust logos -->
<section class="border-y bg-gray-50 py-8">
  <div class="max-w-5xl mx-auto px-6">
    <p class="text-center text-sm text-gray-400 mb-6">ใช้โดยทีมชั้นนำทั่วโลก</p>
    <div class="flex flex-wrap items-center justify-center gap-8 opacity-40">
      <span class="font-extrabold text-xl text-gray-700">Acme</span>
      <span class="font-extrabold text-xl text-gray-700">TechCo</span>
      <span class="font-extrabold text-xl text-gray-700">Globex</span>
      <span class="font-extrabold text-xl text-gray-700">Initech</span>
      <span class="font-extrabold text-xl text-gray-700">Hooli</span>
    </div>
  </div>
</section>

<!-- Features -->
<section id="features" class="max-w-6xl mx-auto px-6 py-20">
  <div class="text-center mb-12">
    <p class="text-sm font-semibold text-indigo-600 uppercase tracking-wide mb-2">Features</p>
    <h2 class="text-3xl md:text-4xl font-extrabold text-gray-900 mb-4">ทุกอย่างที่ทีมต้องการ</h2>
    <p class="text-gray-500 max-w-xl mx-auto">ครบครันในที่เดียว ไม่ต้องใช้หลาย tools</p>
  </div>
  <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
    <div class="bg-white rounded-2xl border p-6 hover:shadow-lg transition-shadow">
      <div class="w-12 h-12 bg-indigo-100 rounded-2xl flex items-center justify-center text-2xl mb-4">⚡</div>
      <h3 class="font-bold text-gray-900 mb-2">Instant Collaboration</h3>
      <p class="text-gray-500 text-sm">Real-time updates, comments, และ @mentions ทำให้ทีมทำงานร่วมกันได้เลย</p>
    </div>
    <div class="bg-white rounded-2xl border p-6 hover:shadow-lg transition-shadow">
      <div class="w-12 h-12 bg-purple-100 rounded-2xl flex items-center justify-center text-2xl mb-4">📊</div>
      <h3 class="font-bold text-gray-900 mb-2">Smart Analytics</h3>
      <p class="text-gray-500 text-sm">Dashboard แบบ real-time บอกว่าทีมทำงานได้ดีแค่ไหน และจะ bottleneck ที่ไหน</p>
    </div>
    <div class="bg-white rounded-2xl border p-6 hover:shadow-lg transition-shadow">
      <div class="w-12 h-12 bg-emerald-100 rounded-2xl flex items-center justify-center text-2xl mb-4">🔒</div>
      <h3 class="font-bold text-gray-900 mb-2">Enterprise Security</h3>
      <p class="text-gray-500 text-sm">SOC2 Type II, SSO, role-based access และ audit log ครบทุก compliance</p>
    </div>
  </div>
</section>
```

---

## Step 824: Pricing Table

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Pricing</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 min-h-screen p-8">
<section id="pricing" class="max-w-5xl mx-auto">
  <div class="text-center mb-10">
    <h2 class="text-3xl font-extrabold text-gray-900 mb-3">Pricing Plans</h2>
    <div class="inline-flex bg-gray-200 rounded-full p-1 text-sm font-medium gap-1">
      <button id="btn-monthly" onclick="toggle('monthly')" class="px-5 py-1.5 rounded-full bg-white shadow-sm transition-all">Monthly</button>
      <button id="btn-annual"  onclick="toggle('annual')"  class="px-5 py-1.5 rounded-full transition-all text-gray-500">Annual <span class="text-emerald-600 font-semibold">-20%</span></button>
    </div>
  </div>
  <div class="grid grid-cols-1 md:grid-cols-3 gap-5">
    <!-- Starter -->
    <div class="bg-white rounded-2xl border p-6">
      <p class="text-sm font-semibold text-gray-500 mb-1">Starter</p>
      <div class="flex items-end gap-1 mb-4">
        <span class="text-4xl font-extrabold text-gray-900" id="price-starter">$9</span>
        <span class="text-gray-400 mb-1">/mo</span>
      </div>
      <ul class="space-y-2 mb-6 text-sm text-gray-600">
        <li class="flex items-center gap-2">✅ 5 projects</li>
        <li class="flex items-center gap-2">✅ 3 team members</li>
        <li class="flex items-center gap-2">✅ 5GB storage</li>
        <li class="flex items-center gap-2 text-gray-300">❌ Analytics</li>
        <li class="flex items-center gap-2 text-gray-300">❌ Priority support</li>
      </ul>
      <button class="w-full py-2.5 border text-gray-700 text-sm font-semibold rounded-xl hover:bg-gray-50 transition-colors">Get started</button>
    </div>
    <!-- Pro (featured) -->
    <div class="bg-indigo-600 rounded-2xl p-6 relative shadow-xl shadow-indigo-200">
      <div class="absolute -top-3 left-1/2 -translate-x-1/2">
        <span class="px-3 py-1 bg-amber-400 text-amber-900 text-xs font-bold rounded-full">Most Popular</span>
      </div>
      <p class="text-sm font-semibold text-indigo-200 mb-1">Pro</p>
      <div class="flex items-end gap-1 mb-4">
        <span class="text-4xl font-extrabold text-white" id="price-pro">$29</span>
        <span class="text-indigo-300 mb-1">/mo</span>
      </div>
      <ul class="space-y-2 mb-6 text-sm text-indigo-100">
        <li class="flex items-center gap-2">✅ Unlimited projects</li>
        <li class="flex items-center gap-2">✅ 25 team members</li>
        <li class="flex items-center gap-2">✅ 50GB storage</li>
        <li class="flex items-center gap-2">✅ Analytics dashboard</li>
        <li class="flex items-center gap-2">✅ Priority support</li>
      </ul>
      <button class="w-full py-2.5 bg-white text-indigo-600 text-sm font-semibold rounded-xl hover:bg-indigo-50 transition-colors">Get started</button>
    </div>
    <!-- Enterprise -->
    <div class="bg-white rounded-2xl border p-6">
      <p class="text-sm font-semibold text-gray-500 mb-1">Enterprise</p>
      <div class="flex items-end gap-1 mb-4">
        <span class="text-4xl font-extrabold text-gray-900" id="price-ent">$99</span>
        <span class="text-gray-400 mb-1">/mo</span>
      </div>
      <ul class="space-y-2 mb-6 text-sm text-gray-600">
        <li class="flex items-center gap-2">✅ Unlimited everything</li>
        <li class="flex items-center gap-2">✅ Unlimited members</li>
        <li class="flex items-center gap-2">✅ Unlimited storage</li>
        <li class="flex items-center gap-2">✅ SSO + SAML</li>
        <li class="flex items-center gap-2">✅ Dedicated support</li>
      </ul>
      <button class="w-full py-2.5 border text-gray-700 text-sm font-semibold rounded-xl hover:bg-gray-50 transition-colors">Contact sales</button>
    </div>
  </div>
</section>
<script>
  const prices = { monthly: [9,29,99], annual: [7,23,79] };
  function toggle(mode) {
    ['monthly','annual'].forEach(m => {
      document.getElementById('btn-'+m).classList.toggle('bg-white', m === mode);
      document.getElementById('btn-'+m).classList.toggle('shadow-sm', m === mode);
      document.getElementById('btn-'+m).classList.toggle('text-gray-500', m !== mode);
    });
    const [s,p,e] = prices[mode];
    document.getElementById('price-starter').textContent = '$'+s;
    document.getElementById('price-pro').textContent = '$'+p;
    document.getElementById('price-ent').textContent = '$'+e;
  }
</script>
</body>
</html>
```

---

## Step 825: Testimonials

```html
<section class="bg-gray-50 py-20">
  <div class="max-w-6xl mx-auto px-6">
    <div class="text-center mb-12">
      <h2 class="text-3xl font-extrabold text-gray-900 mb-3">ลูกค้าพูดถึงเรา</h2>
      <div class="flex justify-center gap-1 text-amber-400">★★★★★</div>
      <p class="text-gray-500 text-sm mt-2">4.9/5 จาก 2,400+ รีวิว</p>
    </div>
    <div class="grid grid-cols-1 md:grid-cols-3 gap-5">
      <div class="bg-white rounded-2xl border p-6 shadow-sm">
        <div class="flex gap-1 text-amber-400 mb-3 text-sm">★★★★★</div>
        <p class="text-gray-700 text-sm mb-4">"Streamly เปลี่ยนวิธีทำงานของทีมเราไปโดยสิ้นเชิง ประหยัดเวลา meeting ไปได้ 60% และ delivery เร็วขึ้นมาก"</p>
        <div class="flex items-center gap-3">
          <div class="w-9 h-9 rounded-full bg-indigo-600 flex items-center justify-center text-white text-xs font-bold">JD</div>
          <div>
            <p class="text-sm font-semibold text-gray-900">John Doe</p>
            <p class="text-xs text-gray-400">CTO, TechCo</p>
          </div>
        </div>
      </div>
      <div class="bg-white rounded-2xl border p-6 shadow-sm">
        <div class="flex gap-1 text-amber-400 mb-3 text-sm">★★★★★</div>
        <p class="text-gray-700 text-sm mb-4">"UI สวยมาก ใช้งานง่าย onboard ทีมใหม่ได้ใน 1 วัน analytics ละเอียดมากช่วย plan sprint ได้แม่นกว่าเดิม"</p>
        <div class="flex items-center gap-3">
          <div class="w-9 h-9 rounded-full bg-rose-500 flex items-center justify-center text-white text-xs font-bold">SA</div>
          <div>
            <p class="text-sm font-semibold text-gray-900">Sarah Ann</p>
            <p class="text-xs text-gray-400">Product Lead, Globex</p>
          </div>
        </div>
      </div>
      <div class="bg-white rounded-2xl border p-6 shadow-sm">
        <div class="flex gap-1 text-amber-400 mb-3 text-sm">★★★★★</div>
        <p class="text-gray-700 text-sm mb-4">"Enterprise plan คุ้มค่ามาก SSO + audit log ตรงตาม compliance ที่ต้องการ support ตอบไวมาก"</p>
        <div class="flex items-center gap-3">
          <div class="w-9 h-9 rounded-full bg-emerald-600 flex items-center justify-center text-white text-xs font-bold">MK</div>
          <div>
            <p class="text-sm font-semibold text-gray-900">Mike K.</p>
            <p class="text-xs text-gray-400">VP Engineering, Hooli</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>
```

---

## Step 826: FAQ Accordion

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>FAQ</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    .faq-answer { max-height: 0; overflow: hidden; transition: max-height 0.3s ease; }
    .faq-item.open .faq-answer { max-height: 200px; }
    .faq-item.open .faq-icon { transform: rotate(45deg); }
    .faq-icon { transition: transform 0.2s ease; }
  </style>
</head>
<body class="bg-white min-h-screen">
<section id="faq" class="max-w-2xl mx-auto px-6 py-20">
  <div class="text-center mb-10">
    <h2 class="text-3xl font-extrabold text-gray-900">คำถามที่พบบ่อย</h2>
  </div>
  <div class="space-y-3" id="faq-list"></div>
</section>
<script>
  const faqs = [
    { q: 'ทดลองใช้ฟรีได้นานแค่ไหน?',     a: 'ทดลองใช้ฟรี 14 วัน ไม่ต้องใส่บัตรเครดิต ใช้ฟีเจอร์ได้ทั้งหมดของ Pro plan' },
    { q: 'ยกเลิก subscription ได้ไหม?',   a: 'ยกเลิกได้ทุกเมื่อ ไม่มีค่าปรับ เมื่อยกเลิก account ยังใช้ได้ถึงวันสิ้นรอบบิล' },
    { q: 'รองรับกี่ภาษา?',                a: 'ปัจจุบันรองรับ 12 ภาษา รวมถึงภาษาไทย อังกฤษ ญี่ปุ่น จีน เกาหลี และยุโรป' },
    { q: 'ข้อมูลเก็บอยู่ที่ไหน?',          a: 'ข้อมูลเก็บบน AWS ใน region ที่เลือกได้ มี encryption at rest + in transit ทุก plan' },
    { q: 'มี self-hosted option ไหม?',     a: 'มีสำหรับ Enterprise plan ติดต่อทีม sales เพื่อขอ pricing และ setup guide' },
  ];
  document.getElementById('faq-list').innerHTML = faqs.map((f,i) => `
    <div class="faq-item border rounded-2xl overflow-hidden" id="faq-${i}">
      <button onclick="toggleFAQ(${i})" class="w-full flex items-center justify-between px-5 py-4 text-left hover:bg-gray-50 transition-colors" aria-expanded="false">
        <span class="font-semibold text-gray-900 text-sm">${f.q}</span>
        <span class="faq-icon text-gray-400 flex-shrink-0 ml-3 text-xl leading-none">+</span>
      </button>
      <div class="faq-answer">
        <p class="px-5 pb-4 text-gray-500 text-sm">${f.a}</p>
      </div>
    </div>`).join('');

  function toggleFAQ(i) {
    const item = document.getElementById('faq-'+i);
    const btn  = item.querySelector('button');
    const open = item.classList.contains('open');
    document.querySelectorAll('.faq-item').forEach(el => {
      el.classList.remove('open');
      el.querySelector('button').setAttribute('aria-expanded','false');
    });
    if (!open) {
      item.classList.add('open');
      btn.setAttribute('aria-expanded','true');
    }
  }
</script>
</body>
</html>
```

---

## Step 827: CTA Banner

```html
<!-- Bottom CTA -->
<section class="bg-indigo-600 py-16 md:py-20">
  <div class="max-w-4xl mx-auto px-6 text-center">
    <h2 class="text-3xl md:text-4xl font-extrabold text-white mb-4">
      พร้อมเริ่มต้นแล้วหรือยัง?
    </h2>
    <p class="text-indigo-200 text-lg mb-8">
      เข้าร่วมกับทีมกว่า 50,000 ทีมที่ใช้ Streamly ในการทำงาน
    </p>
    <div class="flex flex-col sm:flex-row gap-3 justify-center">
      <button class="px-8 py-4 bg-white text-indigo-600 font-bold rounded-2xl hover:bg-indigo-50 transition-colors shadow-lg text-sm">
        เริ่มใช้งานฟรี →
      </button>
      <button class="px-8 py-4 bg-indigo-500 text-white font-bold rounded-2xl hover:bg-indigo-400 transition-colors text-sm">
        ติดต่อ Sales
      </button>
    </div>
    <p class="text-indigo-300 text-sm mt-5">ฟรี 14 วัน · ไม่ต้องบัตรเครดิต · ยกเลิกได้ทุกเมื่อ</p>
  </div>
</section>
```

---

## Step 828: Footer

```html
<footer class="bg-gray-900 text-gray-400 py-12 md:py-16">
  <div class="max-w-6xl mx-auto px-6">
    <div class="grid grid-cols-2 md:grid-cols-5 gap-8 mb-10">
      <!-- Brand -->
      <div class="col-span-2 md:col-span-1">
        <div class="font-extrabold text-xl text-white mb-3">Streamly</div>
        <p class="text-sm leading-relaxed">Build faster, ship better, collaborate smarter.</p>
      </div>
      <!-- Links -->
      <div>
        <p class="text-white text-sm font-semibold mb-3">Product</p>
        <ul class="space-y-2 text-sm">
          <li><a href="#" class="hover:text-white transition-colors">Features</a></li>
          <li><a href="#" class="hover:text-white transition-colors">Pricing</a></li>
          <li><a href="#" class="hover:text-white transition-colors">Changelog</a></li>
          <li><a href="#" class="hover:text-white transition-colors">Roadmap</a></li>
        </ul>
      </div>
      <div>
        <p class="text-white text-sm font-semibold mb-3">Company</p>
        <ul class="space-y-2 text-sm">
          <li><a href="#" class="hover:text-white transition-colors">About</a></li>
          <li><a href="#" class="hover:text-white transition-colors">Blog</a></li>
          <li><a href="#" class="hover:text-white transition-colors">Careers</a></li>
          <li><a href="#" class="hover:text-white transition-colors">Press</a></li>
        </ul>
      </div>
      <div>
        <p class="text-white text-sm font-semibold mb-3">Resources</p>
        <ul class="space-y-2 text-sm">
          <li><a href="#" class="hover:text-white transition-colors">Docs</a></li>
          <li><a href="#" class="hover:text-white transition-colors">API</a></li>
          <li><a href="#" class="hover:text-white transition-colors">Community</a></li>
          <li><a href="#" class="hover:text-white transition-colors">Status</a></li>
        </ul>
      </div>
      <div>
        <p class="text-white text-sm font-semibold mb-3">Legal</p>
        <ul class="space-y-2 text-sm">
          <li><a href="#" class="hover:text-white transition-colors">Privacy</a></li>
          <li><a href="#" class="hover:text-white transition-colors">Terms</a></li>
          <li><a href="#" class="hover:text-white transition-colors">Cookies</a></li>
          <li><a href="#" class="hover:text-white transition-colors">Security</a></li>
        </ul>
      </div>
    </div>
    <div class="border-t border-gray-800 pt-6 flex flex-col sm:flex-row items-center justify-between gap-4">
      <p class="text-sm">© 2025 Streamly, Inc. All rights reserved.</p>
      <div class="flex gap-4">
        <a href="#" class="text-sm hover:text-white transition-colors">Twitter</a>
        <a href="#" class="text-sm hover:text-white transition-colors">GitHub</a>
        <a href="#" class="text-sm hover:text-white transition-colors">LinkedIn</a>
      </div>
    </div>
  </div>
</footer>
```

---

## Step 829: Scroll Animations

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Scroll Animations</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    @keyframes fade-up { from{opacity:0;transform:translateY(24px)} to{opacity:1;transform:translateY(0)} }
    .reveal { opacity:0; }
    .reveal.visible { animation: fade-up 0.6s ease both; }
    .reveal[data-delay="1"] { animation-delay:0.1s; }
    .reveal[data-delay="2"] { animation-delay:0.2s; }
    .reveal[data-delay="3"] { animation-delay:0.3s; }
  </style>
</head>
<body class="bg-white min-h-screen">
  <div class="max-w-4xl mx-auto px-6 py-24 space-y-12">
    <div class="reveal">
      <h2 class="text-3xl font-extrabold text-gray-900">Section 1</h2>
      <p class="text-gray-500 mt-2">Fades in as you scroll down.</p>
    </div>
    <div class="grid grid-cols-3 gap-4">
      <div class="reveal" data-delay="1"><div class="bg-indigo-100 rounded-2xl p-5 text-center text-indigo-700 font-bold">Card A</div></div>
      <div class="reveal" data-delay="2"><div class="bg-purple-100 rounded-2xl p-5 text-center text-purple-700 font-bold">Card B</div></div>
      <div class="reveal" data-delay="3"><div class="bg-rose-100 rounded-2xl p-5 text-center text-rose-700 font-bold">Card C</div></div>
    </div>
    <div class="reveal">
      <div class="bg-gray-900 rounded-2xl p-8 text-white text-center">
        <h3 class="text-2xl font-extrabold mb-2">Call to Action</h3>
        <p class="text-gray-400 mb-4">Revealed on scroll</p>
        <button class="px-6 py-2.5 bg-indigo-500 text-white rounded-xl font-semibold">Start →</button>
      </div>
    </div>
  </div>
  <script>
    const observer = new IntersectionObserver(entries => {
      entries.forEach(e => { if (e.isIntersecting) { e.target.classList.add('visible'); observer.unobserve(e.target); } });
    }, { threshold: 0.15 });
    document.querySelectorAll('.reveal').forEach(el => observer.observe(el));
  </script>
</body>
</html>
```

---

## Step 830: Workshop — Complete Landing Page

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Streamly — Ship Faster</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    @keyframes fade-up { from{opacity:0;transform:translateY(16px)} to{opacity:1;transform:translateY(0)} }
    @keyframes float  { 0%,100%{transform:translateY(0)} 50%{transform:translateY(-8px)} }
    .fade-up { animation: fade-up 0.5s ease both; }
    .delay-1 { animation-delay:.1s; } .delay-2 { animation-delay:.2s; } .delay-3 { animation-delay:.3s; }
    .float   { animation: float 4s ease-in-out infinite; }
    .reveal  { opacity:0; transition:opacity .6s,transform .6s; transform:translateY(16px); }
    .reveal.in { opacity:1; transform:translateY(0); }
  </style>
</head>
<body class="bg-white">
  <nav class="sticky top-0 z-40 bg-white/90 backdrop-blur border-b">
    <div class="max-w-5xl mx-auto px-5 h-14 flex items-center justify-between">
      <span class="font-extrabold text-lg text-indigo-600">Streamly</span>
      <div class="hidden sm:flex gap-4 text-sm font-medium text-gray-600">
        <a href="#feat" class="hover:text-gray-900">Features</a>
        <a href="#price" class="hover:text-gray-900">Pricing</a>
      </div>
      <button class="px-3 py-1.5 bg-indigo-600 text-white text-sm font-semibold rounded-xl hover:bg-indigo-700 transition-colors">Start free</button>
    </div>
  </nav>

  <!-- Hero -->
  <section class="max-w-5xl mx-auto px-5 py-20 text-center">
    <div class="fade-up inline-flex items-center gap-2 px-3 py-1 bg-indigo-50 border border-indigo-200 rounded-full text-xs text-indigo-700 font-medium mb-5">
      <span class="w-1.5 h-1.5 rounded-full bg-indigo-500 animate-pulse"></span> New: AI analytics →
    </div>
    <h1 class="fade-up delay-1 text-4xl md:text-5xl font-extrabold text-gray-900 leading-tight mb-4">
      Ship faster with<br><span class="text-indigo-600">beautiful workflows</span>
    </h1>
    <p class="fade-up delay-2 text-gray-500 text-lg mb-7 max-w-md mx-auto">ลดเวลา setup ลง 80% และ deliver ได้เร็วขึ้น 3x</p>
    <div class="fade-up delay-3 flex flex-col sm:flex-row gap-3 justify-center">
      <button class="px-7 py-3 bg-indigo-600 text-white font-semibold rounded-2xl hover:bg-indigo-700 transition-colors shadow-lg shadow-indigo-200">Get started free →</button>
      <button class="px-7 py-3 border text-gray-700 font-semibold rounded-2xl hover:bg-gray-50">▶ Demo</button>
    </div>
    <div class="mt-12 float">
      <div class="bg-gradient-to-br from-indigo-600 to-purple-700 rounded-3xl p-1 shadow-2xl shadow-indigo-200 max-w-2xl mx-auto">
        <div class="bg-gray-900 rounded-[calc(1.5rem-4px)] h-48 flex items-center justify-center text-gray-600">App Preview</div>
      </div>
    </div>
  </section>

  <!-- Features -->
  <section id="feat" class="bg-gray-50 py-16">
    <div class="max-w-5xl mx-auto px-5">
      <h2 class="reveal text-2xl font-extrabold text-gray-900 text-center mb-8">ทุกอย่างที่ทีมต้องการ</h2>
      <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
        <div class="reveal bg-white rounded-2xl border p-5 hover:shadow-md transition-shadow"><div class="text-2xl mb-2">⚡</div><h3 class="font-bold text-gray-900 text-sm mb-1">Realtime Collaboration</h3><p class="text-gray-500 text-xs">ทำงานร่วมกันได้ทันที ไม่มี delay</p></div>
        <div class="reveal bg-white rounded-2xl border p-5 hover:shadow-md transition-shadow"><div class="text-2xl mb-2">📊</div><h3 class="font-bold text-gray-900 text-sm mb-1">Smart Analytics</h3><p class="text-gray-500 text-xs">Dashboard real-time ครบทุก metric</p></div>
        <div class="reveal bg-white rounded-2xl border p-5 hover:shadow-md transition-shadow"><div class="text-2xl mb-2">🔒</div><h3 class="font-bold text-gray-900 text-sm mb-1">Enterprise Security</h3><p class="text-gray-500 text-xs">SOC2, SSO, audit log ครบ</p></div>
      </div>
    </div>
  </section>

  <!-- CTA -->
  <section class="bg-indigo-600 py-14 text-center reveal">
    <h2 class="text-2xl font-extrabold text-white mb-3">พร้อมเริ่มแล้วหรือยัง?</h2>
    <p class="text-indigo-200 mb-6">เข้าร่วมกับ 50,000+ ทีมที่ใช้ Streamly</p>
    <button class="px-7 py-3 bg-white text-indigo-600 font-bold rounded-2xl hover:bg-indigo-50 transition-colors">เริ่มใช้ฟรี →</button>
  </section>

  <script>
    const obs = new IntersectionObserver(es => es.forEach(e => { if(e.isIntersecting){e.target.classList.add('in');obs.unobserve(e.target);} }), {threshold:.1});
    document.querySelectorAll('.reveal').forEach(el => obs.observe(el));
  </script>
</body>
</html>
```

---

## สรุป Part 83

| Step | เนื้อหา |
|------|---------|
| 821 | Landing page structure planning |
| 822 | Hero section — badge, heading gradient, CTA, floating visual |
| 823 | Social proof logos + feature grid |
| 824 | Pricing table — monthly/annual toggle |
| 825 | Testimonials — star rating, cards |
| 826 | FAQ accordion — smooth height transition |
| 827 | CTA banner section |
| 828 | Footer — multi-column links, copyright |
| 829 | Scroll reveal animations (IntersectionObserver) |
| 830 | Workshop: Complete SaaS landing page |

**Part ถัดไป:** Part 84 — Admin Dashboard (Steps 831–840)
