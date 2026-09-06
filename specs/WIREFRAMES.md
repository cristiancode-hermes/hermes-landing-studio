# Wireframes — Hermes Landing Studio

## Screens Overview

| # | Screen | Purpose |
|---|--------|---------|
| 1 | Login / Register | Authentication with org registration |
| 2 | Dashboard | Page list with KPIs, search, filter |
| 3 | Template Gallery | Template browser with 1-click instantiation |
| 4 | Page Editor | Drag-and-drop section editor with live preview |
| 5 | Section Editor Panel | Inline content editor for each section type |
| 6 | Public Page | Rendered landing page (no UI chrome) |

---

## 1. Login / Register

### Wireframe (ASCII)

```
┌──────────────────────────────────────────────────┐
│                                                  │
│              ┌──────────────────────┐            │
│              │   🏗️ Landing Studio  │            │
│              └──────────────────────┘            │
│                                                  │
│             ┌────────────────────┐               │
│             │  Iniciar Sesión    │               │
│             │  ────────────────  │               │
│             │                    │               │
│             │  Email             │               │
│             │  ┌──────────────┐  │               │
│             │  │ user@mail.com │  │               │
│             │  └──────────────┘  │               │
│             │                    │               │
│             │  Contraseña    👁️  │               │
│             │  ┌──────────────┐  │               │
│             │  │ •••••••••    │  │               │
│             │  └──────────────┘  │               │
│             │                    │               │
│             │  ┌────────────────┐│               │
│             │  │  Iniciar Sesión ││               │
│             │  └────────────────┘│               │
│             │                    │               │
│             │  ¿No tienes cuenta?                │
│             │  [Crear cuenta + organización]      │
│             └────────────────────┘               │
│                                                  │
└──────────────────────────────────────────────────┘
```

### States

| State | Visual | Description |
|-------|--------|-------------|
| Normal | Form with email, password fields | Default login form |
| Loading | "Iniciar Sesión" becomes spinner button | While authenticating |
| Error | Red banner: "Credenciales inválidas" | Wrong credentials |
| Validation | Field borders red + error messages | Empty/invalid fields |

### Navigation
- Login success → Dashboard
- "Crear cuenta" → Toggle to Register form
- Register success → Dashboard (org auto-created)

---

## 2. Dashboard

### Wireframe (ASCII)

```
┌────────────────────────────────────────────────────────┐
│ 🏗️ Landing Studio          🔍 [Buscar páginas...]  👤 │
├────────────────────────────────────────────────────────┤
│                                                        │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐             │
│  │ 📄 Total  │  │ ✅ Pub.  │  │ 👁️ Visitas │             │
│  │   12      │  │   8      │  │  1,234   │             │
│  └──────────┘  └──────────┘  └──────────┘             │
│                                                        │
│  [Todas] [Borrador: 4] [Publicadas: 8]  [+ Nueva Pág] │
│                                                        │
│  ┌─────────────────────────────────────────────────┐   │
│  │ 📄 Summer Campaign 2026  ✅ Publicada   👁️ 458 │   │
│  │   /summer-2026                    Últ: 15/07    │   │
│  │   [Editar] [Prever] [Despublicar] [🗑️]          │   │
│  ├─────────────────────────────────────────────────┤   │
│  │ 📄 Product Launch Beta     ⏳ Borrador   👁️ —   │   │
│  │   /product-beta                        —        │   │
│  │   [Editar] [Prever] [Publicar] [🗑️]             │   │
│  ├─────────────────────────────────────────────────┤   │
│  │ 📄 Webinar Registration   ✅ Publicada   👁️ 89 │   │
│  │   /webinar-reg                    Últ: 14/07    │   │
│  │   [Editar] [Prever] [Despublicar] [🗑️]          │   │
│  └─────────────────────────────────────────────────┘   │
│                                                        │
└────────────────────────────────────────────────────────┘
```

### States

| State | Visual | Description |
|-------|--------|-------------|
| Normal | KPI cards + page list | Default view |
| Loading | 3 skeleton KPI cards + 3 skeleton rows | While loading data |
| Empty | Illustration + "Crea tu primera página" + CTA button | No pages yet |
| Error | Red banner "Error al cargar páginas" + Retry button | API failure |
| Search | Filtered list matching query | Results update as typing |
| Filter | Active filter tab highlighted | Click "Borrador" shows only drafts |

### Interactions
- Click [+ Nueva Pág] → Template Gallery
- Click [Editar] → Page Editor for that page
- Click [Prever] → Public Page preview in new tab
- Click [Publicar] → Confirm dialog → Status changes to "Publicada"
- Click [🗑️] → Confirm dialog → Page deleted

---

## 3. Template Gallery

### Wireframe (ASCII)

```
┌────────────────────────────────────────────────────────┐
│ 🏗️ Landing Studio                          ← Volver    │
├────────────────────────────────────────────────────────┤
│                                                        │
│  Elige una plantilla para empezar                      │
│                                                        │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │  ┌────────┐   │  │  ┌────────┐   │  │  ┌────────┐   │
│  │  │ HERO   │   │  │  │ HERO   │   │  │  │ HERO   │   │
│  │  │────────│   │  │  │────────│   │  │  │────────│   │
│  │  │FEATURES│   │  │  │FEATURES│   │  │  │FEATURES│   │
│  │  │────────│   │  │  │────────│   │  │  │────────│   │
│  │  │  CTA   │   │  │  │PRICING │   │  │  │TESTIM  │   │
│  │  │────────│   │  │  │────────│   │  │  │────────│   │
│  │  │ FOOTER │   │  │  │  CTA   │   │  │  │  CTA   │   │
│  │  └────────┘   │  │  └────────┘   │  │  └────────┘   │
│  │               │  │               │  │               │
│  │ Landing Page   │  │ Product Page  │  │ Coming Soon   │
│  │ Clásica       │  │ SaaS          │  │ Teaser        │
│  └──────────────┘  └──────────────┘  └──────────────┘  │
│                                                        │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │  ┌────────┐   │  │  ┌────────┐   │  │  ┌────────┐   │
│  │  │ HERO   │   │  │  │ HERO   │   │  │  │ HERO   │   │
│  │  │────────│   │  │  │────────│   │  │  │────────│   │
│  │  │FEATURES│   │  │  │FEATURES│   │  │  │FORM    │   │
│  │  │────────│   │  │  │────────│   │  │  │────────│   │
│  │  │ FAQ    │   │  │  │ GALLERY│   │  │  │FOOTER  │   │
│  │  │────────│   │  │  │────────│   │  │  └────────┘   │
│  │  │ FOOTER │   │  │  │  CTA   │   │  │               │
│  │  └────────┘   │  │  └────────┘   │  │  Webinar      │
│  │               │  │               │  │  Registro     │
│  │ Evento        │  │  Portfolio    │  │               │
│  └──────────────┘  └──────────────┘  └──────────────┘  │
│                                                        │
└────────────────────────────────────────────────────────┘
```

### States

| State | Visual | Description |
|-------|--------|-------------|
| Normal | 3×2 template grid with thumbnails | Default view |
| Loading | 6 skeleton cards with animated pulse | While loading templates |
| Empty | "No hay plantillas disponibles" | No templates seeded |
| Error | Red banner + Retry | API failure |

### Interactions
- Click template → Creates new page from template → Redirect to Page Editor
- Hover → Card elevates, slight scale up
- Template cards show section count badge

---

## 4. Page Editor (Main Screen)

### Wireframe (ASCII)

```
┌────────────────────────────────────────────────────────────────────┐
│ 🏗️ Landing Studio                              💾 Guardado 3s ago  │
│ 📄 Summer Campaign 2026  [💻] [📱]    [Volver] [Publicar]          │
├───────────────────────┬────────────────────────────────────────────┤
│  ➕ Añadir Sección    │  ┌──────────────────────────────────────┐   │
│                       │  │  ✨ Launch Your Dream                │   │
│  ┌─────────────────┐  │  │  Subtitle text here                 │   │
│  │ ⬡ Hero Section  │  │  │                                     │   │
│  │   (click to edit)│  │  │  ┌──────────┐  ┌──────────┐       │   │
│  ├═════════════════┤  │  │  │ CTA Btn  │  │ Learn More│       │   │
│  │ ⬡ Features     │  │  │  └──────────┘  └──────────┘       │   │
│  │ ☰ drag ⚙️ 🗑️   │  │  └──────────────────────────────────────┘   │
│  ├═════════════════┤  │                                             │
│  │ ⬡ Pricing      │  │  ┌──────────────────────────────────────┐   │
│  │ ☰ drag ⚙️ 🗑️   │  │  │ 💪 Feature 1    🚀 Feature 2      │   │
│  ├═════════════════┤  │  │  Description 1    Description 2     │   │
│  │ ⬡ Testimonials │  │  │                                      │   │
│  │ ☰ drag ⚙️ 🗑️   │  │  │ ⚡ Feature 3    🔒 Feature 4       │   │
│  ├═════════════════┤  │  │  Description 3    Description 4     │   │
│  │ ⬡ CTA Section  │  │  └──────────────────────────────────────┘   │
│  │ ☰ drag ⚙️ 🗑️   │  │                                             │
│  ├═════════════════┤  │  ┌──────────────────────────────────────┐   │
│  │ ⬡ Footer       │  │  │  💰 Basic $29   Pro $99   Enterprise │   │
│  │ ☰ drag ⚙️ 🗑️   │  │  │  ┌──────────┐  ┌──────────┐        │   │
│  └─────────────────┘  │  │  │Get Started│  │ Get Started│        │   │
│                       │  │  └──────────┘  └──────────┘        │   │
│  SEO                  │  └──────────────────────────────────────┘   │
│  ┌─────────────────┐  │                                             │
│  │ Title: Summer    │  │  ┌──────────────────────────────────────┐   │
│  │ Desc: Landing... │  │  │  ⭐ "Great product!" — John D.      │   │
│  │ OG: /image.jpg   │  │  │  ⭐ "Amazing service" — Jane S.    │   │
│  └─────────────────┘  │  └──────────────────────────────────────┘   │
│                       │                                             │
│                       │  ┌──────────────────────────────────────┐   │
│                       │  │  🎯 Get Started Today!               │   │
│                       │  │  ┌──────────────────────┐            │   │
│                       │  │  │   Call to Action     │            │   │
│                       │  │  └──────────────────────┘            │   │
│                       │  └──────────────────────────────────────┘   │
│                       │                                             │
│                       │  ┌──────────────────────────────────────┐   │
│                       │  │  © 2026 Acme Inc.  [Twitter] [LI]   │   │
│                       │  └──────────────────────────────────────┘   │
└───────────────────────┴────────────────────────────────────────────┘
```

### States

| State | Visual | Description |
|-------|--------|-------------|
| Normal | Side panel + preview with sections | Default editing view |
| Loading | Skeleton sidebar + skeleton preview | While loading page data |
| Empty | Preview shows "Añade tu primera sección" | No sections yet |
| Error | Red banner "Error al cargar página" + Retry | API failure |
| Saving | 💾 icon spins → "Guardado" | Auto-save in progress |
| Edit mode | Section highlighted with edit panel open | Clicked a section to edit |

### Interactions
- Drag section handle (☰) → Reorder sections
- Click section → Opens section editor panel (right side or overlay)
- Click ⚙️ → Opens section editor panel
- Click 🗑️ → Confirms deletion → Section removed
- Responsive toggles [💻] [📱] → Preview scales
- [Publicar] → Confirm dialog → Page goes live
- Auto-save: changes persist to backend every 30s or on explicit save

---

## 5. Section Editor Panel (Hero Example)

### Wireframe (ASCII)

```
┌─────────────────────────────────────────────────────────┐
│ ✏️ Editar: Hero Section                            ✕    │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  Tipo: Hero Section                                     │
│                                                         │
│  ┌─────────────────────────────────────────────────┐    │
│  │ Título                                           │    │
│  │ Launch Your Dream Product Today                  │    │
│  └─────────────────────────────────────────────────┘    │
│                                                         │
│  ┌─────────────────────────────────────────────────┐    │
│  │ Subtítulo                                        │    │
│  │ The easiest way to build your online presence    │    │
│  └─────────────────────────────────────────────────┘    │
│                                                         │
│  ┌─────────────────────────────────────────────────┐    │
│  │ Texto del botón (CTA)                            │    │
│  │ Get Started Free                                 │    │
│  └─────────────────────────────────────────────────┘    │
│                                                         │
│  ┌─────────────────────────────────────────────────┐    │
│  │ URL del botón                                    │    │
│  │ https://app.example.com/register                 │    │
│  └─────────────────────────────────────────────────┘    │
│                                                         │
│  ┌─────────────────────────────────────────────────┐    │
│  │ Fondo — Color / Imagen                          │    │
│  │ [#1a1a2e] ○  [Seleccionar imagen...]            │    │
│  └─────────────────────────────────────────────────┘    │
│                                                         │
│  ┌─────────────────────────────────────────────────┐    │
│  │ Alineación                                       │    │
│  │ ○ Izquierda  ● Centro  ○ Derecha                 │    │
│  └─────────────────────────────────────────────────┘    │
│                                                         │
│  ┌─────────────────────────────────────────────────┐    │
│  │ Colores del texto                                │    │
│  │ Título: [#FFFFFF]   Subtítulo: [#CCCCCC]         │    │
│  │ Botón: [#FFFFFF]   Fondo btn: [#4F46E5]          │    │
│  └─────────────────────────────────────────────────┘    │
│                                                         │
│  ┌─────────────────────────────────────────────────┐    │
│  │              [Cancelar]  [Guardar Cambios]       │    │
│  └─────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────┘
```

### States

| State | Visual | Description |
|-------|--------|-------------|
| Normal | Form with section-specific fields | Default editing |
| Validation | Red borders on required empty fields | Missing required values |
| Saving | "Guardando..." with spinner | While persisting |
| Success | Toast "Sección actualizada" | Save completed |

---

## 6. Public Page (Rendered)

### Wireframe (ASCII)

```
┌──────────────────────────────────────────────────────────────┐
│                                                               │
│   ┌────────────────────────────────────────────────────────┐  │
│   │                                                        │  │
│   │           Launch Your Dream Product Today              │  │
│   │                                                        │  │
│   │     The easiest way to build your online presence      │  │
│   │                                                        │  │
│   │     ┌──────────────────┐  ┌──────────────────┐        │  │
│   │     │ Get Started Free │  │  Learn More      │        │  │
│   │     └──────────────────┘  └──────────────────┘        │  │
│   └────────────────────────────────────────────────────────┘  │
│                                                               │
│   ┌────────────────────────────────────────────────────────┐  │
│   │  💪 Feature 1          🚀 Feature 2          ⚡ Feat.3 │  │
│   │  Description here      Description here     Desc here  │  │
│   └────────────────────────────────────────────────────────┘  │
│                                                               │
│   ┌────────────────────────────────────────────────────────┐  │
│   │  💰 Basic $29       💎 Pro $99       🏢 Enterprise    │  │
│   │  ┌──────────┐      ┌──────────┐     ┌───────────┐    │  │
│   │  │ Get It   │      │ Popular  │     │  Contact  │    │  │
│   │  └──────────┘      └──────────┘     └───────────┘    │  │
│   └────────────────────────────────────────────────────────┘  │
│                                                               │
│   ┌────────────────────────────────────────────────────────┐  │
│   │  ⭐ "Amazing!" — Jane S.   ⭐ "Best tool!" — John D.  │  │
│   └────────────────────────────────────────────────────────┘  │
│                                                               │
│   ┌────────────────────────────────────────────────────────┐  │
│   │              🎯 Get Started Today!                     │  │
│   │         ┌────────────────────────────┐                 │  │
│   │         │   Start Free Trial        │                 │  │
│   │         └────────────────────────────┘                 │  │
│   └────────────────────────────────────────────────────────┘  │
│                                                               │
│   ┌────────────────────────────────────────────────────────┐  │
│   │  © 2026 Acme Inc.   [Twitter] [LinkedIn] [GitHub]     │  │
│   └────────────────────────────────────────────────────────┘  │
│                                                               │
└──────────────────────────────────────────────────────────────┘
```

### States

| State | Visual | Description |
|-------|--------|-------------|
| Normal | Fully rendered landing page | Published page |
| Loading | Skeleton layout | While page loads |
| 404 | "Página no encontrada" | Invalid slug |
| Preview | Same as Normal but with "Vista previa" banner at top | Draft page preview |
| Mobile | Content stacks vertically, hamburger menu | Responsive breakpoint |

### Navigation
- Public page at `/{slug}` — no auth required
- All links and CTAs are clickable (tracked via analytics)
- SEO meta tags rendered in `<head>`
