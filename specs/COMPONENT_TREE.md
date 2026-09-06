# Component Tree — Hermes Landing Studio

## Application Shell

```
AppComponent
├── HeaderComponent
│   ├── LogoComponent
│   ├── SearchBarComponent (dashboard only)
│   ├── ThemeToggleComponent
│   └── UserMenuComponent
└── RouterOutlet (lazy loaded pages)
```

## Page Components (Lazy Loaded)

```
LoginPageComponent
├── LoginFormComponent
│   └── AuthToggleComponent (login ↔ register switch)

DashboardPageComponent
├── KpiCardComponent (×3 — total, published, visits)
├── PageFilterComponent (status tabs + search)
├── PageListComponent
│   └── PageCardComponent (×N)
│       ├── StatusBadgeComponent
│       └── PageActionsComponent
└── EmptyStateComponent (when no pages)
    └── CtaButtonComponent

TemplateGalleryPageComponent
├── TemplateGridComponent
│   └── TemplateCardComponent (×N)
│       ├── TemplatePreviewThumbnailComponent
│       └── SectionCountBadgeComponent
└── EmptyStateComponent (when no templates)

PageEditorComponent (main editor)
├── EditorLayoutComponent
│   ├── SectionSidebarComponent
│   │   ├── AddSectionButtonComponent
│   │   ├── SectionListItemComponent (×N)
│   │   │   ├── DragHandleComponent
│   │   │   ├── SectionIconComponent
│   │   │   ├── SectionNameComponent
│   │   │   ├── EditSectionButtonComponent
│   │   │   └── DeleteSectionButtonComponent
│   │   └── SeoPanelComponent
│   │       ├── SeoTitleInputComponent
│   │       ├── SeoDescriptionInputComponent
│   │       ├── OgImageInputComponent
│   │       └── GoogleSearchPreviewComponent
│   └── PreviewPanelComponent
│       ├── ResponsiveToggleComponent (desktop/tablet/mobile)
│       ├── PagePreviewComponent
│       │   ├── SectionRendererComponent (×N, dynamic by type)
│       │   │   ├── 📄 HeroSectionRendererComponent
│       │   │   ├── 📄 FeaturesSectionRendererComponent
│       │   │   ├── 📄 PricingSectionRendererComponent
│       │   │   ├── 📄 CtaSectionRendererComponent
│       │   │   ├── 📄 TestimonialsSectionRendererComponent
│       │   │   ├── 📄 FaqSectionRendererComponent
│       │   │   └── 📄 FooterSectionRendererComponent
│       │   └── SaveIndicatorComponent
│       └── PublishButtonComponent
│           └── ConfirmDialogComponent
└── SectionEditorPanelComponent (slide-over)
    ├── HeroSectionEditorComponent
    ├── FeaturesSectionEditorComponent
    ├── PricingSectionEditorComponent
    ├── CtaSectionEditorComponent
    ├── TestimonialsSectionEditorComponent
    ├── FaqSectionEditorComponent
    └── FooterSectionEditorComponent

PublicPageComponent (no auth)
├── MetaHeadComponent (SEO tags, server-side)
└── PageRendererComponent
    └── SectionRendererComponent (×N, same renderers as preview)

AnalyticsDashboardComponent (embedded in page detail)
├── KpiCardComponent (view count, clicks)
└── DailyChartComponent (SVG bar chart)
```

## Shared / Reusable Components

```
SpinnerComponent        — Loading spinner animation
SkeletonComponent       — Generic skeleton loader with pulse
BadgeComponent          — Status badges (published/draft/info)
ModalComponent          — Generic modal dialog
ToastComponent          — Notification toast (success/error/info)
IconComponent           — SVG icon wrapper
EmptyStateComponent     — Empty state with message + CTA
ErrorStateComponent     — Error state with message + retry
ConfirmDialogComponent  — Confirmation dialog (delete, publish)
```

---

## Signal-Based State (Key Services)

### AuthService

```typescript
@Injectable({ providedIn: 'root' })
export class AuthService {
  private readonly http = inject(HttpClient);
  private readonly router = inject(Router);

  readonly user = signal<User | null>(null);
  readonly token = signal<string | null>(localStorage.getItem('token'));
  readonly organizationId = signal<string | null>(null);
  readonly isAuthenticated = computed(() => this.token() !== null);
  readonly loading = signal(false);
  readonly error = signal<string | null>(null);

  login(email: string, password: string): Observable<void> {
    this.loading.set(true);
    this.error.set(null);
    return this.http.post<AuthResponse>('/api/auth/login', { email, password }).pipe(
      tap(res => {
        localStorage.setItem('token', res.token);
        this.token.set(res.token);
        this.user.set(res.user);
        this.organizationId.set(res.user.organizationId);
        this.router.navigate(['/dashboard']);
      }),
      catchError(err => {
        this.error.set(err.error?.message || 'Error de autenticación');
        this.loading.set(false);
        return throwError(() => err);
      }),
      finalize(() => this.loading.set(false))
    );
  }

  logout(): void {
    localStorage.removeItem('token');
    this.token.set(null);
    this.user.set(null);
    this.organizationId.set(null);
    this.router.navigate(['/login']);
  }
}
```

### PageService

```typescript
@Injectable({ providedIn: 'root' })
export class PageService {
  private readonly http = inject(HttpClient);

  readonly pages = signal<Page[]>([]);
  readonly loading = signal(false);
  readonly error = signal<string | null>(null);
  readonly currentPage = signal<Page | null>(null);
  readonly sections = signal<PageSection[]>([]);
  readonly saving = signal(false);
  readonly lastSaved = signal<Date | null>(null);

  readonly publishedPages = computed(() =>
    this.pages().filter(p => p.status === 'published')
  );
  readonly draftPages = computed(() =>
    this.pages().filter(p => p.status === 'draft')
  );

  // Filtered + searched
  readonly filterStatus = signal<'all' | 'draft' | 'published'>('all');
  readonly searchQuery = signal('');

  readonly filteredPages = computed(() => {
    let result = this.pages();
    const status = this.filterStatus();
    if (status !== 'all') {
      result = result.filter(p => p.status === status);
    }
    const query = this.searchQuery().toLowerCase();
    if (query) {
      result = result.filter(p =>
        p.title.toLowerCase().includes(query) ||
        p.slug.toLowerCase().includes(query)
      );
    }
    return result;
  });

  readonly isEmpty = computed(() => !this.loading() && this.pages().length === 0);

  loadPages(): void { /* GET /api/pages */ }
  createPageFromTemplate(templateId: string, title: string): Observable<Page> { /* POST /api/pages */ }
  getPage(id: string): void { /* GET /api/pages/:id + sections */ }
  updatePage(id: string, data: Partial<Page>): void { /* PATCH /api/pages/:id */ }
  publishPage(id: string): void { /* PATCH /api/pages/:id { status: 'published' } */ }
  unpublishPage(id: string): void { /* PATCH /api/pages/:id { status: 'draft' } */ }
  deletePage(id: string): void { /* DELETE /api/pages/:id */ }
  reorderSections(pageId: string, sectionIds: string[]): void { /* PATCH /api/pages/:id/sections/reorder */ }
  updateSection(pageId: string, sectionId: string, data: Partial<PageSection>): void { /* PATCH /api/pages/:id/sections/:sectionId */ }
  addSection(pageId: string, type: SectionType): void { /* POST /api/pages/:id/sections */ }
  deleteSection(pageId: string, sectionId: string): void { /* DELETE /api/pages/:id/sections/:sectionId */ }
}
```

### TemplateService

```typescript
@Injectable({ providedIn: 'root' })
export class TemplateService {
  private readonly http = inject(HttpClient);

  readonly templates = signal<Template[]>([]);
  readonly loading = signal(false);
  readonly error = signal<string | null>(null);

  readonly isEmpty = computed(() => !this.loading() && this.templates().length === 0);

  loadTemplates(): void { /* GET /api/templates */ }
}
```

### AnalyticsService

```typescript
@Injectable({ providedIn: 'root' })
export class AnalyticsService {
  private readonly http = inject(HttpClient);

  readonly pageSummary = signal<PageAnalytics | null>(null);
  readonly dailyData = signal<DailyStat[]>([]);
  readonly loading = signal(false);

  readonly totalViews = computed(() => this.pageSummary()?.totalViews ?? 0);
  readonly totalClicks = computed(() => this.pageSummary()?.totalClicks ?? 0);

  loadSummary(pageId: string): void { /* GET /api/analytics/page/:pageId/summary */ }
  loadDaily(pageId: string, days: number = 7): void { /* GET /api/analytics/page/:pageId/daily?days=7 */ }

  trackEvent(pageId: string, eventType: 'view' | 'click', metadata?: Record<string, any>): void {
    // Anonymous, no auth — public page tracking
    this.http.post('/api/analytics', { pageId, eventType, metadata }).subscribe();
  }
}
```

---

## PageEditorComponent State

```typescript
@Component({
  template: `
    @if (pageService.loading()) {
      <app-skeleton />
    } @else if (pageService.error()) {
      <app-error-state [message]="pageService.error()!" (retry)="loadPage()" />
    } @else {
      <app-editor-layout>
        <app-section-sidebar />
        <app-preview-panel />
      </app-editor-layout>
    }
  `
})
export class PageEditorComponent {
  readonly pageService = inject(PageService);
  readonly activeSectionId = signal<string | null>(null);
  readonly previewDevice = signal<'desktop' | 'tablet' | 'mobile'>('desktop');
  readonly showSectionEditor = computed(() => this.activeSectionId() !== null);
  readonly sortedSections = computed(() =>
    [...this.pageService.sections()].sort((a, b) => a.sortOrder - b.sortOrder)
  );

  // Drag state
  readonly draggingSectionId = signal<string | null>(null);
  readonly dragOverSectionId = signal<string | null>(null);
}
```
