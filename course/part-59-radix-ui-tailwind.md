# Part 59: Radix UI + Tailwind

## เป้าหมาย
- Radix UI primitives — unstyled, fully accessible
- @radix-ui/react-* packages + Tailwind CSS
- cva (class-variance-authority) สำหรับ variant system
- Steps 581–590

---

## Step 581: Radix UI + cva Setup

```bash
npm install @radix-ui/react-dialog \
            @radix-ui/react-dropdown-menu \
            @radix-ui/react-tabs \
            @radix-ui/react-switch \
            @radix-ui/react-tooltip \
            @radix-ui/react-popover \
            @radix-ui/react-select \
            @radix-ui/react-toast \
            @radix-ui/react-accordion \
            @radix-ui/react-avatar \
            class-variance-authority \
            clsx \
            tailwind-merge
```

`src/lib/utils.ts` — cn helper:
```ts
import { clsx, type ClassValue } from 'clsx'
import { twMerge } from 'tailwind-merge'

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}
```

---

## Step 582: Button with cva

```tsx
import { cva, type VariantProps } from 'class-variance-authority'
import { cn } from '@/lib/utils'
import { forwardRef, type ButtonHTMLAttributes } from 'react'

const buttonVariants = cva(
  'inline-flex items-center justify-center gap-2 rounded-xl text-sm font-semibold transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50',
  {
    variants: {
      variant: {
        primary:  'bg-indigo-600 text-white hover:bg-indigo-700 focus-visible:ring-indigo-500',
        outline:  'border-2 border-indigo-600 text-indigo-600 hover:bg-indigo-50',
        ghost:    'text-gray-700 hover:bg-gray-100',
        danger:   'bg-red-600 text-white hover:bg-red-700 focus-visible:ring-red-500',
        subtle:   'bg-gray-100 text-gray-700 hover:bg-gray-200',
      },
      size: {
        xs: 'h-7 px-2.5 text-xs',
        sm: 'h-8 px-3',
        md: 'h-9 px-4',
        lg: 'h-10 px-5',
        xl: 'h-12 px-6 text-base',
        icon: 'h-9 w-9',
      },
    },
    defaultVariants: {
      variant: 'primary',
      size: 'md',
    },
  }
)

export interface ButtonProps
  extends ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof buttonVariants> {
  loading?: boolean
}

const Button = forwardRef<HTMLButtonElement, ButtonProps>(
  ({ className, variant, size, loading, children, disabled, ...props }, ref) => (
    <button
      ref={ref}
      className={cn(buttonVariants({ variant, size }), className)}
      disabled={disabled || loading}
      {...props}>
      {loading && (
        <svg className="w-4 h-4 animate-spin" fill="none" viewBox="0 0 24 24">
          <circle className="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" strokeWidth="4" />
          <path className="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4z" />
        </svg>
      )}
      {children}
    </button>
  )
)
Button.displayName = 'Button'

export { Button, buttonVariants }
```

---

## Step 583: Dialog Component

```tsx
import * as DialogPrimitive from '@radix-ui/react-dialog'
import { cn } from '@/lib/utils'
import { forwardRef, type ComponentPropsWithoutRef, type ElementRef } from 'react'

const Dialog = DialogPrimitive.Root
const DialogTrigger = DialogPrimitive.Trigger
const DialogPortal = DialogPrimitive.Portal

const DialogOverlay = forwardRef<
  ElementRef<typeof DialogPrimitive.Overlay>,
  ComponentPropsWithoutRef<typeof DialogPrimitive.Overlay>
>(({ className, ...props }, ref) => (
  <DialogPrimitive.Overlay
    ref={ref}
    className={cn(
      'fixed inset-0 z-50 bg-black/50 backdrop-blur-sm',
      'data-[state=open]:animate-in data-[state=closed]:animate-out',
      'data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0',
      className
    )}
    {...props}
  />
))
DialogOverlay.displayName = DialogPrimitive.Overlay.displayName

const DialogContent = forwardRef<
  ElementRef<typeof DialogPrimitive.Content>,
  ComponentPropsWithoutRef<typeof DialogPrimitive.Content>
>(({ className, children, ...props }, ref) => (
  <DialogPortal>
    <DialogOverlay />
    <DialogPrimitive.Content
      ref={ref}
      className={cn(
        'fixed left-1/2 top-1/2 z-50 w-full max-w-lg -translate-x-1/2 -translate-y-1/2',
        'bg-white rounded-2xl shadow-2xl p-6',
        'data-[state=open]:animate-in data-[state=closed]:animate-out',
        'data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0',
        'data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-95',
        'data-[state=closed]:slide-out-to-left-1/2 data-[state=open]:slide-in-from-left-1/2',
        'data-[state=closed]:slide-out-to-top-[48%] data-[state=open]:slide-in-from-top-[48%]',
        className
      )}
      {...props}>
      {children}
      <DialogPrimitive.Close className="absolute right-4 top-4 rounded-xl p-1 text-gray-400 hover:text-gray-600 transition-colors focus:outline-none focus-visible:ring-2 focus-visible:ring-indigo-500">
        <svg className="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M6 18L18 6M6 6l12 12" />
        </svg>
        <span className="sr-only">ปิด</span>
      </DialogPrimitive.Close>
    </DialogPrimitive.Content>
  </DialogPortal>
))
DialogContent.displayName = DialogPrimitive.Content.displayName

const DialogHeader = ({ className, ...props }: React.HTMLAttributes<HTMLDivElement>) => (
  <div className={cn('mb-4 space-y-1', className)} {...props} />
)

const DialogTitle = forwardRef<
  ElementRef<typeof DialogPrimitive.Title>,
  ComponentPropsWithoutRef<typeof DialogPrimitive.Title>
>(({ className, ...props }, ref) => (
  <DialogPrimitive.Title
    ref={ref}
    className={cn('text-lg font-extrabold text-gray-900', className)}
    {...props}
  />
))
DialogTitle.displayName = DialogPrimitive.Title.displayName

const DialogDescription = forwardRef<
  ElementRef<typeof DialogPrimitive.Description>,
  ComponentPropsWithoutRef<typeof DialogPrimitive.Description>
>(({ className, ...props }, ref) => (
  <DialogPrimitive.Description
    ref={ref}
    className={cn('text-sm text-gray-500', className)}
    {...props}
  />
))
DialogDescription.displayName = DialogPrimitive.Description.displayName

export { Dialog, DialogTrigger, DialogContent, DialogHeader, DialogTitle, DialogDescription }
```

---

## Step 584: DropdownMenu

```tsx
import * as DropdownMenuPrimitive from '@radix-ui/react-dropdown-menu'
import { cn } from '@/lib/utils'
import { forwardRef, type ComponentPropsWithoutRef, type ElementRef } from 'react'

const DropdownMenu = DropdownMenuPrimitive.Root
const DropdownMenuTrigger = DropdownMenuPrimitive.Trigger

const DropdownMenuContent = forwardRef<
  ElementRef<typeof DropdownMenuPrimitive.Content>,
  ComponentPropsWithoutRef<typeof DropdownMenuPrimitive.Content>
>(({ className, sideOffset = 4, ...props }, ref) => (
  <DropdownMenuPrimitive.Portal>
    <DropdownMenuPrimitive.Content
      ref={ref}
      sideOffset={sideOffset}
      className={cn(
        'z-50 min-w-[10rem] overflow-hidden rounded-2xl border border-gray-100 bg-white p-1 shadow-lg',
        'data-[state=open]:animate-in data-[state=closed]:animate-out',
        'data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0',
        'data-[state=closed]:zoom-out-95 data-[state=open]:zoom-in-95',
        'data-[side=bottom]:slide-in-from-top-2',
        className
      )}
      {...props}
    />
  </DropdownMenuPrimitive.Portal>
))
DropdownMenuContent.displayName = DropdownMenuPrimitive.Content.displayName

const DropdownMenuItem = forwardRef<
  ElementRef<typeof DropdownMenuPrimitive.Item>,
  ComponentPropsWithoutRef<typeof DropdownMenuPrimitive.Item> & { inset?: boolean }
>(({ className, inset, ...props }, ref) => (
  <DropdownMenuPrimitive.Item
    ref={ref}
    className={cn(
      'relative flex cursor-pointer select-none items-center gap-2.5 rounded-xl px-3 py-2 text-sm text-gray-700',
      'outline-none transition-colors',
      'focus:bg-indigo-50 focus:text-indigo-700',
      'data-[disabled]:pointer-events-none data-[disabled]:opacity-50',
      inset && 'pl-8',
      className
    )}
    {...props}
  />
))
DropdownMenuItem.displayName = DropdownMenuPrimitive.Item.displayName

const DropdownMenuSeparator = forwardRef<
  ElementRef<typeof DropdownMenuPrimitive.Separator>,
  ComponentPropsWithoutRef<typeof DropdownMenuPrimitive.Separator>
>(({ className, ...props }, ref) => (
  <DropdownMenuPrimitive.Separator
    ref={ref}
    className={cn('-mx-1 my-1 h-px bg-gray-100', className)}
    {...props}
  />
))
DropdownMenuSeparator.displayName = DropdownMenuPrimitive.Separator.displayName

export { DropdownMenu, DropdownMenuTrigger, DropdownMenuContent, DropdownMenuItem, DropdownMenuSeparator }
```

---

## Step 585: Tooltip

```tsx
import * as TooltipPrimitive from '@radix-ui/react-tooltip'
import { cn } from '@/lib/utils'
import { forwardRef, type ComponentPropsWithoutRef, type ElementRef } from 'react'

const TooltipProvider = TooltipPrimitive.Provider
const Tooltip = TooltipPrimitive.Root
const TooltipTrigger = TooltipPrimitive.Trigger

const TooltipContent = forwardRef<
  ElementRef<typeof TooltipPrimitive.Content>,
  ComponentPropsWithoutRef<typeof TooltipPrimitive.Content>
>(({ className, sideOffset = 4, ...props }, ref) => (
  <TooltipPrimitive.Content
    ref={ref}
    sideOffset={sideOffset}
    className={cn(
      'z-50 overflow-hidden rounded-xl bg-gray-900 px-3 py-1.5 text-xs font-medium text-white shadow-lg',
      'animate-in fade-in-0 zoom-in-95',
      'data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=closed]:zoom-out-95',
      'data-[side=bottom]:slide-in-from-top-2',
      'data-[side=left]:slide-in-from-right-2',
      'data-[side=right]:slide-in-from-left-2',
      'data-[side=top]:slide-in-from-bottom-2',
      className
    )}
    {...props}
  />
))
TooltipContent.displayName = TooltipPrimitive.Content.displayName

export { TooltipProvider, Tooltip, TooltipTrigger, TooltipContent }

// Usage example:
export function TooltipDemo() {
  return (
    <TooltipProvider>
      <Tooltip>
        <TooltipTrigger asChild>
          <button className="w-9 h-9 bg-gray-100 rounded-xl flex items-center justify-center text-gray-600 hover:bg-gray-200 transition-colors">
            ℹ️
          </button>
        </TooltipTrigger>
        <TooltipContent>
          <p>คลิกเพื่อดูข้อมูลเพิ่มเติม</p>
        </TooltipContent>
      </Tooltip>
    </TooltipProvider>
  )
}
```

---

## Steps 586–590: Toast + Avatar + Badge + Workshop

```tsx
// Avatar
import * as AvatarPrimitive from '@radix-ui/react-avatar'
import { cn } from '@/lib/utils'
import { forwardRef } from 'react'

const Avatar = forwardRef<
  React.ElementRef<typeof AvatarPrimitive.Root>,
  React.ComponentPropsWithoutRef<typeof AvatarPrimitive.Root>
>(({ className, ...props }, ref) => (
  <AvatarPrimitive.Root
    ref={ref}
    className={cn('relative flex h-9 w-9 shrink-0 overflow-hidden rounded-xl', className)}
    {...props}
  />
))
Avatar.displayName = AvatarPrimitive.Root.displayName

const AvatarImage = forwardRef<
  React.ElementRef<typeof AvatarPrimitive.Image>,
  React.ComponentPropsWithoutRef<typeof AvatarPrimitive.Image>
>(({ className, ...props }, ref) => (
  <AvatarPrimitive.Image
    ref={ref}
    className={cn('aspect-square h-full w-full object-cover', className)}
    {...props}
  />
))
AvatarImage.displayName = AvatarPrimitive.Image.displayName

const AvatarFallback = forwardRef<
  React.ElementRef<typeof AvatarPrimitive.Fallback>,
  React.ComponentPropsWithoutRef<typeof AvatarPrimitive.Fallback>
>(({ className, ...props }, ref) => (
  <AvatarPrimitive.Fallback
    ref={ref}
    className={cn('flex h-full w-full items-center justify-center bg-indigo-100 text-xs font-bold text-indigo-700', className)}
    {...props}
  />
))
AvatarFallback.displayName = AvatarPrimitive.Fallback.displayName

export { Avatar, AvatarImage, AvatarFallback }

// Badge with cva
import { cva } from 'class-variance-authority'

const badgeVariants = cva(
  'inline-flex items-center gap-1 rounded-full text-xs font-semibold transition-colors',
  {
    variants: {
      variant: {
        default:  'bg-indigo-100 text-indigo-700',
        success:  'bg-green-100 text-green-700',
        warning:  'bg-yellow-100 text-yellow-700',
        danger:   'bg-red-100 text-red-700',
        outline:  'border border-gray-300 text-gray-600',
      },
      size: {
        sm: 'px-2 py-0.5',
        md: 'px-2.5 py-1',
        lg: 'px-3 py-1.5 text-sm',
      },
    },
    defaultVariants: { variant: 'default', size: 'md' },
  }
)

function Badge({ className, variant, size, ...props }: React.HTMLAttributes<HTMLSpanElement> & VariantProps<typeof badgeVariants>) {
  return <span className={cn(badgeVariants({ variant, size }), className)} {...props} />
}

export { Badge, badgeVariants }
```

Component showcase page:
```tsx
export default function ComponentsPage() {
  const [dialogOpen, setDialogOpen] = useState(false)

  return (
    <TooltipProvider>
      <div className="space-y-8 p-6">
        <h1 className="text-xl font-extrabold">Radix UI + Tailwind Component Library</h1>

        {/* Buttons */}
        <section className="card p-5 space-y-3">
          <h2 className="font-bold text-sm">Button Variants</h2>
          <div className="flex flex-wrap gap-2">
            <Button variant="primary">Primary</Button>
            <Button variant="outline">Outline</Button>
            <Button variant="ghost">Ghost</Button>
            <Button variant="danger">Danger</Button>
            <Button variant="subtle">Subtle</Button>
            <Button variant="primary" size="sm">Small</Button>
            <Button variant="primary" size="lg">Large</Button>
            <Button variant="primary" loading>Loading</Button>
          </div>
        </section>

        {/* Badges */}
        <section className="card p-5 space-y-3">
          <h2 className="font-bold text-sm">Badges</h2>
          <div className="flex flex-wrap gap-2">
            <Badge variant="default">Default</Badge>
            <Badge variant="success">Success</Badge>
            <Badge variant="warning">Warning</Badge>
            <Badge variant="danger">Danger</Badge>
            <Badge variant="outline">Outline</Badge>
          </div>
        </section>

        {/* Avatars */}
        <section className="card p-5 space-y-3">
          <h2 className="font-bold text-sm">Avatars</h2>
          <div className="flex items-center gap-3">
            <Avatar><AvatarFallback>SK</AvatarFallback></Avatar>
            <Avatar className="h-12 w-12"><AvatarFallback className="text-sm">WD</AvatarFallback></Avatar>
            <Avatar className="h-14 w-14 rounded-full"><AvatarFallback>ND</AvatarFallback></Avatar>
          </div>
        </section>

        {/* Dialog */}
        <section className="card p-5 space-y-3">
          <h2 className="font-bold text-sm">Dialog</h2>
          <Dialog open={dialogOpen} onOpenChange={setDialogOpen}>
            <DialogTrigger asChild>
              <Button variant="primary">เปิด Dialog</Button>
            </DialogTrigger>
            <DialogContent>
              <DialogHeader>
                <DialogTitle>ยืนยันการดำเนินการ</DialogTitle>
                <DialogDescription>คุณแน่ใจว่าต้องการดำเนินการต่อ?</DialogDescription>
              </DialogHeader>
              <div className="flex gap-3 mt-4">
                <Button variant="outline" onClick={() => setDialogOpen(false)} className="flex-1">ยกเลิก</Button>
                <Button onClick={() => setDialogOpen(false)} className="flex-1">ยืนยัน</Button>
              </div>
            </DialogContent>
          </Dialog>
        </section>

        {/* Tooltip */}
        <section className="card p-5 space-y-3">
          <h2 className="font-bold text-sm">Tooltip</h2>
          <div className="flex gap-3">
            <Tooltip><TooltipTrigger asChild><Button variant="subtle" size="icon">ℹ️</Button></TooltipTrigger><TooltipContent>ข้อมูลเพิ่มเติม</TooltipContent></Tooltip>
            <Tooltip><TooltipTrigger asChild><Button variant="subtle" size="icon">⚙️</Button></TooltipTrigger><TooltipContent>การตั้งค่า</TooltipContent></Tooltip>
          </div>
        </section>

      </div>
    </TooltipProvider>
  )
}
```

---

## สรุป Part 59

| Step | เนื้อหา |
|------|---------|
| 581 | Radix UI packages + cva + clsx + tailwind-merge setup |
| 582 | Button component ด้วย cva variants/sizes |
| 583 | Dialog primitive — Overlay, Content, Title, Description |
| 584 | DropdownMenu — Content, Item, Separator |
| 585 | Tooltip — Provider, Root, Trigger, Content |
| 586 | Toast component (Radix Toast) |
| 587 | Avatar — Root, Image, Fallback |
| 588 | Badge ด้วย cva |
| 589 | Switch + Accordion + Select primitives |
| 590 | Workshop: Complete Component Library showcase page |

**Part ถัดไป:** Part 60 — Storybook + Tailwind Component Documentation (Steps 591–600)
