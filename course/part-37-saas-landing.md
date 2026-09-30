# Part 37: SaaS Landing Page Components

## เป้าหมาย
- สร้าง Landing Page สำหรับ SaaS Product ระดับมืออาชีพ
- Hero Section, Feature Grid, Pricing Table, Testimonials
- FAQ Accordion, CTA Section, Footer
- Responsive และ Conversion-Optimized Design

---

## Step 361: Hero Section

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>SaaS Hero - Step 361</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-white">

  <!-- Announcement Banner -->
  <div class="bg-indigo-600 text-white text-center text-sm py-2 px-4">
    🎉 ยินดีต้อนรับ v2.0! ฟีเจอร์ใหม่มากมาย —
    <a href="#" class="underline font-semibold hover:text-indigo-200">อ่านเพิ่มเติม →</a>
  </div>

  <!-- Nav -->
  <nav class="max-w-6xl mx-auto px-4 py-4 flex items-center justify-between">
    <div class="flex items-center gap-2">
      <div class="w-8 h-8 bg-indigo-600 rounded-lg flex items-center justify-center">
        <span class="text-white font-bold text-sm">S</span>
      </div>
      <span class="font-bold text-lg">SaasKit</span>
    </div>
    <div class="hidden md:flex items-center gap-6 text-sm text-gray-600 font-medium">
      <a href="#" class="hover:text-gray-900 transition-colors">ฟีเจอร์</a>
      <a href="#" class="hover:text-gray-900 transition-colors">ราคา</a>
      <a href="#" class="hover:text-gray-900 transition-colors">Docs</a>
      <a href="#" class="hover:text-gray-900 transition-colors">Blog</a>
    </div>
    <div class="flex items-center gap-3">
      <a href="#" class="text-sm text-gray-600 hover:text-gray-900 font-medium">เข้าสู่ระบบ</a>
      <a href="#" class="bg-indigo-600 text-white text-sm px-4 py-2 rounded-lg hover:bg-indigo-700 transition-colors font-medium">
        ทดลองฟรี
      </a>
    </div>
  </nav>

  <!-- Hero -->
  <section class="max-w-6xl mx-auto px-4 py-16 md:py-24 text-center">
    <!-- Badge -->
    <div class="inline-flex items-center gap-2 bg-indigo-50 border border-indigo-100 text-indigo-700 text-sm px-4 py-1.5 rounded-full mb-6">
      <span class="w-2 h-2 bg-indigo-500 rounded-full animate-pulse"></span>
      ใหม่: AI-Powered Analytics Dashboard
    </div>

    <h1 class="text-5xl md:text-6xl font-extrabold text-gray-900 leading-tight mb-6">
      บริหารธุรกิจของคุณ<br>
      <span class="bg-gradient-to-r from-indigo-600 to-purple-600 bg-clip-text text-transparent">
        อย่างชาญฉลาด
      </span>
    </h1>
    <p class="text-xl text-gray-500 max-w-2xl mx-auto mb-10 leading-relaxed">
      Platform ที่รวม CRM, Analytics, และ Automation เข้าด้วยกัน ช่วยให้ทีมของคุณทำงานได้เร็วขึ้น 3 เท่า
    </p>

    <!-- CTA Buttons -->
    <div class="flex flex-col sm:flex-row gap-3 justify-center mb-10">
      <a href="#" class="bg-indigo-600 text-white px-8 py-3.5 rounded-xl font-semibold hover:bg-indigo-700 transition-colors shadow-lg shadow-indigo-200 text-sm">
        เริ่มทดลองฟรี 14 วัน
      </a>
      <a href="#" class="flex items-center justify-center gap-2 border border-gray-300 text-gray-700 px-6 py-3.5 rounded-xl font-semibold hover:bg-gray-50 transition-colors text-sm">
        <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20">
          <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zM9.555 7.168A1 1 0 008 8v4a1 1 0 001.555.832l3-2a1 1 0 000-1.664l-3-2z" clip-rule="evenodd"/>
        </svg>
        ดูวิดีโอ Demo
      </a>
    </div>

    <!-- Social Proof -->
    <div class="flex flex-col sm:flex-row items-center justify-center gap-6 text-sm text-gray-500">
      <div class="flex items-center gap-2">
        <div class="flex -space-x-2">
          <img src="https://i.pravatar.cc/28?img=1" class="w-7 h-7 rounded-full ring-2 ring-white" alt="">
          <img src="https://i.pravatar.cc/28?img=2" class="w-7 h-7 rounded-full ring-2 ring-white" alt="">
          <img src="https://i.pravatar.cc/28?img=3" class="w-7 h-7 rounded-full ring-2 ring-white" alt="">
          <img src="https://i.pravatar.cc/28?img=4" class="w-7 h-7 rounded-full ring-2 ring-white" alt="">
        </div>
        <span>+2,500 บริษัทที่ใช้งานอยู่</span>
      </div>
      <div class="flex items-center gap-1">
        <span class="text-yellow-400">★★★★★</span>
        <span>4.9/5 จาก 800+ reviews</span>
      </div>
      <div class="flex items-center gap-1">
        <svg class="w-4 h-4 text-green-500" fill="currentColor" viewBox="0 0 20 20">
          <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd"/>
        </svg>
        ไม่ต้องใส่บัตรเครดิต
      </div>
    </div>

    <!-- Product Screenshot -->
    <div class="mt-14 relative">
      <div class="absolute inset-x-0 bottom-0 h-32 bg-gradient-to-t from-white to-transparent z-10"></div>
      <div class="bg-gray-900 rounded-2xl shadow-2xl overflow-hidden border border-gray-700 max-w-4xl mx-auto">
        <!-- Fake browser bar -->
        <div class="bg-gray-800 px-4 py-3 flex items-center gap-2">
          <div class="flex gap-1.5">
            <div class="w-3 h-3 rounded-full bg-red-500"></div>
            <div class="w-3 h-3 rounded-full bg-yellow-500"></div>
            <div class="w-3 h-3 rounded-full bg-green-500"></div>
          </div>
          <div class="flex-1 mx-4 bg-gray-700 rounded-md h-6 flex items-center px-3">
            <span class="text-gray-400 text-xs">app.saaskit.com/dashboard</span>
          </div>
        </div>
        <!-- Dashboard preview -->
        <div class="bg-gray-50 p-4 grid grid-cols-4 gap-3 text-xs">
          <div class="col-span-1 bg-gray-900 rounded-lg p-3 text-gray-400 space-y-3">
            <div class="flex items-center gap-2 text-white"><div class="w-4 h-4 bg-indigo-600 rounded"></div> Dashboard</div>
            <div class="flex items-center gap-2"><div class="w-4 h-4 bg-gray-700 rounded"></div> Analytics</div>
            <div class="flex items-center gap-2"><div class="w-4 h-4 bg-gray-700 rounded"></div> Customers</div>
            <div class="flex items-center gap-2"><div class="w-4 h-4 bg-gray-700 rounded"></div> Settings</div>
          </div>
          <div class="col-span-3 space-y-3">
            <div class="grid grid-cols-3 gap-2">
              <div class="bg-white rounded-lg p-3 border"><p class="text-gray-400 mb-1">Revenue</p><p class="font-bold text-gray-900 text-lg">฿2.4M</p><p class="text-green-500">↑ 12%</p></div>
              <div class="bg-white rounded-lg p-3 border"><p class="text-gray-400 mb-1">Users</p><p class="font-bold text-gray-900 text-lg">4,821</p><p class="text-green-500">↑ 8%</p></div>
              <div class="bg-white rounded-lg p-3 border"><p class="text-gray-400 mb-1">Churn</p><p class="font-bold text-gray-900 text-lg">1.2%</p><p class="text-red-400">↑ 0.1%</p></div>
            </div>
            <div class="bg-white rounded-lg p-3 border h-20 flex items-end gap-1">
              <div class="flex-1 bg-indigo-200 rounded-t" style="height:30%"></div>
              <div class="flex-1 bg-indigo-300 rounded-t" style="height:50%"></div>
              <div class="flex-1 bg-indigo-400 rounded-t" style="height:70%"></div>
              <div class="flex-1 bg-indigo-500 rounded-t" style="height:45%"></div>
              <div class="flex-1 bg-indigo-600 rounded-t" style="height:85%"></div>
              <div class="flex-1 bg-indigo-500 rounded-t" style="height:60%"></div>
              <div class="flex-1 bg-indigo-400 rounded-t" style="height:75%"></div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>

</body>
</html>
```

---

## Step 362: Feature Section

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Features - Step 362</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-white py-20 px-4">

  <!-- Logos Bar (Social Proof) -->
  <section class="max-w-5xl mx-auto mb-20 text-center">
    <p class="text-sm text-gray-400 mb-6 uppercase tracking-wide font-medium">บริษัทชั้นนำที่ไว้วางใจ SaasKit</p>
    <div class="flex flex-wrap justify-center items-center gap-8 opacity-60">
      <span class="text-2xl font-black text-gray-400">Acme</span>
      <span class="text-2xl font-black text-gray-400">TechCo</span>
      <span class="text-2xl font-black text-gray-400">Globex</span>
      <span class="text-2xl font-black text-gray-400">Initech</span>
      <span class="text-2xl font-black text-gray-400">Umbrella</span>
      <span class="text-2xl font-black text-gray-400">Oscorp</span>
    </div>
  </section>

  <!-- Feature Grid -->
  <section class="max-w-6xl mx-auto">
    <div class="text-center mb-12">
      <span class="bg-indigo-100 text-indigo-700 text-xs font-semibold px-3 py-1 rounded-full">ฟีเจอร์</span>
      <h2 class="text-3xl md:text-4xl font-extrabold mt-4 mb-4">ทุกอย่างที่คุณต้องการ</h2>
      <p class="text-gray-500 text-lg max-w-xl mx-auto">
        SaasKit รวมเครื่องมือสำคัญทั้งหมดไว้ในที่เดียว ไม่ต้องต่อหลายระบบ
      </p>
    </div>

    <!-- 3-col Features -->
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 mb-12">
      <div class="p-6 border border-gray-100 rounded-2xl hover:border-indigo-200 hover:shadow-md transition-all group">
        <div class="w-12 h-12 bg-indigo-100 rounded-xl flex items-center justify-center mb-4 group-hover:bg-indigo-600 transition-colors">
          <span class="text-2xl group-hover:grayscale transition-all">📊</span>
        </div>
        <h3 class="font-bold text-lg mb-2">Analytics Dashboard</h3>
        <p class="text-gray-500 text-sm leading-relaxed">วิเคราะห์ข้อมูล real-time ด้วย Chart อัจฉริยะ ดู trend และ insight ในภาพเดียว</p>
      </div>

      <div class="p-6 border border-gray-100 rounded-2xl hover:border-purple-200 hover:shadow-md transition-all group">
        <div class="w-12 h-12 bg-purple-100 rounded-xl flex items-center justify-center mb-4 group-hover:bg-purple-600 transition-colors">
          <span class="text-2xl">🤖</span>
        </div>
        <h3 class="font-bold text-lg mb-2">AI Automation</h3>
        <p class="text-gray-500 text-sm leading-relaxed">ระบบ Automation ที่ขับเคลื่อนด้วย AI ช่วยทำงานซ้ำๆ โดยอัตโนมัติ ลดภาระทีมได้ถึง 70%</p>
      </div>

      <div class="p-6 border border-gray-100 rounded-2xl hover:border-blue-200 hover:shadow-md transition-all group">
        <div class="w-12 h-12 bg-blue-100 rounded-xl flex items-center justify-center mb-4 group-hover:bg-blue-600 transition-colors">
          <span class="text-2xl">👥</span>
        </div>
        <h3 class="font-bold text-lg mb-2">CRM Integration</h3>
        <p class="text-gray-500 text-sm leading-relaxed">จัดการ Lead และ Customer ในที่เดียว เชื่อมต่อกับ Salesforce, HubSpot ได้ทันที</p>
      </div>

      <div class="p-6 border border-gray-100 rounded-2xl hover:border-green-200 hover:shadow-md transition-all group">
        <div class="w-12 h-12 bg-green-100 rounded-xl flex items-center justify-center mb-4 group-hover:bg-green-600 transition-colors">
          <span class="text-2xl">🔔</span>
        </div>
        <h3 class="font-bold text-lg mb-2">Smart Notifications</h3>
        <p class="text-gray-500 text-sm leading-relaxed">รับ alert และ notification อัจฉริยะผ่าน Email, Slack, หรือ Line เมื่อมีเหตุการณ์สำคัญ</p>
      </div>

      <div class="p-6 border border-gray-100 rounded-2xl hover:border-orange-200 hover:shadow-md transition-all group">
        <div class="w-12 h-12 bg-orange-100 rounded-xl flex items-center justify-center mb-4 group-hover:bg-orange-600 transition-colors">
          <span class="text-2xl">🔒</span>
        </div>
        <h3 class="font-bold text-lg mb-2">Enterprise Security</h3>
        <p class="text-gray-500 text-sm leading-relaxed">ความปลอดภัยระดับ Enterprise ด้วย SSO, 2FA, audit log และ data encryption</p>
      </div>

      <div class="p-6 border border-gray-100 rounded-2xl hover:border-pink-200 hover:shadow-md transition-all group">
        <div class="w-12 h-12 bg-pink-100 rounded-xl flex items-center justify-center mb-4 group-hover:bg-pink-600 transition-colors">
          <span class="text-2xl">🌍</span>
        </div>
        <h3 class="font-bold text-lg mb-2">Multi-region</h3>
        <p class="text-gray-500 text-sm leading-relaxed">Deploy ได้ทั่วโลก รองรับ GDPR, PDPA และข้อกำหนด compliance ของแต่ละภูมิภาค</p>
      </div>
    </div>

    <!-- Big Feature Highlight -->
    <div class="bg-gradient-to-r from-indigo-50 to-purple-50 rounded-3xl p-8 md:p-12 border border-indigo-100">
      <div class="grid grid-cols-1 md:grid-cols-2 gap-8 items-center">
        <div>
          <span class="bg-indigo-600 text-white text-xs font-semibold px-3 py-1 rounded-full">New Feature</span>
          <h3 class="text-2xl font-extrabold mt-4 mb-4">AI Report Generator</h3>
          <p class="text-gray-600 leading-relaxed mb-6">
            สร้าง Report อัตโนมัติด้วย AI เพียงแค่บอกว่าต้องการอะไร ระบบจะวิเคราะห์ข้อมูลและสร้าง Presentation-ready report ภายในไม่กี่วินาที
          </p>
          <ul class="space-y-2 text-sm text-gray-600">
            <li class="flex items-center gap-2"><span class="text-green-500">✓</span> รองรับข้อมูลจากทุก data source</li>
            <li class="flex items-center gap-2"><span class="text-green-500">✓</span> Export เป็น PDF, Excel, PowerPoint</li>
            <li class="flex items-center gap-2"><span class="text-green-500">✓</span> Schedule ส่งอัตโนมัติ</li>
          </ul>
          <a href="#" class="inline-block mt-6 bg-indigo-600 text-white px-5 py-2.5 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">
            ลองใช้เลย →
          </a>
        </div>
        <div class="bg-white rounded-2xl p-5 shadow-sm border">
          <div class="flex items-center gap-3 mb-4">
            <div class="w-8 h-8 bg-indigo-600 rounded-lg"></div>
            <div>
              <p class="font-semibold text-sm">Monthly Revenue Report</p>
              <p class="text-xs text-gray-400">Generated by AI · June 2024</p>
            </div>
          </div>
          <div class="space-y-2 text-xs text-gray-500">
            <div class="bg-gray-50 rounded p-2">📈 Revenue grew 23% vs last month</div>
            <div class="bg-gray-50 rounded p-2">🎯 Top segment: Enterprise (45%)</div>
            <div class="bg-gray-50 rounded p-2">⚠️ Churn rate slightly elevated in SMB</div>
            <div class="bg-green-50 rounded p-2 text-green-700">💡 Recommendation: Focus retention campaign</div>
          </div>
        </div>
      </div>
    </div>
  </section>

</body>
</html>
```

---

## Step 363: Pricing Table

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Pricing - Step 363</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 py-16 px-4">

  <div class="max-w-5xl mx-auto">
    <div class="text-center mb-10">
      <h2 class="text-3xl font-extrabold mb-3">เลือกแผนที่เหมาะกับคุณ</h2>
      <p class="text-gray-500 mb-6">ทดลองใช้ฟรี 14 วัน ไม่ต้องใส่บัตรเครดิต</p>

      <!-- Toggle Monthly/Yearly -->
      <div class="inline-flex items-center bg-white border rounded-full p-1 gap-1 shadow-sm">
        <button id="btn-monthly" onclick="setPricing('monthly')" class="px-4 py-1.5 rounded-full text-sm font-medium bg-indigo-600 text-white transition-all">รายเดือน</button>
        <button id="btn-yearly" onclick="setPricing('yearly')" class="px-4 py-1.5 rounded-full text-sm font-medium text-gray-600 hover:text-gray-900 transition-all">รายปี <span class="text-green-600 font-semibold">-20%</span></button>
      </div>
    </div>

    <div class="grid grid-cols-1 md:grid-cols-3 gap-5">

      <!-- Starter -->
      <div class="bg-white rounded-2xl p-6 border border-gray-200 hover:border-gray-300 transition-colors">
        <div class="mb-5">
          <p class="font-bold text-lg mb-1">Starter</p>
          <p class="text-gray-500 text-sm">สำหรับทีมขนาดเล็กและ Startup</p>
        </div>
        <div class="mb-5">
          <span class="text-4xl font-extrabold" id="starter-price">฿990</span>
          <span class="text-gray-500 text-sm">/เดือน</span>
        </div>
        <a href="#" class="block text-center border border-gray-300 text-gray-700 py-2.5 rounded-xl text-sm font-semibold hover:bg-gray-50 transition-colors mb-5">
          เริ่มทดลองฟรี
        </a>
        <ul class="space-y-3 text-sm text-gray-600">
          <li class="flex items-center gap-2"><span class="text-green-500 flex-shrink-0">✓</span> ผู้ใช้งาน 5 คน</li>
          <li class="flex items-center gap-2"><span class="text-green-500 flex-shrink-0">✓</span> Analytics Dashboard</li>
          <li class="flex items-center gap-2"><span class="text-green-500 flex-shrink-0">✓</span> 10 Automations</li>
          <li class="flex items-center gap-2"><span class="text-green-500 flex-shrink-0">✓</span> Email Support</li>
          <li class="flex items-center gap-2"><span class="text-gray-300 flex-shrink-0">✗</span><span class="text-gray-400"> AI Features</span></li>
          <li class="flex items-center gap-2"><span class="text-gray-300 flex-shrink-0">✗</span><span class="text-gray-400"> Custom Integrations</span></li>
        </ul>
      </div>

      <!-- Pro (Popular) -->
      <div class="bg-indigo-600 rounded-2xl p-6 text-white shadow-xl shadow-indigo-200 relative overflow-hidden">
        <div class="absolute top-4 right-4 bg-yellow-400 text-yellow-900 text-xs font-bold px-2.5 py-1 rounded-full">
          ยอดนิยม
        </div>
        <div class="mb-5">
          <p class="font-bold text-lg mb-1">Pro</p>
          <p class="text-indigo-300 text-sm">สำหรับทีม Growth-stage</p>
        </div>
        <div class="mb-5">
          <span class="text-4xl font-extrabold" id="pro-price">฿2,490</span>
          <span class="text-indigo-300 text-sm">/เดือน</span>
        </div>
        <a href="#" class="block text-center bg-white text-indigo-700 py-2.5 rounded-xl text-sm font-semibold hover:bg-indigo-50 transition-colors mb-5">
          เริ่มทดลองฟรี
        </a>
        <ul class="space-y-3 text-sm text-indigo-100">
          <li class="flex items-center gap-2"><span class="text-yellow-400 flex-shrink-0">✓</span> ผู้ใช้งาน 25 คน</li>
          <li class="flex items-center gap-2"><span class="text-yellow-400 flex-shrink-0">✓</span> Advanced Analytics</li>
          <li class="flex items-center gap-2"><span class="text-yellow-400 flex-shrink-0">✓</span> Unlimited Automations</li>
          <li class="flex items-center gap-2"><span class="text-yellow-400 flex-shrink-0">✓</span> Priority Support</li>
          <li class="flex items-center gap-2"><span class="text-yellow-400 flex-shrink-0">✓</span> AI Report Generator</li>
          <li class="flex items-center gap-2"><span class="text-yellow-400 flex-shrink-0">✓</span> 50 Integrations</li>
        </ul>
      </div>

      <!-- Enterprise -->
      <div class="bg-white rounded-2xl p-6 border border-gray-200 hover:border-gray-300 transition-colors">
        <div class="mb-5">
          <p class="font-bold text-lg mb-1">Enterprise</p>
          <p class="text-gray-500 text-sm">สำหรับองค์กรขนาดใหญ่</p>
        </div>
        <div class="mb-5">
          <span class="text-2xl font-extrabold">ติดต่อเรา</span>
        </div>
        <a href="#" class="block text-center bg-gray-900 text-white py-2.5 rounded-xl text-sm font-semibold hover:bg-gray-700 transition-colors mb-5">
          ติดต่อ Sales
        </a>
        <ul class="space-y-3 text-sm text-gray-600">
          <li class="flex items-center gap-2"><span class="text-green-500 flex-shrink-0">✓</span> ผู้ใช้งานไม่จำกัด</li>
          <li class="flex items-center gap-2"><span class="text-green-500 flex-shrink-0">✓</span> Custom Analytics</li>
          <li class="flex items-center gap-2"><span class="text-green-500 flex-shrink-0">✓</span> Dedicated Support</li>
          <li class="flex items-center gap-2"><span class="text-green-500 flex-shrink-0">✓</span> SSO & SAML</li>
          <li class="flex items-center gap-2"><span class="text-green-500 flex-shrink-0">✓</span> SLA 99.99%</li>
          <li class="flex items-center gap-2"><span class="text-green-500 flex-shrink-0">✓</span> Custom Integrations</li>
        </ul>
      </div>
    </div>

    <!-- Compare table link -->
    <p class="text-center mt-8 text-sm text-gray-500">
      ต้องการเปรียบเทียบแผน? <a href="#" class="text-indigo-600 hover:underline">ดูตารางเปรียบเทียบ →</a>
    </p>
  </div>

  <script>
    const prices = {
      monthly: { starter: '฿990', pro: '฿2,490' },
      yearly: { starter: '฿792', pro: '฿1,992' }
    };
    function setPricing(type) {
      document.getElementById('starter-price').textContent = prices[type].starter;
      document.getElementById('pro-price').textContent = prices[type].pro;
      document.getElementById('btn-monthly').className = type === 'monthly'
        ? 'px-4 py-1.5 rounded-full text-sm font-medium bg-indigo-600 text-white transition-all'
        : 'px-4 py-1.5 rounded-full text-sm font-medium text-gray-600 hover:text-gray-900 transition-all';
      document.getElementById('btn-yearly').className = type === 'yearly'
        ? 'px-4 py-1.5 rounded-full text-sm font-medium bg-indigo-600 text-white transition-all'
        : 'px-4 py-1.5 rounded-full text-sm font-medium text-gray-600 hover:text-gray-900 transition-all';
    }
  </script>
</body>
</html>
```

---

## Step 364: Testimonials

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Testimonials - Step 364</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 py-16 px-4">

  <div class="max-w-6xl mx-auto">
    <div class="text-center mb-10">
      <h2 class="text-3xl font-extrabold mb-3">ลูกค้าพูดถึงเราอย่างไร?</h2>
      <p class="text-gray-500">เสียงจากผู้ที่ใช้งาน SaasKit จริง</p>
    </div>

    <!-- Featured Testimonial -->
    <div class="bg-white rounded-2xl p-8 shadow-sm border mb-6 relative overflow-hidden">
      <div class="absolute top-6 right-8 text-gray-100 text-8xl font-serif leading-none">"</div>
      <div class="flex items-start gap-4 relative">
        <img src="https://i.pravatar.cc/60?img=10" class="w-14 h-14 rounded-full flex-shrink-0" alt="">
        <div>
          <div class="flex items-center gap-1 mb-2 text-yellow-400">★★★★★</div>
          <p class="text-lg text-gray-700 leading-relaxed mb-4">
            "SaasKit เปลี่ยนชีวิตทีมเราไปเลย ก่อนหน้านี้เราใช้เครื่องมือ 5-6 ตัวแยกกัน แต่ตอนนี้ทุกอย่างอยู่ในที่เดียว ทีมทำงานได้เร็วขึ้นกว่าเดิม 3 เท่า และ error น้อยลงมาก"
          </p>
          <div>
            <p class="font-bold">คุณสมศักดิ์ นาคา</p>
            <p class="text-sm text-gray-500">CTO, TechStart Thailand</p>
          </div>
        </div>
      </div>
    </div>

    <!-- Grid Testimonials -->
    <div class="grid grid-cols-1 md:grid-cols-3 gap-5">
      <div class="bg-white rounded-2xl p-5 border shadow-sm">
        <div class="flex items-center gap-1 text-yellow-400 mb-3 text-sm">★★★★★</div>
        <p class="text-gray-600 text-sm leading-relaxed mb-4">
          "การ setup ง่ายมาก ใช้เวลาแค่ 30 นาทีก็ integrate กับ workflow เดิมได้เลย"
        </p>
        <div class="flex items-center gap-2">
          <img src="https://i.pravatar.cc/32?img=11" class="w-8 h-8 rounded-full" alt="">
          <div>
            <p class="text-sm font-semibold">วันดี สายธรรม</p>
            <p class="text-xs text-gray-400">Product Manager, Acme Corp</p>
          </div>
        </div>
      </div>

      <div class="bg-indigo-600 rounded-2xl p-5 text-white">
        <div class="flex items-center gap-1 text-yellow-400 mb-3 text-sm">★★★★★</div>
        <p class="text-indigo-200 text-sm leading-relaxed mb-4">
          "AI Report Generator ช่วยประหยัดเวลาทีม Analytics ไปได้กว่า 15 ชั่วโมงต่อสัปดาห์"
        </p>
        <div class="flex items-center gap-2">
          <img src="https://i.pravatar.cc/32?img=12" class="w-8 h-8 rounded-full" alt="">
          <div>
            <p class="text-sm font-semibold">ธนากร วงษ์ดี</p>
            <p class="text-xs text-indigo-300">Data Lead, Globex Inc</p>
          </div>
        </div>
      </div>

      <div class="bg-white rounded-2xl p-5 border shadow-sm">
        <div class="flex items-center gap-1 text-yellow-400 mb-3 text-sm">★★★★★</div>
        <p class="text-gray-600 text-sm leading-relaxed mb-4">
          "Support ตอบเร็วมาก มี live chat ตลอด 24 ชั่วโมง ทีม Enterprise ให้ความช่วยเหลือดีมาก"
        </p>
        <div class="flex items-center gap-2">
          <img src="https://i.pravatar.cc/32?img=13" class="w-8 h-8 rounded-full" alt="">
          <div>
            <p class="text-sm font-semibold">สาวิตรี รัตน์</p>
            <p class="text-xs text-gray-400">COO, RetailPlus</p>
          </div>
        </div>
      </div>
    </div>

    <!-- Stats -->
    <div class="mt-10 grid grid-cols-2 md:grid-cols-4 gap-4">
      <div class="bg-white rounded-xl p-5 text-center border">
        <p class="text-3xl font-extrabold text-indigo-600 mb-1">2,500+</p>
        <p class="text-sm text-gray-500">บริษัทที่ใช้งาน</p>
      </div>
      <div class="bg-white rounded-xl p-5 text-center border">
        <p class="text-3xl font-extrabold text-indigo-600 mb-1">4.9★</p>
        <p class="text-sm text-gray-500">คะแนนเฉลี่ย</p>
      </div>
      <div class="bg-white rounded-xl p-5 text-center border">
        <p class="text-3xl font-extrabold text-indigo-600 mb-1">70%</p>
        <p class="text-sm text-gray-500">ลดเวลาทำงาน</p>
      </div>
      <div class="bg-white rounded-xl p-5 text-center border">
        <p class="text-3xl font-extrabold text-indigo-600 mb-1">99.9%</p>
        <p class="text-sm text-gray-500">Uptime SLA</p>
      </div>
    </div>
  </div>
</body>
</html>
```

---

## Step 365: FAQ Accordion

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>FAQ - Step 365</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-white py-16 px-4">

  <div class="max-w-2xl mx-auto">
    <div class="text-center mb-10">
      <h2 class="text-3xl font-extrabold mb-3">คำถามที่พบบ่อย</h2>
      <p class="text-gray-500">หาคำตอบที่ต้องการ หรือ <a href="#" class="text-indigo-600 hover:underline">ติดต่อเรา</a></p>
    </div>

    <div class="space-y-3" id="faq">
      <!-- FAQ items injected by JS -->
    </div>
  </div>

  <script>
    const faqs = [
      { q: 'ทดลองฟรีหมายความว่าอย่างไร?', a: 'คุณสามารถใช้ฟีเจอร์ทั้งหมดของแผน Pro ได้ฟรี 14 วัน โดยไม่ต้องใส่ข้อมูลบัตรเครดิต หากหมดระยะทดลอง ระบบจะถามว่าต้องการ upgrade หรือใช้แผน Free ต่อ' },
      { q: 'สามารถยกเลิกได้เมื่อไหร่?', a: 'คุณสามารถยกเลิก subscription ได้ทุกเมื่อ โดยไม่มีค่าปรับหรือค่าใช้จ่ายเพิ่มเติม ข้อมูลของคุณจะยังคงอยู่ครบ 30 วันหลังยกเลิก' },
      { q: 'รองรับ integration กับ tool อะไรบ้าง?', a: 'SaasKit รองรับ integration กับ Salesforce, HubSpot, Slack, Line, Google Workspace, Microsoft 365, Zapier และอีกกว่า 100+ tools ผ่าน REST API และ Webhooks' },
      { q: 'ข้อมูลปลอดภัยไหม?', a: 'เรา encrypt ข้อมูลทั้งหมดด้วย AES-256 ทั้งใน transit และ at rest ผ่านการ audit ด้าน security ทุกปี และมี SOC 2 Type II, ISO 27001 certification' },
      { q: 'มี API ให้ใช้ไหม?', a: 'มีครับ เรามี REST API และ GraphQL API ที่ครอบคลุมทุก feature พร้อม SDK สำหรับ JavaScript, Python, PHP, Go documentation ครบถ้วน' },
      { q: 'Support ให้บริการกี่โมง?', a: 'แผน Starter: Email support ตอบภายใน 24 ชั่วโมง | Pro: Priority support ตอบภายใน 4 ชั่วโมง + Live chat | Enterprise: Dedicated success manager + 24/7 phone support' },
    ];

    function renderFAQ() {
      document.getElementById('faq').innerHTML = faqs.map((item, i) => `
        <div class="border border-gray-200 rounded-xl overflow-hidden" id="faq-item-${i}">
          <button onclick="toggleFAQ(${i})"
            class="w-full flex items-center justify-between px-5 py-4 text-left font-semibold text-gray-900 hover:bg-gray-50 transition-colors">
            <span class="pr-4">${item.q}</span>
            <svg id="faq-icon-${i}" class="w-5 h-5 text-gray-400 flex-shrink-0 transition-transform duration-300" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"/>
            </svg>
          </button>
          <div id="faq-body-${i}" class="hidden px-5 pb-4 text-gray-600 text-sm leading-relaxed border-t border-gray-100 pt-3">
            ${item.a}
          </div>
        </div>
      `).join('');
    }

    function toggleFAQ(i) {
      const body = document.getElementById('faq-body-' + i);
      const icon = document.getElementById('faq-icon-' + i);
      const isOpen = !body.classList.contains('hidden');
      body.classList.toggle('hidden');
      icon.style.transform = isOpen ? '' : 'rotate(180deg)';
    }

    renderFAQ();
    toggleFAQ(0); // Open first by default
  </script>
</body>
</html>
```

---

## Step 366: CTA Section

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>CTA Sections - Step 366</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-8 space-y-10">

  <!-- Simple CTA -->
  <section class="max-w-4xl mx-auto bg-indigo-600 rounded-3xl p-10 md:p-14 text-white text-center">
    <h2 class="text-3xl md:text-4xl font-extrabold mb-4">พร้อมเปลี่ยนวิธีทำธุรกิจแล้วหรือยัง?</h2>
    <p class="text-indigo-200 text-lg mb-8 max-w-xl mx-auto">
      เริ่มทดลองฟรี 14 วัน ไม่ต้องใส่บัตรเครดิต ยกเลิกได้ทุกเมื่อ
    </p>
    <div class="flex flex-col sm:flex-row gap-3 justify-center">
      <a href="#" class="bg-white text-indigo-700 font-bold px-8 py-3.5 rounded-xl hover:bg-indigo-50 transition-colors">
        เริ่มทดลองฟรีเลย
      </a>
      <a href="#" class="border border-indigo-400 text-white font-semibold px-6 py-3.5 rounded-xl hover:bg-indigo-500 transition-colors">
        นัด Demo
      </a>
    </div>
    <p class="text-indigo-300 text-sm mt-4">✓ ไม่มีค่าใช้จ่าย  ✓ Setup ใน 5 นาที  ✓ ยกเลิกได้ทุกเมื่อ</p>
  </section>

  <!-- Gradient Background CTA -->
  <section class="max-w-4xl mx-auto rounded-3xl overflow-hidden">
    <div class="bg-gradient-to-r from-violet-600 to-indigo-600 p-10 text-white">
      <div class="grid grid-cols-1 md:grid-cols-2 gap-8 items-center">
        <div>
          <h2 class="text-2xl font-extrabold mb-3">คุยกับ Expert ของเรา</h2>
          <p class="text-violet-200 mb-4">ให้เราช่วย design solution ที่เหมาะกับธุรกิจของคุณ ฟรี</p>
          <ul class="space-y-2 text-sm text-violet-200">
            <li class="flex items-center gap-2"><span>✓</span> Workshop 1-on-1</li>
            <li class="flex items-center gap-2"><span>✓</span> Custom implementation plan</li>
            <li class="flex items-center gap-2"><span>✓</span> ROI calculation</li>
          </ul>
        </div>
        <div class="flex flex-col gap-3">
          <input type="text" placeholder="ชื่อของคุณ" class="bg-white/15 border border-white/25 rounded-xl px-4 py-3 text-white placeholder-violet-300 focus:outline-none focus:ring-2 focus:ring-white/50">
          <input type="email" placeholder="Email" class="bg-white/15 border border-white/25 rounded-xl px-4 py-3 text-white placeholder-violet-300 focus:outline-none focus:ring-2 focus:ring-white/50">
          <select class="bg-white/15 border border-white/25 rounded-xl px-4 py-3 text-violet-300 focus:outline-none">
            <option value="">ขนาดบริษัท</option>
            <option>1-10 คน</option>
            <option>11-50 คน</option>
            <option>51-200 คน</option>
            <option>200+ คน</option>
          </select>
          <button class="bg-white text-violet-700 font-bold py-3 rounded-xl hover:bg-violet-50 transition-colors">
            นัด Demo ฟรี →
          </button>
        </div>
      </div>
    </div>
  </section>

</body>
</html>
```

---

## Step 367: Footer

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Footer - Step 367</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-900 text-gray-400">

  <footer class="max-w-6xl mx-auto px-4 py-16">
    <div class="grid grid-cols-2 md:grid-cols-5 gap-8 mb-12">
      <!-- Brand -->
      <div class="col-span-2">
        <div class="flex items-center gap-2 mb-4">
          <div class="w-8 h-8 bg-indigo-600 rounded-lg flex items-center justify-center">
            <span class="text-white font-bold text-sm">S</span>
          </div>
          <span class="font-bold text-lg text-white">SaasKit</span>
        </div>
        <p class="text-sm leading-relaxed mb-6">
          Platform ที่ช่วยธุรกิจทุกขนาดทำงานได้ชาญฉลาดกว่าเดิม ด้วย AI, Automation, และ Analytics ในที่เดียว
        </p>
        <div class="flex gap-3">
          <a href="#" class="w-9 h-9 bg-gray-800 hover:bg-indigo-600 rounded-lg flex items-center justify-center transition-colors">
            <svg class="w-4 h-4 text-gray-400" fill="currentColor" viewBox="0 0 24 24"><path d="M8.29 20.251c7.547 0 11.675-6.253 11.675-11.675 0-.178 0-.355-.012-.53A8.348 8.348 0 0022 5.92a8.19 8.19 0 01-2.357.646 4.118 4.118 0 001.804-2.27 8.224 8.224 0 01-2.605.996 4.107 4.107 0 00-6.993 3.743 11.65 11.65 0 01-8.457-4.287 4.106 4.106 0 001.27 5.477A4.072 4.072 0 012.8 9.713v.052a4.105 4.105 0 003.292 4.022 4.095 4.095 0 01-1.853.07 4.108 4.108 0 003.834 2.85A8.233 8.233 0 012 18.407a11.616 11.616 0 006.29 1.84"/></svg>
          </a>
          <a href="#" class="w-9 h-9 bg-gray-800 hover:bg-indigo-600 rounded-lg flex items-center justify-center transition-colors">
            <svg class="w-4 h-4 text-gray-400" fill="currentColor" viewBox="0 0 24 24"><path fill-rule="evenodd" d="M12 2C6.477 2 2 6.484 2 12.017c0 4.425 2.865 8.18 6.839 9.504.5.092.682-.217.682-.483 0-.237-.008-.868-.013-1.703-2.782.605-3.369-1.343-3.369-1.343-.454-1.158-1.11-1.466-1.11-1.466-.908-.62.069-.608.069-.608 1.003.07 1.531 1.032 1.531 1.032.892 1.53 2.341 1.088 2.91.832.092-.647.35-1.088.636-1.338-2.22-.253-4.555-1.113-4.555-4.951 0-1.093.39-1.988 1.029-2.688-.103-.253-.446-1.272.098-2.65 0 0 .84-.27 2.75 1.026A9.564 9.564 0 0112 6.844c.85.004 1.705.115 2.504.337 1.909-1.296 2.747-1.027 2.747-1.027.546 1.379.202 2.398.1 2.651.64.7 1.028 1.595 1.028 2.688 0 3.848-2.339 4.695-4.566 4.943.359.309.678.92.678 1.855 0 1.338-.012 2.419-.012 2.747 0 .268.18.58.688.482A10.019 10.019 0 0022 12.017C22 6.484 17.522 2 12 2z" clip-rule="evenodd"/></svg>
          </a>
          <a href="#" class="w-9 h-9 bg-gray-800 hover:bg-indigo-600 rounded-lg flex items-center justify-center transition-colors">
            <svg class="w-4 h-4 text-gray-400" fill="currentColor" viewBox="0 0 24 24"><path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433c-1.144 0-2.063-.926-2.063-2.065 0-1.138.92-2.063 2.063-2.063 1.14 0 2.064.925 2.064 2.063 0 1.139-.925 2.065-2.064 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"/></svg>
          </a>
        </div>
      </div>

      <!-- Links -->
      <div>
        <h4 class="text-white font-semibold mb-4 text-sm">Product</h4>
        <ul class="space-y-2 text-sm">
          <li><a href="#" class="hover:text-white transition-colors">Features</a></li>
          <li><a href="#" class="hover:text-white transition-colors">Pricing</a></li>
          <li><a href="#" class="hover:text-white transition-colors">Changelog</a></li>
          <li><a href="#" class="hover:text-white transition-colors">Roadmap</a></li>
          <li><a href="#" class="hover:text-white transition-colors">Status</a></li>
        </ul>
      </div>

      <div>
        <h4 class="text-white font-semibold mb-4 text-sm">Company</h4>
        <ul class="space-y-2 text-sm">
          <li><a href="#" class="hover:text-white transition-colors">About</a></li>
          <li><a href="#" class="hover:text-white transition-colors">Blog</a></li>
          <li><a href="#" class="hover:text-white transition-colors">Careers</a></li>
          <li><a href="#" class="hover:text-white transition-colors">Press</a></li>
        </ul>
      </div>

      <div>
        <h4 class="text-white font-semibold mb-4 text-sm">Resources</h4>
        <ul class="space-y-2 text-sm">
          <li><a href="#" class="hover:text-white transition-colors">Documentation</a></li>
          <li><a href="#" class="hover:text-white transition-colors">API Reference</a></li>
          <li><a href="#" class="hover:text-white transition-colors">Community</a></li>
          <li><a href="#" class="hover:text-white transition-colors">Support</a></li>
        </ul>
      </div>
    </div>

    <!-- Bottom bar -->
    <div class="border-t border-gray-800 pt-6 flex flex-col md:flex-row items-center justify-between gap-3">
      <p class="text-xs">© 2024 SaasKit, Inc. สงวนลิขสิทธิ์</p>
      <div class="flex gap-4 text-xs">
        <a href="#" class="hover:text-white transition-colors">Privacy Policy</a>
        <a href="#" class="hover:text-white transition-colors">Terms of Service</a>
        <a href="#" class="hover:text-white transition-colors">Cookies</a>
      </div>
      <div class="flex items-center gap-2 text-xs">
        <span class="w-2 h-2 bg-green-500 rounded-full"></span>
        All systems operational
      </div>
    </div>
  </footer>

</body>
</html>
```

---

## Step 368: Integration Logos Grid

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Integrations - Step 368</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-white py-16 px-4">
  <div class="max-w-4xl mx-auto text-center">
    <h2 class="text-2xl font-extrabold mb-3">เชื่อมต่อกับ Tool ที่คุณใช้อยู่</h2>
    <p class="text-gray-500 mb-10">รองรับ integration 100+ tools ผ่าน REST API และ Webhooks</p>

    <div class="grid grid-cols-4 md:grid-cols-6 gap-4">
      <!-- Integration tiles -->
      <div class="border border-gray-200 rounded-xl p-4 flex flex-col items-center gap-2 hover:border-indigo-300 hover:shadow-sm transition-all">
        <div class="w-10 h-10 bg-blue-500 rounded-xl flex items-center justify-center text-white font-bold">S</div>
        <span class="text-xs text-gray-600">Slack</span>
      </div>
      <div class="border border-gray-200 rounded-xl p-4 flex flex-col items-center gap-2 hover:border-indigo-300 hover:shadow-sm transition-all">
        <div class="w-10 h-10 bg-green-500 rounded-xl flex items-center justify-center text-white font-bold">H</div>
        <span class="text-xs text-gray-600">HubSpot</span>
      </div>
      <div class="border border-gray-200 rounded-xl p-4 flex flex-col items-center gap-2 hover:border-indigo-300 hover:shadow-sm transition-all">
        <div class="w-10 h-10 bg-blue-600 rounded-xl flex items-center justify-center text-white font-bold">SF</div>
        <span class="text-xs text-gray-600">Salesforce</span>
      </div>
      <div class="border border-gray-200 rounded-xl p-4 flex flex-col items-center gap-2 hover:border-indigo-300 hover:shadow-sm transition-all">
        <div class="w-10 h-10 bg-orange-500 rounded-xl flex items-center justify-center text-white font-bold">Z</div>
        <span class="text-xs text-gray-600">Zapier</span>
      </div>
      <div class="border border-gray-200 rounded-xl p-4 flex flex-col items-center gap-2 hover:border-indigo-300 hover:shadow-sm transition-all">
        <div class="w-10 h-10 bg-red-500 rounded-xl flex items-center justify-center text-white font-bold">G</div>
        <span class="text-xs text-gray-600">Gmail</span>
      </div>
      <div class="border border-gray-200 rounded-xl p-4 flex flex-col items-center gap-2 hover:border-indigo-300 hover:shadow-sm transition-all">
        <div class="w-10 h-10 bg-green-600 rounded-xl flex items-center justify-center text-white font-bold">Li</div>
        <span class="text-xs text-gray-600">Line</span>
      </div>
      <div class="border border-gray-200 rounded-xl p-4 flex flex-col items-center gap-2 hover:border-indigo-300 hover:shadow-sm transition-all">
        <div class="w-10 h-10 bg-blue-700 rounded-xl flex items-center justify-center text-white font-bold">Jira</div>
        <span class="text-xs text-gray-600">Jira</span>
      </div>
      <div class="border border-gray-200 rounded-xl p-4 flex flex-col items-center gap-2 hover:border-indigo-300 hover:shadow-sm transition-all">
        <div class="w-10 h-10 bg-gray-800 rounded-xl flex items-center justify-center text-white font-bold">GH</div>
        <span class="text-xs text-gray-600">GitHub</span>
      </div>
      <div class="border border-gray-200 rounded-xl p-4 flex flex-col items-center gap-2 hover:border-indigo-300 hover:shadow-sm transition-all">
        <div class="w-10 h-10 bg-purple-500 rounded-xl flex items-center justify-center text-white font-bold">Nt</div>
        <span class="text-xs text-gray-600">Notion</span>
      </div>
      <div class="border border-gray-200 rounded-xl p-4 flex flex-col items-center gap-2 hover:border-indigo-300 hover:shadow-sm transition-all">
        <div class="w-10 h-10 bg-teal-500 rounded-xl flex items-center justify-center text-white font-bold">At</div>
        <span class="text-xs text-gray-600">Airtable</span>
      </div>
      <div class="border border-gray-200 rounded-xl p-4 flex flex-col items-center gap-2 hover:border-indigo-300 hover:shadow-sm transition-all">
        <div class="w-10 h-10 bg-pink-500 rounded-xl flex items-center justify-center text-white font-bold">Fi</div>
        <span class="text-xs text-gray-600">Figma</span>
      </div>
      <div class="border border-gray-200 border-dashed rounded-xl p-4 flex flex-col items-center gap-2 hover:border-indigo-300 transition-all">
        <div class="w-10 h-10 bg-gray-100 rounded-xl flex items-center justify-center text-gray-400 text-xl font-bold">+</div>
        <span class="text-xs text-gray-400">100+ อื่นๆ</span>
      </div>
    </div>
  </div>
</body>
</html>
```

---

## Step 369: Comparison Table

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Comparison Table - Step 369</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-white py-16 px-4">
  <div class="max-w-4xl mx-auto">
    <div class="text-center mb-10">
      <h2 class="text-2xl font-extrabold mb-2">SaasKit vs คู่แข่ง</h2>
      <p class="text-gray-500 text-sm">เปรียบเทียบความสามารถ</p>
    </div>

    <div class="overflow-x-auto">
      <table class="w-full">
        <thead>
          <tr>
            <th class="text-left pb-4 pr-6 text-gray-500 font-medium text-sm w-1/3">ฟีเจอร์</th>
            <th class="text-center pb-4 px-4">
              <div class="bg-indigo-600 text-white text-sm font-bold px-4 py-1.5 rounded-full inline-block">SaasKit</div>
            </th>
            <th class="text-center pb-4 px-4 text-gray-400 font-medium text-sm">Competitor A</th>
            <th class="text-center pb-4 px-4 text-gray-400 font-medium text-sm">Competitor B</th>
          </tr>
        </thead>
        <tbody class="divide-y divide-gray-100">
          <tr>
            <td class="py-3.5 pr-6 text-sm text-gray-700">AI Report Generator</td>
            <td class="py-3.5 px-4 text-center text-green-500">✓</td>
            <td class="py-3.5 px-4 text-center text-red-400">✗</td>
            <td class="py-3.5 px-4 text-center text-yellow-500">บางส่วน</td>
          </tr>
          <tr>
            <td class="py-3.5 pr-6 text-sm text-gray-700">Unlimited Automations</td>
            <td class="py-3.5 px-4 text-center text-green-500">✓</td>
            <td class="py-3.5 px-4 text-center text-gray-400 text-xs">max 50</td>
            <td class="py-3.5 px-4 text-center text-gray-400 text-xs">max 100</td>
          </tr>
          <tr>
            <td class="py-3.5 pr-6 text-sm text-gray-700">Multi-brand Support</td>
            <td class="py-3.5 px-4 text-center text-green-500">✓</td>
            <td class="py-3.5 px-4 text-center text-red-400">✗</td>
            <td class="py-3.5 px-4 text-center text-red-400">✗</td>
          </tr>
          <tr>
            <td class="py-3.5 pr-6 text-sm text-gray-700">SLA 99.99%</td>
            <td class="py-3.5 px-4 text-center text-green-500">✓</td>
            <td class="py-3.5 px-4 text-center text-gray-400 text-xs">99.9%</td>
            <td class="py-3.5 px-4 text-center text-gray-400 text-xs">99.5%</td>
          </tr>
          <tr>
            <td class="py-3.5 pr-6 text-sm text-gray-700">ราคาเริ่มต้น/เดือน</td>
            <td class="py-3.5 px-4 text-center font-bold text-indigo-600">฿990</td>
            <td class="py-3.5 px-4 text-center text-gray-400">฿1,800</td>
            <td class="py-3.5 px-4 text-center text-gray-400">฿2,200</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</body>
</html>
```

---

## Step 370: Workshop — Full SaaS Landing Page

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>SaasKit - Full Landing Page Workshop</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    html { scroll-behavior: smooth; }
    @keyframes float { 0%,100%{transform:translateY(0)} 50%{transform:translateY(-8px)} }
    .float { animation: float 3s ease-in-out infinite; }
  </style>
</head>
<body class="bg-white text-gray-900 antialiased">

  <!-- Announcement -->
  <div class="bg-indigo-600 text-white text-center text-xs py-2 px-4">
    🎉 SaasKit v2.0 เปิดตัวแล้ว! — <a href="#" class="underline font-semibold">ดูฟีเจอร์ใหม่ →</a>
  </div>

  <!-- Nav -->
  <header class="sticky top-0 z-50 bg-white/90 backdrop-blur-sm border-b">
    <nav class="max-w-6xl mx-auto px-4 py-3 flex items-center justify-between">
      <div class="flex items-center gap-2">
        <div class="w-8 h-8 bg-indigo-600 rounded-xl flex items-center justify-center"><span class="text-white font-bold text-sm">S</span></div>
        <span class="font-extrabold text-lg">SaasKit</span>
      </div>
      <div class="hidden md:flex items-center gap-6 text-sm font-medium text-gray-600">
        <a href="#features" class="hover:text-gray-900 transition-colors">ฟีเจอร์</a>
        <a href="#pricing" class="hover:text-gray-900 transition-colors">ราคา</a>
        <a href="#testimonials" class="hover:text-gray-900 transition-colors">รีวิว</a>
        <a href="#faq" class="hover:text-gray-900 transition-colors">FAQ</a>
      </div>
      <div class="flex items-center gap-2">
        <a href="#" class="text-sm text-gray-600 hover:text-gray-900 font-medium hidden sm:block">เข้าสู่ระบบ</a>
        <a href="#" class="bg-indigo-600 text-white text-sm px-4 py-2 rounded-lg hover:bg-indigo-700 transition-colors font-medium shadow-lg shadow-indigo-200">
          ทดลองฟรี
        </a>
      </div>
    </nav>
  </header>

  <!-- Hero -->
  <section class="max-w-6xl mx-auto px-4 pt-16 pb-20 text-center">
    <div class="inline-flex items-center gap-2 bg-indigo-50 border border-indigo-100 text-indigo-700 text-xs font-semibold px-4 py-1.5 rounded-full mb-6">
      <span class="w-2 h-2 bg-green-500 rounded-full animate-pulse"></span>
      อัพเดทล่าสุด: AI Dashboard 2.0
    </div>
    <h1 class="text-4xl md:text-6xl font-extrabold leading-tight mb-6 max-w-3xl mx-auto">
      บริหารธุรกิจของคุณ<br>
      <span class="bg-gradient-to-r from-indigo-600 to-purple-600 bg-clip-text text-transparent">อย่างชาญฉลาด</span>
    </h1>
    <p class="text-xl text-gray-500 max-w-2xl mx-auto mb-10">
      Platform ที่รวม CRM, Analytics, และ Automation เข้าด้วยกัน ช่วยทีมทำงานเร็วขึ้น 3 เท่า
    </p>
    <div class="flex flex-col sm:flex-row gap-3 justify-center mb-10">
      <a href="#" class="bg-indigo-600 text-white px-8 py-3.5 rounded-xl font-bold hover:bg-indigo-700 transition-all shadow-xl shadow-indigo-200 text-sm">
        เริ่มทดลองฟรี 14 วัน →
      </a>
      <a href="#" class="flex items-center justify-center gap-2 border border-gray-300 px-6 py-3.5 rounded-xl font-semibold hover:bg-gray-50 text-sm text-gray-700 transition-colors">
        ▶ ดู Demo
      </a>
    </div>
    <div class="flex flex-wrap items-center justify-center gap-5 text-sm text-gray-500">
      <div class="flex items-center gap-1.5">
        <div class="flex -space-x-1.5">
          <img src="https://i.pravatar.cc/24?img=1" class="w-6 h-6 rounded-full ring-2 ring-white">
          <img src="https://i.pravatar.cc/24?img=2" class="w-6 h-6 rounded-full ring-2 ring-white">
          <img src="https://i.pravatar.cc/24?img=3" class="w-6 h-6 rounded-full ring-2 ring-white">
        </div>
        2,500+ บริษัท
      </div>
      <span class="text-yellow-400">★★★★★</span><span>4.9/5</span>
      <span class="text-green-500">✓</span><span>ไม่ต้องใส่บัตรเครดิต</span>
    </div>
  </section>

  <!-- Logos -->
  <section class="border-y bg-gray-50 py-8 px-4">
    <div class="max-w-5xl mx-auto flex flex-wrap justify-center items-center gap-8 opacity-50">
      <span class="text-xl font-black text-gray-500">ACME</span>
      <span class="text-xl font-black text-gray-500">TECHCO</span>
      <span class="text-xl font-black text-gray-500">GLOBEX</span>
      <span class="text-xl font-black text-gray-500">INITECH</span>
      <span class="text-xl font-black text-gray-500">UMBRELLA</span>
    </div>
  </section>

  <!-- Features -->
  <section id="features" class="max-w-6xl mx-auto px-4 py-20">
    <div class="text-center mb-12">
      <h2 class="text-3xl font-extrabold mb-3">ทุกอย่างที่คุณต้องการในที่เดียว</h2>
      <p class="text-gray-500 max-w-xl mx-auto">ไม่ต้องต่อหลาย tool ให้ยุ่งยาก</p>
    </div>
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-5">
      <div class="p-6 border rounded-2xl hover:shadow-md hover:border-indigo-200 transition-all">
        <div class="text-3xl mb-3">📊</div>
        <h3 class="font-bold mb-2">AI Analytics</h3>
        <p class="text-sm text-gray-500">วิเคราะห์ข้อมูล real-time สร้าง insight อัตโนมัติ</p>
      </div>
      <div class="p-6 border rounded-2xl hover:shadow-md hover:border-indigo-200 transition-all">
        <div class="text-3xl mb-3">🤖</div>
        <h3 class="font-bold mb-2">Automation</h3>
        <p class="text-sm text-gray-500">ระบบ Automation ขับเคลื่อนด้วย AI ลดงาน 70%</p>
      </div>
      <div class="p-6 border rounded-2xl hover:shadow-md hover:border-indigo-200 transition-all">
        <div class="text-3xl mb-3">👥</div>
        <h3 class="font-bold mb-2">CRM</h3>
        <p class="text-sm text-gray-500">จัดการ Lead และ Customer ครบในที่เดียว</p>
      </div>
      <div class="p-6 border rounded-2xl hover:shadow-md hover:border-indigo-200 transition-all">
        <div class="text-3xl mb-3">🔔</div>
        <h3 class="font-bold mb-2">Smart Alerts</h3>
        <p class="text-sm text-gray-500">รับ notification เมื่อมีสิ่งสำคัญผ่านทุก channel</p>
      </div>
      <div class="p-6 border rounded-2xl hover:shadow-md hover:border-indigo-200 transition-all">
        <div class="text-3xl mb-3">🔒</div>
        <h3 class="font-bold mb-2">Enterprise Security</h3>
        <p class="text-sm text-gray-500">SSO, 2FA, SOC2, ISO27001 certification</p>
      </div>
      <div class="p-6 border rounded-2xl hover:shadow-md hover:border-indigo-200 transition-all">
        <div class="text-3xl mb-3">🌍</div>
        <h3 class="font-bold mb-2">Global Infrastructure</h3>
        <p class="text-sm text-gray-500">SLA 99.99% deploy ทั่วโลก รองรับ PDPA/GDPR</p>
      </div>
    </div>
  </section>

  <!-- Pricing -->
  <section id="pricing" class="bg-gray-50 py-20 px-4">
    <div class="max-w-5xl mx-auto">
      <div class="text-center mb-10">
        <h2 class="text-3xl font-extrabold mb-3">เลือกแผนที่เหมาะกับคุณ</h2>
        <div class="inline-flex items-center bg-white border rounded-full p-1 gap-1 shadow-sm mt-3">
          <button onclick="toggleBilling(this,'monthly')" data-type="monthly"
            class="active-pill px-4 py-1.5 rounded-full text-sm font-medium bg-indigo-600 text-white">รายเดือน</button>
          <button onclick="toggleBilling(this,'yearly')" data-type="yearly"
            class="px-4 py-1.5 rounded-full text-sm font-medium text-gray-600 hover:text-gray-900">รายปี <span class="text-green-600 font-semibold text-xs">-20%</span></button>
        </div>
      </div>
      <div class="grid grid-cols-1 md:grid-cols-3 gap-5">
        <div class="bg-white rounded-2xl p-6 border">
          <p class="font-bold text-lg mb-1">Starter</p>
          <p class="text-3xl font-extrabold my-3"><span class="price" data-monthly="฿990" data-yearly="฿792">฿990</span><span class="text-sm text-gray-500 font-normal">/เดือน</span></p>
          <a href="#" class="block text-center border border-gray-300 py-2.5 rounded-xl text-sm font-semibold hover:bg-gray-50 transition-colors mb-5">เริ่มทดลองฟรี</a>
          <ul class="space-y-2.5 text-sm text-gray-600">
            <li>✓ ผู้ใช้ 5 คน</li><li>✓ Basic Analytics</li><li>✓ 10 Automations</li><li class="text-gray-300">✗ AI Features</li>
          </ul>
        </div>
        <div class="bg-indigo-600 text-white rounded-2xl p-6 shadow-xl shadow-indigo-200 relative">
          <div class="absolute -top-3 right-5 bg-yellow-400 text-yellow-900 text-xs font-bold px-3 py-0.5 rounded-full">ยอดนิยม</div>
          <p class="font-bold text-lg mb-1">Pro</p>
          <p class="text-3xl font-extrabold my-3"><span class="price" data-monthly="฿2,490" data-yearly="฿1,992">฿2,490</span><span class="text-sm text-indigo-300 font-normal">/เดือน</span></p>
          <a href="#" class="block text-center bg-white text-indigo-700 py-2.5 rounded-xl text-sm font-bold hover:bg-indigo-50 transition-colors mb-5">เริ่มทดลองฟรี</a>
          <ul class="space-y-2.5 text-sm text-indigo-200">
            <li>✓ ผู้ใช้ 25 คน</li><li>✓ Advanced Analytics</li><li>✓ Unlimited Automations</li><li>✓ AI Features</li>
          </ul>
        </div>
        <div class="bg-white rounded-2xl p-6 border">
          <p class="font-bold text-lg mb-1">Enterprise</p>
          <p class="text-2xl font-extrabold my-3">ติดต่อเรา</p>
          <a href="#" class="block text-center bg-gray-900 text-white py-2.5 rounded-xl text-sm font-semibold hover:bg-gray-700 transition-colors mb-5">ติดต่อ Sales</a>
          <ul class="space-y-2.5 text-sm text-gray-600">
            <li>✓ ผู้ใช้ไม่จำกัด</li><li>✓ Custom Analytics</li><li>✓ 24/7 Support</li><li>✓ SSO & SLA</li>
          </ul>
        </div>
      </div>
    </div>
  </section>

  <!-- Testimonials -->
  <section id="testimonials" class="max-w-6xl mx-auto px-4 py-20">
    <div class="text-center mb-10">
      <h2 class="text-3xl font-extrabold mb-2">ลูกค้าพูดถึงเรา</h2>
    </div>
    <div class="grid grid-cols-1 md:grid-cols-3 gap-5">
      <div class="bg-white border rounded-2xl p-5 shadow-sm">
        <div class="text-yellow-400 mb-3">★★★★★</div>
        <p class="text-gray-600 text-sm leading-relaxed mb-4">"ทีมเราทำงานเร็วขึ้นกว่าเดิมมาก ฟีเจอร์ AI ประทับใจมาก"</p>
        <div class="flex items-center gap-2">
          <img src="https://i.pravatar.cc/32?img=20" class="w-8 h-8 rounded-full">
          <div><p class="font-semibold text-sm">สมศักดิ์ นาคา</p><p class="text-xs text-gray-400">CTO, TechStart</p></div>
        </div>
      </div>
      <div class="bg-indigo-600 text-white rounded-2xl p-5 shadow-lg">
        <div class="text-yellow-400 mb-3">★★★★★</div>
        <p class="text-indigo-200 text-sm leading-relaxed mb-4">"AI Report Generator ช่วยประหยัดเวลาทีมไปได้ 15+ ชั่วโมงต่อสัปดาห์!"</p>
        <div class="flex items-center gap-2">
          <img src="https://i.pravatar.cc/32?img=21" class="w-8 h-8 rounded-full">
          <div><p class="font-semibold text-sm">วันดี สายธรรม</p><p class="text-xs text-indigo-300">Data Lead, Globex</p></div>
        </div>
      </div>
      <div class="bg-white border rounded-2xl p-5 shadow-sm">
        <div class="text-yellow-400 mb-3">★★★★★</div>
        <p class="text-gray-600 text-sm leading-relaxed mb-4">"Setup ง่ายมาก 30 นาทีก็ ready ใช้งานได้เลย support ดีเยี่ยม"</p>
        <div class="flex items-center gap-2">
          <img src="https://i.pravatar.cc/32?img=22" class="w-8 h-8 rounded-full">
          <div><p class="font-semibold text-sm">ธนากร วงษ์ดี</p><p class="text-xs text-gray-400">PM, RetailPlus</p></div>
        </div>
      </div>
    </div>
  </section>

  <!-- FAQ -->
  <section id="faq" class="bg-gray-50 py-20 px-4">
    <div class="max-w-2xl mx-auto">
      <h2 class="text-3xl font-extrabold text-center mb-10">คำถามที่พบบ่อย</h2>
      <div class="space-y-3" id="faq-list"></div>
    </div>
  </section>

  <!-- CTA -->
  <section class="bg-indigo-600 py-20 px-4 text-white text-center">
    <div class="max-w-2xl mx-auto">
      <h2 class="text-3xl font-extrabold mb-4">พร้อมเริ่มต้นแล้วหรือยัง?</h2>
      <p class="text-indigo-200 mb-8">ทดลองฟรี 14 วัน ไม่ต้องใส่บัตรเครดิต ยกเลิกได้ทุกเมื่อ</p>
      <a href="#" class="inline-block bg-white text-indigo-700 font-bold px-10 py-3.5 rounded-xl hover:bg-indigo-50 transition-colors shadow-xl">
        เริ่มทดลองฟรีเลย →
      </a>
    </div>
  </section>

  <!-- Footer -->
  <footer class="bg-gray-900 text-gray-400 px-4 py-10">
    <div class="max-w-6xl mx-auto flex flex-col md:flex-row items-center justify-between gap-4 text-sm">
      <div class="flex items-center gap-2">
        <div class="w-7 h-7 bg-indigo-600 rounded-lg flex items-center justify-center"><span class="text-white font-bold text-xs">S</span></div>
        <span class="font-bold text-white">SaasKit</span>
      </div>
      <p>© 2024 SaasKit, Inc. สงวนลิขสิทธิ์</p>
      <div class="flex gap-4 text-xs">
        <a href="#" class="hover:text-white">Privacy</a>
        <a href="#" class="hover:text-white">Terms</a>
        <a href="#" class="hover:text-white">Cookies</a>
      </div>
    </div>
  </footer>

  <script>
    // Billing Toggle
    function toggleBilling(btn, type) {
      document.querySelectorAll('[data-type]').forEach(b => {
        b.className = 'px-4 py-1.5 rounded-full text-sm font-medium text-gray-600 hover:text-gray-900';
      });
      btn.className = 'px-4 py-1.5 rounded-full text-sm font-medium bg-indigo-600 text-white';
      document.querySelectorAll('.price').forEach(el => {
        el.textContent = el.getAttribute('data-' + type);
      });
    }

    // FAQ
    const faqs = [
      { q: 'ทดลองฟรีหมายความว่าอย่างไร?', a: 'ใช้ฟีเจอร์ Pro ได้ครบ 14 วัน ไม่ต้องใส่บัตรเครดิต' },
      { q: 'สามารถยกเลิกได้เมื่อไหร่?', a: 'ยกเลิกได้ทุกเมื่อ ไม่มีค่าปรับ ข้อมูลคงอยู่ 30 วัน' },
      { q: 'รองรับ integration กับอะไรบ้าง?', a: 'Salesforce, HubSpot, Slack, Line, Google Workspace และ 100+ tools ผ่าน API' },
      { q: 'ข้อมูลปลอดภัยไหม?', a: 'Encrypt AES-256 ทั้งหมด SOC2 Type II, ISO27001 certified' },
    ];

    document.getElementById('faq-list').innerHTML = faqs.map((f, i) => `
      <div class="bg-white rounded-xl border overflow-hidden">
        <button onclick="this.nextElementSibling.classList.toggle('hidden'); this.querySelector('span:last-child').textContent = this.nextElementSibling.classList.contains('hidden') ? '+' : '−'"
          class="w-full flex items-center justify-between px-5 py-4 font-semibold text-sm text-left hover:bg-gray-50 transition-colors">
          <span>${f.q}</span><span class="text-gray-400 text-lg font-light">${i === 0 ? '−' : '+'}</span>
        </button>
        <div class="${i === 0 ? '' : 'hidden'} px-5 pb-4 text-sm text-gray-600 border-t pt-3 leading-relaxed">${f.a}</div>
      </div>
    `).join('');
  </script>

</body>
</html>
```

---

## สรุป Part 37

| Step | เนื้อหา |
|------|---------|
| 361 | Hero Section: announcement, headline, CTA, social proof, dashboard preview |
| 362 | Feature Section: logos bar, 3-col features, AI feature highlight |
| 363 | Pricing Table: monthly/yearly toggle, 3 tiers |
| 364 | Testimonials: featured + grid, stats numbers |
| 365 | FAQ Accordion: interactive expand/collapse |
| 366 | CTA Sections: gradient + lead form |
| 367 | Footer: multi-column, social links, bottom bar |
| 368 | Integration logos grid |
| 369 | Comparison table vs competitors |
| 370 | Workshop: Full SaaS Landing Page with all sections + JS |

**Part ถัดไป:** Part 38 — Authentication Pages
