# Part 95: Real-Time Features

## เป้าหมาย
- Server-Sent Events (SSE)
- WebSocket ด้วย Socket.io
- Real-time notifications
- Live data dashboard
- Steps 941–950

---

## Step 941: Server-Sent Events (SSE)

```ts
// src/app/api/events/route.ts
import { NextRequest } from 'next/server'

export const runtime = 'edge'
export const dynamic = 'force-dynamic'

export async function GET(request: NextRequest) {
  const encoder = new TextEncoder()

  const stream = new ReadableStream({
    start(controller) {
      let count = 0

      const send = (data: object) => {
        controller.enqueue(encoder.encode(`data: ${JSON.stringify(data)}\n\n`))
      }

      // Send initial event
      send({ type: 'connected', timestamp: Date.now() })

      // Simulate real-time data
      const interval = setInterval(() => {
        count++
        send({
          type:      'metric',
          value:     Math.floor(Math.random() * 100),
          timestamp: Date.now(),
          count,
        })
        if (count >= 100) { clearInterval(interval); controller.close() }
      }, 1000)

      // Cleanup on disconnect
      request.signal.addEventListener('abort', () => {
        clearInterval(interval)
        controller.close()
      })
    },
  })

  return new Response(stream, {
    headers: {
      'Content-Type':  'text/event-stream',
      'Cache-Control': 'no-cache',
      'Connection':    'keep-alive',
      'X-Accel-Buffering': 'no',
    },
  })
}
```

---

## Step 942: SSE Client Hook

```tsx
// src/hooks/useEventSource.ts
import { useState, useEffect, useRef, useCallback } from 'react'

interface EventSourceOptions {
  onMessage?: (event: MessageEvent) => void
  onError?:   (event: Event) => void
  enabled?:   boolean
}

export function useEventSource(url: string, options: EventSourceOptions = {}) {
  const [status, setStatus]       = useState<'connecting' | 'open' | 'closed'>('closed')
  const [lastEvent, setLastEvent] = useState<any>(null)
  const esRef = useRef<EventSource | null>(null)

  const { onMessage, onError, enabled = true } = options

  const connect = useCallback(() => {
    if (esRef.current) esRef.current.close()
    const es = new EventSource(url)
    esRef.current = es

    es.onopen  = () => setStatus('open')
    es.onerror = (e) => {
      setStatus('closed')
      onError?.(e)
      // Auto-reconnect after 5s
      setTimeout(connect, 5000)
    }
    es.onmessage = (e) => {
      const data = JSON.parse(e.data)
      setLastEvent(data)
      onMessage?.(e)
    }
  }, [url, onMessage, onError])

  useEffect(() => {
    if (!enabled) return
    setStatus('connecting')
    connect()
    return () => { esRef.current?.close(); setStatus('closed') }
  }, [connect, enabled])

  return { status, lastEvent }
}

// Usage
function LiveMetric() {
  const [values, setValues] = useState<number[]>([])
  const { status, lastEvent } = useEventSource('/api/events', {
    onMessage: (e) => {
      const data = JSON.parse(e.data)
      if (data.type === 'metric') {
        setValues(prev => [...prev.slice(-19), data.value])
      }
    }
  })

  return (
    <div className="bg-white rounded-2xl border p-5">
      <div className="flex items-center gap-2 mb-3">
        <div className={`w-2 h-2 rounded-full ${status === 'open' ? 'bg-emerald-500 animate-pulse' : 'bg-gray-300'}`} />
        <span className="text-xs text-gray-500 capitalize">{status}</span>
      </div>
      <div className="flex items-end gap-0.5 h-16">
        {values.map((v, i) => (
          <div key={i} className="flex-1 bg-indigo-500 rounded-sm transition-all" style={{ height: `${v}%`, opacity: 0.4 + (i / values.length) * 0.6 }} />
        ))}
      </div>
    </div>
  )
}
```

---

## Step 943: WebSocket Server (Socket.io with Node.js)

```ts
// server.ts (custom Next.js server)
import { createServer } from 'http'
import { parse }        from 'url'
import next             from 'next'
import { Server }       from 'socket.io'

const dev = process.env.NODE_ENV !== 'production'
const app  = next({ dev })
const handle = app.getRequestHandler()

app.prepare().then(() => {
  const httpServer = createServer((req, res) => {
    const parsedUrl = parse(req.url!, true)
    handle(req, res, parsedUrl)
  })

  const io = new Server(httpServer, {
    cors: { origin: process.env.NEXTAUTH_URL, methods: ['GET', 'POST'] },
  })

  // Namespace for chat
  const chat = io.of('/chat')
  chat.on('connection', (socket) => {
    console.log('User connected:', socket.id)

    socket.on('join-room', (roomId: string) => {
      socket.join(roomId)
      chat.to(roomId).emit('user-joined', { userId: socket.id, roomId })
    })

    socket.on('send-message', ({ roomId, message, userId }: { roomId: string; message: string; userId: string }) => {
      const data = { id: Date.now().toString(), roomId, message, userId, timestamp: new Date().toISOString() }
      chat.to(roomId).emit('new-message', data)
      // await db.message.create({ data })
    })

    socket.on('typing', ({ roomId, userId }: { roomId: string; userId: string }) => {
      socket.to(roomId).emit('user-typing', userId)
    })

    socket.on('disconnect', () => {
      console.log('User disconnected:', socket.id)
    })
  })

  httpServer.listen(3000, () => {
    console.log('> Ready on http://localhost:3000')
  })
})
```

---

## Step 944: Socket.io Client

```tsx
// src/hooks/useSocket.ts
import { useEffect, useRef } from 'react'
import { io, Socket }        from 'socket.io-client'

let socket: Socket | null = null

export function useSocket(namespace = '/') {
  const socketRef = useRef<Socket>()

  useEffect(() => {
    if (!socket) {
      socket = io(namespace, {
        path:       '/socket.io',
        transports: ['websocket', 'polling'],
      })
    }
    socketRef.current = socket
    return () => { /* don't disconnect on unmount — singleton */ }
  }, [namespace])

  return socketRef.current
}

// src/components/ChatRoom.tsx
'use client'
import { useState, useEffect, useRef } from 'react'
import { io } from 'socket.io-client'

interface Message { id: string; message: string; userId: string; timestamp: string }

export function ChatRoom({ roomId, userId }: { roomId: string; userId: string }) {
  const [messages, setMessages] = useState<Message[]>([])
  const [input, setInput]       = useState('')
  const [typing, setTyping]     = useState<string[]>([])
  const [connected, setConnected] = useState(false)
  const socketRef = useRef<any>(null)
  const endRef    = useRef<HTMLDivElement>(null)

  useEffect(() => {
    const s = io('/chat')
    socketRef.current = s

    s.on('connect',     () => { setConnected(true); s.emit('join-room', roomId) })
    s.on('disconnect',  () => setConnected(false))
    s.on('new-message', (msg: Message) => setMessages(prev => [...prev, msg]))
    s.on('user-typing', (uid: string) => {
      setTyping(prev => [...new Set([...prev, uid])])
      setTimeout(() => setTyping(prev => prev.filter(u => u !== uid)), 2000)
    })

    return () => { s.disconnect() }
  }, [roomId])

  useEffect(() => { endRef.current?.scrollIntoView({ behavior: 'smooth' }) }, [messages])

  function sendMessage() {
    if (!input.trim()) return
    socketRef.current?.emit('send-message', { roomId, message: input, userId })
    setInput('')
  }

  return (
    <div className="flex flex-col h-96 bg-white rounded-2xl border overflow-hidden">
      {/* Header */}
      <div className="flex items-center gap-2 px-4 py-3 border-b">
        <div className={`w-2 h-2 rounded-full ${connected ? 'bg-emerald-500' : 'bg-gray-300'}`} />
        <span className="text-sm font-semibold text-gray-900">Room: {roomId}</span>
      </div>

      {/* Messages */}
      <div className="flex-1 overflow-y-auto p-4 space-y-3">
        {messages.map(msg => (
          <div key={msg.id} className={`flex ${msg.userId === userId ? 'justify-end' : 'justify-start'}`}>
            <div className={`max-w-xs px-3 py-2 rounded-2xl text-sm ${msg.userId === userId ? 'bg-indigo-600 text-white' : 'bg-gray-100 text-gray-900'}`}>
              {msg.message}
            </div>
          </div>
        ))}
        {typing.length > 0 && (
          <div className="text-xs text-gray-400 italic">
            {typing.join(', ')} typing...
          </div>
        )}
        <div ref={endRef} />
      </div>

      {/* Input */}
      <div className="border-t px-3 py-2 flex gap-2">
        <input
          value={input}
          onChange={e => { setInput(e.target.value); socketRef.current?.emit('typing', { roomId, userId }) }}
          onKeyDown={e => e.key === 'Enter' && sendMessage()}
          placeholder="Type a message..."
          className="flex-1 px-3 py-2 text-sm border rounded-xl focus:outline-none focus:ring-2 focus:ring-indigo-500"
        />
        <button onClick={sendMessage} className="px-4 py-2 bg-indigo-600 text-white text-sm font-semibold rounded-xl hover:bg-indigo-700">Send</button>
      </div>
    </div>
  )
}
```

---

## Step 945: Real-Time Notifications

```tsx
// src/components/NotificationBell.tsx
'use client'
import { useState, useEffect } from 'react'
import { useEventSource }      from '@/hooks/useEventSource'

interface Notification { id: string; type: string; message: string; read: boolean; createdAt: string }

export function NotificationBell({ userId }: { userId: string }) {
  const [notifications, setNotifications] = useState<Notification[]>([])
  const [open, setOpen] = useState(false)
  const unread = notifications.filter(n => !n.read).length

  useEventSource(`/api/notifications/stream?userId=${userId}`, {
    onMessage: (e) => {
      const data = JSON.parse(e.data)
      if (data.type === 'notification') {
        setNotifications(prev => [data.notification, ...prev])
      }
    }
  })

  function markAllRead() {
    setNotifications(prev => prev.map(n => ({ ...n, read: true })))
    // await fetch('/api/notifications/read-all', { method: 'POST' })
  }

  return (
    <div className="relative">
      <button onClick={() => setOpen(o => !o)} className="relative p-2 rounded-xl hover:bg-gray-100 transition-colors">
        🔔
        {unread > 0 && (
          <span className="absolute -top-0.5 -right-0.5 w-4 h-4 bg-red-500 text-white text-[10px] rounded-full flex items-center justify-center font-bold">
            {unread > 9 ? '9+' : unread}
          </span>
        )}
      </button>

      {open && (
        <div className="absolute right-0 top-12 w-80 bg-white rounded-2xl border shadow-xl z-50 overflow-hidden animate-slide-down">
          <div className="flex items-center justify-between px-4 py-3 border-b">
            <h3 className="font-bold text-gray-900 text-sm">Notifications</h3>
            {unread > 0 && <button onClick={markAllRead} className="text-xs text-indigo-600 hover:text-indigo-700">Mark all read</button>}
          </div>
          <div className="max-h-80 overflow-y-auto">
            {notifications.length === 0 ? (
              <p className="text-center text-sm text-gray-400 py-6">No notifications</p>
            ) : notifications.map(n => (
              <div key={n.id} className={`px-4 py-3 border-b last:border-b-0 ${!n.read ? 'bg-indigo-50' : ''}`}>
                <p className="text-sm text-gray-900">{n.message}</p>
                <p className="text-xs text-gray-400 mt-0.5">{new Date(n.createdAt).toRelativeString?.() ?? n.createdAt}</p>
              </div>
            ))}
          </div>
        </div>
      )}
    </div>
  )
}
```

---

## Step 946: Optimistic Updates with Rollback

```tsx
// src/hooks/useOptimisticList.ts
import { useState, useCallback } from 'react'

export function useOptimisticList<T extends { id: number | string }>(initialItems: T[]) {
  const [items, setItems] = useState(initialItems)

  const optimisticAdd = useCallback((tempItem: T, apiCall: () => Promise<T>): Promise<void> => {
    // Immediately add with temp data
    setItems(prev => [...prev, tempItem])

    return apiCall()
      .then(realItem => {
        // Replace temp item with real data from server
        setItems(prev => prev.map(i => i.id === tempItem.id ? realItem : i))
      })
      .catch(() => {
        // Rollback on error
        setItems(prev => prev.filter(i => i.id !== tempItem.id))
        throw new Error('Failed to add item')
      })
  }, [])

  const optimisticRemove = useCallback((id: T['id'], apiCall: () => Promise<void>): Promise<void> => {
    const snapshot = items.find(i => i.id === id)
    setItems(prev => prev.filter(i => i.id !== id))

    return apiCall().catch(() => {
      if (snapshot) setItems(prev => [...prev, snapshot])
      throw new Error('Failed to remove item')
    })
  }, [items])

  const optimisticUpdate = useCallback((id: T['id'], updates: Partial<T>, apiCall: () => Promise<T>): Promise<void> => {
    const snapshot = items.find(i => i.id === id)
    setItems(prev => prev.map(i => i.id === id ? { ...i, ...updates } : i))

    return apiCall()
      .then(updated => setItems(prev => prev.map(i => i.id === id ? updated : i)))
      .catch(() => {
        if (snapshot) setItems(prev => prev.map(i => i.id === id ? snapshot : i))
        throw new Error('Failed to update item')
      })
  }, [items])

  return { items, setItems, optimisticAdd, optimisticRemove, optimisticUpdate }
}
```

---

## Step 947: Live Dashboard

```tsx
// src/app/dashboard/live/page.tsx
'use client'
import { useState, useEffect } from 'react'
import { useEventSource }      from '@/hooks/useEventSource'

interface DashboardData {
  activeUsers:   number
  ordersToday:   number
  revenue:       number
  pendingOrders: number
}

export default function LiveDashboard() {
  const [data, setData] = useState<DashboardData>({ activeUsers: 0, ordersToday: 0, revenue: 0, pendingOrders: 0 })
  const [history, setHistory] = useState<{ value: number; time: number }[]>([])
  const [lastUpdated, setLastUpdated] = useState<Date | null>(null)

  useEventSource('/api/dashboard/stream', {
    onMessage: (e) => {
      const event = JSON.parse(e.data)
      if (event.type === 'update') {
        setData(event.data)
        setLastUpdated(new Date())
        setHistory(prev => [...prev.slice(-29), { value: event.data.activeUsers, time: Date.now() }])
      }
    }
  })

  return (
    <div className="p-6 space-y-6">
      <div className="flex items-center gap-3">
        <h1 className="text-2xl font-extrabold text-gray-900">Live Dashboard</h1>
        <div className="flex items-center gap-1.5">
          <div className="w-2 h-2 rounded-full bg-emerald-500 animate-pulse" />
          <span className="text-xs text-gray-500">Live</span>
        </div>
        {lastUpdated && <span className="ml-auto text-xs text-gray-400">Updated {lastUpdated.toLocaleTimeString()}</span>}
      </div>

      <div className="grid grid-cols-2 lg:grid-cols-4 gap-4">
        <LiveKpiCard label="Active Users"    value={data.activeUsers}   format="number" color="indigo" />
        <LiveKpiCard label="Orders Today"    value={data.ordersToday}   format="number" color="emerald" />
        <LiveKpiCard label="Revenue"         value={data.revenue}       format="currency" color="violet" />
        <LiveKpiCard label="Pending Orders"  value={data.pendingOrders} format="number" color="amber" />
      </div>
    </div>
  )
}

function LiveKpiCard({ label, value, format, color }: { label: string; value: number; format: string; color: string }) {
  const colors: Record<string, string> = { indigo: 'bg-indigo-50 text-indigo-600', emerald: 'bg-emerald-50 text-emerald-600', violet: 'bg-violet-50 text-violet-600', amber: 'bg-amber-50 text-amber-600' }
  const displayValue = format === 'currency' ? `$${value.toLocaleString()}` : value.toLocaleString()

  return (
    <div className={`rounded-2xl p-5 ${colors[color]}`}>
      <p className="text-xs font-medium opacity-70 mb-1">{label}</p>
      <p className="text-3xl font-extrabold tabular-nums">{displayValue}</p>
    </div>
  )
}
```

---

## Step 948: Presence System

```ts
// Track who is online (using Redis in production)
// Simple in-memory version for demo

const presenceMap = new Map<string, { userId: string; lastSeen: Date; page: string }>()

export function updatePresence(userId: string, page: string) {
  presenceMap.set(userId, { userId, lastSeen: new Date(), page })
}

export function getOnlineUsers(): string[] {
  const fiveMinutesAgo = new Date(Date.now() - 5 * 60 * 1000)
  return Array.from(presenceMap.values())
    .filter(p => p.lastSeen > fiveMinutesAgo)
    .map(p => p.userId)
}

export function removePresence(userId: string) {
  presenceMap.delete(userId)
}

// Heartbeat API
// GET /api/presence/heartbeat?page=/dashboard
// Called every 30s from client
```

---

## Step 949: Collaborative Editing

```tsx
// Simple collaborative text with OT (Operational Transform) concept
// Production: use Yjs + y-websocket

import { useState, useEffect } from 'react'

interface Operation {
  type:     'insert' | 'delete'
  position: number
  char?:    string
  userId:   string
  timestamp: number
}

function applyOp(text: string, op: Operation): string {
  if (op.type === 'insert' && op.char) {
    return text.slice(0, op.position) + op.char + text.slice(op.position)
  }
  if (op.type === 'delete') {
    return text.slice(0, op.position) + text.slice(op.position + 1)
  }
  return text
}

export function CollaborativeEditor({ docId, userId }: { docId: string; userId: string }) {
  const [content, setContent]   = useState('')
  const [collaborators, setCollaborators] = useState<string[]>([])
  // In production: use socket.io to broadcast ops
  // socket.emit('op', operation)
  // socket.on('op', op => setContent(c => applyOp(c, op)))

  return (
    <div className="bg-white rounded-2xl border overflow-hidden">
      <div className="flex items-center gap-2 px-4 py-2 border-b bg-gray-50">
        <span className="text-xs font-semibold text-gray-700">Doc: {docId}</span>
        <div className="flex -space-x-1 ml-auto">
          {collaborators.map(uid => (
            <div key={uid} className="w-6 h-6 rounded-full bg-indigo-500 border-2 border-white flex items-center justify-center text-white text-[10px] font-bold">
              {uid[0].toUpperCase()}
            </div>
          ))}
        </div>
      </div>
      <textarea
        value={content}
        onChange={e => setContent(e.target.value)}
        className="w-full p-4 text-sm font-mono resize-none focus:outline-none h-48"
        placeholder="Start typing... (collaborative)"
      />
    </div>
  )
}
```

---

## Step 950: Workshop — Live Chat Demo

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Live Chat Demo</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    @keyframes bounce-in { 0%{transform:scale(0.8);opacity:0} 100%{transform:scale(1);opacity:1} }
    .msg-enter { animation: bounce-in 0.2s ease; }
  </style>
</head>
<body class="bg-gray-100 min-h-screen flex items-center justify-center p-4">
<div class="w-full max-w-sm">
  <!-- User picker -->
  <div id="picker" class="bg-white rounded-2xl border p-6 text-center">
    <h2 class="font-extrabold text-gray-900 text-lg mb-4">Chat Demo</h2>
    <div class="grid grid-cols-2 gap-2">
      <button onclick="setUser('Alice','bg-indigo-500')" class="py-3 rounded-xl bg-indigo-50 text-indigo-700 font-semibold hover:bg-indigo-100">💜 Alice</button>
      <button onclick="setUser('Bob','bg-emerald-500')"  class="py-3 rounded-xl bg-emerald-50 text-emerald-700 font-semibold hover:bg-emerald-100">💚 Bob</button>
    </div>
  </div>

  <!-- Chat UI (hidden until user selected) -->
  <div id="chat" class="hidden bg-white rounded-2xl border overflow-hidden flex flex-col h-96">
    <div class="px-4 py-3 border-b flex items-center gap-2">
      <div id="user-avatar" class="w-7 h-7 rounded-full flex items-center justify-center text-white text-xs font-bold"></div>
      <span id="user-name" class="font-semibold text-sm text-gray-900"></span>
      <button onclick="resetUser()" class="ml-auto text-xs text-gray-400 hover:text-gray-600">Change</button>
    </div>
    <div id="messages" class="flex-1 overflow-y-auto p-3 space-y-2"></div>
    <div id="typing-indicator" class="px-3 pb-1 text-xs text-gray-400 italic min-h-[20px]"></div>
    <div class="border-t px-3 py-2 flex gap-2">
      <input id="msg-input" placeholder="Type a message..." oninput="onType()" onkeydown="if(event.key==='Enter')send()"
        class="flex-1 px-3 py-2 border rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
      <button onclick="send()" class="px-3 py-2 bg-indigo-600 text-white text-xs font-bold rounded-xl">→</button>
    </div>
  </div>
</div>
<script>
  let userName = '', userColor = '';
  // Shared via BroadcastChannel (cross-tab communication)
  const bc = new BroadcastChannel('demo-chat');
  let typingTimer;

  function setUser(name, color) {
    userName = name; userColor = color;
    document.getElementById('picker').classList.add('hidden');
    document.getElementById('chat').classList.remove('hidden');
    document.getElementById('user-avatar').className = `w-7 h-7 rounded-full flex items-center justify-center text-white text-xs font-bold ${color}`;
    document.getElementById('user-avatar').textContent = name[0];
    document.getElementById('user-name').textContent = name;
    document.getElementById('msg-input').focus();
  }

  function resetUser() {
    document.getElementById('picker').classList.remove('hidden');
    document.getElementById('chat').classList.add('hidden');
    userName = '';
  }

  function send() {
    const input = document.getElementById('msg-input');
    const text = input.value.trim();
    if (!text || !userName) return;
    const msg = { user: userName, color: userColor, text, time: Date.now() };
    bc.postMessage({ type: 'message', msg });
    addMessage(msg, true);
    input.value = '';
    bc.postMessage({ type: 'stop-typing', user: userName });
  }

  function onType() {
    if (!userName) return;
    bc.postMessage({ type: 'typing', user: userName });
    clearTimeout(typingTimer);
    typingTimer = setTimeout(() => bc.postMessage({ type: 'stop-typing', user: userName }), 1500);
  }

  function addMessage(msg, isMine) {
    const el = document.createElement('div');
    el.className = `msg-enter flex ${isMine ? 'justify-end' : 'justify-start'}`;
    el.innerHTML = `<div class="max-w-[70%] px-3 py-1.5 rounded-2xl text-sm ${isMine ? 'bg-indigo-600 text-white' : 'bg-gray-100 text-gray-900'}">
      ${!isMine ? `<span class="block text-[10px] font-bold mb-0.5" style="color:inherit;opacity:0.6">${msg.user}</span>` : ''}
      ${msg.text}</div>`;
    const msgs = document.getElementById('messages');
    msgs.appendChild(el);
    msgs.scrollTop = msgs.scrollHeight;
  }

  bc.onmessage = ({ data }) => {
    if (data.type === 'message' && data.msg.user !== userName) {
      addMessage(data.msg, false);
    } else if (data.type === 'typing' && data.user !== userName) {
      document.getElementById('typing-indicator').textContent = `${data.user} is typing...`;
    } else if (data.type === 'stop-typing') {
      document.getElementById('typing-indicator').textContent = '';
    }
  };
</script>
</body>
</html>
```

---

## สรุป Part 95

| Step | เนื้อหา |
|------|---------|
| 941 | Server-Sent Events: ReadableStream API route (Edge Runtime) |
| 942 | useEventSource hook: connect/reconnect/cleanup |
| 943 | Socket.io server: namespaces, rooms, join/message/typing |
| 944 | Socket.io client: ChatRoom component + typing indicator |
| 945 | Real-time notification bell: SSE + unread badge + mark-read |
| 946 | useOptimisticList: add/remove/update with rollback |
| 947 | Live dashboard: SSE + KPI cards with pulse indicator |
| 948 | Presence system: heartbeat, online users, last-seen |
| 949 | Collaborative editing concept with OT |
| 950 | Workshop: Live cross-tab chat with BroadcastChannel |

**Part ถัดไป:** Part 96 — Performance at Scale (Steps 951–960)
