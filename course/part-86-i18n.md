# Part 86: Internationalization (i18n)

## เป้าหมาย
- React-i18next setup + namespace
- RTL support ด้วย Tailwind
- Locale-aware formatting (date, number, currency)
- Language switcher UI
- Steps 851–860

---

## Step 851: react-i18next Setup

```bash
npm install i18next react-i18next i18next-browser-languagedetector
```

```ts
// src/i18n/index.ts
import i18n from 'i18next'
import { initReactI18next } from 'react-i18next'
import LanguageDetector from 'i18next-browser-languagedetector'

import en from './locales/en.json'
import th from './locales/th.json'

i18n
  .use(LanguageDetector)
  .use(initReactI18next)
  .init({
    resources: {
      en: { translation: en },
      th: { translation: th },
    },
    fallbackLng: 'en',
    interpolation: { escapeValue: false },
    detection: {
      order: ['localStorage', 'navigator'],
      caches: ['localStorage'],
    },
  })

export default i18n
```

```json
// src/i18n/locales/en.json
{
  "nav": { "home": "Home", "about": "About", "pricing": "Pricing" },
  "hero": {
    "badge": "New: AI analytics",
    "title": "Ship faster with {{highlight}}",
    "highlight": "beautiful workflows",
    "description": "Reduce setup time by 80% and deliver 3x faster",
    "cta_primary": "Get started free",
    "cta_secondary": "Watch demo"
  },
  "auth": {
    "email": "Email address",
    "password": "Password",
    "signin": "Sign in",
    "signup": "Create account",
    "no_account": "Don't have an account?",
    "already_account": "Already have an account?"
  },
  "validation": {
    "required": "{{field}} is required",
    "email_invalid": "Please enter a valid email",
    "min_length": "Must be at least {{min}} characters"
  }
}
```

```json
// src/i18n/locales/th.json
{
  "nav": { "home": "หน้าแรก", "about": "เกี่ยวกับ", "pricing": "ราคา" },
  "hero": {
    "badge": "ใหม่: Analytics ด้วย AI",
    "title": "Ship เร็วขึ้นด้วย {{highlight}}",
    "highlight": "workflow ที่สวยงาม",
    "description": "ลดเวลา setup ลง 80% และ deliver ได้เร็วขึ้น 3x",
    "cta_primary": "เริ่มใช้งานฟรี",
    "cta_secondary": "ดู demo"
  },
  "auth": {
    "email": "อีเมล",
    "password": "รหัสผ่าน",
    "signin": "เข้าสู่ระบบ",
    "signup": "สร้างบัญชี",
    "no_account": "ยังไม่มีบัญชี?",
    "already_account": "มีบัญชีอยู่แล้ว?"
  },
  "validation": {
    "required": "{{field}} จำเป็นต้องกรอก",
    "email_invalid": "กรุณากรอกอีเมลให้ถูกต้อง",
    "min_length": "ต้องมีอย่างน้อย {{min}} ตัวอักษร"
  }
}
```

---

## Step 852: Using useTranslation

```tsx
// src/components/Nav.tsx
import { useTranslation } from 'react-i18next'

export function Nav() {
  const { t } = useTranslation()
  return (
    <nav className="flex items-center gap-6">
      <a href="/" className="text-sm font-medium text-gray-700 hover:text-indigo-600">
        {t('nav.home')}
      </a>
      <a href="/about" className="text-sm font-medium text-gray-700 hover:text-indigo-600">
        {t('nav.about')}
      </a>
      <a href="/pricing" className="text-sm font-medium text-gray-700 hover:text-indigo-600">
        {t('nav.pricing')}
      </a>
    </nav>
  )
}

// src/components/Hero.tsx
export function Hero() {
  const { t } = useTranslation()
  return (
    <section className="text-center py-20">
      <div className="inline-flex items-center gap-2 px-3 py-1 bg-indigo-50 border border-indigo-200 rounded-full text-sm text-indigo-700 mb-5">
        {t('hero.badge')}
      </div>
      <h1 className="text-5xl font-extrabold text-gray-900 mb-4">
        {t('hero.title', { highlight: '' })}
        {' '}<span className="text-indigo-600">{t('hero.highlight')}</span>
      </h1>
      <p className="text-gray-500 text-lg mb-7">{t('hero.description')}</p>
      <div className="flex gap-3 justify-center">
        <button className="px-6 py-3 bg-indigo-600 text-white font-semibold rounded-2xl">
          {t('hero.cta_primary')}
        </button>
        <button className="px-6 py-3 border text-gray-700 font-semibold rounded-2xl">
          {t('hero.cta_secondary')}
        </button>
      </div>
    </section>
  )
}
```

---

## Step 853: Language Switcher

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Language Switcher</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 min-h-screen flex items-center justify-center">
<div class="bg-white rounded-2xl border p-8 w-80">
  <h2 class="font-extrabold text-gray-900 mb-6 text-center" id="title">เลือกภาษา</h2>
  <!-- Language picker -->
  <div class="space-y-2 mb-6">
    <button onclick="setLang('th')" class="lang-btn w-full flex items-center gap-3 px-4 py-3 rounded-xl border-2 border-indigo-500 bg-indigo-50 transition-all text-left" data-lang="th">
      <span class="text-xl">🇹🇭</span>
      <div><p class="font-semibold text-gray-900 text-sm">ภาษาไทย</p><p class="text-xs text-gray-400">Thai</p></div>
      <span class="ml-auto text-indigo-500">✓</span>
    </button>
    <button onclick="setLang('en')" class="lang-btn w-full flex items-center gap-3 px-4 py-3 rounded-xl border-2 border-gray-200 hover:border-gray-300 transition-all text-left" data-lang="en">
      <span class="text-xl">🇺🇸</span>
      <div><p class="font-semibold text-gray-900 text-sm">English</p><p class="text-xs text-gray-400">English (US)</p></div>
    </button>
    <button onclick="setLang('ja')" class="lang-btn w-full flex items-center gap-3 px-4 py-3 rounded-xl border-2 border-gray-200 hover:border-gray-300 transition-all text-left" data-lang="ja">
      <span class="text-xl">🇯🇵</span>
      <div><p class="font-semibold text-gray-900 text-sm">日本語</p><p class="text-xs text-gray-400">Japanese</p></div>
    </button>
    <button onclick="setLang('ar')" class="lang-btn w-full flex items-center gap-3 px-4 py-3 rounded-xl border-2 border-gray-200 hover:border-gray-300 transition-all text-left" data-lang="ar">
      <span class="text-xl">🇸🇦</span>
      <div><p class="font-semibold text-gray-900 text-sm">العربية</p><p class="text-xs text-gray-400">Arabic (RTL)</p></div>
    </button>
  </div>
  <p class="text-xs text-gray-400 text-center">ภาษาที่เลือกจะถูกบันทึก</p>
</div>
<script>
  const titles = { th:'เลือกภาษา', en:'Choose Language', ja:'言語を選択', ar:'اختر اللغة' };
  function setLang(lang) {
    document.querySelectorAll('.lang-btn').forEach(btn => {
      const active = btn.dataset.lang === lang;
      btn.classList.toggle('border-indigo-500', active);
      btn.classList.toggle('bg-indigo-50', active);
      btn.classList.toggle('border-gray-200', !active);
      const check = btn.querySelector('.ml-auto');
      if (check) check.remove();
      if (active) btn.innerHTML += '<span class="ml-auto text-indigo-500">✓</span>';
    });
    document.getElementById('title').textContent = titles[lang];
    document.documentElement.lang = lang;
    document.documentElement.dir = lang === 'ar' ? 'rtl' : 'ltr';
    localStorage.setItem('lang', lang);
  }
</script>
</body>
</html>
```

---

## Step 854: RTL Support

```html
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>RTL Support</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 min-h-screen p-6">
<div class="max-w-lg mx-auto space-y-4">
  <h1 class="text-xl font-extrabold text-gray-900">دعم RTL مع Tailwind</h1>
  <p class="text-gray-500 text-sm">Tailwind v3+ มี RTL support ด้วย dir="rtl" บน html</p>

  <!-- RTL card — flex ทำงานถูกต้องด้วย dir="rtl" -->
  <div class="bg-white rounded-2xl border p-5">
    <div class="flex items-center gap-3 mb-3">
      <div class="w-10 h-10 rounded-full bg-indigo-600 flex items-center justify-center text-white font-bold">م</div>
      <div>
        <p class="font-bold text-gray-900">محمد عبد الله</p>
        <p class="text-xs text-gray-400">admin@example.com</p>
      </div>
      <!-- Badge ย้ายไปอยู่ทางซ้ายใน RTL -->
      <span class="ms-auto px-2.5 py-0.5 bg-emerald-100 text-emerald-700 text-xs rounded-full font-semibold">نشط</span>
    </div>
    <div class="flex gap-2">
      <button class="px-4 py-2 bg-indigo-600 text-white text-sm rounded-xl">تعديل</button>
      <button class="px-4 py-2 bg-gray-100 text-gray-700 text-sm rounded-xl">إلغاء</button>
    </div>
  </div>

  <!-- Form in RTL -->
  <div class="bg-white rounded-2xl border p-5">
    <div class="relative mb-3">
      <!-- Icon ย้ายไปอยู่ขวา (= start) ใน RTL -->
      <span class="absolute end-3 top-1/2 -translate-y-1/2 text-gray-400">✉️</span>
      <input type="email" placeholder="البريد الإلكتروني" class="w-full pe-10 ps-3 py-2 border rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 text-right">
    </div>
    <button class="w-full py-2.5 bg-indigo-600 text-white text-sm font-semibold rounded-xl">تسجيل الدخول</button>
  </div>
</div>
</body>
</html>
```

---

## Step 855: Logical CSS Properties

```html
<!-- Tailwind v3 มี logical properties: ms-, me-, ps-, pe-, etc. -->
<!-- ms = margin-start, me = margin-end (LTR: left/right, RTL: right/left) -->

<!-- ❌ ใช้ ml/mr ตรงๆ — ต้องแก้ RTL -->
<div class="ml-4 pr-6">Content</div>

<!-- ✅ ใช้ ms/me/ps/pe — RTL ใช้งานได้เลย -->
<div class="ms-4 pe-6">Content</div>

<!-- Other logical properties -->
<div class="
  ps-4 pe-4         <!-- padding-inline-start + end -->
  ms-2 me-2         <!-- margin-inline-start + end -->
  border-s          <!-- border-inline-start (left in LTR, right in RTL) -->
  rounded-s-xl      <!-- border-radius inline-start corners -->
  start-0 end-0     <!-- inset-inline-start + end (for absolute/fixed) -->
  text-start        <!-- text-align: start -->
">
  Logical properties content
</div>
```

---

## Step 856: Date + Number Formatting

```ts
// src/utils/format.ts
export function formatDate(date: Date | string, locale: string): string {
  return new Intl.DateTimeFormat(locale, {
    year: 'numeric', month: 'long', day: 'numeric',
  }).format(new Date(date))
}

export function formatCurrency(amount: number, locale: string, currency: string = 'USD'): string {
  return new Intl.NumberFormat(locale, {
    style: 'currency', currency,
    minimumFractionDigits: 0,
    maximumFractionDigits: 2,
  }).format(amount)
}

export function formatNumber(n: number, locale: string): string {
  return new Intl.NumberFormat(locale).format(n)
}

export function formatRelativeTime(date: Date, locale: string): string {
  const diff = (date.getTime() - Date.now()) / 1000
  const rtf = new Intl.RelativeTimeFormat(locale, { numeric: 'auto' })
  if (Math.abs(diff) < 60) return rtf.format(Math.round(diff), 'second')
  if (Math.abs(diff) < 3600) return rtf.format(Math.round(diff / 60), 'minute')
  if (Math.abs(diff) < 86400) return rtf.format(Math.round(diff / 3600), 'hour')
  return rtf.format(Math.round(diff / 86400), 'day')
}
```

---

## Step 857: i18n with Namespaces

```ts
// src/i18n/index.ts — multiple namespaces
i18n.init({
  resources: {
    en: {
      common:  require('./locales/en/common.json'),
      auth:    require('./locales/en/auth.json'),
      dashboard: require('./locales/en/dashboard.json'),
      errors:  require('./locales/en/errors.json'),
    },
    th: {
      common:  require('./locales/th/common.json'),
      auth:    require('./locales/th/auth.json'),
      dashboard: require('./locales/th/dashboard.json'),
      errors:  require('./locales/th/errors.json'),
    },
  },
  defaultNS: 'common',
})
```

```tsx
// ใช้ namespace ใน component
import { useTranslation } from 'react-i18next'

function Dashboard() {
  const { t: tDash } = useTranslation('dashboard')
  const { t: tCommon } = useTranslation('common')
  return (
    <div>
      <h1>{tDash('title')}</h1>
      <button>{tCommon('save')}</button>
    </div>
  )
}

// หรือ multi-namespace
function Page() {
  const { t } = useTranslation(['common', 'auth'])
  return <p>{t('auth:signin')}</p>
}
```

---

## Step 858: Lazy Loading Translations

```ts
// src/i18n/index.ts — lazy load with i18next-http-backend
import Backend from 'i18next-http-backend'

i18n
  .use(Backend)
  .use(LanguageDetector)
  .use(initReactI18next)
  .init({
    fallbackLng: 'en',
    ns: ['common', 'auth', 'dashboard'],
    defaultNS: 'common',
    backend: {
      loadPath: '/locales/{{lng}}/{{ns}}.json',
    },
    interpolation: { escapeValue: false },
  })
```

```tsx
// Suspense wrapper สำหรับ translation loading
import { Suspense } from 'react'

function App() {
  return (
    <Suspense fallback={
      <div className="h-screen flex items-center justify-center">
        <div className="w-8 h-8 border-4 border-indigo-600 border-t-transparent rounded-full animate-spin"></div>
      </div>
    }>
      <Router />
    </Suspense>
  )
}
```

---

## Step 859: Pluralization + Interpolation

```json
// en.json
{
  "items_count": "{{count}} item",
  "items_count_plural": "{{count}} items",
  "welcome": "Welcome, {{name}}!",
  "last_seen": "Last seen {{time}} ago",
  "cart": {
    "empty": "Your cart is empty",
    "count": "$t(cart.item, {\"count\": {{count}}})",
    "item": "{{count}} item",
    "item_plural": "{{count}} items"
  }
}
```

```tsx
// Pluralization
const { t } = useTranslation()

// Automatically picks singular/plural
t('items_count', { count: 1 })  // → "1 item"
t('items_count', { count: 5 })  // → "5 items"

// Interpolation
t('welcome', { name: 'John' })  // → "Welcome, John!"

// Nested
t('cart.count', { count: 3 })   // → "3 items"
```

---

## Step 860: Workshop — Multilingual App

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>i18n Workshop</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 min-h-screen p-6">
<div class="max-w-lg mx-auto">
  <!-- Language switcher -->
  <div class="bg-white rounded-2xl border p-4 mb-4 flex gap-2">
    <button onclick="setLocale('th')" id="btn-th" class="flex-1 py-2 text-sm font-semibold rounded-xl bg-indigo-600 text-white transition-all">🇹🇭 TH</button>
    <button onclick="setLocale('en')" id="btn-en" class="flex-1 py-2 text-sm font-semibold rounded-xl bg-gray-100 text-gray-600 transition-all">🇺🇸 EN</button>
    <button onclick="setLocale('ar')" id="btn-ar" class="flex-1 py-2 text-sm font-semibold rounded-xl bg-gray-100 text-gray-600 transition-all">🇸🇦 AR</button>
  </div>

  <!-- Demo content -->
  <div id="content" class="space-y-4"></div>
</div>
<script>
  const translations = {
    th: {
      title: 'ข้อมูลบัญชี',
      name: 'ชื่อ-นามสกุล',
      email: 'อีเมล',
      joined: 'เข้าร่วมเมื่อ',
      balance: 'ยอดเงิน',
      save: 'บันทึก',
      cancel: 'ยกเลิก',
      active: 'ใช้งานอยู่',
    },
    en: {
      title: 'Account Details',
      name: 'Full Name',
      email: 'Email Address',
      joined: 'Joined',
      balance: 'Balance',
      save: 'Save Changes',
      cancel: 'Cancel',
      active: 'Active',
    },
    ar: {
      title: 'تفاصيل الحساب',
      name: 'الاسم الكامل',
      email: 'البريد الإلكتروني',
      joined: 'انضم في',
      balance: 'الرصيد',
      save: 'حفظ التغييرات',
      cancel: 'إلغاء',
      active: 'نشط',
    },
  };

  const dates = {
    th: new Intl.DateTimeFormat('th-TH', {year:'numeric',month:'long',day:'numeric'}).format(new Date('2024-01-15')),
    en: new Intl.DateTimeFormat('en-US', {year:'numeric',month:'long',day:'numeric'}).format(new Date('2024-01-15')),
    ar: new Intl.DateTimeFormat('ar-SA', {year:'numeric',month:'long',day:'numeric'}).format(new Date('2024-01-15')),
  };
  const currencies = {
    th: new Intl.NumberFormat('th-TH',{style:'currency',currency:'THB'}).format(48250),
    en: new Intl.NumberFormat('en-US',{style:'currency',currency:'USD'}).format(1390),
    ar: new Intl.NumberFormat('ar-SA',{style:'currency',currency:'SAR'}).format(5210),
  };

  let currentLocale = 'th';

  function render(locale) {
    const t = translations[locale];
    const isRtl = locale === 'ar';
    document.getElementById('content').innerHTML = `
      <div class="bg-white rounded-2xl border p-5" dir="${isRtl?'rtl':'ltr'}">
        <div class="flex items-center gap-3 mb-4">
          <div class="w-12 h-12 rounded-2xl bg-indigo-600 flex items-center justify-center text-white text-xl font-bold">JD</div>
          <div>
            <p class="font-extrabold text-gray-900">John Doe</p>
            <span class="px-2 py-0.5 text-xs rounded-full bg-emerald-100 text-emerald-700 font-semibold">${t.active}</span>
          </div>
        </div>
        <div class="space-y-3">
          <div class="flex justify-between text-sm"><span class="text-gray-500">${t.name}</span><span class="font-medium text-gray-900">John Doe</span></div>
          <div class="flex justify-between text-sm"><span class="text-gray-500">${t.email}</span><span class="font-medium text-gray-900">john@example.com</span></div>
          <div class="flex justify-between text-sm"><span class="text-gray-500">${t.joined}</span><span class="font-medium text-gray-900">${dates[locale]}</span></div>
          <div class="flex justify-between text-sm"><span class="text-gray-500">${t.balance}</span><span class="font-bold text-emerald-600">${currencies[locale]}</span></div>
        </div>
        <div class="flex gap-2 mt-4">
          <button class="flex-1 py-2.5 bg-indigo-600 text-white text-sm font-semibold rounded-xl">${t.save}</button>
          <button class="flex-1 py-2.5 border text-gray-700 text-sm font-semibold rounded-xl">${t.cancel}</button>
        </div>
      </div>`;
    document.documentElement.lang = locale;
    document.documentElement.dir = isRtl ? 'rtl' : 'ltr';
  }

  function setLocale(locale) {
    currentLocale = locale;
    ['th','en','ar'].forEach(l => {
      const btn = document.getElementById('btn-'+l);
      btn.classList.toggle('bg-indigo-600', l === locale);
      btn.classList.toggle('text-white', l === locale);
      btn.classList.toggle('bg-gray-100', l !== locale);
      btn.classList.toggle('text-gray-600', l !== locale);
    });
    render(locale);
  }

  render('th');
</script>
</body>
</html>
```

---

## สรุป Part 86

| Step | เนื้อหา |
|------|---------|
| 851 | react-i18next setup + locale JSON |
| 852 | useTranslation hook ใน components |
| 853 | Language switcher UI |
| 854 | RTL support ด้วย dir="rtl" |
| 855 | Logical CSS properties (ms/me/ps/pe) |
| 856 | Intl.DateTimeFormat + NumberFormat + RelativeTimeFormat |
| 857 | Namespaces สำหรับ large apps |
| 858 | Lazy loading translations + Suspense |
| 859 | Pluralization + interpolation |
| 860 | Workshop: Multilingual app — TH/EN/AR |

**Part ถัดไป:** Part 87 — Next.js App Router Integration (Steps 861–870)
