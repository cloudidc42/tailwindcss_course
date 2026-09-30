# Part 51: React + Tailwind Integration

## เป้าหมาย
- ใช้ Tailwind กับ React อย่างถูกต้อง
- Component patterns: clsx, class merging
- Headless UI patterns
- Steps 501–510

---

## Step 501: React + Tailwind Setup

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>React + Tailwind - Step 501</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- React via CDN for demo -->
  <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
  <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
</head>
<body class="bg-gray-50 p-8">
  <div id="root"></div>

  <script type="text/babel">
    const { useState } = React;

    // --- Utility: clsx-like function ---
    function cn(...classes) {
      return classes.filter(Boolean).join(' ');
    }

    // --- Button Component ---
    function Button({ variant = 'primary', size = 'md', disabled, loading, children, onClick, className }) {
      const base = 'inline-flex items-center justify-center font-semibold rounded-xl transition-colors focus:outline-none focus-visible:ring-2 focus-visible:ring-offset-2';

      const variants = {
        primary: 'bg-indigo-600 text-white hover:bg-indigo-700 focus-visible:ring-indigo-500',
        outline: 'border-2 border-indigo-600 text-indigo-600 hover:bg-indigo-50 focus-visible:ring-indigo-500',
        ghost:   'bg-gray-100 text-gray-700 hover:bg-gray-200 focus-visible:ring-gray-400',
        danger:  'bg-red-600 text-white hover:bg-red-700 focus-visible:ring-red-500',
      };

      const sizes = {
        sm: 'px-3 py-1.5 text-xs gap-1.5',
        md: 'px-4 py-2 text-sm gap-2',
        lg: 'px-5 py-2.5 text-base gap-2',
      };

      return (
        <button
          className={cn(base, variants[variant], sizes[size], (disabled || loading) && 'opacity-50 cursor-not-allowed', className)}
          disabled={disabled || loading}
          onClick={onClick}
        >
          {loading && (
            <svg className="w-3.5 h-3.5 animate-spin" fill="none" viewBox="0 0 24 24">
              <circle className="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" strokeWidth="4"/>
              <path className="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4z"/>
            </svg>
          )}
          {children}
        </button>
      );
    }

    // --- Badge Component ---
    function Badge({ variant = 'default', children }) {
      const variants = {
        default: 'bg-indigo-100 text-indigo-700',
        success: 'bg-green-100 text-green-700',
        danger:  'bg-red-100 text-red-700',
        warning: 'bg-yellow-100 text-yellow-700',
        info:    'bg-blue-100 text-blue-700',
      };
      return (
        <span className={cn('text-xs font-semibold px-2.5 py-1 rounded-full', variants[variant])}>
          {children}
        </span>
      );
    }

    // --- Card Component ---
    function Card({ title, children, className }) {
      return (
        <div className={cn('bg-white border border-gray-200 rounded-2xl p-5 shadow-sm', className)}>
          {title && <h3 className="font-bold text-gray-900 mb-3">{title}</h3>}
          {children}
        </div>
      );
    }

    // --- App ---
    function App() {
      const [loadingId, setLoadingId] = useState(null);

      function handleClick(id) {
        setLoadingId(id);
        setTimeout(() => setLoadingId(null), 2000);
      }

      return (
        <div className="max-w-4xl mx-auto space-y-8">
          <h1 className="text-2xl font-extrabold">React + Tailwind Components</h1>

          <Card title="Button Component">
            <div className="space-y-4">
              <div>
                <p className="text-xs text-gray-500 mb-2 font-medium">Variants</p>
                <div className="flex flex-wrap gap-3">
                  <Button variant="primary">Primary</Button>
                  <Button variant="outline">Outline</Button>
                  <Button variant="ghost">Ghost</Button>
                  <Button variant="danger">Danger</Button>
                </div>
              </div>
              <div>
                <p className="text-xs text-gray-500 mb-2 font-medium">Sizes</p>
                <div className="flex flex-wrap items-center gap-3">
                  <Button size="sm">Small</Button>
                  <Button size="md">Medium</Button>
                  <Button size="lg">Large</Button>
                </div>
              </div>
              <div>
                <p className="text-xs text-gray-500 mb-2 font-medium">States</p>
                <div className="flex flex-wrap gap-3">
                  <Button disabled>Disabled</Button>
                  <Button loading={loadingId === 1} onClick={() => handleClick(1)}>
                    {loadingId === 1 ? 'Saving...' : 'Click to Load'}
                  </Button>
                </div>
              </div>
            </div>
          </Card>

          <Card title="Badge Component">
            <div className="flex flex-wrap gap-2">
              <Badge>Default</Badge>
              <Badge variant="success">Success</Badge>
              <Badge variant="danger">Danger</Badge>
              <Badge variant="warning">Warning</Badge>
              <Badge variant="info">Info</Badge>
            </div>
          </Card>

          <Card title="Composed Layout">
            <div className="flex items-center justify-between">
              <div className="flex items-center gap-3">
                <div className="w-10 h-10 bg-indigo-100 rounded-xl flex items-center justify-center text-indigo-600 font-bold">SK</div>
                <div>
                  <p className="font-semibold text-sm">สมชาย เทคโน</p>
                  <p className="text-xs text-gray-500">Developer</p>
                </div>
              </div>
              <div className="flex items-center gap-2">
                <Badge variant="success">Active</Badge>
                <Button size="sm" variant="ghost">Edit</Button>
              </div>
            </div>
          </Card>
        </div>
      );
    }

    ReactDOM.createRoot(document.getElementById('root')).render(<App />);
  </script>
</body>
</html>
```

---

## Step 502–504: React State + Tailwind Dynamic Classes

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>React State + Tailwind - Steps 502-504</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
  <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
</head>
<body class="bg-gray-50 p-8">
  <div id="root2"></div>
  <script type="text/babel">
    const { useState, useReducer } = React;

    function cn(...c) { return c.filter(Boolean).join(' '); }

    // --- Tab Component (Step 502) ---
    function Tabs({ tabs, defaultTab }) {
      const [active, setActive] = useState(defaultTab || tabs[0].id);
      const current = tabs.find(t => t.id === active);
      return (
        <div>
          <div className="flex border-b border-gray-200">
            {tabs.map(tab => (
              <button key={tab.id} onClick={() => setActive(tab.id)}
                className={cn('px-4 py-2.5 text-sm font-medium border-b-2 -mb-px transition-colors',
                  active === tab.id
                    ? 'border-indigo-600 text-indigo-600'
                    : 'border-transparent text-gray-500 hover:text-gray-700'
                )}>
                {tab.label}
              </button>
            ))}
          </div>
          <div className="pt-4">{current?.content}</div>
        </div>
      );
    }

    // --- Toggle (Step 503) ---
    function Toggle({ label, defaultChecked = false }) {
      const [on, setOn] = useState(defaultChecked);
      return (
        <label className="flex items-center gap-3 cursor-pointer">
          <button role="switch" aria-checked={on} onClick={() => setOn(!on)}
            className={cn('relative w-11 h-6 rounded-full transition-colors',
              on ? 'bg-indigo-600' : 'bg-gray-300')}>
            <span className={cn('absolute w-5 h-5 bg-white rounded-full top-0.5 left-0.5 transition-transform shadow-sm',
              on && 'translate-x-5')}/>
          </button>
          <span className="text-sm font-medium">{label}</span>
          <span className={cn('text-xs font-semibold ml-auto', on ? 'text-indigo-600' : 'text-gray-400')}>
            {on ? 'ON' : 'OFF'}
          </span>
        </label>
      );
    }

    // --- Accordion (Step 504) ---
    function Accordion({ items }) {
      const [open, setOpen] = useState(null);
      return (
        <div className="space-y-2">
          {items.map((item, i) => (
            <div key={i} className="border rounded-xl overflow-hidden">
              <button onClick={() => setOpen(open === i ? null : i)}
                className="w-full flex items-center justify-between px-4 py-3 text-left text-sm font-semibold hover:bg-gray-50 transition-colors">
                <span>{item.question}</span>
                <svg className={cn('w-4 h-4 text-gray-400 transition-transform', open === i && 'rotate-180')}
                  fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M19 9l-7 7-7-7"/>
                </svg>
              </button>
              {open === i && (
                <div className="px-4 pb-4 text-sm text-gray-600">{item.answer}</div>
              )}
            </div>
          ))}
        </div>
      );
    }

    function App2() {
      return (
        <div className="max-w-2xl mx-auto space-y-8">
          <h2 className="text-xl font-extrabold">React State + Tailwind</h2>

          <div className="bg-white rounded-2xl border p-5">
            <h3 className="font-bold mb-4">Tabs</h3>
            <Tabs tabs={[
              { id: 'overview', label: 'ภาพรวม', content: <p className="text-sm text-gray-600">ข้อมูลภาพรวมทั้งหมด</p> },
              { id: 'analytics', label: 'Analytics', content: <p className="text-sm text-gray-600">กราฟและสถิติ</p> },
              { id: 'settings', label: 'ตั้งค่า', content: <p className="text-sm text-gray-600">การตั้งค่าต่างๆ</p> },
            ]} />
          </div>

          <div className="bg-white rounded-2xl border p-5 space-y-3">
            <h3 className="font-bold mb-4">Toggle Switches</h3>
            <Toggle label="การแจ้งเตือนทางอีเมล" defaultChecked={true} />
            <Toggle label="โหมด Dark" />
            <Toggle label="การแจ้งเตือน Push" defaultChecked={true} />
          </div>

          <div className="bg-white rounded-2xl border p-5">
            <h3 className="font-bold mb-4">Accordion FAQ</h3>
            <Accordion items={[
              { question: 'Tailwind CSS คืออะไร?', answer: 'Utility-first CSS framework ที่ใช้ class สำเร็จรูป' },
              { question: 'ต้องรู้ CSS ก่อนไหม?', answer: 'แนะนำให้รู้ CSS พื้นฐานก่อน จะเข้าใจ Tailwind ได้ดีขึ้น' },
              { question: 'ใช้กับ React ได้ไหม?', answer: 'ใช้ได้เลย เพียงแค่ติดตั้ง tailwindcss และ configure ตามปกติ' },
            ]} />
          </div>
        </div>
      );
    }

    ReactDOM.createRoot(document.getElementById('root2')).render(<App2 />);
  </script>
</body>
</html>
```

---

## Steps 505–510: React Forms, Modal, Toast Workshop

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>React Components Workshop - Steps 505-510</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
  <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
</head>
<body class="bg-gray-50">
  <div id="root3"></div>
  <script type="text/babel">
    const { useState, useEffect, useRef, useCallback } = React;

    function cn(...c) { return c.filter(Boolean).join(' '); }

    // --- Toast System (Step 505) ---
    function useToast() {
      const [toasts, setToasts] = useState([]);
      const add = useCallback((msg, type = 'info') => {
        const id = Date.now();
        setToasts(prev => [...prev, { id, msg, type }]);
        setTimeout(() => setToasts(prev => prev.filter(t => t.id !== id)), 3500);
      }, []);
      return { toasts, toast: add };
    }

    const TOAST_COLORS = { success: 'bg-green-600', error: 'bg-red-600', info: 'bg-blue-600', warning: 'bg-yellow-500' };

    function ToastContainer({ toasts }) {
      return (
        <div className="fixed bottom-6 right-6 z-50 space-y-2">
          {toasts.map(t => (
            <div key={t.id} className={cn(TOAST_COLORS[t.type], 'text-white px-4 py-3 rounded-xl text-sm font-medium shadow-lg max-w-xs animate-bounce-in')}>
              {t.msg}
            </div>
          ))}
        </div>
      );
    }

    // --- Modal (Step 506) ---
    function Modal({ open, onClose, title, children, footer }) {
      useEffect(() => {
        const handler = e => { if (e.key === 'Escape') onClose(); };
        if (open) document.addEventListener('keydown', handler);
        return () => document.removeEventListener('keydown', handler);
      }, [open, onClose]);

      if (!open) return null;
      return (
        <div className="fixed inset-0 bg-black/50 z-50 flex items-center justify-center p-4" onClick={onClose}>
          <div className="bg-white rounded-2xl shadow-2xl w-full max-w-md" onClick={e => e.stopPropagation()}>
            <div className="flex items-center justify-between p-5 border-b">
              <h3 className="font-extrabold text-lg">{title}</h3>
              <button onClick={onClose} className="text-gray-400 hover:text-gray-600 transition-colors text-xl">✕</button>
            </div>
            <div className="p-5">{children}</div>
            {footer && <div className="flex gap-3 p-5 border-t bg-gray-50">{footer}</div>}
          </div>
        </div>
      );
    }

    // --- Input Component (Step 507) ---
    function Input({ label, error, hint, required, className, ...props }) {
      return (
        <div>
          {label && (
            <label className="block text-sm font-medium text-gray-700 mb-1.5">
              {label}{required && <span className="text-red-500 ml-1">*</span>}
            </label>
          )}
          <input
            className={cn('w-full border rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 transition-colors',
              error ? 'border-red-400 focus:ring-red-400' : 'border-gray-300 focus:ring-indigo-500',
              className
            )}
            {...props}
          />
          {error && <p className="text-xs text-red-500 mt-1">{error}</p>}
          {hint && !error && <p className="text-xs text-gray-500 mt-1">{hint}</p>}
        </div>
      );
    }

    // --- Main App (Step 508-510 Workshop) ---
    function App3() {
      const { toasts, toast } = useToast();
      const [modalOpen, setModalOpen] = useState(false);
      const [deleteModalOpen, setDeleteModalOpen] = useState(false);
      const [formData, setFormData] = useState({ name: '', email: '', role: 'member' });
      const [errors, setErrors] = useState({});
      const [users, setUsers] = useState([
        { id: 1, name: 'สมชาย เทคโน', email: 'somchai@tech.co', role: 'admin' },
        { id: 2, name: 'วิชัย ใจดี', email: 'wichai@email.com', role: 'member' },
      ]);
      const [deleteTarget, setDeleteTarget] = useState(null);

      function validate() {
        const e = {};
        if (!formData.name.trim()) e.name = 'กรุณากรอกชื่อ';
        if (!formData.email.trim()) e.email = 'กรุณากรอกอีเมล';
        else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(formData.email)) e.email = 'รูปแบบอีเมลไม่ถูกต้อง';
        setErrors(e);
        return Object.keys(e).length === 0;
      }

      function addUser() {
        if (!validate()) return;
        setUsers(prev => [...prev, { id: Date.now(), ...formData }]);
        setFormData({ name: '', email: '', role: 'member' });
        setErrors({});
        setModalOpen(false);
        toast('เพิ่มผู้ใช้เรียบร้อยแล้ว', 'success');
      }

      function confirmDelete(user) {
        setDeleteTarget(user);
        setDeleteModalOpen(true);
      }

      function doDelete() {
        setUsers(prev => prev.filter(u => u.id !== deleteTarget.id));
        setDeleteModalOpen(false);
        toast(`ลบ ${deleteTarget.name} แล้ว`, 'info');
        setDeleteTarget(null);
      }

      return (
        <div className="min-h-screen">
          <header className="bg-white border-b px-6 h-14 flex items-center justify-between sticky top-0 z-10">
            <h1 className="font-extrabold">User Management</h1>
            <button onClick={() => setModalOpen(true)} className="bg-indigo-600 text-white px-4 py-2 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors flex items-center gap-2">
              <span>+</span> เพิ่มผู้ใช้
            </button>
          </header>

          <main className="max-w-4xl mx-auto p-6">
            <div className="bg-white rounded-2xl border overflow-hidden">
              <div className="px-5 py-4 border-b flex items-center justify-between">
                <p className="font-bold">ผู้ใช้ทั้งหมด ({users.length} คน)</p>
              </div>
              <table className="w-full text-sm">
                <thead className="bg-gray-50">
                  <tr>
                    <th className="px-5 py-3 text-left text-xs font-semibold text-gray-500">ผู้ใช้</th>
                    <th className="px-5 py-3 text-left text-xs font-semibold text-gray-500">อีเมล</th>
                    <th className="px-5 py-3 text-left text-xs font-semibold text-gray-500">บทบาท</th>
                    <th className="px-5 py-3 text-left text-xs font-semibold text-gray-500">Action</th>
                  </tr>
                </thead>
                <tbody className="divide-y">
                  {users.map(user => (
                    <tr key={user.id} className="hover:bg-gray-50 transition-colors">
                      <td className="px-5 py-3 font-medium">{user.name}</td>
                      <td className="px-5 py-3 text-gray-500">{user.email}</td>
                      <td className="px-5 py-3">
                        <span className={cn('text-xs font-semibold px-2.5 py-1 rounded-full',
                          user.role === 'admin' ? 'bg-purple-100 text-purple-700' : 'bg-gray-100 text-gray-600')}>
                          {user.role}
                        </span>
                      </td>
                      <td className="px-5 py-3">
                        <button onClick={() => confirmDelete(user)} className="text-red-500 hover:text-red-700 text-xs font-medium transition-colors">ลบ</button>
                      </td>
                    </tr>
                  ))}
                </tbody>
              </table>
            </div>
          </main>

          {/* Add User Modal */}
          <Modal open={modalOpen} onClose={() => setModalOpen(false)} title="เพิ่มผู้ใช้ใหม่"
            footer={<>
              <button onClick={() => setModalOpen(false)} className="flex-1 border border-gray-300 py-2.5 rounded-xl text-sm font-semibold hover:bg-gray-50 transition-colors">ยกเลิก</button>
              <button onClick={addUser} className="flex-1 bg-indigo-600 text-white py-2.5 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">บันทึก</button>
            </>}>
            <div className="space-y-4">
              <Input label="ชื่อ-นามสกุล" required value={formData.name} error={errors.name}
                onChange={e => setFormData({...formData, name: e.target.value})} placeholder="สมชาย เทคโน" />
              <Input label="อีเมล" required value={formData.email} error={errors.email} type="email"
                onChange={e => setFormData({...formData, email: e.target.value})} placeholder="user@example.com" />
              <div>
                <label className="block text-sm font-medium text-gray-700 mb-1.5">บทบาท</label>
                <select value={formData.role} onChange={e => setFormData({...formData, role: e.target.value})}
                  className="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
                  <option value="member">Member</option>
                  <option value="admin">Admin</option>
                  <option value="viewer">Viewer</option>
                </select>
              </div>
            </div>
          </Modal>

          {/* Delete Confirm Modal */}
          <Modal open={deleteModalOpen} onClose={() => setDeleteModalOpen(false)} title="ยืนยันการลบ"
            footer={<>
              <button onClick={() => setDeleteModalOpen(false)} className="flex-1 border border-gray-300 py-2.5 rounded-xl text-sm font-semibold hover:bg-gray-50 transition-colors">ยกเลิก</button>
              <button onClick={doDelete} className="flex-1 bg-red-600 text-white py-2.5 rounded-xl text-sm font-semibold hover:bg-red-700 transition-colors">ลบ</button>
            </>}>
            <p className="text-sm text-gray-600">คุณแน่ใจหรือไม่ที่จะลบ <strong>{deleteTarget?.name}</strong>?</p>
          </Modal>

          <ToastContainer toasts={toasts} />
        </div>
      );
    }

    ReactDOM.createRoot(document.getElementById('root3')).render(<App3 />);
  </script>
</body>
</html>
```

---

## สรุป Part 51

| Step | เนื้อหา |
|------|---------|
| 501 | React + Tailwind setup, Button/Badge/Card components, cn() utility |
| 502 | Tabs component with active state styling |
| 503 | Toggle switch with aria-checked |
| 504 | Accordion with open/close state |
| 505 | useToast hook + ToastContainer |
| 506 | Modal with ESC key, backdrop click, focus trap |
| 507 | Input component with error/hint states |
| 508–510 | Workshop: User Management CRUD (add/delete + validation + toasts) |

**Part ถัดไป:** Part 52 — Advanced React Patterns (Steps 511–520)
