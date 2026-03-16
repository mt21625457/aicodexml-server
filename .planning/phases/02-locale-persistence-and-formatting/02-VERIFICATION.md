---
phase: 02-locale-persistence-and-formatting
status: passed
verified: 2026-03-12
score: 4/4
---

# Phase 2 Verification

## Goal

Make locale choice stable across sessions and ensure locale-sensitive values follow the active language.

## Requirement Coverage

| Requirement | Result | Evidence |
|-------------|--------|----------|
| PREF-01 | ✓ VERIFIED | `aicodexml-web/src/app/webapp-common/user-preferences.ts` reconciles `views.language` from server preferences and `aicodexml-web/src/app/layout/header/header-user-menu-actions/header-user-menu-actions.component.ts` persists changes from the current switcher |
| PREF-02 | ✓ VERIFIED | `aicodexml-web/src/app/shared/services/locale.service.ts` still resolves anonymous fallback from local/config/browser, and `aicodexml-web/src/app/webapp-common/user-preferences.ts` backfills that active locale when no server preference exists |
| FMT-01 | ✓ VERIFIED | `aicodexml-web/src/app/app.module.ts` registers Angular locale data and representative date outputs now use `smDate` or `LocaleFormatService` across model, experiment, dashboard, report, and queue surfaces |
| FMT-02 | ✓ VERIFIED | `aicodexml-web/src/app/shared/services/locale-format.service.ts` and `aicodexml-web/src/app/webapp-common/shared/pipes/localized-format.pipe.ts` provide locale-aware number formatting, which is used in representative serving and compare surfaces |

## Verification Checks

- `cd aicodexml-web && npm run build` passed on 2026-03-12
- `rg -n "en-US|enGB" ...` across the touched Phase 2 foundational files returned no matches
- Material date-adapter providers in `period-selector.component.ts` and `workloads-page.component.ts` now use locale-service-driven factories instead of static English locale objects
- `sort.pipe.ts` and `parallel-coordinates-graph.component.ts` now use locale-aware collator behavior from the shared formatting service

## Notes

- Production build still reports pre-existing style-budget warnings unrelated to locale work; these were already present and do not block this phase.
- A live authenticated browser session was not run in this environment, so refresh/re-login behavior is verified through code path inspection plus successful build rather than end-to-end UI automation.

## Conclusion

Phase 2 goal achieved. Locale preference is now durable for signed-in users, anonymous-to-authenticated reconciliation is defined, and foundational date/number formatting follows the active locale in the representative support surfaces touched this phase.
