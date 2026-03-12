# Architecture Research

**Domain:** Brownfield Angular admin application runtime internationalization
**Researched:** 2026-03-12
**Confidence:** HIGH

## Standard Architecture

### System Overview

```text
┌─────────────────────────────────────────────────────────────┐
│ UI Layer                                                    │
├─────────────────────────────────────────────────────────────┤
│  Angular components, templates, dialogs, tables, forms     │
│  consume translated labels and locale-aware formatted data  │
├─────────────────────────────────────────────────────────────┤
│ Translation / Locale Layer                                  │
├─────────────────────────────────────────────────────────────┤
│  Language state │ translation loader │ locale formatter     │
│  fallback rules │ missing-key policy │ HTML lang sync       │
├─────────────────────────────────────────────────────────────┤
│ Preference / Runtime Config Layer                           │
├─────────────────────────────────────────────────────────────┤
│  Browser language │ local cache │ user preferences API      │
│  deployment default locale │ bootstrap initializer          │
├─────────────────────────────────────────────────────────────┤
│ Asset / API Layer                                           │
├─────────────────────────────────────────────────────────────┤
│  /i18n/en.json  /i18n/zh-CN.json  users.get_preferences     │
│  users.set_preferences  configuration.json                  │
└─────────────────────────────────────────────────────────────┘
```

### Component Responsibilities

| Component | Responsibility | Typical Implementation |
|-----------|----------------|------------------------|
| Locale bootstrap | Decide initial language before or during app startup | Browser detection + cached value + user preference reconciliation |
| Translation service adapter | Expose current language, switching, fallback, and typed helpers | Wrapper around runtime translation library plus app-specific defaults |
| Catalog storage | Store structured UI text for `en` and `zh-CN` | JSON files grouped by feature/module |
| Preference sync | Persist user locale after login | Existing `users.get_preferences` / `users.set_preferences` APIs |
| Formatting layer | Localize dates, numbers, percentages, and plural behavior | Angular locale APIs and locale data registration |

## Recommended Project Structure

```text
aicodexml-web/
├── src/
│   ├── app/
│   │   ├── core/                 # bootstrap, app init, cross-cutting providers
│   │   ├── shared/               # shared pipes, wrappers, helpers
│   │   ├── features/             # feature-level migration to translation keys
│   │   └── webapp-common/        # legacy/common UI needing most string migration
│   ├── assets/ or public/
│   │   └── i18n/                 # en.json, zh-CN.json, optional split catalogs
│   └── main.ts / app.module.ts   # locale and translation provider wiring
└── package.json
```

### Structure Rationale

- **`core/`** should own language bootstrap so feature modules do not invent their own language initialization.
- **`shared/`** should expose common translation utilities, missing-key handling, and formatting helpers.
- **Feature/module-based keys** make catalog ownership clearer in a large brownfield app.

## Architectural Patterns

### Pattern 1: Bootstrap-Then-Reconcile Locale

**What:** Start with browser or cached locale, then reconcile to server-side user preference after authentication and preference load.
**When to use:** Apps where anonymous startup happens before authenticated profile state is fully available.
**Trade-offs:** Reduces blank or wrong-language initial load, but may require one controlled language flip after login bootstrap.

### Pattern 2: Translation Facade

**What:** Wrap raw translation library calls in an app-level locale service.
**When to use:** Large apps where direct library usage everywhere becomes hard to govern.
**Trade-offs:** Small abstraction cost, but easier testing, migration, and future replacement.

### Pattern 3: Incremental Feature Migration

**What:** Migrate shared shell and common components first, then feature areas in waves.
**When to use:** Brownfield apps with hundreds of templates and TS strings.
**Trade-offs:** Requires inventory and tracking, but avoids destabilizing the whole UI in one patch.

## Data Flow

### Request Flow

```text
Browser language / cached locale
    ↓
App initializer
    ↓
Translation service starts with fallback language
    ↓
Login + user preference load
    ↓
Reconcile preferred locale
    ↓
Load matching translation catalog
    ↓
Components render translated copy and locale-aware formatting
```

### State Management

```text
User action: switch language
    ↓
Locale service updates current language
    ↓
Translation library loads/activates catalog
    ↓
UserPreferences saves locale
    ↓
Server preference persists via users.set_preferences
```

### Key Data Flows

1. **Initial locale resolution:** deployment default → browser/cached locale → authenticated user preference.
2. **Language switch:** UI trigger → locale service → catalog activation → persistence.
3. **Formatting:** active locale state → Angular formatting functions/pipes in templates and TS.

## Anti-Patterns

### Anti-Pattern 1: Half Runtime, Half Build-Time Localization

**What people do:** Use Angular compile-time i18n for some areas and runtime translation for the rest without clear boundaries.
**Why it's wrong:** Produces duplicate workflows, confusing catalogs, and inconsistent switching behavior.
**Do this instead:** Pick runtime translation as the default for this project; use Angular native extraction only as a supporting tool if needed.

### Anti-Pattern 2: Feature Teams Owning Keys Without Shared Convention

**What people do:** Add arbitrary translation key formats per file or component.
**Why it's wrong:** Catalogs become unmaintainable and translators lose context.
**Do this instead:** Use module-scoped nested keys and a documented naming convention.

## Integration Points

### External Services

| Service | Integration Pattern | Notes |
|---------|---------------------|-------|
| `users.get_preferences` | Read authenticated locale preference | Already available in the backend |
| `users.set_preferences` | Persist locale changes | Existing API supports dotted preference updates |
| `configuration.json` | Optional default locale / enabled languages | Good fit for deployment-specific defaults |

### Internal Boundaries

| Boundary | Communication | Notes |
|----------|---------------|-------|
| `core/app-init` ↔ locale bootstrap | initializer + service injection | Startup timing matters to avoid flicker |
| translation layer ↔ shared/common UI | pipe/service/direct helper | Centralize to keep migration consistent |
| locale state ↔ formatting code | `LOCALE_ID` / explicit locale args | Existing locale-aware utilities should be aligned with active app locale |

## Sources

- https://angular.dev/guide/i18n
- https://angular.dev/guide/i18n/merge
- https://angular.dev/guide/i18n/deploy
- https://angular.dev/guide/i18n/import-global-variants
- https://ngx-translate.org/getting-started/installation/
- https://ngx-translate.org/getting-started/translation-files/
- Local codebase files: `aicodexml-web/src/app/core/app-init.ts`, `aicodexml-web/src/app/webapp-common/user-preferences.ts`, `apiserver/services/users.py`

---
*Architecture research for: web multilingual support*
*Researched: 2026-03-12*
