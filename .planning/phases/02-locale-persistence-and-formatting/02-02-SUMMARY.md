---
phase: 02-locale-persistence-and-formatting
plan: 02
subsystem: ui
tags: [angular, locale-data, material-datepicker, formatting]
requires:
  - phase: 02-01
    provides: Durable active language state
provides:
  - Angular locale-data registration and app-level locale provider
  - Shared locale-aware formatting service and pipes
  - Dynamic Material date-adapter locale and locale-aware natural sorting
affects: [phase-3, phase-4, shared-components, formatting]
tech-stack:
  added: []
  patterns: [registered locale data, shared formatting service, locale-aware template pipes, reactive date-adapter locale]
key-files:
  created:
    - aicodexml-web/src/app/shared/services/locale-format.service.ts
    - aicodexml-web/src/app/webapp-common/shared/pipes/localized-format.pipe.ts
  modified:
    - aicodexml-web/src/app/app.module.ts
    - aicodexml-web/src/app/shared/services/locale.service.ts
    - aicodexml-web/src/app/webapp-common/shared/components/period-selector/period-selector.component.ts
    - aicodexml-web/src/app/webapp-common/project-workloads/workloads-page/workloads-page.component.ts
    - aicodexml-web/src/app/webapp-common/shared/pipes/sort.pipe.ts
    - aicodexml-web/src/app/webapp-common/experiments-compare/dumbs/parallel-coordinates-graph/parallel-coordinates-graph.component.ts
key-decisions:
  - "Use one locale mapping source for Angular, Intl, and date-fns rather than scattered per-feature locale decisions."
  - "Standardize template migration through shared `smDate` and `smNumber` pipes for incremental rollout."
patterns-established:
  - "LocaleFormatService provides runtime locale-aware date, number, percent, and collator behavior."
requirements-completed: [FMT-01, FMT-02]
duration: 9min
completed: 2026-03-12
---

# Phase 2: Locale Persistence And Formatting Summary

**App-level locale provider, reusable formatting helpers, and locale-aware date-adapter and sorting behavior**

## Performance

- **Duration:** 9 min
- **Started:** 2026-03-12T17:14:00+0800
- **Completed:** 2026-03-12T17:23:00+0800
- **Tasks:** 3
- **Files modified:** 8

## Accomplishments

- Registered Angular locale data for English and Simplified Chinese.
- Wired `LOCALE_ID` to the active runtime language through `LocaleService`.
- Added `LocaleFormatService` plus shared `smDate`, `smNumber`, and `smPercent` pipes for incremental UI migration.
- Replaced hard-coded English-only Material date-adapter locale pins with locale-service-driven factories and runtime adapter updates.
- Switched human sorting and natural compare logic to locale-aware `Intl.Collator`.

## Task Commits

No commit was created in this execution slice. Changes remain in the working tree.

## Files Created/Modified

- `aicodexml-web/src/app/app.module.ts` - Angular locale-data registration and app-level locale provider
- `aicodexml-web/src/app/shared/services/locale.service.ts` - Angular/Intl/date-fns locale mapping
- `aicodexml-web/src/app/shared/services/locale-format.service.ts` - shared locale-aware formatting API
- `aicodexml-web/src/app/webapp-common/shared/pipes/localized-format.pipe.ts` - shared locale-aware template pipes
- `aicodexml-web/src/app/webapp-common/shared/components/period-selector/period-selector.component.ts` - locale-driven date adapter
- `aicodexml-web/src/app/webapp-common/project-workloads/workloads-page/workloads-page.component.ts` - locale-driven date adapter
- `aicodexml-web/src/app/webapp-common/shared/pipes/sort.pipe.ts` - locale-aware collator usage
- `aicodexml-web/src/app/webapp-common/experiments-compare/dumbs/parallel-coordinates-graph/parallel-coordinates-graph.component.ts` - locale-aware natural compare

## Decisions Made

- Standardized future template localization work around new shared pipes rather than forcing every component to pass locale IDs manually.
- Kept date-adapter updates reactive so picker surfaces follow runtime language changes without new bootstrapping rules.

## Deviations from Plan

None.

## Issues Encountered

- Angular formatting, `Intl`, and date-fns all need different locale representations, so the locale service had to become the mapping source of truth rather than only a translation switcher.

## User Setup Required

None.

## Next Phase Readiness

- Later phases can localize broader UI surfaces using shared formatting pipes instead of ad hoc locale handling.
- Shared shell and auth localization can now rely on consistent locale-sensitive formatting behavior.

---
*Phase: 02-locale-persistence-and-formatting*
*Completed: 2026-03-12*
