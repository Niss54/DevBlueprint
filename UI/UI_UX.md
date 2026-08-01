# 🎨 UI/UX Design Guide

> **Project:** [Project Name]
> **Design Tool:** Figma
> **Figma Link:** [Paste Figma URL here]
> **Component Library:** shadcn/ui + Tailwind CSS
> **Last Updated:** [YYYY-MM-DD]
> **Designer:** [Name]

---

## 🎯 1. Design Philosophy

> Apne product ke design ke baare mein 2-3 lines mein philosophy likhon.

```
Example:
"[Project Name] ka design clean, minimal, aur purposeful hai.
Hum complexity ko hide karte hain aur simplicity ko celebrate karte hain.
Har element koi purpose serve karta hai — koi decoration nahi."
```

### Design Principles

| Principle | Description |
|-----------|-------------|
| ✨ **Clarity** | Har screen ek kaam kare aur clearly communicate kare |
| ⚡ **Speed** | User ko koi bhi action 3 clicks mein complete karna chahiye |
| 🤝 **Consistency** | Ek jaisi language, patterns, aur behaviors everywhere |
| ♿ **Accessibility** | WCAG 2.1 AA compliance — sab ke liye usable |
| 📱 **Mobile First** | Mobile se design karo, desktop pe expand karo |

---

## 🎨 2. Color System

### Brand Colors
```css
/* Primary Brand */
--color-primary-50:  #f0f9ff;
--color-primary-100: #e0f2fe;
--color-primary-500: #0ea5e9;   /* Main brand color */
--color-primary-600: #0284c7;   /* Hover state */
--color-primary-700: #0369a1;   /* Active state */
--color-primary-900: #0c4a6e;

/* Secondary */
--color-secondary-500: #8b5cf6;  /* Accent/highlight */
--color-secondary-600: #7c3aed;
```

### Semantic Colors
```css
/* Status Colors */
--color-success:  #22c55e;  /* Green — success messages */
--color-warning:  #f59e0b;  /* Amber — warnings */
--color-error:    #ef4444;  /* Red — errors */
--color-info:     #3b82f6;  /* Blue — info messages */

/* Neutral Grays */
--color-gray-50:  #f9fafb;   /* Page background */
--color-gray-100: #f3f4f6;   /* Card background */
--color-gray-200: #e5e7eb;   /* Border */
--color-gray-400: #9ca3af;   /* Placeholder text */
--color-gray-600: #4b5563;   /* Secondary text */
--color-gray-800: #1f2937;   /* Primary text */
--color-gray-900: #111827;   /* Headings */
```

### Dark Mode (If applicable)
```css
[data-theme="dark"] {
  --color-bg:       #0f172a;
  --color-surface:  #1e293b;
  --color-border:   #334155;
  --color-text:     #f1f5f9;
  --color-muted:    #94a3b8;
}
```

### Tailwind Config
```javascript
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      colors: {
        brand: {
          50: '#f0f9ff',
          500: '#0ea5e9',
          600: '#0284c7',
          700: '#0369a1',
        }
      }
    }
  }
}
```

---

## 🔤 3. Typography

### Font Family
```css
/* Heading Font */
font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;

/* Body Font */
font-family: 'Inter', sans-serif;

/* Mono (Code) */
font-family: 'JetBrains Mono', 'Fira Code', monospace;
```

### Type Scale
| Name | Size | Weight | Line Height | Usage |
|------|------|--------|-------------|-------|
| `display` | 48px / 3rem | 700 | 1.1 | Hero headlines |
| `h1` | 36px / 2.25rem | 700 | 1.2 | Page titles |
| `h2` | 30px / 1.875rem | 600 | 1.25 | Section titles |
| `h3` | 24px / 1.5rem | 600 | 1.3 | Card titles |
| `h4` | 20px / 1.25rem | 600 | 1.4 | Subsections |
| `body-lg` | 18px / 1.125rem | 400 | 1.7 | Lead paragraphs |
| `body` | 16px / 1rem | 400 | 1.6 | Body text |
| `body-sm` | 14px / 0.875rem | 400 | 1.5 | Secondary text |
| `caption` | 12px / 0.75rem | 400 | 1.4 | Labels, captions |
| `code` | 14px / 0.875rem | 400 | 1.5 | Code snippets |

---

## 📐 4. Spacing & Layout

### Spacing Scale (Tailwind defaults)
```
4px  = space-1
8px  = space-2
12px = space-3
16px = space-4   ← Base unit
24px = space-6
32px = space-8
48px = space-12
64px = space-16
```

### Layout Grid
```
Mobile (< 768px):  1 column, 16px gutters
Tablet (768–1024): 2 columns, 24px gutters
Desktop (> 1024):  12 column grid, 32px gutters, 1280px max-width
```

### Border Radius
```css
--radius-sm: 4px;    /* Badges, tags */
--radius-md: 8px;    /* Buttons, inputs */
--radius-lg: 12px;   /* Cards */
--radius-xl: 16px;   /* Modals */
--radius-full: 9999px; /* Avatars, pills */
```

---

## 🧱 5. Component Library

### Buttons

```jsx
// Primary Button
<Button variant="default" size="default">
  Save Changes
</Button>

// Secondary / Outline
<Button variant="outline">
  Cancel
</Button>

// Danger / Destructive
<Button variant="destructive">
  Delete Account
</Button>

// Loading State
<Button disabled>
  <Loader2 className="animate-spin mr-2" />
  Saving...
</Button>
```

**Button Sizes:**
| Size | Height | Padding | Font |
|------|--------|---------|------|
| `sm` | 32px | 12px | 14px |
| `default` | 40px | 16px | 16px |
| `lg` | 48px | 24px | 18px |

---

### Form Elements

```jsx
// Input
<div className="space-y-2">
  <Label htmlFor="email">Email</Label>
  <Input
    id="email"
    type="email"
    placeholder="you@example.com"
    className="w-full"
  />
  <p className="text-sm text-red-500">Error message here</p>
</div>

// Select
<Select>
  <SelectTrigger>
    <SelectValue placeholder="Select option" />
  </SelectTrigger>
  <SelectContent>
    <SelectItem value="option1">Option 1</SelectItem>
  </SelectContent>
</Select>

// Textarea
<Textarea placeholder="Type your message..." rows={4} />

// Checkbox
<div className="flex items-center space-x-2">
  <Checkbox id="terms" />
  <label htmlFor="terms">I agree to terms</label>
</div>
```

---

### Cards

```jsx
// Standard Card
<Card>
  <CardHeader>
    <CardTitle>Card Title</CardTitle>
    <CardDescription>Card subtitle or description</CardDescription>
  </CardHeader>
  <CardContent>
    {/* Card body content */}
  </CardContent>
  <CardFooter>
    <Button>Action</Button>
  </CardFooter>
</Card>
```

---

### Feedback & States

```jsx
// Toast Notifications
toast.success("Profile updated successfully!")
toast.error("Something went wrong. Please try again.")
toast.info("New version available.")

// Alert Component
<Alert variant="destructive">
  <AlertTitle>Error</AlertTitle>
  <AlertDescription>Your session has expired. Please login again.</AlertDescription>
</Alert>

// Empty State
<div className="flex flex-col items-center justify-center py-16">
  <EmptyIcon className="h-16 w-16 text-gray-300 mb-4" />
  <h3 className="text-lg font-semibold text-gray-700">No items yet</h3>
  <p className="text-gray-500 text-sm mt-1">Create your first item to get started.</p>
  <Button className="mt-4">Create Item</Button>
</div>

// Loading Skeleton
<Skeleton className="h-4 w-full mb-2" />
<Skeleton className="h-4 w-3/4" />
```

---

## 📱 6. Screen Layouts

### Authenticated App Layout
```
┌─────────────────────────────────────────────┐
│              Top Navigation Bar              │
│  [Logo]   [Nav Links]    [User Avatar ▼]    │
├───────────┬─────────────────────────────────┤
│           │                                 │
│  Sidebar  │         Main Content            │
│           │                                 │
│  - Link 1 │  [Page Title]                   │
│  - Link 2 │                                 │
│  - Link 3 │  [Content Area]                 │
│  - Link 4 │                                 │
│           │                                 │
│  [User]   │                                 │
│  [Logout] │                                 │
│           │                                 │
└───────────┴─────────────────────────────────┘
```

### Auth Pages Layout
```
┌─────────────────────────────────────────────┐
│                                             │
│              [Logo]                         │
│                                             │
│         ┌─────────────────┐                 │
│         │                 │                 │
│         │    Auth Form    │                 │
│         │                 │                 │
│         │  [Input field]  │                 │
│         │  [Input field]  │                 │
│         │                 │                 │
│         │  [Submit Btn]   │                 │
│         │                 │                 │
│         │  "Already have  │                 │
│         │   an account?"  │                 │
│         └─────────────────┘                 │
│                                             │
└─────────────────────────────────────────────┘
```

---

## 🗺️ 7. User Flows

### New User Onboarding Flow
```
Landing Page
     ↓
Sign Up (Email + Password)
     ↓
Email Verification Sent
     ↓
Verify Email (Click link)
     ↓
Welcome Screen / Onboarding Tour
     ↓
Setup Profile (name, avatar)
     ↓
Dashboard (First Visit)
     ↓
Empty State with CTA
     ↓
Create First [Feature]
     ↓
Success! 🎉
```

### Happy Path — Core Feature
```
User on Dashboard
     ↓
Click "Create New" button
     ↓
Fill Form (Validation feedback)
     ↓
Submit → Loading state
     ↓
Success toast + redirect
     ↓
See new item in list
     ↓
Click item → Detail view
     ↓
Edit / Delete options
```

---

## ♿ 8. Accessibility Checklist

- [ ] All interactive elements have focus states
- [ ] Color contrast ratio ≥ 4.5:1 (WCAG AA)
- [ ] All images have descriptive `alt` text
- [ ] Forms have proper `label` associations
- [ ] Error messages are descriptive (not just "required")
- [ ] Screen reader compatible (ARIA labels where needed)
- [ ] Keyboard navigation works on all interactive elements
- [ ] Font sizes not smaller than 14px
- [ ] Clickable areas ≥ 44x44px (mobile)
- [ ] No content relies solely on color to convey meaning

---

## 📱 9. Responsive Breakpoints

| Breakpoint | Width | Target Device |
|-----------|-------|---------------|
| `xs` | < 480px | Small phones |
| `sm` | 480–767px | Large phones |
| `md` | 768–1023px | Tablets |
| `lg` | 1024–1279px | Small laptops |
| `xl` | 1280–1535px | Desktops |
| `2xl` | ≥ 1536px | Large monitors |

### Responsive Rules
- Sidebar → Bottom nav on mobile
- Tables → Card list on mobile
- Multi-column grid → Single column on mobile
- Modal → Full screen on mobile
- Font sizes scale down by 2–4px on mobile

---

## 🔗 Related Documents

| Document | Link |
|----------|------|
| 📋 PRD | [00_PRD.md](./00_PRD.md) |
| 🔌 API | [03_API.md](./03_API.md) |
| ✅ Testing | [05_TESTING.md](./05_TESTING.md) |
| 🎨 Figma | [Insert Figma Link] |
