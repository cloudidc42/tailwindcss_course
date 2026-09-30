# Part 52: Advanced React Patterns with Tailwind

## เป้าหมาย
- Context + Tailwind theming
- Compound components
- Render props + slots pattern
- Data tables, forms, filters
- Steps 511–520

---

## Steps 511–514: Theme Context + Data Table

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Advanced React - Steps 511-514</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
  <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
</head>
<body>
  <div id="root"></div>
  <script type="text/babel">
    const { useState, useContext, createContext, useMemo } = React;

    function cn(...c) { return c.filter(Boolean).join(' '); }

    // --- Theme Context (Step 511) ---
    const ThemeCtx = createContext({ primary: 'indigo', dark: false });

    const THEMES = {
      indigo: { btn: 'bg-indigo-600 hover:bg-indigo-700', badge: 'bg-indigo-100 text-indigo-700', ring: 'focus:ring-indigo-500', accent: 'text-indigo-600' },
      green:  { btn: 'bg-green-600 hover:bg-green-700',   badge: 'bg-green-100 text-green-700',   ring: 'focus:ring-green-500',  accent: 'text-green-600' },
      rose:   { btn: 'bg-rose-600 hover:bg-rose-700',     badge: 'bg-rose-100 text-rose-700',     ring: 'focus:ring-rose-500',   accent: 'text-rose-600' },
    };

    function useTheme() { return useContext(ThemeCtx); }

    function ThemeButton({ children, size = 'md', ...props }) {
      const { primary } = useTheme();
      const t = THEMES[primary];
      const sz = { sm: 'px-3 py-1.5 text-xs', md: 'px-4 py-2 text-sm', lg: 'px-5 py-2.5 text-base' }[size];
      return (
        <button className={cn(t.btn, sz, 'text-white rounded-xl font-semibold transition-colors focus:outline-none focus:ring-2 focus:ring-offset-2', t.ring)} {...props}>
          {children}
        </button>
      );
    }

    function ThemeBadge({ children }) {
      const { primary } = useTheme();
      return <span className={cn(THEMES[primary].badge, 'text-xs font-semibold px-2.5 py-1 rounded-full')}>{children}</span>;
    }

    // --- Sortable Data Table (Step 512-513) ---
    function DataTable({ columns, data }) {
      const [sort, setSort] = useState({ key: null, dir: 'asc' });
      const { primary } = useTheme();

      function toggleSort(key) {
        setSort(s => s.key === key ? { key, dir: s.dir === 'asc' ? 'desc' : 'asc' } : { key, dir: 'asc' });
      }

      const sorted = useMemo(() => {
        if (!sort.key) return data;
        return [...data].sort((a, b) => {
          const av = a[sort.key], bv = b[sort.key];
          const cmp = av > bv ? 1 : av < bv ? -1 : 0;
          return sort.dir === 'asc' ? cmp : -cmp;
        });
      }, [data, sort]);

      return (
        <div className="overflow-hidden rounded-2xl border">
          <table className="w-full text-sm">
            <thead className="bg-gray-50">
              <tr>
                {columns.map(col => (
                  <th key={col.key} onClick={() => col.sortable && toggleSort(col.key)}
                    className={cn('px-4 py-3 text-left text-xs font-semibold text-gray-500',
                      col.sortable && 'cursor-pointer hover:text-gray-700 select-none')}>
                    <div className="flex items-center gap-1">
                      {col.label}
                      {col.sortable && sort.key === col.key && (
                        <span className={THEMES[primary].accent}>{sort.dir === 'asc' ? '↑' : '↓'}</span>
                      )}
                    </div>
                  </th>
                ))}
              </tr>
            </thead>
            <tbody className="divide-y">
              {sorted.map((row, i) => (
                <tr key={i} className="hover:bg-gray-50 transition-colors">
                  {columns.map(col => (
                    <td key={col.key} className="px-4 py-3 text-gray-700">
                      {col.render ? col.render(row[col.key], row) : row[col.key]}
                    </td>
                  ))}
                </tr>
              ))}
            </tbody>
          </table>
        </div>
      );
    }

    // --- Filter Bar (Step 514) ---
    function FilterBar({ filters, values, onChange }) {
      return (
        <div className="flex flex-wrap gap-3">
          {filters.map(f => (
            <div key={f.key}>
              {f.type === 'search' && (
                <div className="relative">
                  <svg className="absolute left-3 top-1/2 -translate-y-1/2 w-3.5 h-3.5 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"/></svg>
                  <input type="text" placeholder={f.placeholder} value={values[f.key] || ''}
                    onChange={e => onChange({ ...values, [f.key]: e.target.value })}
                    className="border border-gray-300 rounded-xl pl-8 pr-4 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500 w-48"/>
                </div>
              )}
              {f.type === 'select' && (
                <select value={values[f.key] || ''} onChange={e => onChange({ ...values, [f.key]: e.target.value })}
                  className="border border-gray-300 rounded-xl px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500">
                  <option value="">{f.placeholder}</option>
                  {f.options.map(o => <option key={o.value} value={o.value}>{o.label}</option>)}
                </select>
              )}
            </div>
          ))}
          {Object.values(values).some(Boolean) && (
            <button onClick={() => onChange({})} className="text-xs text-gray-500 hover:text-red-500 transition-colors">✕ ล้างตัวกรอง</button>
          )}
        </div>
      );
    }

    // --- App ---
    function App() {
      const [theme, setTheme] = useState('indigo');
      const [filters, setFilters] = useState({});

      const allUsers = [
        { name: 'สมชาย เทคโน', email: 'somchai@tech.co', role: 'admin', status: 'active', joined: '2024-01' },
        { name: 'วิชัย ใจดี', email: 'wichai@email.com', role: 'member', status: 'active', joined: '2024-02' },
        { name: 'มานี ดีมาก', email: 'manee@corp.io', role: 'viewer', status: 'inactive', joined: '2024-03' },
        { name: 'สุดา เรียนดี', email: 'suda@school.edu', role: 'member', status: 'active', joined: '2024-04' },
        { name: 'ธนา รวยมาก', email: 'thana@money.co', role: 'admin', status: 'active', joined: '2024-05' },
      ];

      const filtered = allUsers.filter(u => {
        if (filters.search && !u.name.toLowerCase().includes(filters.search.toLowerCase()) && !u.email.toLowerCase().includes(filters.search.toLowerCase())) return false;
        if (filters.role && u.role !== filters.role) return false;
        if (filters.status && u.status !== filters.status) return false;
        return true;
      });

      const cols = [
        { key: 'name', label: 'ชื่อ', sortable: true },
        { key: 'email', label: 'อีเมล', sortable: true },
        { key: 'role', label: 'บทบาท', sortable: true, render: v => <span className={cn('text-xs font-semibold px-2 py-0.5 rounded-full', v === 'admin' ? 'bg-purple-100 text-purple-700' : 'bg-gray-100 text-gray-600')}>{v}</span> },
        { key: 'status', label: 'สถานะ', sortable: true, render: v => <span className={cn('text-xs font-semibold px-2 py-0.5 rounded-full', v === 'active' ? 'bg-green-100 text-green-700' : 'bg-gray-100 text-gray-400')}>{v}</span> },
        { key: 'joined', label: 'เข้าร่วม', sortable: true },
      ];

      return (
        <ThemeCtx.Provider value={{ primary: theme }}>
          <div className="min-h-screen bg-gray-50 p-6 space-y-6">
            <div className="flex items-center justify-between">
              <h1 className="text-xl font-extrabold">Advanced React + Tailwind</h1>
              <div className="flex gap-2">
                {['indigo','green','rose'].map(t => (
                  <button key={t} onClick={() => setTheme(t)}
                    className={cn('w-7 h-7 rounded-full border-2 transition-all',
                      t === 'indigo' ? 'bg-indigo-500' : t === 'green' ? 'bg-green-500' : 'bg-rose-500',
                      theme === t ? 'border-gray-900 scale-110' : 'border-transparent')}>
                  </button>
                ))}
              </div>
            </div>

            <div className="bg-white rounded-2xl border p-5 space-y-4">
              <div className="flex items-center justify-between">
                <h3 className="font-bold">ผู้ใช้ ({filtered.length}/{allUsers.length})</h3>
                <ThemeButton size="sm">+ เพิ่มผู้ใช้</ThemeButton>
              </div>
              <FilterBar
                filters={[
                  { key: 'search', type: 'search', placeholder: 'ค้นหาชื่อ/อีเมล...' },
                  { key: 'role', type: 'select', placeholder: 'ทุก role', options: [{ value: 'admin', label: 'Admin' }, { value: 'member', label: 'Member' }, { value: 'viewer', label: 'Viewer' }] },
                  { key: 'status', type: 'select', placeholder: 'ทุกสถานะ', options: [{ value: 'active', label: 'Active' }, { value: 'inactive', label: 'Inactive' }] },
                ]}
                values={filters}
                onChange={setFilters}
              />
              <DataTable columns={cols} data={filtered} />
            </div>
          </div>
        </ThemeCtx.Provider>
      );
    }

    ReactDOM.createRoot(document.getElementById('root')).render(<App />);
  </script>
</body>
</html>
```

---

## Steps 515–520: React Form Builder Workshop

```html
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>React Form Builder - Steps 515-520</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
  <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
</head>
<body class="bg-gray-50">
  <div id="root2"></div>
  <script type="text/babel">
    const { useState, useReducer, useCallback } = React;
    function cn(...c) { return c.filter(Boolean).join(' '); }

    // --- Multi-step form with validation (Step 515-520) ---
    const STEPS = [
      { id: 'personal', label: 'ข้อมูลส่วนตัว', icon: '👤' },
      { id: 'account', label: 'บัญชีผู้ใช้', icon: '🔑' },
      { id: 'plan', label: 'เลือกแผน', icon: '💳' },
      { id: 'confirm', label: 'ยืนยัน', icon: '✅' },
    ];

    const PLANS = [
      { id: 'free', name: 'Free', price: 0, features: ['5 Projects', '1 GB', 'Community Support'] },
      { id: 'pro', name: 'Pro', price: 890, features: ['Unlimited Projects', '50 GB', 'Priority Support'] },
      { id: 'enterprise', name: 'Enterprise', price: 2990, features: ['Unlimited Everything', 'SLA 99.9%', 'Dedicated Manager'] },
    ];

    function useForm(initial) {
      const [values, setValues] = useState(initial);
      const [touched, setTouched] = useState({});
      const [errors, setErrors] = useState({});

      const handleChange = useCallback((key, value) => {
        setValues(v => ({ ...v, [key]: value }));
        setTouched(t => ({ ...t, [key]: true }));
        setErrors(e => ({ ...e, [key]: null }));
      }, []);

      const touch = useCallback(key => setTouched(t => ({ ...t, [key]: true })), []);

      return { values, errors, touched, handleChange, touch, setErrors };
    }

    function StepProgress({ steps, current }) {
      const idx = steps.findIndex(s => s.id === current);
      return (
        <div className="flex items-center justify-between mb-8">
          {steps.map((step, i) => {
            const done = i < idx;
            const active = i === idx;
            return (
              <div key={step.id} className="flex items-center flex-1">
                <div className="flex flex-col items-center">
                  <div className={cn('w-9 h-9 rounded-full flex items-center justify-center text-sm font-bold transition-colors',
                    done ? 'bg-green-500 text-white' : active ? 'bg-indigo-600 text-white' : 'bg-gray-200 text-gray-500')}>
                    {done ? '✓' : step.icon}
                  </div>
                  <p className={cn('text-[10px] mt-1 font-medium hidden sm:block', active ? 'text-indigo-600' : 'text-gray-400')}>
                    {step.label}
                  </p>
                </div>
                {i < steps.length - 1 && (
                  <div className={cn('flex-1 h-0.5 mx-2 mb-4 transition-colors', done ? 'bg-green-400' : 'bg-gray-200')}/>
                )}
              </div>
            );
          })}
        </div>
      );
    }

    function FormField({ label, error, required, children }) {
      return (
        <div>
          {label && <label className="block text-sm font-medium text-gray-700 mb-1.5">{label}{required && <span className="text-red-500 ml-1">*</span>}</label>}
          {children}
          {error && <p className="text-xs text-red-500 mt-1">{error}</p>}
        </div>
      );
    }

    function App2() {
      const [step, setStep] = useState('personal');
      const [submitted, setSubmitted] = useState(false);
      const form = useForm({ firstName: '', lastName: '', email: '', phone: '', username: '', password: '', plan: 'pro', agree: false });

      const idx = STEPS.findIndex(s => s.id === step);

      function validateStep() {
        const errs = {};
        if (step === 'personal') {
          if (!form.values.firstName) errs.firstName = 'กรุณากรอกชื่อ';
          if (!form.values.lastName) errs.lastName = 'กรุณากรอกนามสกุล';
          if (!form.values.email || !/^[^\s@]+@[^\s@]+/.test(form.values.email)) errs.email = 'อีเมลไม่ถูกต้อง';
        }
        if (step === 'account') {
          if (!form.values.username || form.values.username.length < 4) errs.username = 'ชื่อผู้ใช้ต้องมีอย่างน้อย 4 ตัวอักษร';
          if (!form.values.password || form.values.password.length < 8) errs.password = 'รหัสผ่านต้องมีอย่างน้อย 8 ตัวอักษร';
        }
        if (step === 'confirm') {
          if (!form.values.agree) errs.agree = 'กรุณายอมรับเงื่อนไข';
        }
        form.setErrors(errs);
        return Object.keys(errs).length === 0;
      }

      function next() {
        if (!validateStep()) return;
        const nextIdx = idx + 1;
        if (nextIdx < STEPS.length) setStep(STEPS[nextIdx].id);
        else setSubmitted(true);
      }

      function back() {
        if (idx > 0) setStep(STEPS[idx - 1].id);
      }

      const inp = (key, type = 'text') => ({
        type, value: form.values[key],
        onChange: e => form.handleChange(key, e.target.value),
        className: cn('w-full border rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 transition-colors',
          form.errors[key] ? 'border-red-400 focus:ring-red-400' : 'border-gray-300 focus:ring-indigo-500'),
      });

      if (submitted) {
        return (
          <div className="min-h-screen flex items-center justify-center p-6">
            <div className="bg-white rounded-2xl border p-8 text-center max-w-md w-full">
              <div className="w-16 h-16 bg-green-100 rounded-full flex items-center justify-center text-3xl mx-auto mb-4">🎉</div>
              <h2 className="text-2xl font-extrabold mb-2">สมัครสำเร็จ!</h2>
              <p className="text-gray-600 text-sm mb-1">ยินดีต้อนรับ <strong>{form.values.firstName} {form.values.lastName}</strong></p>
              <p className="text-gray-500 text-sm mb-6">แผน: <strong className="text-indigo-600">{PLANS.find(p => p.id === form.values.plan)?.name}</strong></p>
              <button onClick={() => { setSubmitted(false); setStep('personal'); }} className="bg-indigo-600 text-white px-6 py-2.5 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">เริ่มต้นใหม่</button>
            </div>
          </div>
        );
      }

      return (
        <div className="min-h-screen flex items-center justify-center p-6">
          <div className="bg-white rounded-2xl border shadow-sm w-full max-w-xl p-6">
            <h2 className="text-xl font-extrabold mb-6 text-center">สมัครใช้งาน</h2>
            <StepProgress steps={STEPS} current={step} />

            {/* Personal info */}
            {step === 'personal' && (
              <div className="space-y-4">
                <div className="grid grid-cols-2 gap-4">
                  <FormField label="ชื่อ" required error={form.errors.firstName}>
                    <input {...inp('firstName')} placeholder="สมชาย"/>
                  </FormField>
                  <FormField label="นามสกุล" required error={form.errors.lastName}>
                    <input {...inp('lastName')} placeholder="เทคโน"/>
                  </FormField>
                </div>
                <FormField label="อีเมล" required error={form.errors.email}>
                  <input {...inp('email', 'email')} placeholder="user@example.com"/>
                </FormField>
                <FormField label="เบอร์โทร">
                  <input {...inp('phone', 'tel')} placeholder="08X-XXX-XXXX"/>
                </FormField>
              </div>
            )}

            {/* Account */}
            {step === 'account' && (
              <div className="space-y-4">
                <FormField label="ชื่อผู้ใช้" required error={form.errors.username}>
                  <input {...inp('username')} placeholder="somchai_tech"/>
                </FormField>
                <FormField label="รหัสผ่าน" required error={form.errors.password}>
                  <input {...inp('password', 'password')} placeholder="อย่างน้อย 8 ตัวอักษร"/>
                  <div className="mt-2">
                    <div className="flex gap-1">
                      {[...Array(4)].map((_, i) => (
                        <div key={i} className={cn('flex-1 h-1 rounded-full transition-colors',
                          form.values.password.length > i * 2 ? ['bg-red-400', 'bg-yellow-400', 'bg-blue-400', 'bg-green-500'][i] : 'bg-gray-200')}/>
                      ))}
                    </div>
                    <p className="text-[10px] text-gray-400 mt-1">{['', 'อ่อนมาก', 'อ่อน', 'ปานกลาง', 'แข็งแกร่ง'][Math.min(4, Math.ceil(form.values.password.length / 2))]}</p>
                  </div>
                </FormField>
              </div>
            )}

            {/* Plan */}
            {step === 'plan' && (
              <div className="grid grid-cols-1 gap-3">
                {PLANS.map(plan => (
                  <label key={plan.id} className={cn('flex items-center gap-4 p-4 rounded-xl border-2 cursor-pointer transition-colors',
                    form.values.plan === plan.id ? 'border-indigo-600 bg-indigo-50' : 'border-gray-200 hover:border-gray-300')}>
                    <input type="radio" name="plan" value={plan.id} checked={form.values.plan === plan.id}
                      onChange={e => form.handleChange('plan', e.target.value)} className="text-indigo-600"/>
                    <div className="flex-1">
                      <div className="flex items-center justify-between">
                        <p className="font-bold text-sm">{plan.name}</p>
                        <p className="font-extrabold text-indigo-600">{plan.price === 0 ? 'ฟรี' : `฿${plan.price}/เดือน`}</p>
                      </div>
                      <p className="text-xs text-gray-500 mt-1">{plan.features.join(' · ')}</p>
                    </div>
                  </label>
                ))}
              </div>
            )}

            {/* Confirm */}
            {step === 'confirm' && (
              <div className="space-y-4">
                <div className="bg-gray-50 rounded-xl p-4 space-y-2 text-sm">
                  <div className="flex justify-between"><span className="text-gray-500">ชื่อ:</span><span className="font-medium">{form.values.firstName} {form.values.lastName}</span></div>
                  <div className="flex justify-between"><span className="text-gray-500">อีเมล:</span><span className="font-medium">{form.values.email}</span></div>
                  <div className="flex justify-between"><span className="text-gray-500">Username:</span><span className="font-medium">{form.values.username}</span></div>
                  <div className="flex justify-between"><span className="text-gray-500">แผน:</span><span className="font-bold text-indigo-600">{PLANS.find(p => p.id === form.values.plan)?.name}</span></div>
                </div>
                <label className="flex items-start gap-2 cursor-pointer">
                  <input type="checkbox" checked={form.values.agree}
                    onChange={e => form.handleChange('agree', e.target.checked)}
                    className="mt-0.5 w-4 h-4 rounded text-indigo-600"/>
                  <span className="text-xs text-gray-600">ฉันยอมรับ <span className="text-indigo-600 underline cursor-pointer">เงื่อนไขการใช้งาน</span> และ <span className="text-indigo-600 underline cursor-pointer">นโยบายความเป็นส่วนตัว</span></span>
                </label>
                {form.errors.agree && <p className="text-xs text-red-500">{form.errors.agree}</p>}
              </div>
            )}

            <div className="flex gap-3 mt-8">
              {idx > 0 && <button onClick={back} className="flex-1 border border-gray-300 py-2.5 rounded-xl text-sm font-semibold hover:bg-gray-50 transition-colors">← ย้อนกลับ</button>}
              <button onClick={next} className="flex-1 bg-indigo-600 text-white py-2.5 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">
                {step === 'confirm' ? '🚀 สมัครเลย' : 'ถัดไป →'}
              </button>
            </div>
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

## สรุป Part 52

| Step | เนื้อหา |
|------|---------|
| 511 | Theme Context: ThemeCtx, useTheme, THEMES object, ThemeButton/ThemeBadge |
| 512 | Sortable DataTable: useMemo sort, column renderer |
| 513 | Filter bar: search + select + clear |
| 514 | Theme + Table combined app |
| 515 | Multi-step form: StepProgress component |
| 516 | Form fields: validation per step |
| 517 | Password strength indicator |
| 518 | Plan picker: radio card group |
| 519 | Confirm step: summary + checkbox agree |
| 520 | Workshop: Complete Registration Wizard (4-step + validation) |

**Part ถัดไป:** Part 53 — Vue + Tailwind Integration (Steps 521–530)
