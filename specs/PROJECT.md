# Hermes Landing Studio — Drag-and-Drop Landing Page Builder

## Pitch
A multi-tenant landing page builder where marketing teams can create, customize, and publish landing pages using drag-and-drop sections, pre-built templates, and built-in analytics — no coding required.

## Stack
- **Frontend:** Angular 22 + Signals + Zoneless + Tailwind CSS v4
- **Backend:** NestJS 11 + TypeORM 1.0 + JWT Auth (Passport.js)
- **Database:** SQLite (dev, better-sqlite3) / Neon PostgreSQL (prod-ready schema)
- **CI/CD:** GitHub Actions workflow YAML

## Público objetivo
Marketing teams, small business owners, and content creators who need to publish landing pages quickly without developer involvement. Multi-tenant: each organization gets its own isolated pages and templates.

## Cycle Context
| Factor | Decisión |
|--------|----------|
| **Cycle** | Counter=2 — Non-AI build |
| **Weak skill #1** | Neon/Postgres (2→2.5) — JSONB for flexible PageSection content & AnalyticsEvent metadata, PostgreSQL-compatible schema design |
| **Weak skill #2** | Testing (3→3.5) — 15+ unit tests for page validation, SEO meta generation, section ordering, analytics aggregation |
| **Domain** | Productivity / Marketing (≠ last project: data collection/surveys) |
| **Difference** | First drag-and-drop UI; first JSONB-heavy schema with flexible section content; first SEO metadata engine; first template-as-blueprint system |
| **Últimos 10** | Survey Engine (data collection), Job Tracker (career/HR), Team Wiki (content/knowledge), Billing Portal (billing), Time Tracker (time tracking) — all different domains |

## Features Priorizadas

### Must Have (MVP)
- [ ] **Auth** — Register / Login with JWT, multi-tenant organization scoping
- [ ] **Page CRUD** — Create, edit, publish, unpublish landing pages with slug-based routing
- [ ] **Page Sections** — Drag-and-drop section management: hero, features, pricing, CTA, footer, testimonials, FAQ
- [ ] **Section Editor** — Inline editing of section content (headline, subtext, button text, image URLs, colors)
- [ ] **Template Gallery** — Pre-built page templates with 1-click instantiation
- [ ] **Page Preview** — Live preview of draft pages before publishing
- [ ] **Dashboard** — List of all pages with status, published date, and view count

### Should Have
- [ ] **SEO Metadata Editor** — Custom title, description, OG tags per page
- [ ] **Page Analytics** — View counts, section click tracking (anonymous)
- [ ] **Dark Mode** — Full dark mode throughout the app
- [ ] **Responsive Preview** — Mobile/tablet/desktop preview toggle

### Could Have
- [ ] **Custom CSS injection** — Per-page custom CSS overrides
- [ ] **Form builder sections** — Lead capture forms with email notifications
- [ ] **A/B testing** — Page variant comparison
- [ ] **Custom domain** — Publish pages on custom domains
- [ ] **Export to HTML** — Download page as static HTML zip

## Limitaciones Conocidas
- GITHUB_TOKEN: may be empty — git commits prepared locally, push attempted
- NEON_API_KEY: may be invalid — building with SQLite, PostgreSQL-compatible schema
- No OpenAI API key needed (counter=2, non-AI build)
