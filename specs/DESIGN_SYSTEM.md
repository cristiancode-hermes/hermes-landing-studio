# Design System — Hermes Landing Studio

Inspired by the Vercel design system: black/white precision, clean typography, minimal color accents. The palette should feel professional and marketing-focused — the pages our users build should look great by default.

---

## Color Palette

### Brand Colors

```
Primary (Indigo):    #4F46E5 → #6366F1 → #4338CA
  Usage: Buttons, links, active states, brand accent
Secondary (Slate):   #64748B → #475569 → #334155
  Usage: Muted text, borders, secondary buttons
Accent (Emerald):    #10B981 → #059669
  Usage: Success states, "Published" badges
```

### Tailwind v4 @theme Block

```css
@theme {
  /* Primary — Indigo */
  --color-primary-50: #eef2ff;
  --color-primary-100: #e0e7ff;
  --color-primary-200: #c7d2fe;
  --color-primary-300: #a5b4fc;
  --color-primary-400: #818cf8;
  --color-primary-500: #6366f1;
  --color-primary-600: #4f46e5;
  --color-primary-700: #4338ca;
  --color-primary-800: #3730a3;
  --color-primary-900: #312e81;
  --color-primary-950: #1e1b4b;

  /* Secondary — Slate */
  --color-secondary-50: #f8fafc;
  --color-secondary-100: #f1f5f9;
  --color-secondary-200: #e2e8f0;
  --color-secondary-300: #cbd5e1;
  --color-secondary-400: #94a3b8;
  --color-secondary-500: #64748b;
  --color-secondary-600: #475569;
  --color-secondary-700: #334155;
  --color-secondary-800: #1e293b;
  --color-secondary-900: #0f172a;
  --color-secondary-950: #020617;

  /* Accent — Emerald */
  --color-accent-50: #ecfdf5;
  --color-accent-100: #d1fae5;
  --color-accent-200: #a7f3d0;
  --color-accent-300: #6ee7b7;
  --color-accent-400: #34d399;
  --color-accent-500: #10b981;
  --color-accent-600: #059669;

  /* Semantic */
  --color-success: #22c55e;
  --color-warning: #f59e0b;
  --color-danger: #ef4444;
  --color-info: #3b82f6;
}
```

### Color Usage

| Token | Usage | Light Value | Dark Value |
|-------|-------|-------------|------------|
| `bg-primary` | Page background | `#ffffff` | `#0f172a` |
| `bg-secondary` | Card/surface background | `#f8fafc` | `#1e293b` |
| `bg-tertiary` | Hover/active surfaces | `#f1f5f9` | `#334155` |
| `text-primary` | Primary text | `#0f172a` | `#f1f5f9` |
| `text-secondary` | Muted text | `#64748b` | `#94a3b8` |
| `border-default` | Borders | `#e2e8f0` | `#334155` |

---

## Typography

**Primary font:** Inter (Google Fonts)
**Monospace:** JetBrains Mono (for code sections, if applicable)

```html
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
```

### Type Scale

| Level | Size | Weight | Line Height | Letter Spacing | Usage |
|-------|------|--------|-------------|----------------|-------|
| h1 | 3.75rem (60px) | 700 | 1.05 | -0.025em | Hero headlines |
| h2 | 3rem (48px) | 700 | 1.1 | -0.02em | Section titles |
| h3 | 1.875rem (30px) | 600 | 1.2 | -0.015em | Card titles |
| h4 | 1.5rem (24px) | 600 | 1.3 | -0.01em | Subsection titles |
| body-lg | 1.125rem (18px) | 400 | 1.6 | 0 | Lead paragraphs |
| body | 1rem (16px) | 400 | 1.6 | 0 | Body text |
| body-sm | 0.875rem (14px) | 400 | 1.5 | 0 | Small text, metadata |
| caption | 0.75rem (12px) | 500 | 1.4 | 0.025em | Labels, badges |
| overline | 0.75rem (12px) | 600 | 1.0 | 0.05em | Stat labels, section headers |

---

## Spacing

| Token | Size | Usage |
|-------|------|-------|
| p-1 | 4px | Icons, inner padding |
| p-2 | 8px | Avatar, tag gaps |
| p-3 | 12px | Button padding, small cards |
| p-4 | 16px | Card padding, form fields |
| p-6 | 24px | Section gaps, card groups |
| p-8 | 32px | Page sections, modals |
| p-12 | 48px | Section vertical spacing |
| p-16 | 64px | Page margins, hero padding |
| p-24 | 96px | Major section separators |

---

## Shadows

```css
--shadow-card: 0 1px 3px 0 rgb(0 0 0 / 0.1), 0 1px 2px -1px rgb(0 0 0 / 0.1);
--shadow-card-hover: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);
--shadow-modal: 0 20px 25px -5px rgb(0 0 0 / 0.1), 0 8px 10px -6px rgb(0 0 0 / 0.1);
--shadow-sidebar: -4px 0 6px -1px rgb(0 0 0 / 0.05);
--shadow-dropdown: 0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1);
--shadow-section-hover: 0 0 0 2px #6366f1;  /* Focus ring for sections */
```

---

## Border Radius

```css
--radius-sm: 4px;     /* Badges, small indicators */
--radius-md: 8px;     /* Cards, inputs, buttons */
--radius-lg: 12px;    /* Modals, panels, dropdowns */
--radius-xl: 16px;    /* Large modals, hero containers */
--radius-full: 9999px; /* Avatars, pills */
```

---

## Dark Mode

```css
.dark {
  --color-secondary-50: #0f172a;
  --color-secondary-100: #1e293b;
  --color-secondary-200: #334155;
  --color-secondary-300: #475569;
  --color-secondary-400: #64748b;
  --color-secondary-500: #94a3b8;
  --color-secondary-600: #cbd5e1;
  --color-secondary-700: #e2e8f0;
  --color-secondary-800: #f1f5f9;
  --color-secondary-900: #f8fafc;
  --color-secondary-950: #ffffff;
}
```

### Dark Mode Mapping

| Element | Light | Dark |
|---------|-------|------|
| Page background | `#ffffff` | `#0f172a` |
| Card background | `#f8fafc` | `#1e293b` |
| Primary text | `#0f172a` | `#f1f5f9` |
| Muted text | `#64748b` | `#94a3b8` |
| Borders | `#e2e8f0` | `#334155` |
| Input background | `#ffffff` | `#1e293b` |
| Section preview bg | `#ffffff` | `#0f172a` |

### Dark Mode Transition

```css
:root {
  --transition-theme: color 0.15s ease, background-color 0.15s ease, border-color 0.15s ease;
}
* { transition: var(--transition-theme); }
```

---

## Transitions & Animations

### Animation Tokens

```css
:root {
  --animate-fade-in: fadeIn 0.2s ease-out;
  --animate-slide-up: slideUp 0.3s ease-out;
  --animate-slide-right: slideRight 0.25s ease-out;
  --animate-scale-in: scaleIn 0.2s ease-out;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes slideUp {
  from { opacity: 0; transform: translateY(12px); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes slideRight {
  from { opacity: 0; transform: translateX(-8px); }
  to { opacity: 1; transform: translateX(0); }
}

@keyframes scaleIn {
  from { opacity: 0; transform: scale(0.95); }
  to { opacity: 1; transform: scale(1); }
}
```

### Animation Map

| Element | Animation |
|---------|-----------|
| Card hover | `transform: translateY(-2px); transition: transform 0.2s ease;` |
| Modal open | `--animate-scale-in` + overlay `fadeIn` |
| Toast enter | `--animate-slide-up` |
| Skeleton pulse | `@keyframes pulse { 0%,100% { opacity: 1 } 50% { opacity: 0.4 } }` |
| Page sections | `--animate-fade-in` with staggered delay (index × 50ms) |
| Drag placeholder | `opacity: 0.5; border: 2px dashed #6366f1;` |
| Section highlight | `box-shadow: 0 0 0 2px #6366f1; transition: box-shadow 0.2s;` |
| Dark mode toggle | `--transition-theme` on `*` selector |

### Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

---

## Component Patterns

### Buttons

| Variant | Classes | Usage |
|---------|---------|-------|
| Primary | `bg-primary-600 text-white hover:bg-primary-700 px-4 py-2 rounded-md font-medium` | Main actions |
| Secondary | `bg-secondary-100 text-secondary-700 hover:bg-secondary-200 px-4 py-2 rounded-md font-medium` | Secondary actions |
| Ghost | `text-secondary-600 hover:bg-secondary-100 px-3 py-2 rounded-md` | Tertiary actions |
| Danger | `bg-danger text-white hover:bg-red-600 px-4 py-2 rounded-md` | Destructive actions |

### Inputs

```css
.input-base {
  @apply w-full px-3 py-2 rounded-md border border-secondary-200
         bg-white text-secondary-900 placeholder:text-secondary-400
         focus:outline-none focus:ring-2 focus:ring-primary-500 focus:border-primary-500
         dark:bg-secondary-100 dark:border-secondary-200 dark:text-secondary-50;
}
```

### Cards

```css
.card {
  @apply bg-white dark:bg-secondary-100 rounded-lg border border-secondary-200
         shadow-sm transition-shadow duration-200;
}
.card-hover {
  @apply hover:shadow-md hover:-translate-y-0.5 transition-all duration-200;
}
```

### Badges

| Variant | Classes | Usage |
|---------|---------|-------|
| Published | `bg-accent-100 text-accent-700 dark:bg-accent-900 dark:text-accent-300` | "Publicada" |
| Draft | `bg-secondary-200 text-secondary-600 dark:bg-secondary-300 dark:text-secondary-400` | "Borrador" |
| Info | `bg-blue-100 text-blue-700` | Counter badges |
