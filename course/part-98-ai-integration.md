# Part 98: AI Integration

## เป้าหมาย
- Vercel AI SDK + Claude API
- Streaming responses
- AI-powered UI components
- RAG pattern (Retrieval Augmented Generation)
- Steps 971–980

---

## Step 971: Vercel AI SDK Setup

```bash
npm install ai @anthropic-ai/sdk
```

```ts
// src/app/api/chat/route.ts
import Anthropic                  from '@anthropic-ai/sdk'
import { AnthropicStream, StreamingTextResponse } from 'ai'

const anthropic = new Anthropic({ apiKey: process.env.ANTHROPIC_API_KEY! })

export async function POST(request: Request) {
  const { messages } = await request.json()

  const response = await anthropic.messages.create({
    model:       'claude-sonnet-4-6',
    max_tokens:  1024,
    stream:      true,
    system:      'You are a helpful assistant for our e-commerce platform. Answer questions about products, orders, and support issues in a friendly, concise way.',
    messages:    messages.map((m: any) => ({ role: m.role, content: m.content })),
  })

  const stream = AnthropicStream(response)
  return new StreamingTextResponse(stream)
}
```

---

## Step 972: Chat UI Component

```tsx
// src/components/AiChat.tsx
'use client'
import { useChat } from 'ai/react'
import { useRef, useEffect } from 'react'

export function AiChat() {
  const { messages, input, handleInputChange, handleSubmit, isLoading, error } = useChat({
    api: '/api/chat',
  })
  const endRef = useRef<HTMLDivElement>(null)

  useEffect(() => {
    endRef.current?.scrollIntoView({ behavior: 'smooth' })
  }, [messages])

  return (
    <div className="flex flex-col h-[600px] bg-white rounded-2xl border overflow-hidden shadow-sm">
      {/* Header */}
      <div className="px-4 py-3 border-b bg-indigo-600 text-white flex items-center gap-2">
        <div className="w-7 h-7 rounded-full bg-white/20 flex items-center justify-center text-sm">AI</div>
        <div>
          <p className="font-semibold text-sm">AI Assistant</p>
          <p className="text-xs opacity-75">Powered by Claude</p>
        </div>
        <div className="ml-auto flex items-center gap-1.5">
          <div className={`w-2 h-2 rounded-full ${isLoading ? 'bg-amber-400 animate-pulse' : 'bg-emerald-400'}`} />
          <span className="text-xs opacity-75">{isLoading ? 'Thinking...' : 'Ready'}</span>
        </div>
      </div>

      {/* Messages */}
      <div className="flex-1 overflow-y-auto p-4 space-y-4">
        {messages.length === 0 && (
          <div className="text-center py-8">
            <div className="w-12 h-12 rounded-2xl bg-indigo-100 mx-auto mb-3 flex items-center justify-center text-2xl">🤖</div>
            <p className="text-gray-500 text-sm">Ask me anything about our products or your orders!</p>
          </div>
        )}

        {messages.map(m => (
          <div key={m.id} className={`flex ${m.role === 'user' ? 'justify-end' : 'justify-start'}`}>
            {m.role === 'assistant' && (
              <div className="w-7 h-7 rounded-full bg-indigo-100 flex items-center justify-center text-sm mr-2 flex-shrink-0 mt-0.5">AI</div>
            )}
            <div className={`max-w-[80%] rounded-2xl px-4 py-2.5 text-sm leading-relaxed ${
              m.role === 'user'
                ? 'bg-indigo-600 text-white rounded-br-sm'
                : 'bg-gray-100 text-gray-900 rounded-bl-sm'
            }`}>
              {m.content}
            </div>
          </div>
        ))}

        {isLoading && (
          <div className="flex justify-start">
            <div className="w-7 h-7 rounded-full bg-indigo-100 flex items-center justify-center text-sm mr-2">AI</div>
            <div className="bg-gray-100 rounded-2xl rounded-bl-sm px-4 py-3 flex gap-1">
              {[0,1,2].map(i => <span key={i} className="w-2 h-2 bg-gray-400 rounded-full animate-bounce" style={{ animationDelay: `${i*0.15}s` }} />)}
            </div>
          </div>
        )}
        <div ref={endRef} />
      </div>

      {/* Input */}
      <form onSubmit={handleSubmit} className="border-t px-3 py-2 flex gap-2">
        <input
          value={input}
          onChange={handleInputChange}
          placeholder="Ask anything..."
          disabled={isLoading}
          className="flex-1 px-3 py-2 text-sm border rounded-xl focus:outline-none focus:ring-2 focus:ring-indigo-500 disabled:opacity-50"
        />
        <button
          type="submit"
          disabled={isLoading || !input.trim()}
          className="px-4 py-2 bg-indigo-600 text-white text-sm font-semibold rounded-xl hover:bg-indigo-700 disabled:opacity-50 transition-colors"
        >
          →
        </button>
      </form>
    </div>
  )
}
```

---

## Step 973: AI-Powered Product Search

```ts
// src/app/api/search/route.ts — semantic search with embeddings
import Anthropic from '@anthropic-ai/sdk'
import { db }    from '@/lib/db'

const anthropic = new Anthropic()

export async function POST(request: Request) {
  const { query } = await request.json()
  if (!query) return Response.json({ error: 'Query required' }, { status: 400 })

  // 1. Ask Claude to extract search intent
  const intentResponse = await anthropic.messages.create({
    model:     'claude-haiku-4-5-20251001',
    max_tokens: 200,
    messages: [{
      role:    'user',
      content: `Extract search intent from: "${query}". Return JSON only: { "keywords": [...], "category": "...", "maxPrice": number|null, "attributes": {...} }`,
    }],
  })

  let intent = { keywords: [query], category: '', maxPrice: null, attributes: {} }
  try {
    const text = intentResponse.content[0].type === 'text' ? intentResponse.content[0].text : ''
    const jsonMatch = text.match(/\{[\s\S]*\}/)
    if (jsonMatch) intent = JSON.parse(jsonMatch[0])
  } catch {}

  // 2. Use extracted intent for database search
  const products = await db.product.findMany({
    where: {
      isActive: true,
      ...(intent.category && { category: { slug: intent.category } }),
      ...(intent.maxPrice  && { price: { lte: intent.maxPrice } }),
      ...(intent.keywords.length > 0 && {
        OR: intent.keywords.map(kw => ({
          OR: [
            { name:        { contains: kw, mode: 'insensitive' as const } },
            { description: { contains: kw, mode: 'insensitive' as const } },
          ],
        })),
      }),
    },
    take: 10,
    include: { category: true },
  })

  return Response.json({ products, intent })
}
```

---

## Step 974: AI Content Generation

```ts
// src/app/api/ai/generate/route.ts
import Anthropic from '@anthropic-ai/sdk'

const anthropic = new Anthropic()

export async function POST(request: Request) {
  const { type, context } = await request.json()

  const prompts: Record<string, string> = {
    'product-description': `Write a compelling product description for: "${context.name}" in the ${context.category} category. Price: $${context.price}. Be concise (2-3 sentences), focus on benefits. Return plain text only.`,
    'email-subject':       `Create 5 engaging email subject lines for: "${context.campaign}". Return as JSON array of strings.`,
    'seo-meta':            `Create SEO meta title (max 60 chars) and description (max 155 chars) for: "${context.page}". Return JSON: {"title": "...", "description": "..."}`,
    'review-summary':      `Summarize these ${context.count} reviews with average ${context.avgRating}/5 stars. Key themes: ${context.themes}. 2 sentences max.`,
  }

  const prompt = prompts[type]
  if (!prompt) return Response.json({ error: 'Unknown type' }, { status: 400 })

  const response = await anthropic.messages.create({
    model:       'claude-haiku-4-5-20251001',
    max_tokens:  512,
    messages:    [{ role: 'user', content: prompt }],
  })

  const content = response.content[0].type === 'text' ? response.content[0].text : ''
  return Response.json({ content })
}
```

---

## Step 975: AI Form Autofill

```tsx
// src/components/AiProductForm.tsx
'use client'
import { useState } from 'react'

interface ProductFormData {
  name:        string
  description: string
  price:       string
  category:    string
  tags:        string
}

export function AiProductForm() {
  const [form, setForm]     = useState<ProductFormData>({ name:'', description:'', price:'', category:'', tags:'' })
  const [loading, setLoading] = useState(false)

  async function aiAutofill() {
    if (!form.name) return
    setLoading(true)
    try {
      const res = await fetch('/api/ai/generate', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ type: 'product-description', context: { name: form.name, category: form.category || 'general', price: form.price } }),
      })
      const { content } = await res.json()
      setForm(f => ({ ...f, description: content }))
    } finally {
      setLoading(false)
    }
  }

  return (
    <form className="space-y-4 bg-white rounded-2xl border p-6">
      <div>
        <label className="block text-sm font-medium text-gray-700 mb-1.5">Product Name *</label>
        <input value={form.name} onChange={e => setForm(f=>({...f,name:e.target.value}))}
          className="w-full px-3 py-2 border rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500" />
      </div>
      <div>
        <div className="flex items-center justify-between mb-1.5">
          <label className="text-sm font-medium text-gray-700">Description</label>
          <button type="button" onClick={aiAutofill} disabled={loading || !form.name}
            className="flex items-center gap-1.5 px-3 py-1 text-xs font-medium text-indigo-600 border border-indigo-200 rounded-lg hover:bg-indigo-50 disabled:opacity-50 transition-colors">
            {loading ? <span className="w-3 h-3 border-2 border-indigo-600 border-t-transparent rounded-full animate-spin" /> : '✨'}
            AI Generate
          </button>
        </div>
        <textarea value={form.description} onChange={e => setForm(f=>({...f,description:e.target.value}))}
          rows={4} className="w-full px-3 py-2 border rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 resize-none" />
      </div>
      <div className="grid grid-cols-2 gap-3">
        <div>
          <label className="block text-sm font-medium text-gray-700 mb-1.5">Price ($)</label>
          <input type="number" value={form.price} onChange={e => setForm(f=>({...f,price:e.target.value}))}
            className="w-full px-3 py-2 border rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500" />
        </div>
        <div>
          <label className="block text-sm font-medium text-gray-700 mb-1.5">Category</label>
          <select value={form.category} onChange={e => setForm(f=>({...f,category:e.target.value}))}
            className="w-full px-3 py-2 border rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 bg-white">
            <option value="">Select...</option>
            <option>Electronics</option><option>Clothing</option><option>Books</option>
          </select>
        </div>
      </div>
      <button type="submit" className="w-full py-2.5 bg-indigo-600 text-white text-sm font-semibold rounded-xl hover:bg-indigo-700">Save Product</button>
    </form>
  )
}
```

---

## Step 976: RAG Pattern (Retrieval Augmented Generation)

```ts
// RAG: Answer questions using your own data
// src/app/api/rag/route.ts

import Anthropic from '@anthropic-ai/sdk'
import { db }    from '@/lib/db'

const anthropic = new Anthropic()

export async function POST(request: Request) {
  const { question } = await request.json()

  // 1. Retrieve relevant context from DB
  const [products, faqs] = await Promise.all([
    db.product.findMany({
      where:  { isActive: true, name: { contains: question.split(' ')[0], mode: 'insensitive' } },
      take:   3,
      select: { name: true, description: true, price: true },
    }),
    // In production: search a faqs table or vector store
    Promise.resolve([
      { question: 'What is your return policy?', answer: '30-day money-back guarantee.' },
      { question: 'How long does shipping take?', answer: '3-5 business days standard, 1-2 days express.' },
    ])
  ])

  // 2. Build context string
  const context = [
    products.length > 0 ? `Products: ${JSON.stringify(products)}` : '',
    `FAQs: ${JSON.stringify(faqs)}`,
  ].filter(Boolean).join('\n\n')

  // 3. Ask Claude with context (RAG)
  const response = await anthropic.messages.create({
    model:     'claude-sonnet-4-6',
    max_tokens: 512,
    system: `You are a helpful customer support agent. Use ONLY the provided context to answer questions. If the answer is not in the context, say "I don't have that information" rather than making something up.\n\nContext:\n${context}`,
    messages: [{ role: 'user', content: question }],
  })

  const answer = response.content[0].type === 'text' ? response.content[0].text : ''
  return Response.json({ answer, sources: { products, faqs } })
}
```

---

## Step 977: AI Moderation

```ts
// src/lib/moderation.ts
import Anthropic from '@anthropic-ai/sdk'

const anthropic = new Anthropic()

interface ModerationResult {
  safe:     boolean
  category?: 'spam' | 'hate' | 'violence' | 'adult' | 'misleading'
  reason?:  string
}

export async function moderateContent(text: string): Promise<ModerationResult> {
  const response = await anthropic.messages.create({
    model:     'claude-haiku-4-5-20251001',
    max_tokens: 100,
    messages: [{
      role:    'user',
      content: `Moderate this user-generated content. Return JSON only: { "safe": boolean, "category": "spam"|"hate"|"violence"|"adult"|"misleading"|null, "reason": string|null }\n\nContent: "${text.slice(0, 500)}"`,
    }],
  })

  const content = response.content[0].type === 'text' ? response.content[0].text : ''
  try {
    const match = content.match(/\{[\s\S]*\}/)
    return match ? JSON.parse(match[0]) : { safe: true }
  } catch {
    return { safe: true } // fail open — let human review
  }
}

// Use in review submission
export async function submitReview({ productId, userId, comment, rating }: any) {
  if (comment) {
    const { safe, category, reason } = await moderateContent(comment)
    if (!safe) {
      return { success: false, error: `Review flagged: ${reason ?? category}` }
    }
  }
  // await db.review.create({ data: { productId, userId, comment, rating } })
  return { success: true }
}
```

---

## Step 978: AI Image Analysis

```ts
// src/app/api/ai/analyze-image/route.ts
import Anthropic from '@anthropic-ai/sdk'

const anthropic = new Anthropic()

export async function POST(request: Request) {
  const formData = await request.formData()
  const file = formData.get('image') as File
  if (!file) return Response.json({ error: 'No image' }, { status: 400 })

  const buffer = Buffer.from(await file.arrayBuffer())
  const base64 = buffer.toString('base64')
  const mediaType = file.type as 'image/jpeg' | 'image/png' | 'image/webp' | 'image/gif'

  const response = await anthropic.messages.create({
    model:     'claude-sonnet-4-6',
    max_tokens: 512,
    messages: [{
      role: 'user',
      content: [
        {
          type:   'image',
          source: { type: 'base64', media_type: mediaType, data: base64 },
        },
        {
          type: 'text',
          text: 'Analyze this product image. Return JSON: { "name": "...", "category": "...", "description": "...", "suggestedPrice": number, "tags": [...] }',
        },
      ],
    }],
  })

  const content = response.content[0].type === 'text' ? response.content[0].text : ''
  try {
    const match = content.match(/\{[\s\S]*\}/)
    return Response.json(match ? JSON.parse(match[0]) : { error: 'Parse error' })
  } catch {
    return Response.json({ error: 'Parse error' }, { status: 500 })
  }
}
```

---

## Step 979: AI Analytics Insights

```ts
// src/app/api/ai/insights/route.ts
import Anthropic from '@anthropic-ai/sdk'
import { getDashboardStats } from '@/lib/analytics'

const anthropic = new Anthropic()

export async function POST() {
  const stats = await getDashboardStats()

  const response = await anthropic.messages.create({
    model:     'claude-sonnet-4-6',
    max_tokens: 512,
    messages: [{
      role:    'user',
      content: `Analyze these business metrics and provide 3 actionable insights in JSON format:
        { "insights": [{ "title": "...", "finding": "...", "action": "..." }] }

        Metrics:
        - Total orders: ${stats.totalOrders}
        - Revenue: $${stats.totalRevenue}
        - Recent orders: ${stats.recentOrders.length}
        - Top products: ${JSON.stringify(stats.topProducts.slice(0,3))}`,
    }],
  })

  const content = response.content[0].type === 'text' ? response.content[0].text : ''
  try {
    const match = content.match(/\{[\s\S]*\}/)
    return Response.json(match ? JSON.parse(match[0]) : { insights: [] })
  } catch {
    return Response.json({ insights: [] })
  }
}
```

---

## Step 980: Workshop — AI Chat Demo

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AI Chat Demo</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    @keyframes bounce { 0%,100%{transform:translateY(0)} 50%{transform:translateY(-4px)} }
    .dot { animation: bounce 1s infinite; }
    .dot:nth-child(2){ animation-delay:.15s }
    .dot:nth-child(3){ animation-delay:.3s }
  </style>
</head>
<body class="bg-gray-100 min-h-screen flex items-center justify-center p-4">
<div class="w-full max-w-sm">
  <div class="bg-white rounded-2xl border shadow-sm overflow-hidden flex flex-col h-[520px]">
    <!-- Header -->
    <div class="px-4 py-3 bg-indigo-600 text-white flex items-center gap-2">
      <div class="w-7 h-7 rounded-full bg-white/20 flex items-center justify-center text-xs font-bold">AI</div>
      <div><p class="font-semibold text-sm">Shop Assistant</p><p class="text-xs opacity-70">Ask about products & orders</p></div>
    </div>

    <!-- Messages -->
    <div id="msgs" class="flex-1 overflow-y-auto p-3 space-y-3">
      <div class="flex gap-2">
        <div class="w-6 h-6 rounded-full bg-indigo-100 flex items-center justify-center text-xs flex-shrink-0">AI</div>
        <div class="bg-gray-100 rounded-2xl rounded-bl-sm px-3 py-2 text-sm text-gray-900 max-w-[80%]">
          สวัสดีครับ! ผมช่วยอะไรคุณได้บ้างเกี่ยวกับสินค้าหรือคำสั่งซื้อ?
        </div>
      </div>
    </div>

    <!-- Quick replies -->
    <div id="quick" class="px-3 pb-1 flex gap-1.5 flex-wrap">
      <button onclick="quickReply(this)" class="px-2.5 py-1 bg-indigo-50 text-indigo-700 text-xs rounded-full hover:bg-indigo-100 border border-indigo-200">สินค้าราคาถูก</button>
      <button onclick="quickReply(this)" class="px-2.5 py-1 bg-indigo-50 text-indigo-700 text-xs rounded-full hover:bg-indigo-100 border border-indigo-200">นโยบายคืนสินค้า</button>
      <button onclick="quickReply(this)" class="px-2.5 py-1 bg-indigo-50 text-indigo-700 text-xs rounded-full hover:bg-indigo-100 border border-indigo-200">การจัดส่ง</button>
    </div>

    <!-- Input -->
    <div class="border-t px-3 py-2 flex gap-2">
      <input id="input" placeholder="พิมพ์ข้อความ..." onkeydown="if(event.key==='Enter')sendMsg()"
        class="flex-1 px-3 py-2 border rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
      <button onclick="sendMsg()" class="px-3 py-2 bg-indigo-600 text-white text-xs font-bold rounded-xl">→</button>
    </div>
  </div>
</div>
<script>
const RESPONSES = {
  'สินค้าราคาถูก': 'เรามีสินค้าลดราคาหลายรายการครับ! หมวดอิเล็กทรอนิกส์ลด 20% และเสื้อผ้าลด 30% ทุกชิ้น ดูสินค้าทั้งหมดได้ที่หน้า Sale ครับ 🛍️',
  'นโยบายคืนสินค้า': 'เรามีนโยบายคืนสินค้าภายใน 30 วันครับ เพียงส่งสินค้ากลับมาในสภาพเดิมพร้อมกล่อง เราจะคืนเงินภายใน 3-5 วันทำการครับ ✅',
  'การจัดส่ง': 'การจัดส่งมาตรฐาน 3-5 วันทำการ ฟรีเมื่อซื้อครบ 500 บาท และจัดส่งด่วน 1-2 วันทำการ (มีค่าใช้จ่าย) ครับ 🚀',
};
const FALLBACK = ['ขอบคุณสำหรับคำถามครับ! กรุณาติดต่อทีมงานที่ support@shop.com หรือโทร 02-xxx-xxxx เพื่อข้อมูลเพิ่มเติม', 'ผมจะส่งคำถามของคุณให้ทีมงานครับ ทางเราจะตอบกลับภายใน 24 ชั่วโมง 📧', 'คำถามที่ดีมากครับ! ขณะนี้ผมยังไม่มีข้อมูลนั้น แต่ทีมงานจะช่วยคุณได้แน่นอนครับ'];
let busy=false;

function quickReply(btn){ document.getElementById('input').value=btn.textContent; sendMsg(); }

function sendMsg(){
  if(busy) return;
  const input=document.getElementById('input');
  const text=input.value.trim();
  if(!text) return;
  addMsg(text,'user');
  input.value='';
  busy=true;
  const typing=addTyping();
  setTimeout(()=>{
    typing.remove();
    const reply=RESPONSES[text]||FALLBACK[Math.floor(Math.random()*FALLBACK.length)];
    addMsg(reply,'ai');
    busy=false;
  }, 1000+Math.random()*800);
}

function addMsg(text,role){
  const msgs=document.getElementById('msgs');
  const d=document.createElement('div');
  d.className=`flex gap-2 ${role==='user'?'justify-end':''}`;
  d.innerHTML=role==='user'
    ?`<div class="bg-indigo-600 text-white rounded-2xl rounded-br-sm px-3 py-2 text-sm max-w-[80%]">${text}</div>`
    :`<div class="w-6 h-6 rounded-full bg-indigo-100 flex items-center justify-center text-xs flex-shrink-0">AI</div><div class="bg-gray-100 rounded-2xl rounded-bl-sm px-3 py-2 text-sm text-gray-900 max-w-[80%]">${text}</div>`;
  msgs.appendChild(d);
  msgs.scrollTop=msgs.scrollHeight;
}

function addTyping(){
  const msgs=document.getElementById('msgs');
  const d=document.createElement('div');
  d.className='flex gap-2';
  d.innerHTML=`<div class="w-6 h-6 rounded-full bg-indigo-100 flex items-center justify-center text-xs flex-shrink-0">AI</div><div class="bg-gray-100 rounded-2xl rounded-bl-sm px-3 py-2.5 flex gap-1"><span class="dot w-2 h-2 bg-gray-400 rounded-full"></span><span class="dot w-2 h-2 bg-gray-400 rounded-full"></span><span class="dot w-2 h-2 bg-gray-400 rounded-full"></span></div>`;
  msgs.appendChild(d);
  msgs.scrollTop=msgs.scrollHeight;
  return d;
}
</script>
</body>
</html>
```

---

## สรุป Part 98

| Step | เนื้อหา |
|------|---------|
| 971 | Vercel AI SDK + Anthropic Claude streaming API route |
| 972 | Chat UI: useChat hook + typing indicator + message bubbles |
| 973 | AI semantic search: intent extraction + structured DB query |
| 974 | AI content generation: product descriptions, SEO meta, emails |
| 975 | AI form autofill: "✨ Generate" button ใน product form |
| 976 | RAG pattern: retrieve DB context → augment Claude prompt |
| 977 | AI content moderation: auto-flag user reviews |
| 978 | AI image analysis: upload product photo → auto-fill form |
| 979 | AI analytics insights: metrics → actionable recommendations |
| 980 | Workshop: AI chat demo with quick replies + typing animation |

**Part ถัดไป:** Part 99 — Production Patterns & Final Review (Steps 981–990)
