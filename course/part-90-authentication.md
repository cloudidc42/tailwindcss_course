# Part 90: Authentication & Authorization

## เป้าหมาย
- Auth.js (NextAuth v5) setup
- JWT + Session strategies
- Role-based access control (RBAC)
- OAuth providers (Google, GitHub)
- Steps 891–900

---

## Step 891: Auth.js Setup

```bash
npm install next-auth@beta
npx auth secret   # generates AUTH_SECRET env var
```

```ts
// src/auth.ts
import NextAuth from 'next-auth'
import GitHub  from 'next-auth/providers/github'
import Google  from 'next-auth/providers/google'
import Credentials from 'next-auth/providers/credentials'
import { z } from 'zod'

export const { handlers, auth, signIn, signOut } = NextAuth({
  providers: [
    GitHub({
      clientId:     process.env.GITHUB_ID!,
      clientSecret: process.env.GITHUB_SECRET!,
    }),
    Google({
      clientId:     process.env.GOOGLE_ID!,
      clientSecret: process.env.GOOGLE_SECRET!,
    }),
    Credentials({
      credentials: {
        email:    { label: 'Email',    type: 'email'    },
        password: { label: 'Password', type: 'password' },
      },
      async authorize(credentials) {
        const parsed = z.object({
          email:    z.string().email(),
          password: z.string().min(8),
        }).safeParse(credentials)

        if (!parsed.success) return null

        // const user = await db.user.findUnique({ where: { email: parsed.data.email } })
        // if (!user || !await bcrypt.compare(parsed.data.password, user.passwordHash)) return null
        // return { id: user.id, email: user.email, name: user.name, role: user.role }

        // Demo: accept any valid email/password
        if (parsed.data.password.length >= 8) {
          return { id: '1', email: parsed.data.email, name: 'Demo User', role: 'user' }
        }
        return null
      },
    }),
  ],
  callbacks: {
    jwt({ token, user }) {
      if (user) token.role = (user as any).role
      return token
    },
    session({ session, token }) {
      if (session.user) (session.user as any).role = token.role
      return session
    },
  },
  pages: {
    signIn: '/login',
    error:  '/login',
  },
})
```

---

## Step 892: Auth Route Handlers

```ts
// src/app/api/auth/[...nextauth]/route.ts
import { handlers } from '@/auth'
export const { GET, POST } = handlers
```

```tsx
// src/app/login/page.tsx
import { LoginForm } from './LoginForm'
import { auth }      from '@/auth'
import { redirect }  from 'next/navigation'

export default async function LoginPage() {
  const session = await auth()
  if (session) redirect('/dashboard')

  return (
    <div className="min-h-screen flex items-center justify-center bg-gray-50 p-4">
      <div className="w-full max-w-sm">
        <div className="text-center mb-8">
          <div className="w-12 h-12 bg-indigo-600 rounded-2xl mx-auto mb-4 flex items-center justify-center text-white text-xl font-bold">A</div>
          <h1 className="text-2xl font-extrabold text-gray-900">Welcome back</h1>
          <p className="text-gray-500 text-sm mt-1">Sign in to your account</p>
        </div>
        <LoginForm />
      </div>
    </div>
  )
}
```

---

## Step 893: Login Form with Server Actions

```tsx
// src/app/login/LoginForm.tsx
'use client'
import { useState } from 'react'
import { signIn }   from 'next-auth/react'
import { useRouter } from 'next/navigation'

export function LoginForm() {
  const router = useRouter()
  const [error, setError]   = useState('')
  const [loading, setLoading] = useState(false)

  async function handleCredentials(e: React.FormEvent<HTMLFormElement>) {
    e.preventDefault()
    setError('')
    setLoading(true)
    const fd = new FormData(e.currentTarget)
    const result = await signIn('credentials', {
      email:    fd.get('email'),
      password: fd.get('password'),
      redirect: false,
    })
    setLoading(false)
    if (result?.error) setError('อีเมลหรือรหัสผ่านไม่ถูกต้อง')
    else router.push('/dashboard')
  }

  return (
    <div className="bg-white rounded-2xl border p-6 shadow-sm">
      {/* OAuth */}
      <div className="grid grid-cols-2 gap-2 mb-5">
        <button onClick={() => signIn('github', { callbackUrl: '/dashboard' })}
          className="flex items-center justify-center gap-2 py-2.5 border rounded-xl text-sm font-medium hover:bg-gray-50 transition-colors">
          <svg className="w-4 h-4" viewBox="0 0 24 24" fill="currentColor"><path d="M12 0C5.37 0 0 5.37 0 12c0 5.31 3.435 9.795 8.205 11.385.6.105.825-.255.825-.57 0-.285-.015-1.23-.015-2.235-3.015.555-3.795-.735-4.035-1.41-.135-.345-.72-1.41-1.23-1.695-.42-.225-1.02-.78-.015-.795.945-.015 1.62.87 1.845 1.23 1.08 1.815 2.805 1.305 3.495.99.105-.78.42-1.305.765-1.605-2.67-.3-5.46-1.335-5.46-5.925 0-1.305.465-2.385 1.23-3.225-.12-.3-.54-1.53.12-3.18 0 0 1.005-.315 3.3 1.23.96-.27 1.98-.405 3-.405s2.04.135 3 .405c2.295-1.56 3.3-1.23 3.3-1.23.66 1.65.24 2.88.12 3.18.765.84 1.23 1.905 1.23 3.225 0 4.605-2.805 5.625-5.475 5.925.435.375.81 1.095.81 2.22 0 1.605-.015 2.895-.015 3.3 0 .315.225.69.825.57A12.02 12.02 0 0 0 24 12c0-6.63-5.37-12-12-12z"/></svg>
          GitHub
        </button>
        <button onClick={() => signIn('google', { callbackUrl: '/dashboard' })}
          className="flex items-center justify-center gap-2 py-2.5 border rounded-xl text-sm font-medium hover:bg-gray-50 transition-colors">
          <svg className="w-4 h-4" viewBox="0 0 24 24"><path d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z" fill="#4285F4"/><path d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z" fill="#34A853"/><path d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l2.85-2.22.81-.62z" fill="#FBBC05"/><path d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z" fill="#EA4335"/></svg>
          Google
        </button>
      </div>

      <div className="relative mb-5">
        <div className="absolute inset-0 flex items-center"><div className="w-full border-t"></div></div>
        <div className="relative flex justify-center"><span className="bg-white px-3 text-xs text-gray-400">or continue with email</span></div>
      </div>

      <form onSubmit={handleCredentials} className="space-y-3">
        <div>
          <label className="block text-xs font-medium text-gray-700 mb-1">Email</label>
          <input name="email" type="email" required autoComplete="email"
            className="w-full px-3 py-2 border rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500" />
        </div>
        <div>
          <label className="block text-xs font-medium text-gray-700 mb-1">Password</label>
          <input name="password" type="password" required autoComplete="current-password"
            className="w-full px-3 py-2 border rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500" />
        </div>
        {error && <p className="text-xs text-red-600">{error}</p>}
        <button type="submit" disabled={loading}
          className="w-full py-2.5 bg-indigo-600 text-white text-sm font-semibold rounded-xl hover:bg-indigo-700 disabled:opacity-50 transition-colors flex items-center justify-center gap-2">
          {loading && <span className="w-4 h-4 border-2 border-white border-t-transparent rounded-full animate-spin"></span>}
          {loading ? 'Signing in...' : 'Sign in'}
        </button>
      </form>
    </div>
  )
}
```

---

## Step 894: Session in Server Components

```tsx
// Access session in any Server Component
import { auth } from '@/auth'
import { redirect } from 'next/navigation'

export default async function ProfilePage() {
  const session = await auth()
  if (!session?.user) redirect('/login')

  const { name, email, image, role } = session.user as any

  return (
    <div className="max-w-lg mx-auto p-6">
      <div className="bg-white rounded-2xl border p-6">
        <div className="flex items-center gap-4 mb-6">
          {image ? (
            <img src={image} alt={name ?? ''} className="w-16 h-16 rounded-full" />
          ) : (
            <div className="w-16 h-16 rounded-full bg-indigo-100 flex items-center justify-center text-indigo-600 text-xl font-bold">
              {name?.[0]?.toUpperCase()}
            </div>
          )}
          <div>
            <p className="font-bold text-gray-900">{name}</p>
            <p className="text-gray-500 text-sm">{email}</p>
            <span className="mt-1 px-2 py-0.5 bg-indigo-100 text-indigo-700 text-xs rounded-full font-medium">{role}</span>
          </div>
        </div>
        <form action={async () => { 'use server'; await signOut() }}>
          <button type="submit" className="px-4 py-2 border rounded-xl text-sm text-gray-700 hover:bg-gray-50 transition-colors">Sign out</button>
        </form>
      </div>
    </div>
  )
}
```

---

## Step 895: RBAC (Role-Based Access Control)

```ts
// src/lib/permissions.ts
export type Role = 'guest' | 'user' | 'admin' | 'superadmin'

export const PERMISSIONS = {
  'products:read':   ['guest', 'user', 'admin', 'superadmin'],
  'products:write':  ['admin', 'superadmin'],
  'products:delete': ['superadmin'],
  'users:read':      ['admin', 'superadmin'],
  'users:write':     ['admin', 'superadmin'],
  'users:delete':    ['superadmin'],
  'settings:read':   ['admin', 'superadmin'],
  'settings:write':  ['superadmin'],
} as const

export type Permission = keyof typeof PERMISSIONS

export function can(role: Role, permission: Permission): boolean {
  return (PERMISSIONS[permission] as readonly string[]).includes(role)
}

// Usage in Server Component
export async function requirePermission(permission: Permission) {
  const session = await auth()
  const role = (session?.user as any)?.role as Role ?? 'guest'
  if (!can(role, permission)) {
    throw new Error('FORBIDDEN')
  }
  return session!
}
```

```tsx
// src/components/PermissionGate.tsx
import { auth } from '@/auth'
import { can, type Permission, type Role } from '@/lib/permissions'

export async function PermissionGate({
  permission,
  children,
  fallback = null,
}: {
  permission: Permission
  children: React.ReactNode
  fallback?: React.ReactNode
}) {
  const session = await auth()
  const role = (session?.user as any)?.role as Role ?? 'guest'

  if (!can(role, permission)) return <>{fallback}</>
  return <>{children}</>
}

// Usage
// <PermissionGate permission="products:delete">
//   <DeleteButton />
// </PermissionGate>
```

---

## Step 896: API Route Authorization

```ts
// src/lib/auth-helpers.ts
import { auth } from '@/auth'
import { can, type Permission, type Role } from '@/lib/permissions'
import { NextRequest, NextResponse } from 'next/server'

export function withAuth(
  permission: Permission,
  handler: (req: NextRequest, ctx: any, session: any) => Promise<NextResponse>
) {
  return async (req: NextRequest, ctx: any) => {
    const session = await auth()
    if (!session?.user) {
      return NextResponse.json({ error: 'Unauthorized' }, { status: 401 })
    }
    const role = (session.user as any).role as Role ?? 'user'
    if (!can(role, permission)) {
      return NextResponse.json({ error: 'Forbidden' }, { status: 403 })
    }
    return handler(req, ctx, session)
  }
}

// src/app/api/products/route.ts
export const POST = withAuth('products:write', async (req, _ctx, session) => {
  const body = await req.json()
  // session.user is typed with role
  return NextResponse.json({ ...body, createdBy: session.user.email }, { status: 201 })
})

export const DELETE = withAuth('products:delete', async (req, { params }) => {
  return new NextResponse(null, { status: 204 })
})
```

---

## Step 897: Password Reset Flow

```ts
// src/app/api/auth/forgot-password/route.ts
import { randomBytes } from 'crypto'

export async function POST(request: NextRequest) {
  const { email } = await request.json()
  if (!email) return NextResponse.json({ error: 'Email required' }, { status: 400 })

  // Find user
  // const user = await db.user.findUnique({ where: { email } })
  // if (!user) return NextResponse.json({ message: 'If email exists, reset link sent' }) // Don't reveal

  const token   = randomBytes(32).toString('hex')
  const expires = new Date(Date.now() + 60 * 60 * 1000) // 1 hour

  // await db.passwordReset.create({ data: { token, userId: user.id, expiresAt: expires } })
  // await sendEmail({ to: email, subject: 'Reset password', body: `${process.env.APP_URL}/reset-password?token=${token}` })

  return NextResponse.json({ message: 'If email exists, reset link sent' })
}

// src/app/api/auth/reset-password/route.ts
export async function POST(request: NextRequest) {
  const { token, password } = await request.json()
  if (!token || !password) return NextResponse.json({ error: 'Missing fields' }, { status: 400 })
  if (password.length < 8) return NextResponse.json({ error: 'Password too short' }, { status: 400 })

  // const reset = await db.passwordReset.findUnique({ where: { token } })
  // if (!reset || reset.expiresAt < new Date()) return NextResponse.json({ error: 'Token expired' }, { status: 400 })

  // const hash = await bcrypt.hash(password, 12)
  // await db.user.update({ where: { id: reset.userId }, data: { passwordHash: hash } })
  // await db.passwordReset.delete({ where: { token } })

  return NextResponse.json({ message: 'Password updated' })
}
```

---

## Step 898: Auth Workshop Demo

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Auth Demo</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 min-h-screen flex items-center justify-center p-4">
<div class="w-full max-w-md" id="app">
  <!-- Login screen shown by default -->
</div>
<script>
  const USERS = [
    { id:1, email:'admin@demo.com', password:'password123', name:'Admin User', role:'admin' },
    { id:2, email:'user@demo.com',  password:'password123', name:'Regular User', role:'user' },
  ];
  let session = JSON.parse(sessionStorage.getItem('demo-session') || 'null');

  const PERMS = {
    'products:read':   ['user','admin'],
    'products:write':  ['admin'],
    'products:delete': ['admin'],
    'users:read':      ['admin'],
  };

  function can(role, perm) { return (PERMS[perm] || []).includes(role); }

  function render() {
    const app = document.getElementById('app');
    if (!session) {
      app.innerHTML = `
        <div class="bg-white rounded-2xl border p-6 shadow-sm">
          <h1 class="text-xl font-extrabold text-gray-900 mb-5 text-center">Sign In</h1>
          <form onsubmit="login(event)" class="space-y-3">
            <div><label class="block text-xs font-medium text-gray-700 mb-1">Email</label>
            <input id="email" type="email" value="admin@demo.com" class="w-full px-3 py-2 border rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500"></div>
            <div><label class="block text-xs font-medium text-gray-700 mb-1">Password</label>
            <input id="password" type="password" value="password123" class="w-full px-3 py-2 border rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500"></div>
            <p id="err" class="text-xs text-red-600 hidden"></p>
            <button type="submit" class="w-full py-2.5 bg-indigo-600 text-white text-sm font-semibold rounded-xl hover:bg-indigo-700">Sign in</button>
          </form>
          <p class="text-xs text-gray-400 mt-4 text-center">Admin: admin@demo.com / password123<br>User: user@demo.com / password123</p>
        </div>`;
    } else {
      const perms = Object.keys(PERMS).filter(p => can(session.role, p));
      app.innerHTML = `
        <div class="bg-white rounded-2xl border p-6 shadow-sm">
          <div class="flex items-center gap-3 mb-5">
            <div class="w-10 h-10 rounded-full bg-indigo-100 flex items-center justify-center text-indigo-600 font-bold">${session.name[0]}</div>
            <div>
              <p class="font-semibold text-gray-900">${session.name}</p>
              <span class="px-2 py-0.5 text-xs rounded-full font-medium ${session.role==='admin'?'bg-amber-100 text-amber-700':'bg-gray-100 text-gray-600'}">${session.role}</span>
            </div>
            <button onclick="logout()" class="ml-auto text-xs text-gray-500 hover:text-gray-700 border rounded-xl px-3 py-1.5">Sign out</button>
          </div>
          <h2 class="text-xs font-bold text-gray-500 uppercase tracking-wide mb-3">Permissions</h2>
          <div class="space-y-1.5 mb-5">
            ${Object.keys(PERMS).map(p => `
              <div class="flex items-center gap-2 text-sm">
                <span class="${can(session.role,p)?'text-emerald-500':'text-gray-300'}">${can(session.role,p)?'✓':'✗'}</span>
                <span class="${can(session.role,p)?'text-gray-800':'text-gray-400'}">${p}</span>
              </div>`).join('')}
          </div>
          <div class="grid grid-cols-2 gap-2">
            ${Object.keys(PERMS).map(p => `
              <button onclick="tryAction('${p}')" class="py-2 text-xs rounded-xl border font-medium ${can(session.role,p)?'bg-indigo-50 text-indigo-700 border-indigo-200 hover:bg-indigo-100':'bg-gray-50 text-gray-400 cursor-not-allowed'}">${p}</button>`).join('')}
          </div>
          <div id="action-result" class="mt-3 text-xs text-center text-gray-500"></div>
        </div>`;
    }
  }

  function login(e) {
    e.preventDefault();
    const email = document.getElementById('email').value;
    const password = document.getElementById('password').value;
    const user = USERS.find(u => u.email===email && u.password===password);
    if (!user) { document.getElementById('err').classList.remove('hidden'); document.getElementById('err').textContent='Invalid credentials'; return; }
    session = { id:user.id, name:user.name, email:user.email, role:user.role };
    sessionStorage.setItem('demo-session', JSON.stringify(session));
    render();
  }

  function logout() { session=null; sessionStorage.removeItem('demo-session'); render(); }

  function tryAction(perm) {
    const el = document.getElementById('action-result');
    if (can(session.role, perm)) {
      el.textContent = `✅ "${perm}" allowed for ${session.role}`;
      el.className = 'mt-3 text-xs text-center text-emerald-600';
    } else {
      el.textContent = `❌ "${perm}" forbidden for ${session.role} (403)`;
      el.className = 'mt-3 text-xs text-center text-red-600';
    }
  }

  render();
</script>
</body>
</html>
```

---

## Step 899: Type Augmentation for Session

```ts
// src/types/next-auth.d.ts
import type { DefaultSession, DefaultUser } from 'next-auth'
import type { DefaultJWT } from 'next-auth/jwt'
import type { Role } from '@/lib/permissions'

declare module 'next-auth' {
  interface Session {
    user: DefaultSession['user'] & { role: Role; id: string }
  }
  interface User extends DefaultUser { role: Role }
}

declare module 'next-auth/jwt' {
  interface JWT extends DefaultJWT { role: Role; id: string }
}
```

---

## Step 900: Auth Checklist

```
✅ Authentication Checklist

SETUP
  ✅ AUTH_SECRET generated (npx auth secret)
  ✅ OAuth credentials in .env.local (never commit)
  ✅ NEXTAUTH_URL set in production

SECURITY
  ✅ Passwords hashed with bcrypt (rounds ≥ 12)
  ✅ Rate limit on /api/auth/signin
  ✅ CSRF protection (built into Auth.js)
  ✅ Secure + HttpOnly session cookies
  ✅ Token expiry (access: 1h, refresh: 30d)

AUTHORIZATION
  ✅ Middleware ป้องกัน protected routes
  ✅ RBAC permissions defined + PermissionGate component
  ✅ API routes ใช้ withAuth() wrapper
  ✅ Client-side: hide UI เท่านั้น — server-side enforce

UX
  ✅ Redirect to original URL หลัง login
  ✅ Loading states ขณะ signing in/out
  ✅ Error messages ไม่ reveal user existence
  ✅ Remember me / persistent sessions
```

---

## สรุป Part 90

| Step | เนื้อหา |
|------|---------|
| 891 | Auth.js setup: GitHub + Google + Credentials providers |
| 892 | Route handlers + Login page Server Component |
| 893 | Login form: OAuth buttons + credential form + signIn() |
| 894 | Session in Server Components + signOut Server Action |
| 895 | RBAC: permissions map + can() + PermissionGate component |
| 896 | withAuth() wrapper สำหรับ API routes |
| 897 | Password reset flow: token generation + expiry |
| 898 | Workshop: Interactive auth + RBAC demo |
| 899 | TypeScript type augmentation สำหรับ session |
| 900 | Auth security checklist (Milestone: Step 900!) |

**Part ถัดไป:** Part 91 — Database Integration with Prisma (Steps 901–910)
