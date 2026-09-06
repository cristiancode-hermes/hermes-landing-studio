# Routes & API — Hermes Landing Studio

## NestJS Module Structure

```
src/
├── main.ts
├── app.module.ts
├── app.controller.ts
├── config/
│   └── config.module.ts
├── auth/
│   ├── auth.module.ts
│   ├── auth.controller.ts
│   ├── auth.service.ts
│   ├── jwt-auth.guard.ts
│   ├── current-user.decorator.ts
│   ├── jwt.strategy.ts
│   └── dto/
│       ├── login.dto.ts
│       └── register.dto.ts
├── users/
│   ├── users.module.ts
│   ├── users.controller.ts
│   ├── users.service.ts
│   └── entities/
│       └── user.entity.ts
├── organizations/
│   ├── organizations.module.ts
│   ├── organizations.service.ts
│   └── entities/
│       └── organization.entity.ts
├── pages/
│   ├── pages.module.ts
│   ├── pages.controller.ts
│   ├── pages.service.ts
│   ├── dto/
│   │   ├── create-page.dto.ts
│   │   ├── update-page.dto.ts
│   │   └── page-response.dto.ts
│   └── entities/
│       └── page.entity.ts
├── sections/
│   ├── sections.module.ts
│   ├── sections.controller.ts
│   ├── sections.service.ts
│   ├── dto/
│   │   ├── create-section.dto.ts
│   │   ├── update-section.dto.ts
│   │   └── reorder-sections.dto.ts
│   └── entities/
│       └── page-section.entity.ts
├── templates/
│   ├── templates.module.ts
│   ├── templates.controller.ts
│   ├── templates.service.ts
│   ├── dto/
│   │   └── template-response.dto.ts
│   └── entities/
│       └── template.entity.ts
├── analytics/
│   ├── analytics.module.ts
│   ├── analytics.controller.ts
│   ├── analytics.service.ts
│   ├── dto/
│   │   └── track-event.dto.ts
│   └── entities/
│       └── analytics-event.entity.ts
├── seed/
│   ├── seed.module.ts
│   └── seed.service.ts
└── shared/
    └── entities/
        └── base.entity.ts
```

---

## Frontend Routes (Angular)

| Path | Component | Auth | Lazy Chunk |
|------|-----------|------|------------|
| `/login` | `LoginPageComponent` | No | login |
| `/dashboard` | `DashboardPageComponent` | Yes | dashboard |
| `/templates` | `TemplateGalleryPageComponent` | Yes | templates |
| `/editor/:id` | `PageEditorComponent` | Yes | editor |
| `/editor/:id/sections/:sectionId` | `SectionEditorPanelComponent` (embedded) | Yes | — |
| `/:slug` | `PublicPageComponent` | No | public |
| `**` | `NotFoundComponent` | — | not-found |

### Route Notes
- Auth guard redirects to `/login` if no valid token
- Public page `/:slug` loads page by slug, no auth required
- All data routes use Angular 22 lazy loading with `loadComponent`
- Public page must render SEO meta tags in `<head>` via `Title` + `Meta` services

---

## API Endpoints (NestJS)

### Auth Module

| Method | Path | Auth | Body / Query | Response | Description |
|--------|------|------|-------------|----------|-------------|
| POST | `/api/auth/register` | No | `{ name, email, password, orgName }` | `{ token, user }` | Register + create org |
| POST | `/api/auth/login` | No | `{ email, password }` | `{ token, user }` | Login |
| GET | `/api/auth/me` | Yes | — | `{ user }` | Current user profile |

**POST /api/auth/register**
```json
// Request
{
  "name": "María García",
  "email": "maria@acme.com",
  "password": "SecurePass1",
  "orgName": "Acme Inc"
}
// 201 Response
{
  "token": "eyJhbGciOiJIUzI1NiIs...",
  "user": {
    "id": "uuid",
    "name": "María García",
    "email": "maria@acme.com",
    "organizationId": "uuid",
    "role": "admin"
  }
}
// 409 — Email exists
{ "message": "Email already registered", "statusCode": 409 }
// 400 — Validation
{ "message": ["name must be a string"], "error": "Bad Request", "statusCode": 400 }
```

### Pages Module

| Method | Path | Auth | Body / Query | Response | Description |
|--------|------|------|-------------|----------|-------------|
| GET | `/api/pages` | Yes | `?status=draft&search=summer` | `Page[]` | List org's pages (filtered) |
| POST | `/api/pages` | Yes | `{ templateId, title }` | `Page` | Create page from template |
| GET | `/api/pages/:id` | Yes | — | `Page + PageSection[]` | Get page with sections |
| PATCH | `/api/pages/:id` | Yes | `{ title?, slug?, seo? }` | `Page` | Update page metadata |
| PATCH | `/api/pages/:id/status` | Yes | `{ status: "published" \| "draft" }` | `Page` | Publish/unpublish page |
| DELETE | `/api/pages/:id` | Yes | — | `204` | Delete page + sections |

**POST /api/pages**
```json
// Request
{
  "templateId": "uuid-of-landing-template",
  "title": "Summer Campaign 2026"
}
// 201 Response
{
  "id": "uuid",
  "title": "Summer Campaign 2026",
  "slug": "summer-campaign-2026",
  "status": "draft",
  "seo": { "title": null, "description": null, "ogImage": null },
  "publishedAt": null,
  "createdAt": "2026-07-16T02:30:00Z",
  "sections": [
    { "id": "uuid", "type": "hero", "content": { "headline": "...", "subtitle": "..." }, "sortOrder": 0 },
    { "id": "uuid", "type": "features", "content": { "features": [...] }, "sortOrder": 1 }
  ]
}
```

### Sections Module

| Method | Path | Auth | Body | Response | Description |
|--------|------|------|------|----------|-------------|
| POST | `/api/pages/:pageId/sections` | Yes | `{ type: "hero" }` | `PageSection` | Add section to page |
| PATCH | `/api/pages/:pageId/sections/:sectionId` | Yes | `{ content: {...} }` | `PageSection` | Update section content |
| PATCH | `/api/pages/:pageId/sections/reorder` | Yes | `{ sectionIds: ["id1","id2","id3"] }` | `PageSection[]` | Reorder sections |
| DELETE | `/api/pages/:pageId/sections/:sectionId` | Yes | — | `204` | Delete section |

**PATCH /api/pages/:pageId/sections/:sectionId**
```json
// Request — Hero section
{
  "content": {
    "headline": "Launch Your Dream Product Today",
    "subtitle": "The easiest way to build your online presence",
    "ctaText": "Get Started Free",
    "ctaUrl": "https://app.example.com/register",
    "bgColor": "#1a1a2e",
    "bgImage": null,
    "alignment": "center",
    "textColor": "#ffffff",
    "ctaBgColor": "#4F46E5",
    "ctaTextColor": "#ffffff"
  }
}
// 200 Response
{
  "id": "uuid",
  "pageId": "uuid",
  "type": "hero",
  "content": { /* updated */ },
  "sortOrder": 0
}
```

### Templates Module

| Method | Path | Auth | Response | Description |
|--------|------|------|----------|-------------|
| GET | `/api/templates` | Yes | `Template[]` | List available templates |
| GET | `/api/templates/:id` | Yes | `Template + sections` | Get template detail |

### Analytics Module

| Method | Path | Auth | Body / Query | Response | Description |
|--------|------|------|-------------|----------|-------------|
| POST | `/api/analytics` | No | `{ pageId, eventType, metadata }` | `201` | Track page event (anonymous) |
| GET | `/api/analytics/page/:pageId/summary` | Yes | — | `{ totalViews, totalClicks, uniqueDays }` | Page analytics summary |
| GET | `/api/analytics/page/:pageId/daily` | Yes | `?days=7` | `{ date, views, clicks }[]` | Daily event breakdown |

**POST /api/analytics**
```json
// Request
{
  "pageId": "uuid",
  "eventType": "view",
  "metadata": { "referrer": "https://twitter.com/...", "userAgent": "Mozilla/5.0..." }
}
// 201 Response — empty body
```

**GET /api/analytics/page/:pageId/daily?days=7**
```json
// Response
[
  { "date": "2026-07-10", "views": 125, "clicks": 12 },
  { "date": "2026-07-11", "views": 89, "clicks": 8 },
  { "date": "2026-07-12", "views": 234, "clicks": 21 },
  { "date": "2026-07-13", "views": 312, "clicks": 34 },
  { "date": "2026-07-14", "views": 178, "clicks": 19 },
  { "date": "2026-07-15", "views": 256, "clicks": 28 },
  { "date": "2026-07-16", "views": 89, "clicks": 7 }
]
```

### Public Page Module

| Method | Path | Auth | Response | Description |
|--------|------|------|----------|-------------|
| GET | `/api/public/:slug` | No | `Page + PageSection[]` | Get published page by slug |

**GET /api/public/summer-campaign-2026**
```json
// 200 — Page found
{
  "id": "uuid",
  "title": "Summer Campaign 2026",
  "slug": "summer-campaign-2026",
  "seo": {
    "title": "Summer Campaign 2026 — Acme Inc",
    "description": "Descubre nuestra oferta especial de verano",
    "ogImage": "https://images.example.com/og-summer.jpg"
  },
  "sections": [
    { "type": "hero", "content": { "headline": "Summer Sale", ... }, "sortOrder": 0 },
    { "type": "features", "content": { "features": [...] }, "sortOrder": 1 }
  ]
}
// 404 — Page not found or not published
{ "message": "Page not found", "statusCode": 404 }
```

---

## API Security

| Guard | Applied to | Behavior |
|-------|-----------|----------|
| `JwtAuthGuard` | All `/api/pages/*`, `/api/templates/*`, `/api/auth/me` | Validates JWT, extracts org ID |
| None | `/api/public/*`, `/api/analytics` (POST only) | Public access |
| None | `/api/auth/login`, `/api/auth/register` | Public access |
