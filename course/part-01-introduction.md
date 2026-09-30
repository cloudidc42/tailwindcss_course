# Part 01: แนะนำ Tailwind CSS และ Utility-First CSS
## Steps 1–10: รู้จัก Tailwind CSS ตั้งแต่ต้น

---

## 🎯 เป้าหมายของ Part นี้

- เข้าใจว่า Tailwind CSS คืออะไร
- เข้าใจแนวคิด Utility-First CSS
- เปรียบเทียบ Tailwind กับ CSS แบบดั้งเดิม
- เปรียบเทียบกับ Bootstrap และ Framework อื่นๆ
- เข้าใจ workflow การทำงานกับ Tailwind

---

## Step 1: Tailwind CSS คืออะไร?

Tailwind CSS คือ **Utility-First CSS Framework** ที่แตกต่างจาก Framework อื่นๆ อย่างสิ้นเชิง

### แนวคิดหลัก

แทนที่จะมี component สำเร็จรูป เช่น `.btn`, `.card`, `.navbar` แบบ Bootstrap  
Tailwind ให้ **utility class** เล็กๆ จำนวนมาก ที่แต่ละ class ทำหน้าที่เดียว

```html
<!-- Bootstrap style -->
<button class="btn btn-primary btn-lg">Click Me</button>

<!-- Tailwind style -->
<button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
  Click Me
</button>
```

### ทำไมถึงเป็นแบบนี้?

| Bootstrap / Traditional | Tailwind CSS |
|------------------------|--------------|
| มี component สำเร็จรูป | ไม่มี component สำเร็จรูป |
| Override CSS เพื่อ custom | ประกอบ class เพื่อ custom |
| CSS เติบโตใหญ่ขึ้นเรื่อยๆ | CSS ขนาดเล็กสม่ำเสมอ |
| ต้องตั้งชื่อ class เสมอ | ไม่ต้องคิดชื่อ class |
| Design อาจดูเหมือนกัน | Design ยืดหยุ่นสูง |

---

## Step 2: ทำไม Tailwind ถึงได้รับความนิยม?

### เหตุผลที่ 1: ไม่ต้องตั้งชื่อ CSS Class

ปัญหาใหญ่ที่สุดในการเขียน CSS คือการตั้งชื่อ class  
เช่น `.sidebar-wrapper`, `.main-nav-item-active`, `.card-header-title`

Tailwind แก้ปัญหานี้ด้วยการไม่ต้องตั้งชื่อเลย

```html
<!-- ไม่ต้องคิดชื่อ class อีกต่อไป -->
<div class="flex items-center justify-between p-4 bg-white shadow rounded-lg">
  <h2 class="text-xl font-semibold text-gray-800">หัวข้อ</h2>
  <button class="text-sm text-blue-500 hover:text-blue-700">ดูเพิ่มเติม</button>
</div>
```

### เหตุผลที่ 2: ไม่ต้อง Switch Context

เขียน HTML และ Styling พร้อมกันในที่เดียว  
ไม่ต้องสลับไปมาระหว่างไฟล์ `.html` และ `.css`

### เหตุผลที่ 3: CSS ไม่โต

ด้วย Tailwind คุณใช้ class เดิมซ้ำๆ ทั่วทั้งโปรเจกต์  
CSS file ของคุณจึงมีขนาดเล็กและสม่ำเสมอ ไม่ว่าโปรเจกต์จะใหญ่แค่ไหน

```
โปรเจกต์เล็ก:  CSS ~3KB
โปรเจกต์ใหญ่: CSS ~3KB (เหมือนเดิม!)
```

### เหตุผลที่ 4: Design ที่สม่ำเสมอ

Tailwind มี design system ในตัว  
สีทุกสี spacing ทุกขนาด font ทุก size ล้วนมาจากระบบเดียวกัน

```html
<!-- ทุกอย่างมาจาก design system เดียวกัน -->
<div class="p-4 m-2 text-sm bg-gray-100">...</div>
<!-- p-4 = 1rem, m-2 = 0.5rem, text-sm = 0.875rem -->
```

---

## Step 3: เปรียบเทียบ CSS แบบดั้งเดิม vs Tailwind

### สถานการณ์: สร้าง Button

#### CSS แบบดั้งเดิม

```css
/* styles.css */
.btn {
  display: inline-flex;
  align-items: center;
  padding: 0.5rem 1rem;
  background-color: #3b82f6;
  color: white;
  font-weight: 600;
  font-size: 0.875rem;
  border-radius: 0.375rem;
  border: none;
  cursor: pointer;
  transition: background-color 0.2s;
}

.btn:hover {
  background-color: #2563eb;
}

.btn:focus {
  outline: 2px solid #3b82f6;
  outline-offset: 2px;
}

.btn-large {
  padding: 0.75rem 1.5rem;
  font-size: 1rem;
}
```

```html
<!-- index.html -->
<button class="btn btn-large">Click Me</button>
```

#### Tailwind CSS

```html
<!-- ทุกอย่างอยู่ที่เดียว ไม่ต้องเปิด CSS file -->
<button class="
  inline-flex items-center 
  px-4 py-2 
  bg-blue-500 hover:bg-blue-600 
  text-white font-semibold text-sm 
  rounded-md 
  transition-colors duration-200
  focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2
  cursor-pointer
">
  Click Me
</button>
```

### เปรียบเทียบทั้งสองแบบ

| หัวข้อ | CSS ดั้งเดิม | Tailwind |
|--------|-------------|---------|
| ไฟล์ที่ต้องเปิด | 2 ไฟล์ (HTML + CSS) | 1 ไฟล์ (HTML) |
| ความยาว | เท่ากัน | เท่ากัน |
| ความยืดหยุ่น | ต้อง override | เพิ่ม/ลด class |
| Reuse | ต้องตั้งชื่อ class | ใช้ซ้ำได้ทันที |
| Maintenance | ต้อง sync 2 ไฟล์ | ดู HTML เดียวพอ |

---

## Step 4: Utility-First ต่างกับ Component-Based อย่างไร?

### Component-Based (Bootstrap)

```html
<!-- Bootstrap: สำเร็จรูป แต่ตายตัว -->
<div class="card">
  <div class="card-header">
    Featured
  </div>
  <div class="card-body">
    <h5 class="card-title">Special title treatment</h5>
    <p class="card-text">With supporting text below as a natural lead-in to additional content.</p>
    <a href="#" class="btn btn-primary">Go somewhere</a>
  </div>
</div>
```

### Utility-First (Tailwind)

```html
<!-- Tailwind: ยืดหยุ่น สร้างเองได้ทุกอย่าง -->
<div class="bg-white rounded-xl shadow-md overflow-hidden">
  <div class="bg-blue-500 px-4 py-2">
    <span class="text-white text-sm font-medium">Featured</span>
  </div>
  <div class="p-6">
    <h5 class="text-lg font-bold text-gray-900 mb-2">Special title treatment</h5>
    <p class="text-gray-600 text-sm mb-4">
      With supporting text below as a natural lead-in to additional content.
    </p>
    <a href="#" class="inline-block bg-blue-500 hover:bg-blue-600 text-white text-sm font-medium px-4 py-2 rounded-lg transition-colors">
      Go somewhere
    </a>
  </div>
</div>
```

**ข้อดี Utility-First:**
- ออกแบบได้อิสระ 100% — ไม่ถูกจำกัดด้วย component
- ไม่ต้อง override CSS ที่ไม่ต้องการ
- ทุกคนในทีมทำงานบน design system เดียวกัน

---

## Step 5: Tailwind CSS เวอร์ชันต่างๆ

### v1.x (2019)
- เวอร์ชันแรก
- ยังต้องตั้งค่ามาก
- PurgeCSS แยกต่างหาก

### v2.x (2020)
- JIT Mode เพิ่มเข้ามา (ยังเป็น opt-in)
- Dark Mode support
- Extended color palette

### v3.x (2021 – ปัจจุบัน)
- JIT Mode เป็น default
- Arbitrary values `[...]`
- CDN ที่ง่ายขึ้น
- Performance ดีขึ้นมาก

```html
<!-- v3 arbitrary values — ใส่ค่าอะไรก็ได้ -->
<div class="w-[347px] bg-[#1a1a2e] text-[13px]">
  ค่าที่ไม่อยู่ใน config ก็ใช้ได้
</div>
```

### v4.x (2024)
- ไม่ต้องมี config file (ใช้ CSS ล้วน)
- เร็วขึ้น 5x จาก Rust-based engine
- Native cascade layers
- Container queries built-in

```css
/* v4 — config อยู่ใน CSS โดยตรง */
@import "tailwindcss";

@theme {
  --color-primary: #3b82f6;
  --font-body: "Inter", sans-serif;
}
```

---

## Step 6: โครงสร้างการทำงานของ Tailwind

```
┌─────────────────────────────────────────┐
│           การทำงานของ Tailwind           │
│                                         │
│  HTML Files        Tailwind Config      │
│  ┌──────────┐      ┌──────────────┐     │
│  │ class=   │      │ tailwind.    │     │
│  │ "flex    │ ──▶  │ config.js    │     │
│  │  p-4     │      │              │     │
│  │  bg-blue"│      └──────────────┘     │
│  └──────────┘              │            │
│                            ▼            │
│                    ┌──────────────┐     │
│                    │  Tailwind    │     │
│                    │  Compiler    │     │
│                    └──────────────┘     │
│                            │            │
│                            ▼            │
│                    ┌──────────────┐     │
│                    │  output.css  │     │
│                    │  (small!)    │     │
│                    └──────────────┘     │
└─────────────────────────────────────────┘
```

### กระบวนการทำงาน

1. **Scan**: Tailwind สแกนไฟล์ HTML, JS, JSX ทั้งหมด
2. **Detect**: หา class ที่ถูกใช้งานจริงทั้งหมด
3. **Generate**: สร้าง CSS เฉพาะ class ที่ถูกใช้เท่านั้น
4. **Output**: ได้ CSS ไฟล์ขนาดเล็ก

```bash
# สมมติมี class เหล่านี้ในโปรเจกต์
bg-blue-500    → .bg-blue-500 { background-color: #3b82f6; }
text-white     → .text-white { color: #ffffff; }
p-4            → .p-4 { padding: 1rem; }
hover:bg-blue-600  → .hover\:bg-blue-600:hover { background-color: #2563eb; }
```

---

## Step 7: ติดตั้ง Tailwind แบบ CDN (ทดสอบด่วน)

วิธีเร็วที่สุดในการลองใช้ Tailwind — ไม่ต้องติดตั้งอะไรเลย

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Tailwind CSS - Hello World</title>
  <!-- เพิ่มแค่บรรทัดเดียว! -->
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 min-h-screen flex items-center justify-center">
  
  <div class="bg-white rounded-2xl shadow-lg p-8 max-w-md w-full">
    
    <h1 class="text-3xl font-bold text-gray-900 mb-2">
      สวัสดี Tailwind CSS! 👋
    </h1>
    
    <p class="text-gray-500 mb-6">
      นี่คือประโยคแรกที่เขียนด้วย Tailwind CSS
    </p>
    
    <button class="w-full bg-blue-500 hover:bg-blue-600 text-white font-semibold py-3 px-6 rounded-xl transition-colors duration-200">
      เริ่มต้นเลย!
    </button>
    
  </div>

</body>
</html>
```

**ลองเปิดไฟล์นี้ใน Browser** — คุณจะเห็น Card สวยๆ ทันที!

> ⚠️ **หมายเหตุ**: CDN ใช้สำหรับทดสอบเท่านั้น  
> สำหรับโปรเจกต์จริงให้ใช้การติดตั้งแบบ npm (จะสอนใน Part 02)

---

## Step 8: คลาสพื้นฐานที่ควรรู้จักก่อน

Tailwind class มีรูปแบบที่สม่ำเสมอ — เมื่อเข้าใจ pattern แล้วจะเดาได้เอง

### Pattern: `[property]-[value]`

```html
<!-- Text size -->
<p class="text-sm">เล็ก</p>      <!-- font-size: 0.875rem -->
<p class="text-base">ปกติ</p>    <!-- font-size: 1rem -->
<p class="text-lg">ใหญ่</p>      <!-- font-size: 1.125rem -->
<p class="text-xl">ใหญ่มาก</p>  <!-- font-size: 1.25rem -->
<p class="text-2xl">2x</p>       <!-- font-size: 1.5rem -->

<!-- Padding -->
<div class="p-2">padding 0.5rem ทุกด้าน</div>
<div class="px-4">padding 1rem ซ้าย-ขวา</div>
<div class="py-3">padding 0.75rem บน-ล่าง</div>
<div class="pt-2 pb-4">top 0.5rem, bottom 1rem</div>

<!-- Colors -->
<div class="bg-red-500">พื้นหลังแดง</div>
<div class="bg-green-300">พื้นหลังเขียวอ่อน</div>
<div class="text-blue-700">ข้อความน้ำเงินเข้ม</div>
```

### Pattern: `[breakpoint]:[class]`

```html
<!-- Responsive — เปลี่ยนสไตล์ตาม screen size -->
<div class="text-sm md:text-base lg:text-lg">
  จอเล็ก: sm, จอกลาง: base, จอใหญ่: lg
</div>
```

### Pattern: `[variant]:[class]`

```html
<!-- State variants -->
<button class="bg-blue-500 hover:bg-blue-700 focus:ring-2 active:scale-95">
  Hover, Focus, Active
</button>
```

---

## Step 9: Tailwind กับ Developer Experience

### VS Code Extension

ติดตั้ง **Tailwind CSS IntelliSense** — จำเป็นมาก!

```
Extensions → ค้นหา "Tailwind CSS IntelliSense" → Install
```

**ฟีเจอร์ที่ได้:**
- Autocomplete class names
- Preview สีเมื่อ hover
- Lint ตรวจ class ผิด
- Hover เพื่อดู CSS จริงที่ generate

```html
<!-- พิมพ์ "bg-b" แล้ว IntelliSense จะแสดง -->
<!-- bg-black, bg-blue-50 ~ bg-blue-950, ... -->
<div class="bg-blue-500">
```

### Prettier Plugin

จัดเรียง Tailwind class อัตโนมัติตาม standard order

```bash
npm install -D prettier prettier-plugin-tailwindcss
```

```json
// .prettierrc
{
  "plugins": ["prettier-plugin-tailwindcss"]
}
```

```html
<!-- ก่อน prettier -->
<div class="shadow-lg text-white p-4 flex bg-blue-500 rounded items-center">

<!-- หลัง prettier — จัดเรียงตาม Tailwind's recommended order -->
<div class="flex items-center rounded bg-blue-500 p-4 text-white shadow-lg">
```

---

## Step 10: Workshop แรก — สร้าง Profile Card

มาลองเขียน Tailwind CSS จริงๆ กัน!

### เป้าหมาย

สร้าง Profile Card ที่ประกอบด้วย:
- รูปโปรไฟล์ (circle)
- ชื่อและตำแหน่ง
- Social links
- Follow button

### โค้ดสมบูรณ์

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Profile Card</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="min-h-screen bg-gradient-to-br from-purple-50 to-blue-50 flex items-center justify-center p-4">

  <!-- Profile Card -->
  <div class="bg-white rounded-2xl shadow-xl p-8 max-w-sm w-full text-center">
    
    <!-- Avatar -->
    <div class="relative inline-block mb-4">
      <img 
        src="https://api.dicebear.com/7.x/avataaars/svg?seed=Felix"
        alt="Profile Avatar"
        class="w-24 h-24 rounded-full border-4 border-purple-100 mx-auto"
      >
      <!-- Online indicator -->
      <div class="absolute bottom-1 right-1 w-5 h-5 bg-green-400 rounded-full border-2 border-white"></div>
    </div>

    <!-- Name & Role -->
    <h2 class="text-2xl font-bold text-gray-900 mb-1">สมชาย ใจดี</h2>
    <p class="text-purple-500 font-medium text-sm mb-1">Senior Frontend Developer</p>
    <p class="text-gray-400 text-sm mb-6">Bangkok, Thailand 🇹🇭</p>

    <!-- Stats -->
    <div class="flex justify-center gap-6 mb-6 py-4 border-y border-gray-100">
      <div>
        <div class="text-xl font-bold text-gray-900">128</div>
        <div class="text-xs text-gray-400">Projects</div>
      </div>
      <div class="w-px bg-gray-200"></div>
      <div>
        <div class="text-xl font-bold text-gray-900">4.2k</div>
        <div class="text-xs text-gray-400">Followers</div>
      </div>
      <div class="w-px bg-gray-200"></div>
      <div>
        <div class="text-xl font-bold text-gray-900">891</div>
        <div class="text-xs text-gray-400">Following</div>
      </div>
    </div>

    <!-- Bio -->
    <p class="text-gray-500 text-sm mb-6 leading-relaxed">
      ชอบเขียน React และ Tailwind CSS พัฒนา UI/UX ที่สวยงามและใช้งานง่าย ☕
    </p>

    <!-- Action Buttons -->
    <div class="flex gap-3">
      <button class="flex-1 bg-purple-500 hover:bg-purple-600 text-white font-semibold py-2.5 rounded-xl transition-colors duration-200 text-sm">
        Follow
      </button>
      <button class="flex-1 border-2 border-gray-200 hover:border-purple-300 hover:text-purple-500 text-gray-600 font-semibold py-2.5 rounded-xl transition-colors duration-200 text-sm">
        Message
      </button>
    </div>

    <!-- Social Links -->
    <div class="flex justify-center gap-4 mt-6">
      <a href="#" class="text-gray-400 hover:text-blue-400 transition-colors text-sm">
        Twitter
      </a>
      <a href="#" class="text-gray-400 hover:text-blue-700 transition-colors text-sm">
        LinkedIn
      </a>
      <a href="#" class="text-gray-400 hover:text-gray-900 transition-colors text-sm">
        GitHub
      </a>
    </div>

  </div>

</body>
</html>
```

### วิเคราะห์ class ที่ใช้

```
Layout:
  min-h-screen      — height ขั้นต่ำเต็มจอ
  flex items-center justify-center — จัดกลางทั้งแนวตั้งและแนวนอน
  p-4               — padding รอบนอก

Card:
  bg-white          — พื้นหลังขาว
  rounded-2xl       — มุมโค้งมาก
  shadow-xl         — เงาใหญ่
  p-8               — padding ข้างใน
  max-w-sm          — ความกว้างสูงสุด
  w-full            — เต็มความกว้าง
  text-center       — จัดกลาง

Avatar:
  w-24 h-24         — ขนาด 96px x 96px
  rounded-full      — วงกลม
  border-4          — ขอบหนา 4px

Buttons:
  flex-1            — แบ่งพื้นที่เท่าๆ กัน
  transition-colors — animation สี
  duration-200      — เวลา 200ms
```

---

## 📝 สรุป Part 01

| สิ่งที่เรียนรู้ | ความสำคัญ |
|----------------|-----------|
| Tailwind คือ Utility-First CSS | ⭐⭐⭐⭐⭐ |
| ทำไมถึงดีกว่า CSS ดั้งเดิม | ⭐⭐⭐⭐ |
| pattern ของ class `[property]-[value]` | ⭐⭐⭐⭐⭐ |
| CDN สำหรับทดสอบเร็ว | ⭐⭐⭐ |
| Responsive และ State variants | ⭐⭐⭐⭐ |

---

## 🏋️ Exercises

### Exercise 1 (ง่าย)
สร้าง Card ที่มี: หัวข้อ, รูปภาพ, ข้อความ, และปุ่ม Read More  
ใช้ CDN ในการทดสอบ

### Exercise 2 (กลาง)
สร้าง Notification Card 3 แบบ: Success (เขียว), Warning (เหลือง), Error (แดง)  
แต่ละอันมี icon emoji, หัวข้อ, และข้อความ

### Exercise 3 (ท้าทาย)
สร้าง Login Form ที่มี:
- Email input
- Password input
- Remember me checkbox
- Login button
- Forgot password link
- Sign up link

---

## 🔜 Part ถัดไป

**Part 02: การติดตั้งและตั้งค่า Tailwind CSS**  
เรียนรู้การติดตั้งจริงด้วย npm, การตั้งค่า config file, และ workflow การพัฒนา

---

*Part 01 — จาก 100 Parts | Steps 1–10 จาก 1,000 Steps*
