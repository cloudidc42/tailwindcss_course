# Part 97: Security Hardening

## เป้าหมาย
- OWASP Top 10 prevention
- Input validation & sanitization
- CSRF, XSS, SQL injection protection
- Security headers
- Steps 961–970

---

## Step 961: XSS Prevention

```ts
// ❌ XSS vulnerability
function BadComponent({ userInput }: { userInput: string }) {
  return <div dangerouslySetInnerHTML={{ __html: userInput }} />
  // If userInput = '<script>document.cookie</script>' → XSS!
}

// ✅ React auto-escapes by default
function GoodComponent({ userInput }: { userInput: string }) {
  return <div>{userInput}</div>  // Auto-escaped by React
}

// ✅ When you MUST render HTML (from trusted CMS)
import DOMPurify from 'isomorphic-dompurify'

function SafeHtml({ html }: { html: string }) {
  const clean = DOMPurify.sanitize(html, {
    ALLOWED_TAGS:  ['p', 'b', 'i', 'em', 'strong', 'a', 'ul', 'ol', 'li', 'h1', 'h2', 'h3', 'br'],
    ALLOWED_ATTR:  ['href', 'target', 'rel'],
    FORCE_BODY:    true,
    ADD_ATTR:      ['rel'],
  })

  // Force noopener on external links
  const withNoopener = clean.replace(/<a /g, '<a rel="noopener noreferrer" ')

  return <div className="prose" dangerouslySetInnerHTML={{ __html: withNoopener }} />
}
```

---

## Step 962: SQL Injection Prevention

```ts
// ❌ SQL injection vulnerability
async function badGetUser(email: string) {
  // If email = "admin@x.com' OR '1'='1" → returns all users!
  return db.$queryRawUnsafe(`SELECT * FROM users WHERE email = '${email}'`)
}

// ✅ Parameterized queries (Prisma does this automatically)
async function goodGetUser(email: string) {
  return db.user.findUnique({ where: { email } })
  // Prisma uses parameterized queries internally
}

// ✅ Raw SQL: use $queryRaw with template literals (parameterized)
async function searchUsers(query: string) {
  return db.$queryRaw`
    SELECT id, name, email FROM users
    WHERE (name ILIKE ${`%${query}%`} OR email ILIKE ${`%${query}%`})
    AND "isActive" = true
    LIMIT 20
  `
  // Parameters are properly escaped by Prisma
}

// ✅ Input validation before any DB call
import { z } from 'zod'

const EmailSchema = z.string().email().max(255)

export async function getUserByEmail(email: string) {
  const validEmail = EmailSchema.parse(email) // throws if invalid
  return db.user.findUnique({ where: { email: validEmail } })
}
```

---

## Step 963: CSRF Protection

```ts
// Auth.js (NextAuth) includes built-in CSRF protection for form actions.
// For custom API routes:

// src/lib/csrf.ts
import { createHmac, randomBytes } from 'crypto'

const SECRET = process.env.CSRF_SECRET ?? process.env.AUTH_SECRET!

export function generateCsrfToken(): { token: string; hash: string } {
  const token = randomBytes(32).toString('hex')
  const hash  = createHmac('sha256', SECRET).update(token).digest('hex')
  return { token, hash }
}

export function verifyCsrfToken(token: string, hash: string): boolean {
  const expected = createHmac('sha256', SECRET).update(token).digest('hex')
  // Constant-time comparison to prevent timing attacks
  return Buffer.from(expected, 'hex').equals(Buffer.from(hash, 'hex'))
}

// Middleware to verify CSRF on state-changing requests
export function withCsrf(handler: Function) {
  return async (req: NextRequest, ctx: any) => {
    if (['POST', 'PUT', 'PATCH', 'DELETE'].includes(req.method)) {
      const token = req.headers.get('X-CSRF-Token')
      const hash  = req.cookies.get('csrf-hash')?.value
      if (!token || !hash || !verifyCsrfToken(token, hash)) {
        return NextResponse.json({ error: 'Invalid CSRF token' }, { status: 403 })
      }
    }
    return handler(req, ctx)
  }
}
```

---

## Step 964: Content Security Policy

```ts
// src/middleware.ts — generate nonce for inline scripts
import { NextRequest, NextResponse } from 'next/server'
import { randomBytes } from 'crypto'

export function middleware(request: NextRequest) {
  const nonce = randomBytes(16).toString('base64')

  const cspHeader = [
    `default-src 'self'`,
    `script-src 'self' 'nonce-${nonce}' 'strict-dynamic'`,
    `style-src 'self' 'unsafe-inline' https://fonts.googleapis.com`,
    `img-src 'self' data: https: blob:`,
    `font-src 'self' https://fonts.gstatic.com`,
    `connect-src 'self' https://api.example.com wss:`,
    `frame-ancestors 'none'`,
    `base-uri 'self'`,
    `form-action 'self'`,
    `upgrade-insecure-requests`,
  ].join('; ')

  const requestHeaders = new Headers(request.headers)
  requestHeaders.set('x-nonce', nonce)

  const response = NextResponse.next({ request: { headers: requestHeaders } })
  response.headers.set('Content-Security-Policy', cspHeader)
  response.headers.set('X-Frame-Options', 'DENY')
  response.headers.set('X-Content-Type-Options', 'nosniff')
  response.headers.set('Referrer-Policy', 'strict-origin-when-cross-origin')
  response.headers.set('Permissions-Policy', 'camera=(), microphone=(), geolocation=(), payment=()')
  response.headers.set('Strict-Transport-Security', 'max-age=31536000; includeSubDomains; preload')

  return response
}
```

---

## Step 965: Input Sanitization

```ts
// src/lib/sanitize.ts
import DOMPurify from 'isomorphic-dompurify'

// Sanitize for display (HTML contexts)
export function sanitizeHtml(input: string): string {
  return DOMPurify.sanitize(input, { ALLOWED_TAGS: [], ALLOWED_ATTR: [] })
  // Strips ALL HTML tags — returns plain text
}

// Sanitize for rich content
export function sanitizeRichText(html: string): string {
  return DOMPurify.sanitize(html, {
    ALLOWED_TAGS: ['p','b','i','strong','em','a','ul','ol','li','br','h1','h2','h3','blockquote','code','pre'],
    ALLOWED_ATTR: ['href', 'class'],
  })
}

// Validate and sanitize common inputs
export const sanitizers = {
  name:  (v: string) => v.trim().replace(/[<>]/g, '').slice(0, 100),
  email: (v: string) => v.trim().toLowerCase().slice(0, 255),
  url:   (v: string) => {
    try {
      const url = new URL(v)
      if (!['http:', 'https:'].includes(url.protocol)) throw new Error()
      return url.href
    } catch { return '' }
  },
  phone: (v: string) => v.replace(/[^\d+\-\s()]/g, '').slice(0, 20),
  slug:  (v: string) => v.toLowerCase().replace(/[^a-z0-9-]/g, '-').replace(/-+/g, '-'),
}
```

---

## Step 966: Rate Limiting with Upstash Redis

```bash
npm install @upstash/ratelimit @upstash/redis
```

```ts
// src/lib/ratelimit.ts
import { Ratelimit } from '@upstash/ratelimit'
import { Redis }     from '@upstash/redis'

const redis = new Redis({
  url:   process.env.UPSTASH_REDIS_URL!,
  token: process.env.UPSTASH_REDIS_TOKEN!,
})

// Different limits for different endpoints
export const rateLimits = {
  api: new Ratelimit({
    redis,
    limiter: Ratelimit.slidingWindow(100, '1 m'),  // 100 req/min
    prefix: 'rl:api',
  }),
  auth: new Ratelimit({
    redis,
    limiter: Ratelimit.fixedWindow(5, '15 m'),    // 5 attempts per 15min
    prefix: 'rl:auth',
  }),
  upload: new Ratelimit({
    redis,
    limiter: Ratelimit.tokenBucket(10, '1 h', 10), // 10/hour
    prefix: 'rl:upload',
  }),
}

// Middleware helper
export async function checkRateLimit(
  key: string,
  limiter: typeof rateLimits.api
): Promise<{ success: boolean; remaining: number; resetAt: Date }> {
  const { success, remaining, reset } = await limiter.limit(key)
  return { success, remaining, resetAt: new Date(reset) }
}
```

---

## Step 967: Secure File Upload

```ts
// src/app/api/upload/route.ts
import { NextRequest, NextResponse }  from 'next/server'
import { auth }                       from '@/auth'

const ALLOWED_TYPES  = ['image/jpeg', 'image/png', 'image/webp', 'image/gif']
const MAX_SIZE_BYTES = 5 * 1024 * 1024  // 5MB
const ALLOWED_EXTENSIONS = ['.jpg', '.jpeg', '.png', '.webp', '.gif']

export async function POST(request: NextRequest) {
  const session = await auth()
  if (!session) return NextResponse.json({ error: 'Unauthorized' }, { status: 401 })

  const formData = await request.formData()
  const file = formData.get('file') as File | null
  if (!file) return NextResponse.json({ error: 'No file' }, { status: 400 })

  // 1. Check MIME type
  if (!ALLOWED_TYPES.includes(file.type)) {
    return NextResponse.json({ error: 'Invalid file type' }, { status: 400 })
  }

  // 2. Check file extension (don't trust MIME type alone)
  const ext = '.' + file.name.split('.').pop()?.toLowerCase()
  if (!ALLOWED_EXTENSIONS.includes(ext)) {
    return NextResponse.json({ error: 'Invalid file extension' }, { status: 400 })
  }

  // 3. Check file size
  if (file.size > MAX_SIZE_BYTES) {
    return NextResponse.json({ error: 'File too large (max 5MB)' }, { status: 400 })
  }

  // 4. Read magic bytes to verify actual type
  const buffer = Buffer.from(await file.arrayBuffer())
  const isPng  = buffer[0] === 0x89 && buffer[1] === 0x50 && buffer[2] === 0x4e
  const isJpeg = buffer[0] === 0xff && buffer[1] === 0xd8
  const isWebp = buffer.slice(8, 12).toString() === 'WEBP'
  const isGif  = buffer.slice(0, 6).toString() === 'GIF87a' || buffer.slice(0, 6).toString() === 'GIF89a'

  if (!isPng && !isJpeg && !isWebp && !isGif) {
    return NextResponse.json({ error: 'File content does not match type' }, { status: 400 })
  }

  // 5. Generate safe filename
  const safeFilename = `${session.user.id}-${Date.now()}-${Math.random().toString(36).slice(2)}${ext}`

  // 6. Upload to storage (S3, Cloudflare R2, Vercel Blob)
  // const { url } = await put(safeFilename, buffer, { access: 'public', contentType: file.type })

  return NextResponse.json({ url: `/uploads/${safeFilename}` })
}
```

---

## Step 968: Secrets Management

```bash
# Never hardcode secrets. Use environment variables.

# .env.local (development)
DATABASE_URL=postgresql://...
AUTH_SECRET=$(openssl rand -base64 32)
ENCRYPTION_KEY=$(openssl rand -hex 32)

# Vercel: store in project settings
vercel env add AUTH_SECRET production

# GitHub Actions: store in repo secrets
# Settings → Secrets and variables → Actions → New secret
```

```ts
// src/lib/encryption.ts — encrypt sensitive data before storing
import { createCipheriv, createDecipheriv, randomBytes } from 'crypto'

const ALGORITHM = 'aes-256-gcm'
const KEY = Buffer.from(process.env.ENCRYPTION_KEY!, 'hex') // 32 bytes

export function encrypt(text: string): string {
  const iv   = randomBytes(12)
  const cipher = createCipheriv(ALGORITHM, KEY, iv)
  const encrypted = Buffer.concat([cipher.update(text, 'utf8'), cipher.final()])
  const tag = cipher.getAuthTag()
  return [iv.toString('hex'), tag.toString('hex'), encrypted.toString('hex')].join(':')
}

export function decrypt(data: string): string {
  const [ivHex, tagHex, encryptedHex] = data.split(':')
  const decipher = createDecipheriv(ALGORITHM, KEY, Buffer.from(ivHex, 'hex'))
  decipher.setAuthTag(Buffer.from(tagHex, 'hex'))
  return decipher.update(Buffer.from(encryptedHex, 'hex')) + decipher.final('utf8')
}
```

---

## Step 969: Audit Logging

```ts
// src/lib/audit.ts
import { db }    from './db'
import { auth }  from '@/auth'

interface AuditEvent {
  action:   string         // 'product.created', 'user.deleted'
  entityId: string
  userId?:  string
  metadata?: object
  ip?:      string
}

export async function auditLog(event: AuditEvent) {
  if (process.env.NODE_ENV === 'test') return // skip in tests

  await db.$executeRaw`
    INSERT INTO audit_logs (action, entity_id, user_id, metadata, ip, created_at)
    VALUES (${event.action}, ${event.entityId}, ${event.userId ?? null},
            ${JSON.stringify(event.metadata ?? {})}::jsonb, ${event.ip ?? null}, NOW())
  `
}

// Higher-order function to add audit logging to handlers
export function withAudit(action: string, handler: Function) {
  return async (req: NextRequest, ctx: any) => {
    const session = await auth()
    const result  = await handler(req, ctx)

    if (result.ok) {
      await auditLog({
        action,
        entityId: ctx?.params?.id ?? 'unknown',
        userId:   session?.user?.id as string,
        ip:       req.ip ?? req.headers.get('x-forwarded-for') ?? undefined,
      })
    }

    return result
  }
}
```

---

## Step 970: Workshop — Security Checklist

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Security Checklist</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 min-h-screen p-6">
<div class="max-w-2xl mx-auto">
  <h1 class="text-2xl font-extrabold text-gray-900 mb-2">Security Hardening Checklist</h1>
  <p class="text-sm text-gray-500 mb-6">OWASP Top 10 + Production Security</p>
  <div id="checklist" class="space-y-4"></div>
  <div class="mt-5 bg-white rounded-2xl border p-4 flex items-center gap-4">
    <div>
      <p class="text-sm text-gray-500">Security Score</p>
      <p id="score-text" class="text-3xl font-extrabold text-gray-900">0%</p>
    </div>
    <div class="flex-1">
      <div class="w-full bg-gray-200 rounded-full h-3">
        <div id="score-bar" class="h-3 rounded-full bg-red-500 transition-all" style="width:0%"></div>
      </div>
    </div>
  </div>
</div>
<script>
const SECTIONS = [
  { title: 'Injection Prevention', items: ['Use Prisma ORM (parameterized queries)','Validate + sanitize all inputs with Zod','No eval() or dynamic SQL strings'] },
  { title: 'Authentication', items: ['Strong AUTH_SECRET (32+ chars)','bcrypt password hashing (rounds ≥12)','Rate limit login endpoint (5/15min)','Account lockout after failed attempts'] },
  { title: 'XSS Prevention', items: ['Avoid dangerouslySetInnerHTML','Sanitize HTML with DOMPurify when needed','Content-Security-Policy header set','Nonce-based inline scripts'] },
  { title: 'CSRF Protection', items: ['Auth.js CSRF protection on form actions','CSRF token on custom state-change APIs','SameSite=Strict cookies'] },
  { title: 'Security Headers', items: ['X-Frame-Options: DENY','X-Content-Type-Options: nosniff','Strict-Transport-Security (HSTS)','Referrer-Policy set'] },
  { title: 'Data Protection', items: ['Sensitive data encrypted at rest','No secrets in code/git history','Secure file upload (type + magic bytes)','Audit log for sensitive actions'] },
];

let checks={};
function render(){
  document.getElementById('checklist').innerHTML=SECTIONS.map(s=>`
    <div class="bg-white rounded-2xl border p-5">
      <h2 class="font-bold text-gray-900 mb-3">${s.title}</h2>
      ${s.items.map(item=>{const id=btoa(item);const c=checks[id]||false;return`<label class="flex items-center gap-3 mb-2 cursor-pointer"><input type="checkbox" ${c?'checked':''} onchange="toggle('${id}')" class="w-4 h-4 rounded accent-red-600"><span class="text-sm ${c?'line-through opacity-40':'text-gray-700'}">${item}</span></label>`}).join('')}
    </div>`).join('');
  const all=SECTIONS.flatMap(s=>s.items).length;
  const done=Object.values(checks).filter(Boolean).length;
  const pct=Math.round(done/all*100);
  document.getElementById('score-text').textContent=pct+'%';
  document.getElementById('score-bar').style.width=pct+'%';
  document.getElementById('score-bar').className=`h-3 rounded-full transition-all ${pct<50?'bg-red-500':pct<80?'bg-amber-500':'bg-emerald-500'}`;
}
function toggle(id){checks[id]=!checks[id];render();}
render();
</script>
</body>
</html>
```

---

## สรุป Part 97

| Step | เนื้อหา |
|------|---------|
| 961 | XSS: React auto-escape, DOMPurify with allowlists |
| 962 | SQL injection: Prisma parameterized + $queryRaw template literals |
| 963 | CSRF: HMAC token generation + constant-time verification |
| 964 | CSP: nonce-based middleware + security response headers |
| 965 | Input sanitization: DOMPurify + sanitizers for name/email/url/slug |
| 966 | Rate limiting with Upstash Redis: sliding window + fixed window |
| 967 | Secure file upload: MIME + extension + size + magic bytes check |
| 968 | Secrets management: env vars, Vercel env, AES-256-GCM encryption |
| 969 | Audit logging: higher-order withAudit wrapper + DB log |
| 970 | Workshop: OWASP security checklist with score |

**Part ถัดไป:** Part 98 — AI Integration (Steps 971–980)
