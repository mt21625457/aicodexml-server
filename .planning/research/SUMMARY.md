# Project Research Summary

**Project:** AI Code XML Web Multilingual
**Domain:** Brownfield Angular enterprise web app internationalization
**Researched:** 2026-03-12
**Confidence:** HIGH

## Executive Summary

This project is not a greenfield “add translation support” exercise. It is a retrofit of a large Angular 20 admin-style web application with roughly 1,700+ TypeScript/HTML files under `src/app`, existing runtime configuration injection, and existing user preference persistence APIs. The user chose full visible UI coverage, so the real challenge is not just selecting an i18n library but creating a migration path that can systematically convert a mature codebase without shipping a half-translated product.

Research shows a clear architectural split. Angular’s official i18n workflow is centered on build-time localized app variants and per-locale deployment, which is a mismatch for a single deployed product where signed-in users must switch languages at runtime. For this project, the recommended approach is a runtime translation layer for UI text, Angular locale APIs for formatting, and user-preference-backed locale persistence with browser fallback.

## Key Findings

### Recommended Stack

Use the existing Angular 20 stack, add a runtime translation layer using `@ngx-translate/core` with `@ngx-translate/http-loader`, keep Angular locale APIs for formatting, and store `en` / `zh-CN` catalogs in structured JSON files. The backend already exposes `users.get_preferences` and `users.set_preferences`, so locale persistence should build on that instead of inventing a new storage path.

**Core technologies:**
- Angular 20.3.x: keep the current framework stable while adding i18n incrementally
- `@ngx-translate/core` + `@ngx-translate/http-loader` 17.x: runtime language switching and catalog loading
- Angular locale APIs: dates, numbers, percentages, and locale-aware formatting

### Expected Features

**Must have (table stakes):**
- Runtime language switcher
- English and Simplified Chinese translation catalogs
- Persistent user locale plus browser fallback
- Locale-aware formatting
- Full visible UI coverage across existing pages, dialogs, forms, and shared components

**Should have (competitive):**
- Translation coverage audits
- Runtime-configurable default locale
- Guardrails against new hard-coded UI strings

**Defer (v2+):**
- Additional locales
- Broad server-side API message localization
- Localization of user-generated content

### Architecture Approach

The architecture should add a dedicated locale bootstrap and translation layer near app startup, then migrate the UI in waves. Initial locale resolution should use browser/cached state until login and preferences load, then reconcile to the authenticated preference. Shared components and application shell come first, because they influence every route and reduce duplicate migration effort.

**Major components:**
1. Locale bootstrap and reconciliation service
2. Runtime translation catalogs and loading strategy
3. Preference persistence integration
4. Formatting localization layer
5. Feature-by-feature migration and regression checks

### Critical Pitfalls

1. **Using build-per-locale as the main strategy** — avoid because the product needs runtime user switching in a single deployment
2. **Underestimating string inventory** — avoid by auditing shared components, dialogs, notifications, and TS-built text, not just templates
3. **Ignoring formatting localization** — avoid by treating `LOCALE_ID` and locale data work as first-class deliverables
4. **Saving locale only in browser storage** — avoid by persisting to user preferences
5. **Shipping with missing-key leaks** — avoid by enforcing fallback and verification checks

## Implications for Roadmap

Based on research, suggested phase structure:

### Phase 1: I18n Foundation
**Rationale:** The app needs one chosen architecture before any bulk migration starts.
**Delivers:** translation service wiring, locale bootstrap, supported language config, initial catalogs
**Addresses:** architecture choice, language switching baseline
**Avoids:** wrong build-time-only strategy

### Phase 2: Preference And Formatting
**Rationale:** Language should feel stable and correct before mass translation work.
**Delivers:** user preference persistence, browser fallback, locale data registration, formatting alignment
**Uses:** existing user preference APIs and bootstrap flow
**Implements:** locale reconciliation path

### Phase 3: Shared Shell And Core Components
**Rationale:** Header, sidenav, settings, common dialogs, and shared UI affect most routes.
**Delivers:** translated shell and reusable components, shared key conventions
**Implements:** highest-leverage migration surface

### Phase 4: Feature Sweep
**Rationale:** After shared infrastructure is stable, migrate feature modules systematically.
**Delivers:** translation coverage across remaining user-visible pages
**Addresses:** full-UI coverage commitment

### Phase 5: Verification And Guardrails
**Rationale:** A large brownfield localization project needs regression protection.
**Delivers:** bilingual QA pass, missing-key checks, maintenance conventions, optional CI/lint guardrails

### Phase Ordering Rationale

- Foundation must precede migration or the codebase will fragment into multiple translation patterns.
- Preference and formatting come early because they affect perceived correctness across all pages.
- Shared shell before feature sweep provides maximum leverage and reduces duplicate work.
- Verification is a dedicated phase because “looks translated” is not enough in a large enterprise UI.

### Research Flags

Phases likely needing deeper research during planning:
- **Phase 1:** exact NgModule integration pattern for the chosen translation library in this specific app structure
- **Phase 2:** best locale bootstrap order to minimize flicker around login and preference load

Phases with standard patterns:
- **Phase 3:** shared component translation migration
- **Phase 4:** feature-by-feature catalog extraction and replacement

## Confidence Assessment

| Area | Confidence | Notes |
|------|------------|-------|
| Stack | HIGH | Core recommendation validated against official Angular and ngx-translate docs |
| Features | HIGH | Strongly grounded in product requirement and brownfield app reality |
| Architecture | HIGH | Existing bootstrap and preference plumbing make the recommended shape clear |
| Pitfalls | HIGH | Risks are visible both in official docs and in the local codebase size/shape |

**Overall confidence:** HIGH

### Gaps to Address

- Exact current count and distribution of hard-coded strings still needs implementation-time inventorying
- Whether runtime default locale should also be configurable via `configuration.json` should be finalized during phase planning

## Sources

### Primary (HIGH confidence)
- https://angular.dev/guide/i18n
- https://angular.dev/guide/i18n/merge
- https://angular.dev/guide/i18n/deploy
- https://angular.dev/guide/i18n/import-global-variants
- https://ngx-translate.org/getting-started/installation/
- https://ngx-translate.org/getting-started/translation-files/
- https://ngx-translate.org/getting-started/angular-compatibility/

### Primary local context (HIGH confidence)
- `aicodexml-web/angular.json`
- `aicodexml-web/src/app/core/app-init.ts`
- `aicodexml-web/src/app/webapp-common/user-preferences.ts`
- `apiserver/services/users.py`

---
*Research completed: 2026-03-12*
*Ready for roadmap: yes*
