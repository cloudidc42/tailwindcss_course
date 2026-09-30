# Part 58: Headless UI + Tailwind

## เป้าหมาย
- @headlessui/react — accessible UI primitives
- Tailwind CSS + WAI-ARIA compliant components
- Dialog, Menu, Combobox, Tabs, Switch, Disclosure
- Steps 571–580

---

## Step 571: Headless UI Setup

```bash
npm install @headlessui/react
# หรือสำหรับ Vue:
npm install @headlessui/vue
```

Headless UI คือ unstyled, accessible components — เราเพิ่ม Tailwind classes เองทั้งหมด

---

## Step 572: Dialog (Modal)

```tsx
'use client'
import { Dialog, Transition } from '@headlessui/react'
import { Fragment, useState } from 'react'

export function ExampleModal() {
  const [isOpen, setIsOpen] = useState(false)

  return (
    <>
      <button onClick={() => setIsOpen(true)}
        className="bg-indigo-600 text-white px-4 py-2.5 rounded-xl text-sm font-semibold hover:bg-indigo-700 transition-colors">
        เปิด Modal
      </button>

      <Transition appear show={isOpen} as={Fragment}>
        <Dialog as="div" className="relative z-50" onClose={() => setIsOpen(false)}>
          {/* Backdrop */}
          <Transition.Child
            as={Fragment}
            enter="ease-out duration-200"
            enterFrom="opacity-0"
            enterTo="opacity-100"
            leave="ease-in duration-150"
            leaveFrom="opacity-100"
            leaveTo="opacity-0">
            <div className="fixed inset-0 bg-black/50" aria-hidden="true" />
          </Transition.Child>

          <div className="fixed inset-0 overflow-y-auto">
            <div className="flex min-h-full items-center justify-center p-4">
              <Transition.Child
                as={Fragment}
                enter="ease-out duration-200"
                enterFrom="opacity-0 scale-95"
                enterTo="opacity-100 scale-100"
                leave="ease-in duration-150"
                leaveFrom="opacity-100 scale-100"
                leaveTo="opacity-0 scale-95">
                <Dialog.Panel className="w-full max-w-md bg-white rounded-2xl shadow-2xl p-6">
                  <Dialog.Title className="text-lg font-extrabold text-gray-900 mb-2">
                    ยืนยันการลบ
                  </Dialog.Title>
                  <Dialog.Description className="text-sm text-gray-500 mb-6">
                    คุณแน่ใจว่าต้องการลบรายการนี้? การดำเนินการนี้ไม่สามารถย้อนกลับได้
                  </Dialog.Description>
                  <div className="flex gap-3">
                    <button onClick={() => setIsOpen(false)}
                      className="flex-1 border border-gray-300 py-2.5 rounded-xl text-sm font-semibold hover:bg-gray-50 transition-colors">
                      ยกเลิก
                    </button>
                    <button onClick={() => setIsOpen(false)}
                      className="flex-1 bg-red-600 text-white py-2.5 rounded-xl text-sm font-semibold hover:bg-red-700 transition-colors">
                      ลบ
                    </button>
                  </div>
                </Dialog.Panel>
              </Transition.Child>
            </div>
          </div>
        </Dialog>
      </Transition>
    </>
  )
}
```

---

## Step 573: Menu (Dropdown)

```tsx
import { Menu, Transition } from '@headlessui/react'
import { Fragment } from 'react'

const menuItems = [
  { label: 'แก้ไขโปรไฟล์', icon: '✏️', href: '#' },
  { label: 'การตั้งค่า', icon: '⚙️', href: '#' },
  { label: 'ช่วยเหลือ', icon: '❓', href: '#' },
  { label: 'ออกจากระบบ', icon: '🚪', href: '#', danger: true },
]

export function UserMenu() {
  return (
    <Menu as="div" className="relative">
      <Menu.Button className="flex items-center gap-2 px-3 py-1.5 rounded-xl hover:bg-gray-100 transition-colors">
        <div className="w-7 h-7 bg-indigo-100 rounded-lg flex items-center justify-center text-xs font-bold text-indigo-600">SK</div>
        <span className="text-sm font-medium text-gray-700">สมชาย</span>
        <svg className="w-4 h-4 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M19 9l-7 7-7-7" />
        </svg>
      </Menu.Button>

      <Transition
        as={Fragment}
        enter="transition ease-out duration-100"
        enterFrom="transform opacity-0 scale-95"
        enterTo="transform opacity-100 scale-100"
        leave="transition ease-in duration-75"
        leaveFrom="transform opacity-100 scale-100"
        leaveTo="transform opacity-0 scale-95">
        <Menu.Items className="absolute right-0 mt-2 w-48 bg-white rounded-2xl shadow-lg border border-gray-100 py-1 focus:outline-none z-10">
          {menuItems.map(item => (
            <Menu.Item key={item.label}>
              {({ active }) => (
                <a href={item.href}
                  className={`flex items-center gap-3 px-4 py-2.5 text-sm transition-colors ${
                    active
                      ? item.danger ? 'bg-red-50 text-red-600' : 'bg-indigo-50 text-indigo-700'
                      : item.danger ? 'text-red-500' : 'text-gray-700'
                  }`}>
                  <span>{item.icon}</span>
                  {item.label}
                </a>
              )}
            </Menu.Item>
          ))}
        </Menu.Items>
      </Transition>
    </Menu>
  )
}
```

---

## Step 574: Tabs

```tsx
import { Tab } from '@headlessui/react'

const tabs = [
  { label: 'ภาพรวม', content: <p className="text-sm text-gray-600">ข้อมูลภาพรวมทั้งหมด</p> },
  { label: 'สถิติ', content: <p className="text-sm text-gray-600">สถิติการใช้งาน</p> },
  { label: 'รายงาน', content: <p className="text-sm text-gray-600">ดาวน์โหลดรายงาน</p> },
]

export function ExampleTabs() {
  return (
    <Tab.Group>
      <Tab.List className="flex border-b border-gray-200 gap-1">
        {tabs.map(tab => (
          <Tab key={tab.label}
            className={({ selected }) =>
              `px-5 py-3 text-sm font-semibold border-b-2 transition-colors focus:outline-none ${
                selected
                  ? 'border-indigo-600 text-indigo-600'
                  : 'border-transparent text-gray-500 hover:text-gray-700 hover:border-gray-300'
              }`
            }>
            {tab.label}
          </Tab>
        ))}
      </Tab.List>
      <Tab.Panels className="mt-4">
        {tabs.map(tab => (
          <Tab.Panel key={tab.label} className="focus:outline-none">
            {tab.content}
          </Tab.Panel>
        ))}
      </Tab.Panels>
    </Tab.Group>
  )
}
```

---

## Step 575: Switch (Toggle)

```tsx
import { Switch } from '@headlessui/react'
import { useState } from 'react'

export function ToggleExample() {
  const [enabled, setEnabled] = useState(false)

  return (
    <Switch.Group>
      <div className="flex items-center justify-between py-3 border-b">
        <div>
          <Switch.Label className="text-sm font-medium text-gray-900 cursor-pointer">
            การแจ้งเตือน Push
          </Switch.Label>
          <p className="text-xs text-gray-500 mt-0.5">รับการแจ้งเตือนผ่านเบราว์เซอร์</p>
        </div>
        <Switch
          checked={enabled}
          onChange={setEnabled}
          className={`relative inline-flex h-6 w-11 items-center rounded-full transition-colors focus:outline-none focus-visible:ring-2 focus-visible:ring-indigo-500 focus-visible:ring-offset-2 ${
            enabled ? 'bg-indigo-600' : 'bg-gray-300'
          }`}>
          <span className="sr-only">Toggle notification</span>
          <span className={`inline-block h-4 w-4 transform rounded-full bg-white shadow-sm transition-transform ${
            enabled ? 'translate-x-6' : 'translate-x-1'
          }`} />
        </Switch>
      </div>
    </Switch.Group>
  )
}
```

---

## Step 576: Disclosure (Accordion)

```tsx
import { Disclosure, Transition } from '@headlessui/react'

const faqs = [
  { q: 'Tailwind CSS คืออะไร?', a: 'Tailwind CSS คือ utility-first CSS framework ที่ให้คุณสร้าง UI ได้รวดเร็ว' },
  { q: 'ต้องใช้ความรู้อะไรบ้าง?', a: 'ต้องรู้ HTML/CSS พื้นฐาน และมีความเข้าใจ box model' },
  { q: 'Headless UI คืออะไร?', a: 'Headless UI คือ unstyled components ที่ built-in accessibility ให้แล้ว' },
]

export function FaqAccordion() {
  return (
    <div className="space-y-2">
      {faqs.map(faq => (
        <Disclosure key={faq.q}>
          {({ open }) => (
            <div className="bg-white rounded-2xl border border-gray-200 overflow-hidden">
              <Disclosure.Button className="flex w-full items-center justify-between px-5 py-4 text-left focus:outline-none">
                <span className="text-sm font-semibold text-gray-900">{faq.q}</span>
                <svg
                  className={`w-5 h-5 text-gray-500 transition-transform ${open ? 'rotate-180' : ''}`}
                  fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M19 9l-7 7-7-7" />
                </svg>
              </Disclosure.Button>
              <Transition
                enter="transition ease-out duration-200"
                enterFrom="opacity-0 -translate-y-1"
                enterTo="opacity-100 translate-y-0"
                leave="transition ease-in duration-150"
                leaveFrom="opacity-100 translate-y-0"
                leaveTo="opacity-0 -translate-y-1">
                <Disclosure.Panel className="px-5 pb-4 text-sm text-gray-600 border-t bg-gray-50 py-3">
                  {faq.a}
                </Disclosure.Panel>
              </Transition>
            </div>
          )}
        </Disclosure>
      ))}
    </div>
  )
}
```

---

## Step 577: Combobox (Autocomplete)

```tsx
import { Combobox, Transition } from '@headlessui/react'
import { useState, Fragment } from 'react'

const people = ['สมชาย', 'วิชัย', 'นิดา', 'สมหญิง', 'อนุชา', 'ปรีชา', 'มนัส']

export function ComboboxExample() {
  const [selected, setSelected] = useState('')
  const [query, setQuery] = useState('')

  const filtered = query === ''
    ? people
    : people.filter(p => p.toLowerCase().includes(query.toLowerCase()))

  return (
    <Combobox value={selected} onChange={setSelected}>
      <div className="relative">
        <Combobox.Input
          className="w-full border border-gray-300 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:ring-2 focus:ring-indigo-500"
          displayValue={(p: string) => p}
          onChange={e => setQuery(e.target.value)}
          placeholder="ค้นหาชื่อ..."
        />
        <Combobox.Button className="absolute inset-y-0 right-3 flex items-center text-gray-400">▼</Combobox.Button>

        <Transition
          as={Fragment}
          leave="transition ease-in duration-100"
          leaveFrom="opacity-100"
          leaveTo="opacity-0"
          afterLeave={() => setQuery('')}>
          <Combobox.Options className="absolute z-10 mt-1 w-full bg-white rounded-2xl border border-gray-200 shadow-lg py-1 focus:outline-none">
            {filtered.length === 0 ? (
              <div className="px-4 py-2.5 text-sm text-gray-400">ไม่พบผลลัพธ์</div>
            ) : (
              filtered.map(p => (
                <Combobox.Option key={p} value={p}
                  className={({ active }) =>
                    `flex items-center gap-2 px-4 py-2.5 text-sm cursor-pointer ${
                      active ? 'bg-indigo-50 text-indigo-700' : 'text-gray-700'
                    }`
                  }>
                  {({ selected: sel }) => (
                    <>
                      <span>{p}</span>
                      {sel && <span className="ml-auto text-indigo-600 text-xs">✓</span>}
                    </>
                  )}
                </Combobox.Option>
              ))
            )}
          </Combobox.Options>
        </Transition>
      </div>
    </Combobox>
  )
}
```

---

## Step 578: Listbox (Select)

```tsx
import { Listbox, Transition } from '@headlessui/react'
import { Fragment, useState } from 'react'

const roles = [
  { id: 'user', label: 'User', icon: '👤' },
  { id: 'editor', label: 'Editor', icon: '✏️' },
  { id: 'admin', label: 'Admin', icon: '🛡️' },
]

export function RoleSelect() {
  const [selected, setSelected] = useState(roles[0])

  return (
    <Listbox value={selected} onChange={setSelected}>
      <div className="relative">
        <Listbox.Button className="relative w-full border border-gray-300 rounded-xl px-4 py-2.5 text-left text-sm focus:outline-none focus-visible:ring-2 focus-visible:ring-indigo-500">
          <span className="flex items-center gap-2">
            <span>{selected.icon}</span>
            <span>{selected.label}</span>
          </span>
          <span className="absolute inset-y-0 right-3 flex items-center text-gray-400">▼</span>
        </Listbox.Button>

        <Transition as={Fragment} leave="transition ease-in duration-100" leaveFrom="opacity-100" leaveTo="opacity-0">
          <Listbox.Options className="absolute z-10 mt-1 w-full bg-white rounded-2xl border border-gray-200 shadow-lg py-1 focus:outline-none">
            {roles.map(role => (
              <Listbox.Option key={role.id} value={role}
                className={({ active }) =>
                  `flex items-center gap-3 px-4 py-2.5 text-sm cursor-pointer ${
                    active ? 'bg-indigo-50 text-indigo-700' : 'text-gray-700'
                  }`
                }>
                {({ selected: sel }) => (
                  <>
                    <span>{role.icon}</span>
                    <span className={sel ? 'font-semibold' : ''}>{role.label}</span>
                    {sel && <span className="ml-auto text-indigo-600 text-xs">✓</span>}
                  </>
                )}
              </Listbox.Option>
            ))}
          </Listbox.Options>
        </Transition>
      </div>
    </Listbox>
  )
}
```

---

## Steps 579–580: RadioGroup + Workshop

```tsx
import { RadioGroup } from '@headlessui/react'
import { useState } from 'react'

const plans = [
  { id: 'free', label: 'Free', price: '฿0/เดือน', features: '5 projects, 1GB' },
  { id: 'pro', label: 'Pro', price: '฿299/เดือน', features: 'Unlimited projects, 100GB' },
  { id: 'enterprise', label: 'Enterprise', price: 'Custom', features: 'Unlimited everything, SLA' },
]

export function PlanPicker() {
  const [selected, setSelected] = useState(plans[0])

  return (
    <RadioGroup value={selected} onChange={setSelected}>
      <RadioGroup.Label className="text-sm font-bold text-gray-900 mb-3 block">เลือกแผนบริการ</RadioGroup.Label>
      <div className="space-y-2">
        {plans.map(plan => (
          <RadioGroup.Option key={plan.id} value={plan}
            className={({ checked }) =>
              `flex items-center justify-between p-4 rounded-2xl border-2 cursor-pointer transition-colors ${
                checked ? 'border-indigo-600 bg-indigo-50' : 'border-gray-200 hover:border-gray-300'
              }`
            }>
            {({ checked }) => (
              <>
                <div>
                  <RadioGroup.Label className={`text-sm font-semibold ${checked ? 'text-indigo-900' : 'text-gray-900'}`}>
                    {plan.label}
                  </RadioGroup.Label>
                  <RadioGroup.Description className="text-xs text-gray-500 mt-0.5">{plan.features}</RadioGroup.Description>
                </div>
                <div className="text-right">
                  <p className={`text-sm font-bold ${checked ? 'text-indigo-700' : 'text-gray-700'}`}>{plan.price}</p>
                  <div className={`w-5 h-5 rounded-full border-2 mt-1 ml-auto flex items-center justify-center ${
                    checked ? 'border-indigo-600 bg-indigo-600' : 'border-gray-400'
                  }`}>
                    {checked && <span className="w-2 h-2 bg-white rounded-full" />}
                  </div>
                </div>
              </>
            )}
          </RadioGroup.Option>
        ))}
      </div>
    </RadioGroup>
  )
}
```

---

## สรุป Part 58

| Step | เนื้อหา |
|------|---------|
| 571 | Headless UI คืออะไร, setup, concept |
| 572 | Dialog — Transition backdrop + panel, accessible modal |
| 573 | Menu — dropdown with active state, keyboard navigation |
| 574 | Tab.Group — tab panels with focus management |
| 575 | Switch — accessible toggle with focus-visible ring |
| 576 | Disclosure — accordion with Transition |
| 577 | Combobox — autocomplete with filtered list |
| 578 | Listbox — custom select with icons |
| 579 | RadioGroup — plan picker with checked state |
| 580 | Workshop: Form page combining Dialog + Tabs + Switch + Listbox + RadioGroup |

**Part ถัดไป:** Part 59 — Radix UI + Tailwind (Steps 581–590)
