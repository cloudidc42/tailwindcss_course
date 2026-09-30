# Part 80: Design System Component Library

## เป้าหมาย
- สร้าง component library ด้วย Tailwind
- Button, Input, Badge, Card, Modal, Alert, Avatar, Tooltip
- Consistent API + variants
- Steps 791–800

---

## Step 791: Button Component

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Button Component</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 min-h-screen p-8">
  <h1 class="text-xl font-extrabold text-gray-900 mb-6">Button Variants</h1>

  <!-- Variants -->
  <section class="mb-8">
    <p class="text-xs font-semibold text-gray-400 uppercase tracking-wide mb-3">Variant</p>
    <div class="flex flex-wrap gap-3">
      <button class="px-4 py-2 bg-indigo-600 text-white text-sm font-semibold rounded-xl hover:bg-indigo-700 active:scale-95 transition-all focus-visible:ring-2 focus-visible:ring-indigo-500 focus-visible:ring-offset-2">Primary</button>
      <button class="px-4 py-2 bg-gray-100 text-gray-800 text-sm font-semibold rounded-xl hover:bg-gray-200 active:scale-95 transition-all focus-visible:ring-2 focus-visible:ring-gray-400 focus-visible:ring-offset-2">Secondary</button>
      <button class="px-4 py-2 bg-transparent text-indigo-600 text-sm font-semibold rounded-xl border border-indigo-300 hover:bg-indigo-50 active:scale-95 transition-all">Outline</button>
      <button class="px-4 py-2 bg-transparent text-gray-700 text-sm font-semibold rounded-xl hover:bg-gray-100 active:scale-95 transition-all">Ghost</button>
      <button class="px-4 py-2 bg-red-600 text-white text-sm font-semibold rounded-xl hover:bg-red-700 active:scale-95 transition-all">Destructive</button>
    </div>
  </section>

  <!-- Sizes -->
  <section class="mb-8">
    <p class="text-xs font-semibold text-gray-400 uppercase tracking-wide mb-3">Size</p>
    <div class="flex items-center flex-wrap gap-3">
      <button class="px-2.5 py-1 bg-indigo-600 text-white text-xs font-semibold rounded-lg hover:bg-indigo-700 transition-colors">XS</button>
      <button class="px-3 py-1.5 bg-indigo-600 text-white text-sm font-semibold rounded-lg hover:bg-indigo-700 transition-colors">SM</button>
      <button class="px-4 py-2 bg-indigo-600 text-white text-sm font-semibold rounded-xl hover:bg-indigo-700 transition-colors">MD</button>
      <button class="px-5 py-2.5 bg-indigo-600 text-white text-base font-semibold rounded-xl hover:bg-indigo-700 transition-colors">LG</button>
      <button class="px-6 py-3 bg-indigo-600 text-white text-lg font-semibold rounded-2xl hover:bg-indigo-700 transition-colors">XL</button>
    </div>
  </section>

  <!-- States -->
  <section class="mb-8">
    <p class="text-xs font-semibold text-gray-400 uppercase tracking-wide mb-3">States</p>
    <div class="flex flex-wrap gap-3">
      <button disabled class="px-4 py-2 bg-indigo-600 text-white text-sm font-semibold rounded-xl opacity-50 cursor-not-allowed">Disabled</button>
      <button class="px-4 py-2 bg-indigo-600 text-white text-sm font-semibold rounded-xl flex items-center gap-2">
        <svg class="w-4 h-4 animate-spin" fill="none" viewBox="0 0 24 24"><circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"/><path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4z"/></svg>
        Loading
      </button>
      <button class="px-4 py-2 bg-indigo-600 text-white text-sm font-semibold rounded-xl flex items-center gap-2">
        <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4"/></svg>
        With Icon
      </button>
      <button class="w-9 h-9 bg-indigo-600 text-white rounded-xl flex items-center justify-center hover:bg-indigo-700 transition-colors" aria-label="Add">
        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4"/></svg>
      </button>
    </div>
  </section>
</body>
</html>
```

---

## Step 792: Input Component

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Input Component</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    .input-base {
      display: block; width: 100%; padding: 0.5rem 0.75rem;
      font-size: 0.875rem; border-radius: 0.75rem;
      border: 1px solid #d1d5db; background: #fff;
      outline: none; transition: border-color 0.2s, box-shadow 0.2s;
    }
    .input-base:focus { border-color: #6366f1; box-shadow: 0 0 0 3px rgba(99,102,241,.15); }
    .input-error { border-color: #ef4444 !important; }
    .input-error:focus { box-shadow: 0 0 0 3px rgba(239,68,68,.15) !important; }
    .input-success { border-color: #10b981 !important; }
  </style>
</head>
<body class="bg-gray-50 min-h-screen p-8">
  <div class="max-w-md mx-auto space-y-5">
    <h1 class="text-xl font-extrabold text-gray-900">Input Variants</h1>

    <!-- Default -->
    <div>
      <label class="block text-sm font-medium text-gray-700 mb-1.5">Default</label>
      <input type="text" placeholder="Enter text..." class="input-base">
    </div>

    <!-- With icons -->
    <div>
      <label class="block text-sm font-medium text-gray-700 mb-1.5">With Left Icon</label>
      <div class="relative">
        <span class="absolute left-3 top-1/2 -translate-y-1/2 text-gray-400">
          <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"/></svg>
        </span>
        <input type="text" placeholder="Search..." class="input-base pl-9">
      </div>
    </div>

    <!-- Error -->
    <div>
      <label class="block text-sm font-medium text-gray-700 mb-1.5">Error State</label>
      <input type="email" value="invalid-email" class="input-base input-error" aria-invalid="true" aria-describedby="email-error">
      <p id="email-error" class="mt-1.5 text-sm text-red-600">กรุณาใส่ email ที่ถูกต้อง</p>
    </div>

    <!-- Success -->
    <div>
      <label class="block text-sm font-medium text-gray-700 mb-1.5">Success State</label>
      <div class="relative">
        <input type="text" value="john.doe" class="input-base input-success pr-9">
        <span class="absolute right-3 top-1/2 -translate-y-1/2 text-emerald-500">
          <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"/></svg>
        </span>
      </div>
    </div>

    <!-- Textarea -->
    <div>
      <label class="block text-sm font-medium text-gray-700 mb-1.5">Textarea</label>
      <textarea rows="3" placeholder="Write something..." class="input-base resize-none"></textarea>
    </div>

    <!-- Select -->
    <div>
      <label class="block text-sm font-medium text-gray-700 mb-1.5">Select</label>
      <select class="input-base">
        <option value="">เลือก...</option>
        <option>Option A</option>
        <option>Option B</option>
        <option>Option C</option>
      </select>
    </div>
  </div>
</body>
</html>
```

---

## Step 793: Badge Component

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Badge Component</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-white min-h-screen p-8">
  <div class="space-y-8">
    <!-- Solid badges -->
    <section>
      <p class="text-xs font-semibold text-gray-400 uppercase tracking-wide mb-3">Solid</p>
      <div class="flex flex-wrap gap-2">
        <span class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-semibold bg-indigo-600 text-white">Indigo</span>
        <span class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-semibold bg-emerald-600 text-white">Emerald</span>
        <span class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-semibold bg-rose-600 text-white">Rose</span>
        <span class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-semibold bg-amber-500 text-white">Amber</span>
        <span class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-semibold bg-gray-600 text-white">Gray</span>
      </div>
    </section>

    <!-- Soft badges -->
    <section>
      <p class="text-xs font-semibold text-gray-400 uppercase tracking-wide mb-3">Soft</p>
      <div class="flex flex-wrap gap-2">
        <span class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-semibold bg-indigo-100 text-indigo-700">Indigo</span>
        <span class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-semibold bg-emerald-100 text-emerald-700">Emerald</span>
        <span class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-semibold bg-rose-100 text-rose-700">Rose</span>
        <span class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-semibold bg-amber-100 text-amber-700">Amber</span>
        <span class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-semibold bg-gray-100 text-gray-600">Gray</span>
      </div>
    </section>

    <!-- With dot -->
    <section>
      <p class="text-xs font-semibold text-gray-400 uppercase tracking-wide mb-3">Status</p>
      <div class="flex flex-wrap gap-2">
        <span class="inline-flex items-center gap-1.5 px-2.5 py-0.5 rounded-full text-xs font-semibold bg-emerald-100 text-emerald-700">
          <span class="w-1.5 h-1.5 rounded-full bg-emerald-500 animate-pulse"></span>Active
        </span>
        <span class="inline-flex items-center gap-1.5 px-2.5 py-0.5 rounded-full text-xs font-semibold bg-yellow-100 text-yellow-700">
          <span class="w-1.5 h-1.5 rounded-full bg-yellow-500"></span>Pending
        </span>
        <span class="inline-flex items-center gap-1.5 px-2.5 py-0.5 rounded-full text-xs font-semibold bg-gray-100 text-gray-500">
          <span class="w-1.5 h-1.5 rounded-full bg-gray-400"></span>Inactive
        </span>
        <span class="inline-flex items-center gap-1.5 px-2.5 py-0.5 rounded-full text-xs font-semibold bg-red-100 text-red-700">
          <span class="w-1.5 h-1.5 rounded-full bg-red-500"></span>Error
        </span>
      </div>
    </section>

    <!-- Number badges -->
    <section>
      <p class="text-xs font-semibold text-gray-400 uppercase tracking-wide mb-3">Number</p>
      <div class="flex items-center gap-4">
        <div class="relative inline-flex">
          <button class="px-4 py-2 bg-gray-100 text-gray-700 rounded-xl text-sm font-medium">Messages</button>
          <span class="absolute -top-1.5 -right-1.5 w-5 h-5 bg-red-500 text-white text-xs rounded-full flex items-center justify-center font-bold">3</span>
        </div>
        <div class="relative inline-flex">
          <button class="px-4 py-2 bg-gray-100 text-gray-700 rounded-xl text-sm font-medium">Notifications</button>
          <span class="absolute -top-1.5 -right-1.5 min-w-5 h-5 px-1 bg-indigo-500 text-white text-xs rounded-full flex items-center justify-center font-bold">99+</span>
        </div>
      </div>
    </section>
  </div>
</body>
</html>
```

---

## Step 794: Alert Component

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Alert Component</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 min-h-screen p-8">
  <div class="max-w-lg mx-auto space-y-4">
    <h1 class="text-xl font-extrabold text-gray-900 mb-6">Alert Variants</h1>

    <!-- Info -->
    <div class="flex gap-3 p-4 bg-blue-50 border border-blue-200 rounded-2xl" role="alert">
      <div class="flex-shrink-0 w-5 h-5 text-blue-500 mt-0.5">ℹ️</div>
      <div>
        <p class="font-semibold text-blue-800 text-sm">Information</p>
        <p class="text-blue-700 text-sm mt-0.5">Your account settings have been updated.</p>
      </div>
    </div>

    <!-- Success -->
    <div class="flex gap-3 p-4 bg-emerald-50 border border-emerald-200 rounded-2xl" role="alert">
      <div class="flex-shrink-0 text-emerald-500 mt-0.5">✅</div>
      <div>
        <p class="font-semibold text-emerald-800 text-sm">Success</p>
        <p class="text-emerald-700 text-sm mt-0.5">Payment processed successfully.</p>
      </div>
    </div>

    <!-- Warning -->
    <div class="flex gap-3 p-4 bg-amber-50 border border-amber-200 rounded-2xl" role="alert">
      <div class="flex-shrink-0 text-amber-500 mt-0.5">⚠️</div>
      <div class="flex-1">
        <p class="font-semibold text-amber-800 text-sm">Warning</p>
        <p class="text-amber-700 text-sm mt-0.5">Storage is almost full. Please upgrade your plan.</p>
        <button class="mt-2 text-xs font-semibold text-amber-800 underline">Upgrade Now</button>
      </div>
      <button class="flex-shrink-0 text-amber-500 hover:text-amber-700" aria-label="Dismiss">✕</button>
    </div>

    <!-- Error -->
    <div class="flex gap-3 p-4 bg-red-50 border border-red-200 rounded-2xl" role="alert">
      <div class="flex-shrink-0 text-red-500 mt-0.5">❌</div>
      <div>
        <p class="font-semibold text-red-800 text-sm">Error</p>
        <p class="text-red-700 text-sm mt-0.5">Failed to save changes. Please try again.</p>
      </div>
    </div>

    <!-- Dismissible with animation -->
    <div id="alert-dismiss" class="flex gap-3 p-4 bg-indigo-50 border border-indigo-200 rounded-2xl transition-all duration-300 overflow-hidden" role="alert">
      <div class="flex-shrink-0 text-indigo-500 mt-0.5">📢</div>
      <div class="flex-1">
        <p class="text-indigo-700 text-sm">This is a dismissible alert. Click × to hide it.</p>
      </div>
      <button onclick="dismissAlert()" class="flex-shrink-0 text-indigo-400 hover:text-indigo-600 transition-colors font-bold">✕</button>
    </div>
  </div>
  <script>
    function dismissAlert() {
      const el = document.getElementById('alert-dismiss');
      el.style.maxHeight = el.offsetHeight + 'px';
      el.style.opacity = '0';
      el.style.maxHeight = '0';
      el.style.paddingTop = '0';
      el.style.paddingBottom = '0';
      el.style.marginBottom = '0';
      setTimeout(() => el.remove(), 300);
    }
  </script>
</body>
</html>
```

---

## Step 795: Avatar Component

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Avatar Component</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    .avatar { position: relative; display: inline-flex; }
    .avatar-status {
      position: absolute; bottom: 0; right: 0;
      border: 2px solid white; border-radius: 9999px;
    }
  </style>
</head>
<body class="bg-gray-50 min-h-screen p-8">
  <div class="space-y-8">
    <!-- Sizes -->
    <section>
      <p class="text-xs font-semibold text-gray-400 uppercase tracking-wide mb-3">Sizes</p>
      <div class="flex items-end gap-4">
        <div class="avatar"><div class="w-6 h-6 rounded-full bg-indigo-600 flex items-center justify-center text-white text-xs font-bold">A</div></div>
        <div class="avatar"><div class="w-8 h-8 rounded-full bg-indigo-600 flex items-center justify-center text-white text-sm font-bold">AB</div></div>
        <div class="avatar"><div class="w-10 h-10 rounded-full bg-indigo-600 flex items-center justify-center text-white font-bold">AB</div></div>
        <div class="avatar"><div class="w-12 h-12 rounded-full bg-indigo-600 flex items-center justify-center text-white text-lg font-bold">AB</div></div>
        <div class="avatar"><div class="w-16 h-16 rounded-full bg-indigo-600 flex items-center justify-center text-white text-2xl font-bold">AB</div></div>
      </div>
    </section>

    <!-- With status -->
    <section>
      <p class="text-xs font-semibold text-gray-400 uppercase tracking-wide mb-3">With Status</p>
      <div class="flex items-center gap-4">
        <div class="avatar">
          <div class="w-10 h-10 rounded-full bg-emerald-600 flex items-center justify-center text-white font-bold">JD</div>
          <span class="avatar-status w-3 h-3 bg-emerald-400"></span>
        </div>
        <div class="avatar">
          <div class="w-10 h-10 rounded-full bg-amber-500 flex items-center justify-center text-white font-bold">SA</div>
          <span class="avatar-status w-3 h-3 bg-amber-400"></span>
        </div>
        <div class="avatar">
          <div class="w-10 h-10 rounded-full bg-gray-400 flex items-center justify-center text-white font-bold">MK</div>
          <span class="avatar-status w-3 h-3 bg-gray-300"></span>
        </div>
      </div>
    </section>

    <!-- Avatar group -->
    <section>
      <p class="text-xs font-semibold text-gray-400 uppercase tracking-wide mb-3">Avatar Group</p>
      <div class="flex -space-x-2">
        <div class="w-9 h-9 rounded-full bg-indigo-600 border-2 border-white flex items-center justify-center text-white text-xs font-bold ring-0">JD</div>
        <div class="w-9 h-9 rounded-full bg-rose-500 border-2 border-white flex items-center justify-center text-white text-xs font-bold">SA</div>
        <div class="w-9 h-9 rounded-full bg-amber-500 border-2 border-white flex items-center justify-center text-white text-xs font-bold">MK</div>
        <div class="w-9 h-9 rounded-full bg-emerald-600 border-2 border-white flex items-center justify-center text-white text-xs font-bold">TW</div>
        <div class="w-9 h-9 rounded-full bg-gray-200 border-2 border-white flex items-center justify-center text-gray-600 text-xs font-bold">+8</div>
      </div>
    </section>
  </div>
</body>
</html>
```

---

## Step 796: Tooltip Component

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Tooltip Component</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    .tooltip-wrapper { position: relative; display: inline-flex; }
    .tooltip {
      position: absolute; z-index: 50; bottom: calc(100% + 8px); left: 50%; transform: translateX(-50%);
      background: #1e293b; color: #f8fafc; font-size: 0.75rem; font-weight: 500;
      padding: 0.375rem 0.625rem; border-radius: 0.5rem; white-space: nowrap;
      opacity: 0; pointer-events: none; transition: opacity 0.15s;
    }
    .tooltip::after {
      content: ''; position: absolute; top: 100%; left: 50%; transform: translateX(-50%);
      border: 5px solid transparent; border-top-color: #1e293b;
    }
    .tooltip-wrapper:hover .tooltip,
    .tooltip-wrapper:focus-within .tooltip { opacity: 1; }

    /* Directions */
    .tooltip-bottom { bottom: auto; top: calc(100% + 8px); }
    .tooltip-bottom::after { top: auto; bottom: 100%; border-top-color: transparent; border-bottom-color: #1e293b; }
    .tooltip-right { bottom: auto; left: calc(100% + 8px); top: 50%; transform: translateY(-50%); }
    .tooltip-right::after { top: 50%; left: auto; right: 100%; transform: translateY(-50%); border-right-color: #1e293b; border-top-color: transparent; }
  </style>
</head>
<body class="bg-gray-50 min-h-screen flex items-center justify-center">
  <div class="flex flex-col items-center gap-12">
    <div class="flex gap-8 items-center">
      <!-- Top -->
      <div class="tooltip-wrapper">
        <button class="px-4 py-2 bg-gray-800 text-white text-sm rounded-xl">Top</button>
        <span class="tooltip" role="tooltip">Tooltip on top</span>
      </div>
      <!-- Bottom -->
      <div class="tooltip-wrapper">
        <button class="px-4 py-2 bg-gray-800 text-white text-sm rounded-xl">Bottom</button>
        <span class="tooltip tooltip-bottom" role="tooltip">Tooltip on bottom</span>
      </div>
      <!-- Right -->
      <div class="tooltip-wrapper">
        <button class="px-4 py-2 bg-gray-800 text-white text-sm rounded-xl">Right</button>
        <span class="tooltip tooltip-right" role="tooltip">Tooltip on right</span>
      </div>
    </div>
    <!-- On icon -->
    <div class="tooltip-wrapper">
      <button class="w-8 h-8 rounded-full bg-gray-200 flex items-center justify-center text-gray-500 text-xs font-bold" aria-describedby="help-tip">?</button>
      <span id="help-tip" class="tooltip" role="tooltip">Click for help documentation</span>
    </div>
  </div>
</body>
</html>
```

---

## Step 797: Card Variants

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Card Variants</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 min-h-screen p-8">
  <div class="max-w-2xl mx-auto grid grid-cols-2 gap-4">

    <!-- Basic -->
    <div class="bg-white rounded-2xl border border-gray-200 p-5 shadow-sm">
      <h3 class="font-semibold text-gray-900 mb-1">Basic Card</h3>
      <p class="text-gray-500 text-sm">Simple card with border and shadow.</p>
    </div>

    <!-- Elevated -->
    <div class="bg-white rounded-2xl p-5 shadow-lg">
      <h3 class="font-semibold text-gray-900 mb-1">Elevated Card</h3>
      <p class="text-gray-500 text-sm">Shadow-only, no border.</p>
    </div>

    <!-- Gradient border -->
    <div class="p-[1px] bg-gradient-to-br from-indigo-400 to-purple-400 rounded-2xl">
      <div class="bg-white rounded-[calc(1rem-1px)] p-5 h-full">
        <h3 class="font-semibold text-gray-900 mb-1">Gradient Border</h3>
        <p class="text-gray-500 text-sm">Gradient border via wrapper.</p>
      </div>
    </div>

    <!-- Interactive -->
    <div class="bg-white rounded-2xl border border-gray-200 p-5 shadow-sm hover:shadow-lg hover:-translate-y-0.5 transition-all cursor-pointer group">
      <h3 class="font-semibold text-gray-900 mb-1 group-hover:text-indigo-600 transition-colors">Interactive Card</h3>
      <p class="text-gray-500 text-sm">Hover for lift + color effect.</p>
    </div>

    <!-- With image -->
    <div class="bg-white rounded-2xl overflow-hidden border border-gray-200 shadow-sm col-span-2">
      <div class="h-32 bg-gradient-to-br from-indigo-500 to-purple-600 flex items-center justify-center text-white text-4xl">🎨</div>
      <div class="p-5">
        <h3 class="font-semibold text-gray-900 mb-1">Card with Image</h3>
        <p class="text-gray-500 text-sm">Card with a full-bleed header image area.</p>
        <div class="flex gap-2 mt-3">
          <button class="px-3 py-1.5 bg-indigo-600 text-white text-xs rounded-lg font-semibold">Action</button>
          <button class="px-3 py-1.5 text-gray-600 text-xs rounded-lg font-semibold hover:bg-gray-100 transition-colors">Cancel</button>
        </div>
      </div>
    </div>
  </div>
</body>
</html>
```

---

## Step 798: Modal Component

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Modal Component</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    @keyframes fadeIn  { from{opacity:0} to{opacity:1} }
    @keyframes slideUp { from{transform:translateY(16px);opacity:0} to{transform:translateY(0);opacity:1} }
    .modal-overlay { animation: fadeIn 0.2s ease; }
    .modal-panel   { animation: slideUp 0.25s ease; }
  </style>
</head>
<body class="bg-gray-100 min-h-screen flex items-center justify-center">
  <div class="flex gap-3">
    <button onclick="openModal('sm')"   class="px-4 py-2 bg-indigo-600 text-white rounded-xl text-sm font-semibold hover:bg-indigo-700">SM Modal</button>
    <button onclick="openModal('md')"   class="px-4 py-2 bg-indigo-600 text-white rounded-xl text-sm font-semibold hover:bg-indigo-700">MD Modal</button>
    <button onclick="openModal('confirm')" class="px-4 py-2 bg-red-600 text-white rounded-xl text-sm font-semibold hover:bg-red-700">Confirm Modal</button>
  </div>

  <!-- Modal -->
  <div id="modal" class="hidden fixed inset-0 z-50 flex items-center justify-center p-4">
    <!-- Overlay -->
    <div class="modal-overlay absolute inset-0 bg-black/50" onclick="closeModal()"></div>
    <!-- Panel -->
    <div id="modal-panel" class="modal-panel relative bg-white rounded-2xl shadow-2xl w-full max-w-md overflow-hidden" role="dialog" aria-modal="true" aria-labelledby="modal-title">
      <div id="modal-content"></div>
    </div>
  </div>

  <script>
    const content = {
      sm: `
        <div class="p-6">
          <h2 id="modal-title" class="text-lg font-bold text-gray-900 mb-2">Small Modal</h2>
          <p class="text-gray-500 text-sm mb-4">A compact modal for simple messages or actions.</p>
          <div class="flex justify-end gap-2">
            <button onclick="closeModal()" class="px-4 py-2 text-gray-600 text-sm rounded-xl hover:bg-gray-100">Cancel</button>
            <button class="px-4 py-2 bg-indigo-600 text-white text-sm rounded-xl font-semibold hover:bg-indigo-700">Confirm</button>
          </div>
        </div>`,
      md: `
        <div class="border-b px-6 py-4 flex items-center justify-between">
          <h2 id="modal-title" class="font-bold text-gray-900">Edit Profile</h2>
          <button onclick="closeModal()" class="text-gray-400 hover:text-gray-600 text-xl leading-none">✕</button>
        </div>
        <div class="p-6 space-y-4">
          <div><label class="block text-sm font-medium text-gray-700 mb-1">Name</label><input class="w-full border rounded-xl px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500" value="John Doe"></div>
          <div><label class="block text-sm font-medium text-gray-700 mb-1">Email</label><input class="w-full border rounded-xl px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500" value="john@example.com"></div>
        </div>
        <div class="border-t px-6 py-4 flex justify-end gap-2">
          <button onclick="closeModal()" class="px-4 py-2 text-gray-600 text-sm rounded-xl hover:bg-gray-100">Cancel</button>
          <button class="px-4 py-2 bg-indigo-600 text-white text-sm rounded-xl font-semibold">Save Changes</button>
        </div>`,
      confirm: `
        <div class="p-6 text-center">
          <div class="w-12 h-12 bg-red-100 rounded-full flex items-center justify-center mx-auto mb-4 text-2xl">🗑️</div>
          <h2 id="modal-title" class="text-lg font-bold text-gray-900 mb-2">Delete Account?</h2>
          <p class="text-gray-500 text-sm mb-6">This action cannot be undone. All your data will be permanently deleted.</p>
          <div class="flex gap-3">
            <button onclick="closeModal()" class="flex-1 px-4 py-2 border text-gray-700 rounded-xl text-sm font-semibold hover:bg-gray-50">Cancel</button>
            <button class="flex-1 px-4 py-2 bg-red-600 text-white rounded-xl text-sm font-semibold hover:bg-red-700">Delete</button>
          </div>
        </div>`
    };

    function openModal(type) {
      document.getElementById('modal-content').innerHTML = content[type];
      document.getElementById('modal').classList.remove('hidden');
      document.getElementById('modal-title')?.focus();
    }
    function closeModal() { document.getElementById('modal').classList.add('hidden'); }
    document.addEventListener('keydown', e => { if (e.key === 'Escape') closeModal(); });
  </script>
</body>
</html>
```

---

## Step 799: Dropdown Menu

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Dropdown Menu</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    @keyframes menuIn { from{opacity:0;transform:translateY(-6px)} to{opacity:1;transform:translateY(0)} }
    .menu-animate { animation: menuIn 0.15s ease; }
  </style>
</head>
<body class="bg-gray-100 min-h-screen flex items-center justify-center gap-8">

  <!-- Basic dropdown -->
  <div class="relative" id="dd1">
    <button onclick="toggle('dd1')" class="flex items-center gap-2 px-4 py-2 bg-white border border-gray-200 rounded-xl text-sm font-medium shadow-sm hover:bg-gray-50 transition-colors" aria-haspopup="true" aria-expanded="false">
      Options <span class="text-gray-400">▾</span>
    </button>
    <div id="dd1-menu" class="hidden absolute right-0 mt-2 w-48 bg-white border border-gray-200 rounded-2xl shadow-lg overflow-hidden menu-animate z-10">
      <a href="#" class="flex items-center gap-2.5 px-4 py-2.5 text-sm text-gray-700 hover:bg-gray-50 transition-colors">✏️ Edit</a>
      <a href="#" class="flex items-center gap-2.5 px-4 py-2.5 text-sm text-gray-700 hover:bg-gray-50 transition-colors">📋 Duplicate</a>
      <a href="#" class="flex items-center gap-2.5 px-4 py-2.5 text-sm text-gray-700 hover:bg-gray-50 transition-colors">📤 Export</a>
      <hr class="my-1 border-gray-100">
      <a href="#" class="flex items-center gap-2.5 px-4 py-2.5 text-sm text-red-600 hover:bg-red-50 transition-colors">🗑️ Delete</a>
    </div>
  </div>

  <!-- User menu -->
  <div class="relative" id="dd2">
    <button onclick="toggle('dd2')" class="flex items-center gap-2 focus:outline-none focus-visible:ring-2 focus-visible:ring-indigo-500 rounded-xl" aria-haspopup="true">
      <div class="w-9 h-9 bg-indigo-600 rounded-full flex items-center justify-center text-white text-sm font-bold">JD</div>
      <span class="text-sm font-medium text-gray-700">John Doe</span>
      <span class="text-gray-400 text-xs">▾</span>
    </button>
    <div id="dd2-menu" class="hidden absolute right-0 mt-2 w-56 bg-white border border-gray-200 rounded-2xl shadow-lg overflow-hidden z-10">
      <div class="px-4 py-3 border-b">
        <p class="font-semibold text-gray-900 text-sm">John Doe</p>
        <p class="text-xs text-gray-500">john@example.com</p>
      </div>
      <a href="#" class="flex items-center gap-2.5 px-4 py-2.5 text-sm text-gray-700 hover:bg-gray-50">👤 Profile</a>
      <a href="#" class="flex items-center gap-2.5 px-4 py-2.5 text-sm text-gray-700 hover:bg-gray-50">⚙️ Settings</a>
      <hr class="my-1 border-gray-100">
      <a href="#" class="flex items-center gap-2.5 px-4 py-2.5 text-sm text-red-600 hover:bg-red-50">🚪 Sign Out</a>
    </div>
  </div>

  <script>
    function toggle(id) {
      const menu = document.getElementById(id + '-menu');
      const btn = document.querySelector(`#${id} button`);
      const isOpen = !menu.classList.contains('hidden');
      // Close all
      document.querySelectorAll('[id$="-menu"]').forEach(m => m.classList.add('hidden'));
      document.querySelectorAll('[aria-expanded]').forEach(b => b.setAttribute('aria-expanded','false'));
      if (!isOpen) {
        menu.classList.remove('hidden');
        btn.setAttribute('aria-expanded', 'true');
      }
    }
    document.addEventListener('click', e => {
      if (!e.target.closest('[id^="dd"]')) {
        document.querySelectorAll('[id$="-menu"]').forEach(m => m.classList.add('hidden'));
      }
    });
  </script>
</body>
</html>
```

---

## Step 800: Design System Workshop — Component Gallery

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Component Gallery</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 min-h-screen p-6">
  <div class="max-w-4xl mx-auto">
    <h1 class="text-3xl font-extrabold text-gray-900 mb-1">Component Gallery</h1>
    <p class="text-gray-500 mb-8">Design System — Tailwind CSS</p>

    <div class="grid grid-cols-2 gap-4">
      <!-- Buttons -->
      <div class="bg-white rounded-2xl border p-5">
        <p class="text-xs font-semibold text-gray-400 uppercase tracking-wide mb-3">Buttons</p>
        <div class="flex flex-wrap gap-2">
          <button class="px-4 py-2 bg-indigo-600 text-white text-sm rounded-xl font-semibold hover:bg-indigo-700 transition-colors">Primary</button>
          <button class="px-4 py-2 bg-gray-100 text-gray-700 text-sm rounded-xl font-semibold hover:bg-gray-200 transition-colors">Secondary</button>
          <button class="px-4 py-2 text-indigo-600 border border-indigo-300 text-sm rounded-xl font-semibold hover:bg-indigo-50 transition-colors">Outline</button>
        </div>
      </div>
      <!-- Badges -->
      <div class="bg-white rounded-2xl border p-5">
        <p class="text-xs font-semibold text-gray-400 uppercase tracking-wide mb-3">Badges</p>
        <div class="flex flex-wrap gap-2">
          <span class="px-2.5 py-0.5 rounded-full text-xs font-semibold bg-indigo-100 text-indigo-700">Active</span>
          <span class="px-2.5 py-0.5 rounded-full text-xs font-semibold bg-emerald-100 text-emerald-700">Success</span>
          <span class="px-2.5 py-0.5 rounded-full text-xs font-semibold bg-amber-100 text-amber-700">Warning</span>
          <span class="px-2.5 py-0.5 rounded-full text-xs font-semibold bg-red-100 text-red-700">Error</span>
        </div>
      </div>
      <!-- Inputs -->
      <div class="bg-white rounded-2xl border p-5">
        <p class="text-xs font-semibold text-gray-400 uppercase tracking-wide mb-3">Inputs</p>
        <div class="space-y-2">
          <input type="text" placeholder="Default input" class="w-full px-3 py-2 text-sm border rounded-xl focus:outline-none focus:ring-2 focus:ring-indigo-500">
          <input type="text" placeholder="Error state" class="w-full px-3 py-2 text-sm border border-red-400 rounded-xl focus:outline-none focus:ring-2 focus:ring-red-400">
        </div>
      </div>
      <!-- Alerts -->
      <div class="bg-white rounded-2xl border p-5">
        <p class="text-xs font-semibold text-gray-400 uppercase tracking-wide mb-3">Alerts</p>
        <div class="space-y-2">
          <div class="flex gap-2 p-3 bg-emerald-50 border border-emerald-200 rounded-xl text-sm text-emerald-700">✅ Success message</div>
          <div class="flex gap-2 p-3 bg-red-50 border border-red-200 rounded-xl text-sm text-red-700">❌ Error message</div>
        </div>
      </div>
    </div>

    <!-- Avatar row -->
    <div class="bg-white rounded-2xl border p-5 mt-4">
      <p class="text-xs font-semibold text-gray-400 uppercase tracking-wide mb-3">Avatars</p>
      <div class="flex items-center gap-4">
        <div class="flex -space-x-2">
          <div class="w-9 h-9 rounded-full bg-indigo-600 border-2 border-white flex items-center justify-center text-white text-xs font-bold">JD</div>
          <div class="w-9 h-9 rounded-full bg-rose-500 border-2 border-white flex items-center justify-center text-white text-xs font-bold">SA</div>
          <div class="w-9 h-9 rounded-full bg-amber-500 border-2 border-white flex items-center justify-center text-white text-xs font-bold">MK</div>
          <div class="w-9 h-9 rounded-full bg-gray-200 border-2 border-white flex items-center justify-center text-gray-600 text-xs font-bold">+5</div>
        </div>
        <span class="text-sm text-gray-500">Team: 8 members</span>
      </div>
    </div>
  </div>
</body>
</html>
```

---

## สรุป Part 80

| Step | เนื้อหา |
|------|---------|
| 791 | Button — variant, size, state (loading, disabled, icon) |
| 792 | Input — error, success, icon, textarea, select |
| 793 | Badge — solid, soft, status, number badge |
| 794 | Alert — info, success, warning, error, dismissible |
| 795 | Avatar — size, status, group |
| 796 | Tooltip — top, bottom, right, on icon |
| 797 | Card — basic, elevated, gradient border, interactive, with image |
| 798 | Modal — SM, MD form, confirm dialog |
| 799 | Dropdown — basic, user menu |
| 800 | Workshop: Component gallery |

**Part ถัดไป:** Part 81 — Responsive Design Patterns (Steps 801–810)
