---
phase: 01-i18n-foundation
plan: 01
subsystem: ui
tags: [angular, ngx-translate, i18n, runtime-locale]
requires: []
provides:
  - Runtime translation service and fallback behavior
  - Angular bootstrap wiring for locale initialization
  - Seed translation catalogs for shared shell labels
affects: [phase-2, phase-3, phase-4, locale-persistence, shared-shell]
tech-stack:
  added: [@ngx-translate/core, @ngx-translate/http-loader]
  patterns: [centralized locale service, runtime JSON catalogs, fallback missing translation handler]
key-files:
  created:
    - aicodexml-web/src/app/shared/services/locale.service.ts
    - aicodexml-web/src/assets/i18n/en.json
    - aicodexml-web/src/assets/i18n/zh-CN.json
  modified:
    - aicodexml-web/src/app/app.module.ts
    - aicodexml-web/src/app/core/app-init.ts
    - aicodexml-web/src/app/webapp-common/layout/header/header.component.ts
    - aicodexml-web/src/app/webapp-common/layout/header/header.component.html
key-decisions:
  - "Use runtime translation catalogs instead of per-locale builds for the admin app."
  - "Keep locale bootstrap in a dedicated service so later preference persistence can plug in cleanly."
patterns-established:
  - "LocaleService owns language normalization, fallback, local cache, and document language updates."
  - "Shared shell text uses nested translation keys under feature-oriented JSON catalogs."
requirements-completed: [I18N-01, I18N-03]
duration: 10min
completed: 2026-03-12
---

# Phase 1: I18n Foundation Summary

**Angular runtime i18n foundation with ngx-translate, bootstrap locale initialization, and shared-shell translation catalogs**

## Performance

- **Duration:** 10 min
- **Started:** 2026-03-12T16:41:00+0800
- **Completed:** 2026-03-12T16:51:00+0800
- **Tasks:** 3
- **Files modified:** 7

## Accomplishments
- Added runtime translation dependencies and a centralized locale service.
- Wired locale initialization into Angular bootstrap before login flow proceeds.
- Seeded bilingual `en` and `zh-CN` catalogs and translated the first shared header labels.

## Task Commits

Each task was committed atomically:

1. **Task 1: Add runtime translation dependencies and locale scaffolding** - `18ab8d69` (`feat(i18n): add locale runtime foundation`)
2. **Task 2: Bootstrap locale initialization into the Angular app** - `18ab8d69` (`feat(i18n): add locale runtime foundation`)
3. **Task 3: Seed bilingual runtime catalogs for the shared shell baseline** - `1ab87fd9` (`feat(i18n): translate shared header labels`)

## Files Created/Modified
- `aicodexml-web/src/app/shared/services/locale.service.ts` - runtime locale state, fallback handling, and browser-based language resolution
- `aicodexml-web/src/app/app.module.ts` - root translation module wiring and missing-translation handler registration
- `aicodexml-web/src/app/core/app-init.ts` - locale initialization inserted into startup flow
- `aicodexml-web/src/assets/i18n/en.json` - English shell catalog
- `aicodexml-web/src/assets/i18n/zh-CN.json` - Simplified Chinese shell catalog
- `aicodexml-web/src/app/webapp-common/layout/header/header.component.html` - shared header labels now consume translation keys

## Decisions Made

- Used `ngx-translate` runtime loading because the product needs one deployed build with in-app switching.
- Added a friendly missing-translation handler so missing keys degrade more safely than raw dot-paths.

## Deviations from Plan

None - plan executed as intended, with header shell labels translated immediately to make the new runtime path visibly testable.

## Issues Encountered

- `@ngx-translate/http-loader` v17 no longer supports the old constructor signature; switched to `provideTranslateHttpLoader(...)`.
- Repository-wide `npm run lint` fails because of large pre-existing lint debt unrelated to this phase, so `npm run build` was used as the authoritative gate for this plan.

## User Setup Required

None - no external service configuration required.

## Next Phase Readiness

- Locale runtime foundation is in place for user-facing switching and later preference persistence.
- Phase 2 can now focus on durable locale storage and locale-sensitive formatting instead of architecture selection.

---
*Phase: 01-i18n-foundation*
*Completed: 2026-03-12*
