# Part 16: Forms และ Input Styling
## Steps 151–160: Form Design

---

## 🎯 เป้าหมายของ Part นี้

- Styling inputs, textareas, selects
- Form layout patterns
- Validation states
- Custom checkboxes & radios
- File upload, range slider, toggle switch

---

## Step 151: Text Input Styling

```html
<!-- Base input -->
<input 
  type="text"
  class="
    w-full px-4 py-3 rounded-xl
    border border-gray-300
    text-gray-900 text-sm
    placeholder:text-gray-400
    focus:outline-none 
    focus:border-indigo-500 
    focus:ring-2 focus:ring-indigo-200
    transition-all
  "
  placeholder="กรอกข้อความ..."
>

<!-- Input with icon left -->
<div class="relative">
  <div class="absolute left-3 top-1/2 -translate-y-1/2 text-gray-400">🔍</div>
  <input 
    type="text"
    class="
      w-full pl-10 pr-4 py-3 rounded-xl
      border border-gray-300
      focus:outline-none focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200
      transition-all
    "
    placeholder="ค้นหา..."
  >
</div>

<!-- Input with icon right (password toggle) -->
<div class="relative">
  <input 
    type="password"
    class="
      w-full px-4 pr-12 py-3 rounded-xl
      border border-gray-300
      focus:outline-none focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200
    "
    placeholder="รหัสผ่าน"
  >
  <button class="absolute right-3 top-1/2 -translate-y-1/2 text-gray-400 hover:text-gray-600 text-sm">
    👁
  </button>
</div>

<!-- Input with prefix text -->
<div class="flex">
  <span class="
    flex items-center px-3 
    bg-gray-100 border border-r-0 border-gray-300 
    rounded-l-xl text-sm text-gray-500
  ">
    https://
  </span>
  <input 
    type="text" 
    class="
      flex-1 px-4 py-3 rounded-r-xl
      border border-gray-300
      focus:outline-none focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200
    "
    placeholder="yourdomain.com"
  >
</div>
```

---

## Step 152: Floating Label Input

```html
<!-- Floating label pattern -->
<div class="relative mt-4">
  <input 
    id="floating-name"
    type="text" 
    placeholder=" "
    class="
      peer w-full px-4 pt-5 pb-2 
      border border-gray-300 rounded-xl
      text-gray-900 focus:outline-none
      focus:border-indigo-500 transition-colors
    "
  >
  <label 
    for="floating-name"
    class="
      absolute left-4 
      text-gray-400 pointer-events-none
      transition-all duration-200
      top-3.5 text-base 
      peer-focus:top-1.5 peer-focus:text-xs peer-focus:text-indigo-500
      peer-not-placeholder-shown:top-1.5 peer-not-placeholder-shown:text-xs
    "
  >
    ชื่อ-นามสกุล
  </label>
</div>

<!-- Complete form with floating labels -->
<form class="space-y-6 p-6 max-w-md">
  <!-- First name -->
  <div class="relative mt-2">
    <input id="f-firstname" type="text" placeholder=" "
      class="peer w-full px-4 pt-6 pb-2 border-2 border-gray-200 rounded-xl text-gray-900 focus:outline-none focus:border-indigo-500 transition-colors">
    <label for="f-firstname"
      class="absolute left-4 text-gray-400 pointer-events-none transition-all top-4 text-base peer-focus:top-2 peer-focus:text-xs peer-focus:text-indigo-500 peer-not-placeholder-shown:top-2 peer-not-placeholder-shown:text-xs">
      ชื่อ
    </label>
  </div>
  
  <!-- Last name -->
  <div class="relative mt-2">
    <input id="f-lastname" type="text" placeholder=" "
      class="peer w-full px-4 pt-6 pb-2 border-2 border-gray-200 rounded-xl text-gray-900 focus:outline-none focus:border-indigo-500 transition-colors">
    <label for="f-lastname"
      class="absolute left-4 text-gray-400 pointer-events-none transition-all top-4 text-base peer-focus:top-2 peer-focus:text-xs peer-focus:text-indigo-500 peer-not-placeholder-shown:top-2 peer-not-placeholder-shown:text-xs">
      นามสกุล
    </label>
  </div>
</form>
```

---

## Step 153: Textarea

```html
<!-- Basic textarea -->
<textarea 
  rows="4"
  class="
    w-full px-4 py-3 rounded-xl
    border border-gray-300
    text-gray-900 text-sm
    placeholder:text-gray-400
    focus:outline-none focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200
    resize-y min-h-[100px] max-h-[300px]
    transition-all
  "
  placeholder="เขียนข้อความ..."
></textarea>

<!-- Non-resizable textarea -->
<textarea class="resize-none w-full border rounded-xl px-4 py-3 focus:outline-none" rows="3"></textarea>

<!-- Auto-grow with JS -->
<textarea 
  class="w-full border border-gray-300 rounded-xl px-4 py-3 focus:outline-none focus:border-indigo-500 resize-none overflow-hidden"
  rows="1"
  placeholder="What's on your mind?"
  oninput="this.style.height = 'auto'; this.style.height = this.scrollHeight + 'px'"
></textarea>

<!-- Textarea with char count -->
<div class="relative">
  <textarea 
    id="msg"
    rows="4" 
    maxlength="280"
    class="w-full px-4 py-3 pb-8 rounded-xl border border-gray-300 focus:outline-none focus:border-indigo-500 resize-none"
    placeholder="Tweet something..."
    oninput="document.getElementById('count').textContent = this.value.length"
  ></textarea>
  <span id="count" class="absolute bottom-3 right-3 text-xs text-gray-400">0</span>
  <span class="absolute bottom-3 left-3 text-xs text-gray-400">/280</span>
</div>
```

---

## Step 154: Select Dropdown

```html
<!-- Basic select -->
<select class="
  w-full px-4 py-3 rounded-xl
  border border-gray-300
  text-gray-900 text-sm
  bg-white
  focus:outline-none focus:border-indigo-500 focus:ring-2 focus:ring-indigo-200
  cursor-pointer transition-all
  appearance-none
  bg-[url('data:image/svg+xml;charset=US-ASCII,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20viewBox%3D%220%200%2020%2020%22%3E%3Cpath%20fill%3D%22%236b7280%22%20d%3D%22M5.293%207.293a1%201%200%20011.414%200L10%2010.586l3.293-3.293a1%201%200%20111.414%201.414l-4%204a1%201%200%2001-1.414%200l-4-4a1%201%200%20010-1.414z%22%2F%3E%3C%2Fsvg%3E')]
  bg-no-repeat bg-[right_0.75rem_center] bg-[length:1.25rem]
  pr-10
">
  <option value="">เลือกจังหวัด...</option>
  <option value="bkk">กรุงเทพมหานคร</option>
  <option value="cnx">เชียงใหม่</option>
  <option value="pkn">ภูเก็ต</option>
  <option value="ptya">พัทยา</option>
</select>

<!-- Select with optgroup -->
<select class="w-full px-4 py-3 rounded-xl border border-gray-300 focus:outline-none focus:border-indigo-500">
  <option value="">เลือกทักษะ...</option>
  <optgroup label="Frontend">
    <option>HTML/CSS</option>
    <option>JavaScript</option>
    <option>React</option>
  </optgroup>
  <optgroup label="Backend">
    <option>Node.js</option>
    <option>Python</option>
    <option>Go</option>
  </optgroup>
</select>
```

---

## Step 155: Checkbox and Radio

```html
<!-- Native (accent-color) -->
<input type="checkbox" class="accent-indigo-600 size-5 cursor-pointer">
<input type="radio" class="accent-indigo-600 size-5 cursor-pointer">

<!-- Custom checkbox -->
<label class="flex items-center gap-3 cursor-pointer group">
  <div class="
    relative size-5 rounded border-2 border-gray-300 
    group-has-[:checked]:bg-indigo-600 group-has-[:checked]:border-indigo-600
    transition-all flex items-center justify-center
  ">
    <svg class="size-3 text-white opacity-0 group-has-[:checked]:opacity-100 transition-opacity" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="3">
      <path stroke-linecap="round" stroke-linejoin="round" d="M5 13l4 4L19 7"/>
    </svg>
    <input type="checkbox" class="sr-only">
  </div>
  <span class="text-gray-700 text-sm">Remember me</span>
</label>

<!-- Custom radio buttons -->
<div class="space-y-2">
  <label class="flex items-center gap-3 cursor-pointer group">
    <div class="
      relative size-5 rounded-full border-2 border-gray-300
      group-has-[:checked]:border-indigo-600
      transition-all flex items-center justify-center
    ">
      <div class="size-2.5 rounded-full bg-indigo-600 opacity-0 group-has-[:checked]:opacity-100 transition-opacity"></div>
      <input type="radio" name="choice" class="sr-only">
    </div>
    <span class="text-gray-700 text-sm">Option A</span>
  </label>
  
  <label class="flex items-center gap-3 cursor-pointer group">
    <div class="relative size-5 rounded-full border-2 border-gray-300 group-has-[:checked]:border-indigo-600 transition-all flex items-center justify-center">
      <div class="size-2.5 rounded-full bg-indigo-600 opacity-0 group-has-[:checked]:opacity-100 transition-opacity"></div>
      <input type="radio" name="choice" class="sr-only">
    </div>
    <span class="text-gray-700 text-sm">Option B</span>
  </label>
</div>
```

---

## Step 156: Toggle Switch

```html
<!-- Toggle switch (CSS only) -->
<label class="flex items-center gap-3 cursor-pointer group">
  <div class="
    relative w-11 h-6 
    bg-gray-200 group-has-[:checked]:bg-indigo-600
    rounded-full transition-colors
  ">
    <div class="
      absolute top-1 left-1 
      size-4 bg-white rounded-full shadow 
      group-has-[:checked]:translate-x-5
      transition-transform
    "></div>
    <input type="checkbox" class="sr-only">
  </div>
  <span class="text-sm text-gray-700">Enable notifications</span>
</label>

<!-- Large toggle -->
<label class="flex items-center gap-3 cursor-pointer group">
  <div class="relative w-14 h-7 bg-gray-200 group-has-[:checked]:bg-green-500 rounded-full transition-colors">
    <div class="absolute top-1 left-1 size-5 bg-white rounded-full shadow-sm group-has-[:checked]:translate-x-7 transition-transform"></div>
    <input type="checkbox" class="sr-only" checked>
  </div>
  <span class="text-sm font-medium text-gray-700 group-has-[:checked]:text-green-600">Online</span>
</label>

<!-- Toggle with icon -->
<label class="flex items-center gap-3 cursor-pointer group">
  <div class="relative w-12 h-6 bg-gray-200 group-has-[:checked]:bg-indigo-600 rounded-full transition-colors flex items-center px-0.5">
    <div class="size-5 bg-white rounded-full shadow flex items-center justify-center text-xs group-has-[:checked]:translate-x-6 transition-transform">
      <span class="group-has-[:checked]:hidden">🌙</span>
      <span class="hidden group-has-[:checked]:block">☀️</span>
    </div>
    <input type="checkbox" class="sr-only">
  </div>
  <span class="text-sm text-gray-700">Dark Mode</span>
</label>
```

---

## Step 157: Range Slider

```html
<!-- Range slider -->
<input 
  type="range" 
  min="0" max="100" value="60"
  class="
    w-full h-2 rounded-full 
    appearance-none cursor-pointer
    accent-indigo-600
    bg-gray-200
    [&::-webkit-slider-thumb]:size-5
    [&::-webkit-slider-thumb]:appearance-none
    [&::-webkit-slider-thumb]:rounded-full
    [&::-webkit-slider-thumb]:bg-indigo-600
    [&::-webkit-slider-thumb]:shadow
    [&::-webkit-slider-thumb]:cursor-pointer
    [&::-webkit-slider-thumb]:border-2
    [&::-webkit-slider-thumb]:border-white
  "
>

<!-- Range with labels -->
<div class="space-y-2">
  <div class="flex justify-between text-sm text-gray-600">
    <span>ราคาต่ำสุด</span>
    <span id="range-val" class="font-bold text-indigo-600">฿5,000</span>
    <span>ราคาสูงสุด</span>
  </div>
  <input 
    type="range" min="0" max="10000" value="5000" step="500"
    class="w-full accent-indigo-600 cursor-pointer"
    oninput="document.getElementById('range-val').textContent = '฿' + Number(this.value).toLocaleString()"
  >
  <div class="flex justify-between text-xs text-gray-400">
    <span>฿0</span>
    <span>฿10,000</span>
  </div>
</div>
```

---

## Step 158: File Upload

```html
<!-- Styled file input -->
<div class="relative">
  <input 
    type="file" 
    class="
      w-full text-sm text-gray-500
      file:mr-4 file:py-2 file:px-4
      file:rounded-lg file:border-0
      file:text-sm file:font-semibold
      file:bg-indigo-50 file:text-indigo-700
      hover:file:bg-indigo-100
      cursor-pointer
      file:cursor-pointer
      file:transition-colors
    "
  >
</div>

<!-- Drag & drop zone -->
<div class="
  border-2 border-dashed border-gray-300 
  hover:border-indigo-400 hover:bg-indigo-50/50
  rounded-2xl p-10 text-center cursor-pointer 
  transition-all group
">
  <div class="text-4xl mb-3 group-hover:scale-110 transition-transform">📁</div>
  <p class="font-medium text-gray-700">ลากไฟล์มาวางที่นี่</p>
  <p class="text-sm text-gray-400 mt-1">หรือ</p>
  <label class="mt-3 inline-block cursor-pointer">
    <span class="bg-indigo-600 hover:bg-indigo-700 text-white text-sm font-medium px-4 py-2 rounded-lg transition-colors">
      เลือกไฟล์
    </span>
    <input type="file" class="sr-only" multiple>
  </label>
  <p class="text-xs text-gray-400 mt-3">PNG, JPG, PDF up to 10MB</p>
</div>

<!-- Image preview upload -->
<label class="
  relative block size-32 rounded-2xl overflow-hidden cursor-pointer
  border-2 border-dashed border-gray-300 hover:border-indigo-400
  transition-colors group bg-gray-50
">
  <div class="absolute inset-0 flex flex-col items-center justify-center gap-1 group-hover:scale-110 transition-transform">
    <span class="text-3xl">📷</span>
    <span class="text-xs text-gray-500">Upload</span>
  </div>
  <input type="file" accept="image/*" class="sr-only">
</label>
```

---

## Step 159: Validation States

```html
<!-- Error state -->
<div class="space-y-1.5">
  <label class="text-sm font-medium text-gray-700">Email</label>
  <input 
    type="email" 
    value="invalid-email"
    class="
      w-full px-4 py-3 rounded-xl
      border-2 border-red-400 bg-red-50/50
      text-gray-900 focus:outline-none
      focus:border-red-500 focus:ring-2 focus:ring-red-200
    "
  >
  <p class="text-xs text-red-500 flex items-center gap-1">
    <span>⚠</span> กรุณากรอกอีเมลที่ถูกต้อง
  </p>
</div>

<!-- Success state -->
<div class="space-y-1.5">
  <label class="text-sm font-medium text-gray-700">Username</label>
  <div class="relative">
    <input 
      type="text" 
      value="johndoe"
      class="
        w-full px-4 py-3 pr-10 rounded-xl
        border-2 border-green-400 bg-green-50/50
        text-gray-900 focus:outline-none
        focus:border-green-500 focus:ring-2 focus:ring-green-200
      "
    >
    <span class="absolute right-3 top-1/2 -translate-y-1/2 text-green-500">✓</span>
  </div>
  <p class="text-xs text-green-600">Username นี้ว่างอยู่</p>
</div>

<!-- Warning state -->
<div class="space-y-1.5">
  <label class="text-sm font-medium text-gray-700">Password strength</label>
  <input 
    type="password" 
    value="weak"
    class="w-full px-4 py-3 rounded-xl border-2 border-yellow-400 bg-yellow-50/50 text-gray-900 focus:outline-none focus:border-yellow-500"
  >
  <div class="flex gap-1">
    <div class="flex-1 h-1.5 rounded-full bg-red-400"></div>
    <div class="flex-1 h-1.5 rounded-full bg-gray-200"></div>
    <div class="flex-1 h-1.5 rounded-full bg-gray-200"></div>
    <div class="flex-1 h-1.5 rounded-full bg-gray-200"></div>
  </div>
  <p class="text-xs text-yellow-600">รหัสผ่านอ่อนเกินไป เพิ่มตัวเลขหรืออักขระพิเศษ</p>
</div>
```

---

## Step 160: Workshop — Complete Registration Form

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Registration Form</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="min-h-screen bg-gray-50 flex items-center justify-center py-12 px-4">

<div class="w-full max-w-lg">
  <!-- Header -->
  <div class="text-center mb-8">
    <div class="inline-flex size-16 bg-indigo-600 rounded-2xl items-center justify-center text-white text-2xl mb-4">🎓</div>
    <h1 class="text-2xl font-bold text-gray-900">สมัครเรียน</h1>
    <p class="text-gray-500 text-sm mt-1">กรอกข้อมูลเพื่อเข้าสู่หลักสูตร</p>
  </div>

  <form class="bg-white rounded-2xl shadow-sm border border-gray-200 p-8 space-y-5">
    
    <!-- Name row -->
    <div class="grid grid-cols-2 gap-4">
      <div class="space-y-1.5">
        <label class="text-sm font-medium text-gray-700">ชื่อ <span class="text-red-500">*</span></label>
        <input type="text" class="w-full px-4 py-2.5 rounded-xl border border-gray-300 focus:outline-none focus:border-indigo-500 focus:ring-2 focus:ring-indigo-100 text-sm transition-all" placeholder="สมชาย">
      </div>
      <div class="space-y-1.5">
        <label class="text-sm font-medium text-gray-700">นามสกุล <span class="text-red-500">*</span></label>
        <input type="text" class="w-full px-4 py-2.5 rounded-xl border border-gray-300 focus:outline-none focus:border-indigo-500 focus:ring-2 focus:ring-indigo-100 text-sm transition-all" placeholder="ใจดี">
      </div>
    </div>

    <!-- Email -->
    <div class="space-y-1.5">
      <label class="text-sm font-medium text-gray-700">อีเมล <span class="text-red-500">*</span></label>
      <div class="relative">
        <span class="absolute left-3 top-1/2 -translate-y-1/2 text-gray-400 text-sm">✉</span>
        <input type="email" class="w-full pl-9 pr-4 py-2.5 rounded-xl border border-gray-300 focus:outline-none focus:border-indigo-500 focus:ring-2 focus:ring-indigo-100 text-sm transition-all" placeholder="your@email.com">
      </div>
    </div>

    <!-- Phone -->
    <div class="space-y-1.5">
      <label class="text-sm font-medium text-gray-700">เบอร์โทรศัพท์</label>
      <div class="flex">
        <span class="flex items-center px-3 text-sm bg-gray-50 border border-r-0 border-gray-300 rounded-l-xl text-gray-500">+66</span>
        <input type="tel" class="flex-1 px-4 py-2.5 rounded-r-xl border border-gray-300 focus:outline-none focus:border-indigo-500 text-sm" placeholder="8X-XXX-XXXX">
      </div>
    </div>

    <!-- Experience level -->
    <div class="space-y-1.5">
      <label class="text-sm font-medium text-gray-700">ระดับประสบการณ์ <span class="text-red-500">*</span></label>
      <div class="grid grid-cols-3 gap-2">
        <label>
          <input type="radio" name="level" value="beginner" class="peer sr-only" checked>
          <div class="p-2.5 border border-gray-200 rounded-xl cursor-pointer text-center peer-checked:border-indigo-500 peer-checked:bg-indigo-50 hover:border-gray-300 transition-all">
            <div class="text-xl mb-1">🌱</div>
            <div class="text-xs font-medium text-gray-700 peer-checked:text-indigo-700">มือใหม่</div>
          </div>
        </label>
        <label>
          <input type="radio" name="level" value="intermediate" class="peer sr-only">
          <div class="p-2.5 border border-gray-200 rounded-xl cursor-pointer text-center peer-checked:border-indigo-500 peer-checked:bg-indigo-50 hover:border-gray-300 transition-all">
            <div class="text-xl mb-1">🌿</div>
            <div class="text-xs font-medium text-gray-700">กลาง</div>
          </div>
        </label>
        <label>
          <input type="radio" name="level" value="advanced" class="peer sr-only">
          <div class="p-2.5 border border-gray-200 rounded-xl cursor-pointer text-center peer-checked:border-indigo-500 peer-checked:bg-indigo-50 hover:border-gray-300 transition-all">
            <div class="text-xl mb-1">🌳</div>
            <div class="text-xs font-medium text-gray-700">ขั้นสูง</div>
          </div>
        </label>
      </div>
    </div>

    <!-- Goals textarea -->
    <div class="space-y-1.5">
      <label class="text-sm font-medium text-gray-700">เป้าหมายการเรียน</label>
      <textarea rows="3" class="w-full px-4 py-2.5 rounded-xl border border-gray-300 focus:outline-none focus:border-indigo-500 focus:ring-2 focus:ring-indigo-100 text-sm resize-none transition-all" placeholder="อยากพัฒนาทักษะ UI/UX เพื่อสมัครงาน..."></textarea>
    </div>

    <!-- Notifications toggle -->
    <div class="flex items-center justify-between py-3 border-t border-gray-100">
      <div>
        <p class="text-sm font-medium text-gray-700">รับการแจ้งเตือน</p>
        <p class="text-xs text-gray-400 mt-0.5">อัพเดตบทเรียนและข่าวสาร</p>
      </div>
      <label class="flex items-center cursor-pointer group">
        <div class="relative w-11 h-6 bg-gray-200 group-has-[:checked]:bg-indigo-600 rounded-full transition-colors">
          <div class="absolute top-1 left-1 size-4 bg-white rounded-full shadow group-has-[:checked]:translate-x-5 transition-transform"></div>
          <input type="checkbox" class="sr-only" checked>
        </div>
      </label>
    </div>

    <!-- Terms -->
    <label class="flex items-start gap-3 cursor-pointer group">
      <div class="flex-none mt-0.5 size-4 rounded border border-gray-300 group-has-[:checked]:bg-indigo-600 group-has-[:checked]:border-indigo-600 flex items-center justify-center transition-all">
        <svg class="size-3 text-white opacity-0 group-has-[:checked]:opacity-100" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="3">
          <path stroke-linecap="round" stroke-linejoin="round" d="M5 13l4 4L19 7"/>
        </svg>
        <input type="checkbox" class="sr-only">
      </div>
      <span class="text-sm text-gray-600">ฉันยอมรับ <a href="#" class="text-indigo-600 hover:underline">เงื่อนไขการใช้งาน</a> และ <a href="#" class="text-indigo-600 hover:underline">นโยบายความเป็นส่วนตัว</a></span>
    </label>

    <!-- Submit -->
    <button type="submit" class="w-full bg-indigo-600 hover:bg-indigo-700 active:scale-[0.98] text-white font-semibold py-3 rounded-xl transition-all">
      สมัครเลย →
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

## 📝 สรุป Part 16

| Component | Key Classes |
|-----------|------------|
| Input | `border focus:ring-2 focus:border-indigo-500 focus:outline-none` |
| Floating label | `peer` + `peer-focus:` + `peer-not-placeholder-shown:` |
| Select | `appearance-none` + custom background arrow |
| Toggle | `has-[:checked]:bg-indigo-600` + `translate-x-5` |
| File drop zone | `border-dashed hover:border-indigo-400` |
| Error state | `border-red-400 bg-red-50/50` |
| Success state | `border-green-400 bg-green-50/50` |

---

*Part 16 — จาก 100 Parts | Steps 151–160 จาก 1,000 Steps*
