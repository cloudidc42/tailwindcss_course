# Part 43: Chat & Messaging UI

## เป้าหมาย
- สร้าง Chat Interface ระดับ Professional
- Sidebar contact list, Message bubbles, Reactions, File uploads
- Steps 421–430

---

## Step 421: Chat Layout

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Chat App - Step 421</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-white h-screen flex overflow-hidden text-gray-900">

  <!-- Sidebar -->
  <aside class="w-72 border-r flex flex-col flex-shrink-0">
    <!-- Sidebar Header -->
    <div class="px-4 py-4 border-b">
      <div class="flex items-center justify-between mb-3">
        <h2 class="font-extrabold text-lg">Messages</h2>
        <button class="w-8 h-8 bg-indigo-600 rounded-xl text-white flex items-center justify-center hover:bg-indigo-700 transition-colors">
          <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.232 5.232l3.536 3.536m-2.036-5.036a2.5 2.5 0 113.536 3.536L6.5 21.036H3v-3.572L16.732 3.732z"/></svg>
        </button>
      </div>
      <div class="relative">
        <svg class="absolute left-3 top-1/2 -translate-y-1/2 w-4 h-4 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"/></svg>
        <input type="text" placeholder="ค้นหา..." class="w-full bg-gray-100 rounded-xl pl-9 pr-4 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
      </div>
    </div>

    <!-- Tabs -->
    <div class="flex border-b">
      <button class="flex-1 py-2.5 text-xs font-semibold text-indigo-600 border-b-2 border-indigo-600">ทั้งหมด</button>
      <button class="flex-1 py-2.5 text-xs font-semibold text-gray-400 hover:text-gray-600 transition-colors relative">
        ยังไม่อ่าน
        <span class="absolute top-1.5 right-4 w-4 h-4 bg-red-500 rounded-full text-white text-[10px] flex items-center justify-center font-bold">4</span>
      </button>
      <button class="flex-1 py-2.5 text-xs font-semibold text-gray-400 hover:text-gray-600 transition-colors">กลุ่ม</button>
    </div>

    <!-- Contact List -->
    <div class="flex-1 overflow-y-auto">
      <!-- Active conversation -->
      <div class="flex items-center gap-3 px-4 py-3 bg-indigo-50 cursor-pointer hover:bg-indigo-100 transition-colors">
        <div class="relative flex-shrink-0">
          <img src="https://i.pravatar.cc/40?img=1" class="w-10 h-10 rounded-full" alt="">
          <div class="absolute bottom-0 right-0 w-3 h-3 bg-green-400 rounded-full border-2 border-white"></div>
        </div>
        <div class="flex-1 min-w-0">
          <div class="flex items-center justify-between">
            <p class="text-sm font-semibold">สมชาย เทคโน</p>
            <p class="text-[11px] text-gray-400">10:42</p>
          </div>
          <p class="text-xs text-gray-500 truncate">ส่ง design spec ให้เลย</p>
        </div>
      </div>

      <div class="flex items-center gap-3 px-4 py-3 cursor-pointer hover:bg-gray-50 transition-colors">
        <div class="relative flex-shrink-0">
          <img src="https://i.pravatar.cc/40?img=2" class="w-10 h-10 rounded-full" alt="">
          <div class="absolute bottom-0 right-0 w-3 h-3 bg-gray-300 rounded-full border-2 border-white"></div>
        </div>
        <div class="flex-1 min-w-0">
          <div class="flex items-center justify-between">
            <p class="text-sm font-semibold">วิไล รัตนมงคล</p>
            <p class="text-[11px] text-gray-400">09:18</p>
          </div>
          <div class="flex items-center justify-between">
            <p class="text-xs text-gray-500 truncate">Ok ขอบคุณนะคะ 🙏</p>
            <span class="w-4 h-4 bg-indigo-600 rounded-full text-white text-[10px] flex items-center justify-center font-bold ml-1 flex-shrink-0">2</span>
          </div>
        </div>
      </div>

      <!-- Group chat -->
      <div class="flex items-center gap-3 px-4 py-3 cursor-pointer hover:bg-gray-50 transition-colors">
        <div class="relative w-10 h-10 flex-shrink-0">
          <img src="https://i.pravatar.cc/28?img=3" class="w-7 h-7 rounded-full absolute top-0 left-0 border-2 border-white" alt="">
          <img src="https://i.pravatar.cc/28?img=4" class="w-7 h-7 rounded-full absolute bottom-0 right-0 border-2 border-white" alt="">
        </div>
        <div class="flex-1 min-w-0">
          <div class="flex items-center justify-between">
            <p class="text-sm font-semibold">Dev Team</p>
            <p class="text-[11px] text-gray-400">เมื่อวาน</p>
          </div>
          <p class="text-xs text-gray-500 truncate">ธนพล: merge แล้วนะ</p>
        </div>
      </div>

      <div class="flex items-center gap-3 px-4 py-3 cursor-pointer hover:bg-gray-50 transition-colors">
        <div class="relative flex-shrink-0">
          <img src="https://i.pravatar.cc/40?img=5" class="w-10 h-10 rounded-full" alt="">
          <div class="absolute bottom-0 right-0 w-3 h-3 bg-yellow-400 rounded-full border-2 border-white"></div>
        </div>
        <div class="flex-1 min-w-0">
          <div class="flex items-center justify-between">
            <p class="text-sm font-semibold">อรทัย สุข</p>
            <p class="text-[11px] text-gray-400">เมื่อวาน</p>
          </div>
          <div class="flex items-center gap-1">
            <svg class="w-3 h-3 text-gray-400 flex-shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.172 7l-6.586 6.586a2 2 0 102.828 2.828l6.414-6.586a4 4 0 00-5.656-5.656l-6.415 6.585a6 6 0 108.486 8.486L20.5 13"/></svg>
            <p class="text-xs text-gray-500 truncate">design_v2.fig</p>
          </div>
        </div>
      </div>
    </div>

    <!-- Current User -->
    <div class="border-t px-4 py-3 flex items-center gap-3">
      <div class="relative">
        <img src="https://i.pravatar.cc/36?img=10" class="w-9 h-9 rounded-full" alt="">
        <div class="absolute bottom-0 right-0 w-2.5 h-2.5 bg-green-400 rounded-full border-2 border-white"></div>
      </div>
      <div class="flex-1 min-w-0">
        <p class="text-sm font-semibold">ฉัน</p>
        <p class="text-xs text-green-500">Online</p>
      </div>
      <button class="text-gray-400 hover:text-gray-600 transition-colors p-1">⚙️</button>
    </div>
  </aside>

  <!-- Chat Area -->
  <div class="flex-1 flex flex-col min-w-0">
    <!-- Chat Header -->
    <div class="border-b px-5 h-14 flex items-center justify-between flex-shrink-0">
      <div class="flex items-center gap-3">
        <div class="relative">
          <img src="https://i.pravatar.cc/36?img=1" class="w-9 h-9 rounded-full" alt="">
          <div class="absolute bottom-0 right-0 w-2.5 h-2.5 bg-green-400 rounded-full border-2 border-white"></div>
        </div>
        <div>
          <p class="font-semibold text-sm">สมชาย เทคโน</p>
          <p class="text-xs text-green-500">Online</p>
        </div>
      </div>
      <div class="flex items-center gap-1">
        <button class="p-2 hover:bg-gray-100 rounded-xl transition-colors text-gray-500">📞</button>
        <button class="p-2 hover:bg-gray-100 rounded-xl transition-colors text-gray-500">🎥</button>
        <button class="p-2 hover:bg-gray-100 rounded-xl transition-colors text-gray-500">🔍</button>
        <button class="p-2 hover:bg-gray-100 rounded-xl transition-colors text-gray-500">⋯</button>
      </div>
    </div>

    <!-- Messages -->
    <div class="flex-1 overflow-y-auto px-5 py-5 space-y-4" id="messages">
      <!-- Date divider -->
      <div class="flex items-center gap-3 my-2">
        <div class="flex-1 h-px bg-gray-200"></div>
        <span class="text-xs text-gray-400 px-2">วันนี้</span>
        <div class="flex-1 h-px bg-gray-200"></div>
      </div>

      <!-- Received message -->
      <div class="flex items-end gap-2">
        <img src="https://i.pravatar.cc/28?img=1" class="w-7 h-7 rounded-full flex-shrink-0 mb-1" alt="">
        <div class="max-w-xs lg:max-w-md">
          <div class="bg-gray-100 rounded-2xl rounded-bl-sm px-4 py-2.5">
            <p class="text-sm">สวัสดีครับ อยากถามเรื่อง spec ของ feature ใหม่</p>
          </div>
          <p class="text-[11px] text-gray-400 mt-1 ml-1">10:30 น.</p>
        </div>
      </div>

      <!-- Sent message -->
      <div class="flex items-end gap-2 justify-end">
        <div class="max-w-xs lg:max-w-md">
          <div class="bg-indigo-600 text-white rounded-2xl rounded-br-sm px-4 py-2.5">
            <p class="text-sm">ได้เลยครับ มีอะไรอยากรู้บ้าง?</p>
          </div>
          <p class="text-[11px] text-gray-400 mt-1 text-right mr-1">10:31 น. ✓✓</p>
        </div>
      </div>

      <!-- Image message -->
      <div class="flex items-end gap-2">
        <img src="https://i.pravatar.cc/28?img=1" class="w-7 h-7 rounded-full flex-shrink-0 mb-1" alt="">
        <div class="max-w-xs lg:max-w-sm">
          <div class="bg-gray-100 rounded-2xl rounded-bl-sm overflow-hidden">
            <div class="w-full h-36 bg-gradient-to-br from-indigo-400 to-purple-600 flex items-center justify-center text-white text-4xl cursor-pointer hover:opacity-90 transition-opacity">
              🖼️
            </div>
            <p class="text-xs text-gray-500 px-3 py-1.5">design_mockup.png · 2.4MB</p>
          </div>
          <p class="text-[11px] text-gray-400 mt-1 ml-1">10:38 น.</p>
        </div>
      </div>

      <!-- Sent with reaction -->
      <div class="flex items-end gap-2 justify-end">
        <div class="max-w-xs lg:max-w-md">
          <div class="bg-indigo-600 text-white rounded-2xl rounded-br-sm px-4 py-2.5 relative">
            <p class="text-sm">โอเคครับ รอดูสักครู่</p>
            <div class="absolute -bottom-3 right-2 bg-white border rounded-full px-1.5 py-0.5 text-[11px] shadow-sm">👍 2</div>
          </div>
          <p class="text-[11px] text-gray-400 mt-4 text-right mr-1">10:40 น. ✓✓</p>
        </div>
      </div>

      <!-- Typing indicator -->
      <div class="flex items-end gap-2">
        <img src="https://i.pravatar.cc/28?img=1" class="w-7 h-7 rounded-full flex-shrink-0" alt="">
        <div class="bg-gray-100 rounded-2xl rounded-bl-sm px-4 py-3">
          <div class="flex gap-1 items-center">
            <span class="w-2 h-2 bg-gray-400 rounded-full animate-bounce" style="animation-delay:0s"></span>
            <span class="w-2 h-2 bg-gray-400 rounded-full animate-bounce" style="animation-delay:0.15s"></span>
            <span class="w-2 h-2 bg-gray-400 rounded-full animate-bounce" style="animation-delay:0.3s"></span>
          </div>
        </div>
      </div>
    </div>

    <!-- Message Input -->
    <div class="border-t px-4 py-3 flex-shrink-0">
      <div class="flex items-end gap-3 bg-gray-50 rounded-2xl px-4 py-3 border">
        <button class="text-gray-400 hover:text-indigo-600 transition-colors mb-0.5">📎</button>
        <textarea id="msg-input" rows="1" placeholder="พิมพ์ข้อความ..." onkeydown="handleKey(event)"
          class="flex-1 bg-transparent text-sm resize-none focus:outline-none max-h-32 leading-relaxed"></textarea>
        <div class="flex items-center gap-1.5">
          <button class="text-gray-400 hover:text-indigo-600 transition-colors">😊</button>
          <button onclick="sendMessage()" class="w-8 h-8 bg-indigo-600 rounded-xl text-white flex items-center justify-center hover:bg-indigo-700 transition-colors">
            <svg class="w-4 h-4 rotate-90" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 19l9 2-9-18-9 18 9-2zm0 0v-8"/></svg>
          </button>
        </div>
      </div>
    </div>
  </div>

  <script>
    function sendMessage() {
      const input = document.getElementById('msg-input');
      const text = input.value.trim();
      if (!text) return;

      const msgs = document.getElementById('messages');
      const bubble = document.createElement('div');
      bubble.className = 'flex items-end gap-2 justify-end';
      bubble.innerHTML = `
        <div class="max-w-xs lg:max-w-md">
          <div class="bg-indigo-600 text-white rounded-2xl rounded-br-sm px-4 py-2.5">
            <p class="text-sm">${text.replace(/</g,'&lt;')}</p>
          </div>
          <p class="text-[11px] text-gray-400 mt-1 text-right mr-1">ตอนนี้ ✓</p>
        </div>
      `;
      msgs.appendChild(bubble);
      msgs.scrollTop = msgs.scrollHeight;
      input.value = '';
      input.style.height = 'auto';
    }

    function handleKey(e) {
      if (e.key === 'Enter' && !e.shiftKey) {
        e.preventDefault();
        sendMessage();
      }
      const ta = document.getElementById('msg-input');
      ta.style.height = 'auto';
      ta.style.height = Math.min(ta.scrollHeight, 128) + 'px';
    }
  </script>

</body>
</html>
```

---

## Steps 422–429: Reactions, Files, Thread, Group Chat, Status

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Chat Features - Steps 422-429</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 p-6">
  <div class="max-w-3xl mx-auto space-y-6">

    <!-- Message Reactions (Step 422) -->
    <div class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4 text-sm text-gray-500 uppercase tracking-wide">Message Reactions</h3>
      <div class="space-y-4">
        <div class="flex items-end gap-2">
          <img src="https://i.pravatar.cc/28?img=1" class="w-7 h-7 rounded-full flex-shrink-0" alt="">
          <div>
            <div class="bg-gray-100 rounded-2xl rounded-bl-sm px-4 py-2.5 relative group max-w-xs">
              <p class="text-sm">อยากนัดคุย spec รอบหน้าได้เลยนะครับ</p>
              <!-- Hover reaction bar -->
              <div class="absolute -top-8 left-0 bg-white border shadow-lg rounded-xl px-2 py-1 flex gap-1 text-lg hidden group-hover:flex">
                <button onclick="addReaction(this,'👍')" class="hover:scale-125 transition-transform cursor-pointer">👍</button>
                <button onclick="addReaction(this,'❤️')" class="hover:scale-125 transition-transform cursor-pointer">❤️</button>
                <button onclick="addReaction(this,'😂')" class="hover:scale-125 transition-transform cursor-pointer">😂</button>
                <button onclick="addReaction(this,'🎉')" class="hover:scale-125 transition-transform cursor-pointer">🎉</button>
                <button onclick="addReaction(this,'😮')" class="hover:scale-125 transition-transform cursor-pointer">😮</button>
              </div>
            </div>
            <!-- Reactions -->
            <div class="flex gap-1 mt-1.5" id="reaction-row">
              <button class="bg-indigo-50 border border-indigo-200 rounded-full px-2 py-0.5 text-xs flex items-center gap-1 hover:bg-indigo-100 transition-colors">
                <span>👍</span><span class="text-indigo-700 font-semibold">3</span>
              </button>
              <button class="bg-gray-50 border rounded-full px-2 py-0.5 text-xs flex items-center gap-1 hover:bg-gray-100 transition-colors">
                <span>❤️</span><span class="font-semibold">1</span>
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- File Messages (Step 423) -->
    <div class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4 text-sm text-gray-500 uppercase tracking-wide">File & Media Messages</h3>
      <div class="space-y-3">
        <!-- PDF -->
        <div class="flex items-end gap-2 justify-end">
          <div class="max-w-xs">
            <div class="bg-indigo-600 text-white rounded-2xl rounded-br-sm p-3">
              <div class="flex items-center gap-3">
                <div class="w-10 h-10 bg-white/20 rounded-xl flex items-center justify-center text-xl">📄</div>
                <div>
                  <p class="text-xs font-semibold">proposal_v3.pdf</p>
                  <p class="text-[11px] text-indigo-200">1.8 MB · PDF</p>
                </div>
                <button class="ml-auto text-white/70 hover:text-white transition-colors text-lg">↓</button>
              </div>
            </div>
          </div>
        </div>

        <!-- Audio message -->
        <div class="flex items-end gap-2">
          <img src="https://i.pravatar.cc/28?img=2" class="w-7 h-7 rounded-full flex-shrink-0" alt="">
          <div class="bg-gray-100 rounded-2xl rounded-bl-sm px-4 py-2.5 flex items-center gap-3 min-w-48">
            <button class="w-8 h-8 bg-indigo-600 rounded-full flex items-center justify-center text-white hover:bg-indigo-700 transition-colors">▶</button>
            <div class="flex-1 flex items-center gap-0.5 h-6">
              <div class="w-0.5 bg-indigo-400 rounded-full" style="height:40%"></div>
              <div class="w-0.5 bg-indigo-400 rounded-full" style="height:60%"></div>
              <div class="w-0.5 bg-indigo-600 rounded-full" style="height:80%"></div>
              <div class="w-0.5 bg-indigo-600 rounded-full" style="height:100%"></div>
              <div class="w-0.5 bg-indigo-400 rounded-full" style="height:70%"></div>
              <div class="w-0.5 bg-gray-300 rounded-full" style="height:50%"></div>
              <div class="w-0.5 bg-gray-300 rounded-full" style="height:30%"></div>
              <div class="w-0.5 bg-gray-300 rounded-full" style="height:60%"></div>
              <div class="w-0.5 bg-gray-300 rounded-full" style="height:40%"></div>
            </div>
            <span class="text-xs text-gray-500">0:32</span>
          </div>
        </div>
      </div>
    </div>

    <!-- Thread Reply (Step 424) -->
    <div class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4 text-sm text-gray-500 uppercase tracking-wide">Thread Replies</h3>
      <div class="space-y-2">
        <div class="flex items-start gap-2">
          <img src="https://i.pravatar.cc/28?img=1" class="w-7 h-7 rounded-full flex-shrink-0 mt-0.5" alt="">
          <div>
            <div class="bg-gray-100 rounded-2xl rounded-bl-sm px-4 py-2.5">
              <p class="text-xs font-semibold text-indigo-600 mb-0.5">สมชาย เทคโน</p>
              <p class="text-sm">ช่วยส่ง design system file ให้หน่อยได้ไหม?</p>
            </div>
            <!-- Thread count -->
            <button class="flex items-center gap-1.5 mt-1 ml-2 text-[11px] text-indigo-600 hover:underline">
              <div class="flex -space-x-1">
                <img src="https://i.pravatar.cc/16?img=2" class="w-4 h-4 rounded-full border border-white" alt="">
                <img src="https://i.pravatar.cc/16?img=3" class="w-4 h-4 rounded-full border border-white" alt="">
              </div>
              3 ตอบกลับ
            </button>
            <!-- Thread replies -->
            <div class="ml-4 mt-2 pl-3 border-l-2 border-indigo-200 space-y-2">
              <div class="flex items-start gap-2">
                <img src="https://i.pravatar.cc/22?img=2" class="w-5 h-5 rounded-full flex-shrink-0 mt-0.5" alt="">
                <div class="bg-gray-50 rounded-xl px-3 py-1.5">
                  <p class="text-[11px] font-semibold text-gray-600">วิไล:</p>
                  <p class="text-xs">ส่งไปแล้วนะคะ ตรวจสอบ email ด้วย</p>
                </div>
              </div>
              <div class="flex items-start gap-2">
                <img src="https://i.pravatar.cc/22?img=3" class="w-5 h-5 rounded-full flex-shrink-0 mt-0.5" alt="">
                <div class="bg-gray-50 rounded-xl px-3 py-1.5">
                  <p class="text-[11px] font-semibold text-gray-600">ธนพล:</p>
                  <p class="text-xs">ได้แล้วครับ ขอบคุณ</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Status Messages (Step 425) -->
    <div class="bg-white rounded-2xl border p-5">
      <h3 class="font-bold mb-4 text-sm text-gray-500 uppercase tracking-wide">System Messages</h3>
      <div class="space-y-3">
        <div class="flex items-center gap-3 text-center justify-center">
          <div class="flex-1 h-px bg-gray-100"></div>
          <span class="text-xs text-gray-400 bg-gray-50 px-3 py-1 rounded-full">วิไล เข้าร่วมห้องแชท</span>
          <div class="flex-1 h-px bg-gray-100"></div>
        </div>
        <div class="flex items-center justify-center">
          <span class="text-xs text-gray-400 bg-blue-50 text-blue-500 px-3 py-1 rounded-full">🔐 ข้อความที่ส่งถูกเข้ารหัส end-to-end</span>
        </div>
      </div>
    </div>

  </div>

  <script>
    function addReaction(btn, emoji) {
      const row = btn.closest('.flex.items-end').querySelector('#reaction-row');
      const existing = [...row.querySelectorAll('button')].find(b => b.querySelector('span').textContent === emoji);
      if (existing) {
        const count = existing.querySelectorAll('span')[1];
        count.textContent = parseInt(count.textContent) + 1;
      } else {
        const newBtn = document.createElement('button');
        newBtn.className = 'bg-gray-50 border rounded-full px-2 py-0.5 text-xs flex items-center gap-1 hover:bg-gray-100 transition-colors';
        newBtn.innerHTML = `<span>${emoji}</span><span class="font-semibold">1</span>`;
        row.appendChild(newBtn);
      }
    }
  </script>
</body>
</html>
```

---

## Step 430: Workshop — Full Chat App

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Chat Workshop - Step 430</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-white h-screen flex overflow-hidden text-gray-900 text-sm">

  <!-- Sidebar -->
  <aside class="w-64 border-r flex flex-col flex-shrink-0 bg-gray-900 text-white">
    <div class="px-4 py-4 border-b border-gray-700">
      <h2 class="font-extrabold text-base mb-3">TechShop Chat</h2>
      <input type="text" placeholder="ค้นหา..." class="w-full bg-gray-800 rounded-xl px-3 py-2 text-xs focus:outline-none focus:ring-2 focus:ring-indigo-500 placeholder-gray-500">
    </div>

    <!-- Channels -->
    <div class="p-3 overflow-y-auto flex-1">
      <p class="text-[10px] font-bold text-gray-500 uppercase tracking-widest px-2 mb-1.5">Channels</p>
      <div id="channel-list" class="space-y-0.5 mb-4"></div>
      <p class="text-[10px] font-bold text-gray-500 uppercase tracking-widest px-2 mb-1.5">Direct Messages</p>
      <div id="dm-list" class="space-y-0.5"></div>
    </div>

    <!-- User -->
    <div class="px-3 py-3 border-t border-gray-700 flex items-center gap-2">
      <div class="relative">
        <div class="w-8 h-8 bg-indigo-600 rounded-xl flex items-center justify-center font-bold text-xs">ฉ</div>
        <div class="absolute bottom-0 right-0 w-2 h-2 bg-green-400 rounded-full border border-gray-900"></div>
      </div>
      <div class="flex-1 min-w-0">
        <p class="text-xs font-semibold truncate">ฉัน (สมชาย)</p>
        <p class="text-[10px] text-gray-400">Online</p>
      </div>
    </div>
  </aside>

  <!-- Chat -->
  <div class="flex-1 flex flex-col min-w-0 bg-white">
    <div class="border-b px-5 h-12 flex items-center justify-between flex-shrink-0 bg-white">
      <div class="flex items-center gap-2">
        <span class="text-gray-500 font-medium" id="chat-name">#general</span>
        <span class="text-xs text-gray-400 hidden md:block" id="chat-sub">18 สมาชิก · Tailwind CSS ทีม</span>
      </div>
      <div class="flex items-center gap-1 text-gray-400">
        <button class="p-1.5 hover:bg-gray-100 rounded-lg transition-colors text-xs">🔍</button>
        <button class="p-1.5 hover:bg-gray-100 rounded-lg transition-colors text-xs">📌</button>
        <button class="p-1.5 hover:bg-gray-100 rounded-lg transition-colors text-xs">👥</button>
      </div>
    </div>

    <!-- Messages Area -->
    <div class="flex-1 overflow-y-auto px-5 py-4 space-y-3" id="chat-messages"></div>

    <!-- Input -->
    <div class="border-t px-4 py-3 flex-shrink-0">
      <div class="flex items-center gap-2 bg-gray-50 border rounded-xl px-3 py-2">
        <button class="text-gray-400 hover:text-indigo-600 transition-colors text-base">+</button>
        <input id="chat-input" type="text" placeholder="พิมพ์ใน #general..."
          onkeydown="if(event.key==='Enter')sendMsg()"
          class="flex-1 bg-transparent text-sm focus:outline-none placeholder-gray-400">
        <button onclick="sendMsg()" class="w-7 h-7 bg-indigo-600 rounded-lg text-white flex items-center justify-center hover:bg-indigo-700 transition-colors">
          <svg class="w-3.5 h-3.5 rotate-90" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 19l9 2-9-18-9 18 9-2zm0 0v-8"/></svg>
        </button>
      </div>
    </div>
  </div>

  <script>
    const channels = [
      { name: 'general', unread: 0, active: true },
      { name: 'design', unread: 3, active: false },
      { name: 'dev', unread: 0, active: false },
      { name: 'announcements', unread: 1, active: false },
    ];
    const dms = [
      { name: 'วิไล', online: true, img: 2 },
      { name: 'ธนพล', online: false, img: 3 },
      { name: 'อรทัย', online: true, img: 5 },
    ];
    const history = {
      general: [
        { from: 'วิไล', text: 'สวัสดีทุกคน! 👋', time: '09:00', mine: false, img: 2 },
        { from: 'ธนพล', text: 'สวัสดีครับ มี standup วันนี้กี่โมงครับ?', time: '09:05', mine: false, img: 3 },
        { from: 'ฉัน', text: '10:00 น. ครับ ใน Zoom เหมือนเดิม', time: '09:06', mine: true, img: 10 },
        { from: 'วิไล', text: 'โอเคค่ะ ขอบคุณ 🙏', time: '09:07', mine: false, img: 2 },
      ],
      design: [
        { from: 'อรทัย', text: 'อัปโหลด mockup ใหม่แล้วนะคะ', time: '08:30', mine: false, img: 5 },
        { from: 'ฉัน', text: 'รับทราบครับ จะดูเดี๋ยวนี้เลย', time: '08:32', mine: true, img: 10 },
      ],
      dev: [],
      announcements: [
        { from: 'System', text: '🎉 Version 2.0 release ในสัปดาห์หน้า!', time: '08:00', mine: false, img: 0 },
      ],
    };
    let currentCh = 'general';

    function renderChannels() {
      document.getElementById('channel-list').innerHTML = channels.map(c => `
        <button onclick="selectChannel('${c.name}')" class="w-full text-left flex items-center justify-between px-2 py-1.5 rounded-lg ${c.active ? 'bg-indigo-600 text-white' : 'text-gray-400 hover:bg-gray-800 hover:text-white'} transition-colors text-xs">
          <span># ${c.name}</span>
          ${c.unread ? `<span class="bg-red-500 text-white text-[10px] font-bold w-4 h-4 rounded-full flex items-center justify-center">${c.unread}</span>` : ''}
        </button>
      `).join('');
      document.getElementById('dm-list').innerHTML = dms.map(d => `
        <button class="w-full text-left flex items-center gap-2 px-2 py-1.5 rounded-lg text-gray-400 hover:bg-gray-800 hover:text-white transition-colors text-xs">
          <div class="relative">
            <img src="https://i.pravatar.cc/20?img=${d.img}" class="w-5 h-5 rounded-full" alt="">
            <div class="absolute bottom-0 right-0 w-1.5 h-1.5 ${d.online ? 'bg-green-400' : 'bg-gray-500'} rounded-full border border-gray-900"></div>
          </div>
          ${d.name}
        </button>
      `).join('');
    }

    function selectChannel(ch) {
      currentCh = ch;
      channels.forEach(c => c.active = c.name === ch);
      if (channels.find(c => c.name === ch)) {
        const c = channels.find(x => x.name === ch);
        c.unread = 0;
      }
      document.getElementById('chat-name').textContent = '#' + ch;
      renderChannels();
      renderMessages();
    }

    function renderMessages() {
      const msgs = history[currentCh] || [];
      const el = document.getElementById('chat-messages');
      if (!msgs.length) {
        el.innerHTML = '<p class="text-center text-gray-400 text-xs mt-10">ยังไม่มีข้อความ</p>';
        return;
      }
      el.innerHTML = msgs.map(m => m.mine ? `
        <div class="flex justify-end">
          <div class="max-w-xs">
            <div class="bg-indigo-600 text-white rounded-2xl rounded-br-sm px-3 py-2 text-sm">${m.text}</div>
            <p class="text-[10px] text-gray-400 text-right mt-0.5">${m.time} ✓✓</p>
          </div>
        </div>
      ` : `
        <div class="flex items-end gap-2">
          <img src="https://i.pravatar.cc/24?img=${m.img}" class="w-6 h-6 rounded-full flex-shrink-0 mb-0.5" alt="">
          <div class="max-w-xs">
            <p class="text-[11px] text-gray-400 ml-1 mb-0.5">${m.from}</p>
            <div class="bg-gray-100 rounded-2xl rounded-bl-sm px-3 py-2 text-sm">${m.text}</div>
            <p class="text-[10px] text-gray-400 mt-0.5">${m.time}</p>
          </div>
        </div>
      `).join('');
      el.scrollTop = el.scrollHeight;
    }

    function sendMsg() {
      const input = document.getElementById('chat-input');
      const text = input.value.trim();
      if (!text) return;
      if (!history[currentCh]) history[currentCh] = [];
      history[currentCh].push({ from: 'ฉัน', text, time: new Date().toLocaleTimeString('th', {hour:'2-digit', minute:'2-digit'}), mine: true, img: 10 });
      input.value = '';
      renderMessages();
    }

    renderChannels();
    renderMessages();
  </script>

</body>
</html>
```

---

## สรุป Part 43

| Step | เนื้อหา |
|------|---------|
| 421 | Chat layout: sidebar, messages, typing indicator, send |
| 422 | Emoji reactions: hover bar, add reaction, counter |
| 423 | File messages: PDF card, audio waveform player |
| 424 | Thread replies: nested conversation |
| 425 | System messages: join notice, encryption notice |
| 426–429 | Slack-style: channels, DMs, online status, unread badges |
| 430 | Workshop: Full Slack-style chat with channel switching |

**Part ถัดไป:** Part 44 — File Manager & Media Gallery (Steps 431–440)
