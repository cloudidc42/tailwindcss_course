# Part 14: Hover, Focus, Active States
## Steps 131–140: Interactive UI States

---

## 🎯 เป้าหมายของ Part นี้

- Pseudo-class modifiers: hover, focus, active, disabled
- Focus-visible สำหรับ accessibility
- Group modifier: hover parent → style child
- Peer modifier: sibling relationships
- State-based styling patterns

---

## Step 131: Hover State

```html
<!-- hover: prefix -->
<button class="bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded-lg">
  Hover me
</button>

<!-- hover ที่ทุก property ได้ -->
<div class="
  bg-white 
  hover:bg-gray-50 
  hover:shadow-lg 
  hover:scale-105 
  hover:-translate-y-1
  transition-all duration-300
  rounded-xl p-4 cursor-pointer
">
  Hover card
</div>

<!-- hover text color -->
<a class="text-gray-600 hover:text-indigo-600 transition-colors">Link text</a>

<!-- hover border -->
<input class="border border-gray-300 hover:border-indigo-400 rounded-lg px-3 py-2 outline-none">
```

---

## Step 132: Focus State

```html
<!-- focus: สำหรับ input, button, a -->
<input class="
  border border-gray-300 
  focus:border-indigo-500 
  focus:ring-2 
  focus:ring-indigo-200 
  focus:outline-none
  rounded-lg px-3 py-2
  transition-all
">

<!-- focus-within: เมื่อ child ของ element มี focus -->
<label class="
  block 
  border border-gray-300 
  focus-within:border-indigo-500 
  focus-within:ring-2 
  focus-within:ring-indigo-200
  rounded-lg p-3 transition-all
">
  <span class="text-xs text-gray-500 block mb-1">Email</span>
  <input type="email" class="w-full outline-none text-gray-900">
</label>

<!-- focus-visible: แสดง focus ring เฉพาะ keyboard navigation -->
<button class="
  bg-indigo-600 text-white px-4 py-2 rounded-lg
  focus:outline-none
  focus-visible:ring-2 
  focus-visible:ring-offset-2 
  focus-visible:ring-indigo-500
">
  Accessible Button
</button>
```

---

## Step 133: Active State

```html
<!-- active: เมื่อกำลัง press/click -->
<button class="
  bg-indigo-600 
  hover:bg-indigo-700 
  active:bg-indigo-800 
  active:scale-95
  text-white px-6 py-3 rounded-xl 
  transition-all
">
  Press me
</button>

<!-- active เปลี่ยน shadow -->
<button class="
  bg-white 
  shadow-md
  hover:shadow-lg
  active:shadow-sm
  active:translate-y-0.5
  px-4 py-2 rounded-xl 
  transition-all
">
  3D Button Effect
</button>

<!-- Link active state -->
<a href="#" class="
  text-indigo-600 
  hover:text-indigo-800 
  active:text-indigo-900
  underline-offset-2 
  hover:underline
">
  Active link
</a>
```

---

## Step 134: Disabled State

```html
<!-- disabled: สำหรับ form elements -->
<button 
  disabled
  class="
    bg-indigo-600 text-white px-4 py-2 rounded-lg
    disabled:bg-gray-300 
    disabled:text-gray-500 
    disabled:cursor-not-allowed
    disabled:pointer-events-none
  "
>
  Disabled Button
</button>

<!-- disabled input -->
<input 
  disabled
  value="Read only value"
  class="
    border border-gray-300 rounded-lg px-3 py-2
    disabled:bg-gray-100 
    disabled:text-gray-400 
    disabled:cursor-not-allowed
  "
>

<!-- ใช้ aria-disabled สำหรับ non-native disabled -->
<div 
  aria-disabled="true"
  class="
    bg-indigo-600 text-white px-4 py-2 rounded-lg
    aria-disabled:opacity-50
    aria-disabled:cursor-not-allowed
  "
>
  Aria Disabled
</div>
```

---

## Step 135: Checked, Selected States

```html
<!-- checked: สำหรับ checkbox, radio -->
<input 
  type="checkbox" 
  class="
    size-5 rounded
    checked:bg-indigo-600 
    checked:border-indigo-600
    border-2 border-gray-300
    accent-indigo-600
  "
>

<!-- Custom checkbox card -->
<label class="
  flex items-center gap-3 p-4 
  border-2 border-gray-200 
  has-[:checked]:border-indigo-500 
  has-[:checked]:bg-indigo-50
  rounded-xl cursor-pointer transition-all
">
  <input type="checkbox" class="sr-only">
  <div class="
    size-5 rounded border-2 border-gray-300 
    flex items-center justify-center
    has-[+*:checked]:bg-indigo-600 has-[+*:checked]:border-indigo-600
  "></div>
  <span class="text-gray-700">Option 1</span>
</label>

<!-- select option -->
<select class="
  border border-gray-300 
  focus:border-indigo-500 
  focus:ring-2 focus:ring-indigo-200 
  focus:outline-none
  rounded-lg px-3 py-2
">
  <option>Option 1</option>
  <option selected>Option 2 (selected)</option>
</select>
```

---

## Step 136: Group Modifier

**Group** ช่วยให้ hover/focus ที่ parent ส่งผลถึง child

```html
<!-- group + group-hover: -->
<div class="group bg-white hover:bg-indigo-600 rounded-xl p-6 cursor-pointer transition-all shadow hover:shadow-xl">
  <div class="size-12 bg-indigo-100 group-hover:bg-indigo-500 rounded-xl mb-4 flex items-center justify-center transition-colors">
    <span class="text-2xl">🎨</span>
  </div>
  <h3 class="font-bold text-gray-900 group-hover:text-white transition-colors">
    Card Title
  </h3>
  <p class="text-gray-500 group-hover:text-indigo-200 mt-1 text-sm transition-colors">
    Card description text here
  </p>
  <div class="mt-4 flex items-center gap-1 text-indigo-600 group-hover:text-white text-sm font-medium transition-colors">
    <span>Learn more</span>
    <span class="group-hover:translate-x-1 transition-transform">→</span>
  </div>
</div>

<!-- Named groups (Tailwind v3.2+) -->
<div class="group/item hover:bg-gray-100 rounded-lg p-3 flex items-center gap-3">
  <span class="font-medium">List Item</span>
  <button class="
    ml-auto opacity-0 
    group-hover/item:opacity-100 
    transition-opacity
    text-sm text-gray-500 hover:text-red-500
  ">
    Delete
  </button>
</div>
```

---

## Step 137: Peer Modifier

**Peer** ช่วยให้ element ที่อยู่หลัง sibling ปรับ style ตาม sibling's state

```html
<!-- peer + peer-focus: -->
<div class="relative">
  <input 
    type="text" 
    id="email"
    placeholder=" "
    class="peer w-full border border-gray-300 focus:border-indigo-500 rounded-lg px-3 pt-5 pb-2 focus:outline-none"
  >
  <label 
    for="email"
    class="
      absolute left-3 top-2 text-xs text-gray-400 pointer-events-none
      peer-placeholder-shown:text-base 
      peer-placeholder-shown:top-3.5
      peer-focus:text-xs 
      peer-focus:top-2
      peer-focus:text-indigo-600
      transition-all
    "
  >
    Email
  </label>
</div>

<!-- peer-invalid: แสดง error message -->
<div>
  <input 
    type="email" 
    required
    class="peer border border-gray-300 invalid:border-red-400 focus:outline-none rounded-lg px-3 py-2"
    placeholder="your@email.com"
  >
  <p class="text-red-500 text-sm mt-1 hidden peer-invalid:block">
    Please enter a valid email
  </p>
</div>

<!-- peer-checked: styled radio -->
<div class="flex gap-3">
  <label class="flex-1">
    <input type="radio" name="plan" value="free" class="peer sr-only">
    <div class="
      p-4 border-2 border-gray-200 rounded-xl cursor-pointer
      peer-checked:border-indigo-500 peer-checked:bg-indigo-50
      hover:border-gray-300 transition-all text-center
    ">
      <span class="font-bold">Free</span>
      <p class="text-sm text-gray-500 mt-1">฿0/เดือน</p>
    </div>
  </label>
  <label class="flex-1">
    <input type="radio" name="plan" value="pro" class="peer sr-only">
    <div class="
      p-4 border-2 border-gray-200 rounded-xl cursor-pointer
      peer-checked:border-indigo-500 peer-checked:bg-indigo-50
      hover:border-gray-300 transition-all text-center
    ">
      <span class="font-bold">Pro</span>
      <p class="text-sm text-gray-500 mt-1">฿299/เดือน</p>
    </div>
  </label>
</div>
```

---

## Step 138: Has Modifier (CSS :has())

```html
<!-- has-[selector]: parent มี child ที่ตรง selector -->
<form class="
  p-6 border-2 border-gray-200 rounded-xl
  has-[:invalid]:border-red-400
  has-[:focus]:border-indigo-500
  transition-colors
">
  <input type="email" required class="w-full outline-none" placeholder="email">
</form>

<!-- has-[:checked] card selection -->
<label class="
  block p-4 border-2 border-gray-200 rounded-xl cursor-pointer
  has-[:checked]:border-indigo-500 has-[:checked]:bg-indigo-50
  transition-all
">
  <input type="radio" name="option" class="sr-only">
  <h3 class="font-bold">Option Title</h3>
  <p class="text-sm text-gray-500 mt-1">Description</p>
</label>
```

---

## Step 139: Visited, Placeholder, Selection States

```html
<!-- visited: link ที่เคยเข้าแล้ว -->
<a href="#visited" class="
  text-blue-600 
  visited:text-purple-600
">
  Visit this link
</a>

<!-- placeholder: -->
<input 
  class="
    border border-gray-300 rounded-lg px-3 py-2
    placeholder:text-gray-400 
    placeholder:text-sm
    focus:outline-none focus:border-indigo-500
  "
  placeholder="Enter your name..."
>

<!-- placeholder-shown: ใช้กับ peer/group -->
<input 
  class="
    peer border rounded-lg px-3 py-2 outline-none
    focus:border-indigo-500
  " 
  placeholder=" "
>

<!-- selection: text ที่ถูก select/highlight -->
<p class="selection:bg-indigo-200 selection:text-indigo-800">
  ลองลาก select ข้อความนี้ดูสิ
</p>

<!-- file input styling -->
<input 
  type="file" 
  class="
    file:mr-3 file:py-2 file:px-4 
    file:rounded-lg file:border-0 
    file:text-sm file:font-medium
    file:bg-indigo-50 file:text-indigo-700
    hover:file:bg-indigo-100
    cursor-pointer
  "
>
```

---

## Step 140: Workshop — Interactive Form

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Interactive Form Workshop</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="min-h-screen bg-gradient-to-br from-slate-50 to-indigo-50 flex items-center justify-center p-4">

<div class="w-full max-w-md bg-white rounded-2xl shadow-xl overflow-hidden">
  <!-- Header -->
  <div class="bg-gradient-to-r from-indigo-600 to-purple-600 p-8 text-white">
    <h1 class="text-2xl font-bold">สร้างบัญชี</h1>
    <p class="text-indigo-200 text-sm mt-1">เริ่มต้นเรียน Tailwind CSS วันนี้</p>
  </div>
  
  <form class="p-8 space-y-5">
    <!-- Name field -->
    <div>
      <label class="block text-sm font-medium text-gray-700 mb-1.5">ชื่อ-นามสกุล</label>
      <input 
        type="text" 
        class="
          w-full border-2 border-gray-200 
          focus:border-indigo-500 focus:outline-none
          hover:border-gray-300
          rounded-xl px-4 py-3 
          text-gray-900 placeholder:text-gray-400
          transition-colors
        "
        placeholder="เช่น สมชาย ใจดี"
      >
    </div>
    
    <!-- Email field with validation -->
    <div>
      <label class="block text-sm font-medium text-gray-700 mb-1.5">อีเมล</label>
      <div class="relative">
        <input 
          type="email" 
          required
          class="
            peer w-full border-2 border-gray-200 
            focus:border-indigo-500 focus:outline-none
            invalid:border-red-400 invalid:focus:border-red-500
            hover:border-gray-300
            rounded-xl px-4 py-3 pr-10
            text-gray-900 placeholder:text-gray-400
            transition-colors
          "
          placeholder="your@email.com"
        >
        <div class="
          absolute right-3 top-1/2 -translate-y-1/2 size-5 rounded-full
          hidden peer-invalid:peer-not-placeholder-shown:flex items-center justify-center
          bg-red-100 text-red-500 text-xs font-bold
        ">!</div>
      </div>
      <p class="text-red-500 text-xs mt-1 hidden peer-invalid:block">
        กรุณากรอกอีเมลที่ถูกต้อง
      </p>
    </div>
    
    <!-- Password -->
    <div>
      <label class="block text-sm font-medium text-gray-700 mb-1.5">รหัสผ่าน</label>
      <input 
        type="password"
        class="
          w-full border-2 border-gray-200 
          focus:border-indigo-500 focus:outline-none
          hover:border-gray-300
          rounded-xl px-4 py-3 
          text-gray-900 placeholder:text-gray-400
          transition-colors
        "
        placeholder="อย่างน้อย 8 ตัวอักษร"
      >
    </div>
    
    <!-- Plan Selection -->
    <div>
      <label class="block text-sm font-medium text-gray-700 mb-2">เลือกแผน</label>
      <div class="grid grid-cols-2 gap-3">
        <label>
          <input type="radio" name="plan" value="free" class="peer sr-only" checked>
          <div class="
            p-3 border-2 border-gray-200 rounded-xl cursor-pointer text-center
            peer-checked:border-indigo-500 peer-checked:bg-indigo-50
            hover:border-gray-300 transition-all
          ">
            <div class="text-2xl mb-1">🆓</div>
            <div class="font-bold text-sm">ฟรี</div>
            <div class="text-xs text-gray-500">฿0/เดือน</div>
          </div>
        </label>
        <label>
          <input type="radio" name="plan" value="pro" class="peer sr-only">
          <div class="
            p-3 border-2 border-gray-200 rounded-xl cursor-pointer text-center
            peer-checked:border-indigo-500 peer-checked:bg-indigo-50
            hover:border-gray-300 transition-all
          ">
            <div class="text-2xl mb-1">⭐</div>
            <div class="font-bold text-sm">Pro</div>
            <div class="text-xs text-gray-500">฿299/เดือน</div>
          </div>
        </label>
      </div>
    </div>
    
    <!-- Terms checkbox -->
    <label class="
      flex items-start gap-3 p-3 
      border border-gray-200 
      has-[:checked]:border-indigo-300 
      has-[:checked]:bg-indigo-50
      rounded-xl cursor-pointer transition-all group
    ">
      <div class="
        flex-none mt-0.5 size-5 rounded border-2 
        border-gray-300 
        group-has-[:checked]:bg-indigo-600 
        group-has-[:checked]:border-indigo-600
        flex items-center justify-center transition-all
      ">
        <span class="text-white text-xs hidden group-has-[:checked]:block">✓</span>
      </div>
      <input type="checkbox" class="sr-only">
      <span class="text-sm text-gray-600">
        ฉันยอมรับ <a href="#" class="text-indigo-600 hover:text-indigo-800 underline">ข้อกำหนดการใช้งาน</a> และ <a href="#" class="text-indigo-600 hover:text-indigo-800 underline">นโยบายความเป็นส่วนตัว</a>
      </span>
    </label>
    
    <!-- Submit Button -->
    <button 
      type="submit"
      class="
        w-full bg-indigo-600 
        hover:bg-indigo-700 
        active:bg-indigo-800 active:scale-[0.98]
        focus:outline-none focus-visible:ring-2 focus-visible:ring-indigo-500 focus-visible:ring-offset-2
        disabled:bg-gray-300 disabled:cursor-not-allowed
        text-white font-semibold px-6 py-3.5 rounded-xl 
        transition-all
        flex items-center justify-center gap-2
      "
    >
      <span>สร้างบัญชี</span>
      <span>→</span>
    </button>
    
    <p class="text-center text-sm text-gray-500">
      มีบัญชีแล้ว? <a href="#" class="text-indigo-600 hover:text-indigo-800 font-medium">เข้าสู่ระบบ</a>
    </p>
  </form>
</div>

</body>
</html>
```

---

## 📝 สรุป Part 14

| Modifier | เมื่อใช้ | ตัวอย่าง |
|----------|---------|---------|
| `hover:` | เมื่อ mouse hover | `hover:bg-blue-600` |
| `focus:` | เมื่อ focused | `focus:ring-2` |
| `focus-visible:` | keyboard focus เท่านั้น | `focus-visible:ring-2` |
| `active:` | ขณะ press/click | `active:scale-95` |
| `disabled:` | ปุ่ม/input ที่ disabled | `disabled:opacity-50` |
| `group-hover:` | เมื่อ parent `.group` hover | `group-hover:text-white` |
| `peer-focus:` | เมื่อ sibling `.peer` focus | `peer-focus:top-2` |
| `has-[:checked]:` | เมื่อมี child ที่ checked | `has-[:checked]:bg-blue-50` |

---

*Part 14 — จาก 100 Parts | Steps 131–140 จาก 1,000 Steps*
