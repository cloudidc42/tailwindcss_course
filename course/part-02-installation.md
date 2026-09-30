# Part 02: การติดตั้งและตั้งค่า Tailwind CSS
## Steps 11–20: Setup สำหรับโปรเจกต์จริง

---

## 🎯 เป้าหมายของ Part นี้

- ติดตั้ง Tailwind CSS ด้วย npm
- ตั้งค่า `tailwind.config.js`
- เข้าใจ PostCSS และ build process
- ตั้งค่า VS Code สำหรับ Tailwind
- สร้าง development workflow ที่ดี

---

## Step 11: Prerequisites — สิ่งที่ต้องมีก่อน

### ติดตั้ง Node.js

```bash
# ตรวจสอบว่ามี Node.js หรือยัง
node --version   # ควรได้ v18+ หรือสูงกว่า
npm --version    # ควรได้ v8+
```

ถ้ายังไม่มี ดาวน์โหลดที่ https://nodejs.org/

### เครื่องมือที่แนะนำ

| เครื่องมือ | ทำไมต้องมี |
|-----------|-----------|
| VS Code | Editor ที่ดีที่สุดสำหรับ Tailwind |
| Tailwind CSS IntelliSense | Autocomplete + Preview |
| Prettier + Tailwind Plugin | จัดเรียง class อัตโนมัติ |
| Live Server Extension | Auto-reload |

---

## Step 12: ติดตั้ง Tailwind CSS แบบพื้นฐาน

### สร้างโปรเจกต์ใหม่

```bash
# สร้างโฟลเดอร์
mkdir my-tailwind-project
cd my-tailwind-project

# สร้าง package.json
npm init -y
```

### ติดตั้ง Tailwind

```bash
# ติดตั้ง tailwindcss และ dependencies
npm install -D tailwindcss postcss autoprefixer

# สร้าง config files
npx tailwindcss init -p
```

คำสั่งนี้จะสร้าง 2 ไฟล์:
- `tailwind.config.js`
- `postcss.config.js`

### โครงสร้างโปรเจกต์

```
my-tailwind-project/
├── node_modules/
├── src/
│   ├── input.css          ← CSS source file
│   └── index.html         ← HTML file
├── dist/
│   └── output.css         ← CSS ที่ build แล้ว (auto-generated)
├── tailwind.config.js
├── postcss.config.js
└── package.json
```

---

## Step 13: ตั้งค่า tailwind.config.js

```javascript
// tailwind.config.js
/** @type {import('tailwindcss').Config} */
module.exports = {
  // บอก Tailwind ให้ scan ไฟล์ไหนบ้าง
  content: [
    "./src/**/*.{html,js,jsx,ts,tsx}",
    "./index.html",
  ],
  
  theme: {
    extend: {
      // Custom config จะเพิ่มตรงนี้ (จะเรียนใน Part 21+)
    },
  },
  
  plugins: [],
}
```

### ความสำคัญของ `content`

```javascript
// ถ้า content ไม่ถูกตั้ง Tailwind จะไม่ generate CSS!
content: [
  "./src/**/*.html",      // HTML ทุกไฟล์ใน src/
  "./src/**/*.js",        // JS ทุกไฟล์ใน src/
  "./src/**/*.jsx",       // JSX (React)
  "./src/**/*.ts",        // TypeScript
  "./src/**/*.tsx",       // TSX (React + TypeScript)
  "./src/**/*.vue",       // Vue.js
  "./pages/**/*.{js,ts,jsx,tsx}",  // Next.js pages
  "./components/**/*.{js,ts,jsx,tsx}", // Components
]
```

> ⚠️ **สำคัญมาก**: ถ้า path ไม่ถูกต้อง class จะไม่ถูก generate!

---

## Step 14: สร้าง CSS Source File

```css
/* src/input.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
```

### ทั้งสาม directive ทำอะไร?

```
@tailwind base
  ├── CSS Reset (Preflight)
  ├── กำหนด box-sizing: border-box
  ├── ลบ default margin/padding
  └── ตั้งค่า base styles สำหรับ typography

@tailwind components
  ├── Component classes จาก plugins
  └── Custom component classes (@layer components)

@tailwind utilities
  ├── Utility classes ทั้งหมด (flex, p-4, text-red-500 ฯลฯ)
  └── Custom utility classes (@layer utilities)
```

---

## Step 15: Build CSS

### วิธีที่ 1: Build ครั้งเดียว

```bash
npx tailwindcss -i ./src/input.css -o ./dist/output.css
```

### วิธีที่ 2: Watch Mode (สำหรับ Development)

```bash
# Auto rebuild เมื่อไฟล์เปลี่ยน
npx tailwindcss -i ./src/input.css -o ./dist/output.css --watch
```

### วิธีที่ 3: เพิ่ม npm scripts

```json
// package.json
{
  "name": "my-tailwind-project",
  "scripts": {
    "build": "tailwindcss -i ./src/input.css -o ./dist/output.css",
    "dev": "tailwindcss -i ./src/input.css -o ./dist/output.css --watch",
    "build:prod": "NODE_ENV=production tailwindcss -i ./src/input.css -o ./dist/output.css --minify"
  },
  "devDependencies": {
    "autoprefixer": "^10.4.0",
    "postcss": "^8.4.0",
    "tailwindcss": "^3.0.0"
  }
}
```

```bash
npm run dev    # เปิด watch mode
npm run build  # build ครั้งเดียว
npm run build:prod  # build สำหรับ production (minify)
```

---

## Step 16: HTML File และการ Link CSS

```html
<!-- src/index.html -->
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>My Tailwind Project</title>
  
  <!-- Link ไปที่ output CSS -->
  <link rel="stylesheet" href="../dist/output.css">
</head>
<body class="bg-gray-50">
  
  <div class="container mx-auto px-4 py-8">
    <h1 class="text-4xl font-bold text-gray-900">
      Hello, Tailwind CSS! 🎨
    </h1>
    <p class="text-gray-600 mt-2">
      นี่คือโปรเจกต์แรกที่ติดตั้ง Tailwind CSS จริง
    </p>
  </div>

</body>
</html>
```

### การทดสอบ

```bash
# Terminal 1: เปิด watch mode
npm run dev

# Terminal 2: เปิด file ใน browser
# หรือใช้ Live Server extension ใน VS Code
```

---

## Step 17: ตั้งค่า VS Code

### Extension ที่ต้องติดตั้ง

**1. Tailwind CSS IntelliSense**
```
Publisher: Tailwind Labs
ID: bradlc.vscode-tailwindcss
```

**2. Prettier - Code formatter**
```
Publisher: Prettier
ID: esbenp.prettier-vscode
```

**3. PostCSS Language Support**
```
Publisher: csstools
ID: csstools.postcss
```

### settings.json สำหรับ Tailwind

```json
// .vscode/settings.json (สร้างในโปรเจกต์)
{
  // Autocomplete ใน class=""
  "editor.quickSuggestions": {
    "strings": "on"
  },
  
  // Format ด้วย Prettier
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  
  // CSS IntelliSense
  "css.validate": false,
  "scss.validate": false,
  
  // Tailwind class sorting
  "tailwindCSS.experimental.classRegex": [
    ["clsx\\(([^)]*)\\)", "(?:'|\"|`)([^']*)(?:'|\"|`)"],
    ["className\\s*=\\s*{([^}]*)}", "(?:'|\"|`)([^']*)(?:'|\"|`)"]
  ]
}
```

### .prettierrc สำหรับ Tailwind

```bash
# ติดตั้ง prettier และ plugin
npm install -D prettier prettier-plugin-tailwindcss
```

```json
// .prettierrc
{
  "semi": false,
  "singleQuote": true,
  "tabWidth": 2,
  "plugins": ["prettier-plugin-tailwindcss"],
  "tailwindConfig": "./tailwind.config.js"
}
```

---

## Step 18: Setup กับ Vite (วิธีแนะนำ 2024)

Vite เป็น build tool ที่เร็วที่สุด แนะนำสำหรับโปรเจกต์ใหม่ทั้งหมด

```bash
# สร้างโปรเจกต์ Vite
npm create vite@latest my-project -- --template vanilla

cd my-project
npm install

# ติดตั้ง Tailwind
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

### ตั้งค่า tailwind.config.js สำหรับ Vite

```javascript
// tailwind.config.js
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

### เพิ่ม Tailwind ใน CSS

```css
/* src/style.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
```

### Import CSS ใน main.js

```javascript
// src/main.js
import './style.css'

document.querySelector('#app').innerHTML = `
  <div class="min-h-screen bg-gray-50 flex items-center justify-center">
    <div class="text-center">
      <h1 class="text-4xl font-bold text-gray-900 mb-4">
        Vite + Tailwind CSS 🚀
      </h1>
      <p class="text-gray-600">
        เร็วสุดๆ ด้วย Vite HMR
      </p>
    </div>
  </div>
`
```

```bash
npm run dev   # เปิด dev server ที่ localhost:5173
```

---

## Step 19: ตั้งค่า Tailwind กับ React (Create React App)

```bash
# สร้าง React app ด้วย Vite (แนะนำ)
npm create vite@latest my-react-app -- --template react
cd my-react-app
npm install

# ติดตั้ง Tailwind
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

```javascript
// tailwind.config.js
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./src/**/*.{js,jsx,ts,tsx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

```css
/* src/index.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
```

```jsx
// src/App.jsx
function App() {
  return (
    <div className="min-h-screen bg-gray-100">
      <header className="bg-white shadow">
        <div className="max-w-7xl mx-auto py-6 px-4">
          <h1 className="text-3xl font-bold text-gray-900">
            React + Tailwind CSS
          </h1>
        </div>
      </header>
      
      <main className="max-w-7xl mx-auto py-6 px-4">
        <div className="bg-white rounded-xl shadow p-6">
          <p className="text-gray-600">
            React app ที่ใช้ Tailwind CSS สำหรับ styling
          </p>
        </div>
      </main>
    </div>
  )
}

export default App
```

> ⚠️ **สำเหตุ**: ใน React ใช้ `className` ไม่ใช่ `class`

---

## Step 20: ตรวจสอบและแก้ปัญหาที่พบบ่อย

### ปัญหา 1: Tailwind class ไม่ทำงาน

```bash
# ตรวจสอบ content path ใน config
cat tailwind.config.js

# ดู output CSS ว่า generate ถูกไหม
cat dist/output.css | grep "bg-blue-500"
# ถ้าไม่เจอ แปลว่า content path ผิด
```

```javascript
// แก้ไข tailwind.config.js
content: [
  "./src/**/*.{html,js,jsx,ts,tsx}",
  // เพิ่ม path ที่ขาดไป
]
```

### ปัญหา 2: Purge ลบ class ที่ Dynamic Generated

```javascript
// ❌ อย่าทำแบบนี้ — class จะถูก purge
const color = 'blue'
const cls = `bg-${color}-500`  // Tailwind ไม่เห็น class นี้

// ✅ ทำแบบนี้ — เขียน class เต็มๆ
const cls = color === 'blue' ? 'bg-blue-500' : 'bg-red-500'
```

### ปัญหา 3: CSS ไม่อัปเดต

```bash
# ลบ output file แล้ว rebuild
rm dist/output.css
npm run build
```

### ปัญหา 4: Multiple Files Compilation

```json
// package.json — compile หลายไฟล์พร้อมกัน
{
  "scripts": {
    "dev": "tailwindcss -i ./src/input.css -o ./dist/output.css --watch",
    "build": "tailwindcss -i ./src/input.css -o ./dist/output.css --minify"
  }
}
```

### Checklist ตรวจสอบ Setup

```
✅ node_modules/ ถูกสร้างแล้ว (npm install สำเร็จ)
✅ tailwind.config.js มี content path ถูกต้อง  
✅ src/input.css มี @tailwind directives
✅ HTML link ไปที่ dist/output.css
✅ npm run dev กำลัง watch อยู่
✅ VS Code Tailwind IntelliSense ทำงาน (เห็น autocomplete)
```

---

## 🛠️ Workshop: สร้าง Starter Template สมบูรณ์

```bash
mkdir tailwind-starter && cd tailwind-starter
npm init -y
npm install -D tailwindcss postcss autoprefixer prettier prettier-plugin-tailwindcss
npx tailwindcss init -p
mkdir -p src dist
```

```javascript
// tailwind.config.js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./src/**/*.{html,js}"],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

```css
/* src/input.css */
@tailwind base;
@tailwind components;
@tailwind utilities;
```

```html
<!-- src/index.html -->
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Tailwind Starter</title>
  <link rel="stylesheet" href="../dist/output.css">
</head>
<body class="bg-gray-50 min-h-screen">
  
  <!-- Navbar -->
  <nav class="bg-white border-b border-gray-200">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="flex justify-between h-16 items-center">
        <div class="text-xl font-bold text-indigo-600">MyApp</div>
        <div class="flex gap-6">
          <a href="#" class="text-gray-600 hover:text-gray-900 text-sm font-medium">หน้าแรก</a>
          <a href="#" class="text-gray-600 hover:text-gray-900 text-sm font-medium">เกี่ยวกับ</a>
          <a href="#" class="text-gray-600 hover:text-gray-900 text-sm font-medium">ติดต่อ</a>
        </div>
        <button class="bg-indigo-600 hover:bg-indigo-700 text-white text-sm font-medium px-4 py-2 rounded-lg transition-colors">
          เริ่มต้น
        </button>
      </div>
    </div>
  </nav>

  <!-- Hero -->
  <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-16 text-center">
    <h1 class="text-5xl font-bold text-gray-900 mb-4">
      ยินดีต้อนรับสู่ <span class="text-indigo-600">Tailwind</span>
    </h1>
    <p class="text-xl text-gray-500 mb-8 max-w-2xl mx-auto">
      เริ่มสร้างเว็บไซต์ที่สวยงามด้วย Tailwind CSS ได้เลยตอนนี้
    </p>
    <div class="flex justify-center gap-4">
      <button class="bg-indigo-600 hover:bg-indigo-700 text-white font-semibold px-8 py-3 rounded-xl transition-colors">
        เริ่มต้น →
      </button>
      <button class="border-2 border-gray-300 hover:border-indigo-300 text-gray-700 font-semibold px-8 py-3 rounded-xl transition-colors">
        เรียนรู้เพิ่มเติม
      </button>
    </div>
  </main>

</body>
</html>
```

```json
// package.json
{
  "scripts": {
    "dev": "tailwindcss -i ./src/input.css -o ./dist/output.css --watch",
    "build": "tailwindcss -i ./src/input.css -o ./dist/output.css --minify"
  }
}
```

```bash
npm run dev
# เปิด src/index.html ใน browser
```

---

## 📝 สรุป Part 02

| สิ่งที่เรียนรู้ | คำสั่งสำคัญ |
|----------------|------------|
| ติดตั้ง Tailwind | `npm install -D tailwindcss postcss autoprefixer` |
| สร้าง config | `npx tailwindcss init -p` |
| Build CSS | `npm run dev` / `npm run build` |
| VS Code setup | Tailwind IntelliSense + Prettier |
| React setup | Vite + Tailwind |

---

## 🏋️ Exercises

### Exercise 1
ติดตั้ง Tailwind CSS กับโปรเจกต์ HTML ธรรมดา และสร้างหน้าที่มี navbar + hero section

### Exercise 2
ติดตั้ง Tailwind CSS กับ Vite และสร้าง landing page อย่างง่าย

### Exercise 3
ตั้งค่า Prettier กับ Tailwind Plugin และทดสอบว่า class ถูกจัดเรียงอัตโนมัติหรือเปล่า

---

## 🔜 Part ถัดไป

**Part 03: Typography — ตัวอักษรและข้อความ**  
เรียนรู้ทุกอย่างเกี่ยวกับ font, text size, weight, color, alignment

---

*Part 02 — จาก 100 Parts | Steps 11–20 จาก 1,000 Steps*
