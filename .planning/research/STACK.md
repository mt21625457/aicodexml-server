# Stack Research

**Domain:** Brownfield Angular web application internationalization for a single deployed product with runtime language switching
**Researched:** 2026-03-12
**Confidence:** HIGH

## Recommended Stack

### Core Technologies

| Technology | Version | Purpose | Why Recommended |
|------------|---------|---------|-----------------|
| Angular | 20.3.x (existing project line) | Existing application framework | The app already runs on Angular 20 and should stay on its current major while i18n is introduced incrementally instead of coupling localization to a framework migration |
| `@angular/common` locale APIs (`LOCALE_ID`, locale data, locale-aware pipes) | 20.3.x | Date/number/currency formatting by locale | Angular officially supports locale-sensitive formatting and locale data registration; this should remain the formatting layer even if runtime text translation is handled separately |
| Runtime translation catalogs | `@ngx-translate/core` 17.x with `@ngx-translate/http-loader` 17.x | Runtime loading of `en` / `zh-CN` translation files and user-triggered language switching | Official Angular i18n docs focus on build-time localized app variants. This product needs one deployment where users switch language at runtime, so a runtime loader-based translation layer fits the requirement better |

### Supporting Libraries

| Library | Version | Purpose | When to Use |
|---------|---------|---------|-------------|
| `@angular/localize` | Angular-compatible current 20.x package | Optional extraction/marking support for selected strings or future hybrid strategy | Use only if the team wants Angular-native extraction for a subset of templates; not the primary runtime switching mechanism |
| JSON translation files under app static assets | n/a | Store structured translation catalogs such as `en.json` and `zh-CN.json` | Use for the main UI translation source of truth loaded by the runtime translation service |
| Existing `UserPreferences` + `users.set_preferences` / `users.get_preferences` APIs | existing | Persist preferred locale per authenticated user | Use for logged-in source of truth, with browser/local fallback before server preference is loaded |

### Development Tools

| Tool | Purpose | Notes |
|------|---------|-------|
| `ng extract-i18n` | Inventory and extraction aid | The app already exposes `extract-i18n` targets in `angular.json`; useful for audits even if runtime switching is the chosen delivery path |
| ESLint + template linting | Catch raw string regressions | Add rules or project conventions so new hard-coded UI strings are easier to detect during review |
| Translation editor or structured JSON review process | Keep `en` and `zh-CN` files aligned | Optional, but useful once catalog size grows across hundreds of templates |

## Installation

```bash
# Runtime translation layer
npm install @ngx-translate/core@^17 @ngx-translate/http-loader@^17

# Optional Angular-native helpers if needed later
npm install @angular/localize@^20
```

## Alternatives Considered

| Recommended | Alternative | When to Use Alternative |
|-------------|-------------|-------------------------|
| Runtime translation catalogs with user switching | Angular compile-time i18n using localized builds | Use only if each locale is deployed as a separate app variant or subpath and runtime user switching is not a requirement |
| JSON files grouped by feature/module keys | Translation text as IDs | Avoid text-as-key unless the app is tiny; contextual IDs are safer for large, evolving enterprise UIs |
| Persist locale in user preferences | Browser-only locale storage | Use only if anonymous-only or no server-backed profile exists |

## What NOT to Use

| Avoid | Why | Use Instead |
|-------|-----|-------------|
| Build-per-locale as the primary v1 strategy | Angular’s official localized-build flow creates separate app variants and is a poor fit for a single deployed admin app where users switch language in-session | Runtime translation catalogs plus locale-aware formatting |
| Ad hoc `if (lang === 'zh')` conditionals across components | Creates brittle duplication and guarantees inconsistent wording over time | Central translation service and structured keys |
| Mixing translation key styles (`flat` and `nested`) | Official ngx-translate docs warn that mixed structures create confusion and inconsistency | Pick one convention; nested by feature/module is preferable here |

## Stack Patterns by Variant

**If the app remains a single deployment with in-app language switching:**
- Use runtime JSON catalogs loaded on demand
- Use Angular locale APIs for formatting and `ngx-translate` for UI copy

**If the team later wants SEO-facing public localized routes:**
- Re-evaluate Angular localized builds or SSR routing for public pages
- Keep the internal admin surface on runtime translation if per-user switching remains required

## Version Compatibility

| Package A | Compatible With | Notes |
|-----------|-----------------|-------|
| `@angular/core@20.x` | `@ngx-translate/core@17.x` | Official ngx-translate compatibility docs list Angular 16-20+ support on v17 |
| `@ngx-translate/core@17.x` | `@ngx-translate/http-loader@17.x` | Supported current major pair |
| `LOCALE_ID` / locale data APIs | Angular 20 project | Already usable in the current codebase; some files already inject `LOCALE_ID` |

## Sources

- https://angular.dev/guide/i18n — Angular i18n overview and scope of built-in localization
- https://angular.dev/guide/i18n/merge — Angular localized build variants and single-locale dev-server limitation
- https://angular.dev/guide/i18n/deploy — Angular deployment model for multiple locales in separate directories
- https://angular.dev/guide/i18n/import-global-variants — Angular locale data import behavior
- https://ngx-translate.org/getting-started/installation/ — official runtime translation installation guidance
- https://ngx-translate.org/getting-started/translation-files/ — official translation file loading and structure guidance
- https://ngx-translate.org/getting-started/angular-compatibility/ — official Angular compatibility table for ngx-translate

---
*Stack research for: web multilingual support*
*Researched: 2026-03-12*
