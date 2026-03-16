---
phase: 02-locale-persistence-and-formatting
plan: 03
subsystem: ui
tags: [template-migration, formatting, verification]
requires:
  - phase: 02-01
    provides: Durable locale preference
  - phase: 02-02
    provides: Shared locale-aware formatting utilities
provides:
  - Locale-aware formatting in representative cards, tables, dashboard, serving, and compare surfaces
  - Removal of foundational static English formatting call sites
  - Phase verification and planning-state sync
affects: [phase-3, phase-4, dashboard, serving, experiments, models]
tech-stack:
  added: []
  patterns: [representative template migration, locale-aware TS formatting helpers]
key-files:
  created: []
  modified:
    - aicodexml-web/src/app/features/settings/settings.effects.ts
    - aicodexml-web/src/app/webapp-common/models/dumbs/model-general-info/model-general-info.component.ts
    - aicodexml-web/src/app/webapp-common/experiments-compare/services/experiment-details-reverter.service.ts
    - aicodexml-web/src/app/webapp-common/experiments-compare/services/model-details-reverter.service.ts
    - representative template component html/ts pairs across report, serving, dashboard, dataset, experiments, model, and queue surfaces
key-decisions:
  - "Use representative formatting surfaces as the Phase 2 support baseline instead of claiming full feature localization."
patterns-established:
  - "Programmatic formatting should route through LocaleFormatService when locale-sensitive output is built in TypeScript."
requirements-completed: [PREF-01, FMT-01, FMT-02]
duration: 11min
completed: 2026-03-12
---

# Phase 2: Locale Persistence And Formatting Summary

**Representative UI formatting migration, TypeScript call-site cleanup, and build-backed phase verification**

## Performance

- **Duration:** 11 min
- **Started:** 2026-03-12T17:23:00+0800
- **Completed:** 2026-03-12T17:34:00+0800
- **Tasks:** 3
- **Files modified:** 30+

## Accomplishments

- Replaced hard-coded English-only TypeScript formatting paths in model and experiment detail builders.
- Removed the unused `new DatePipe('en-US')` settings effect pin.
- Migrated representative template date and number outputs to shared locale-aware pipes across report cards, serving tables, dashboard tables, dataset cards, experiment views, compare views, model cards, and queue tables.
- Updated project statistics tooltip text to use the locale-aware formatter.
- Verified the full Angular production build after the Phase 2 migration.

## Task Commits

No commit was created in this execution slice. Changes remain in the working tree.

## Files Created/Modified

- `aicodexml-web/src/app/features/settings/settings.effects.ts` - removed static English date-pipe instantiation
- `aicodexml-web/src/app/webapp-common/models/dumbs/model-general-info/model-general-info.component.ts` - locale-aware model metadata formatting
- `aicodexml-web/src/app/webapp-common/experiments-compare/services/experiment-details-reverter.service.ts` - locale-aware experiment detail formatting
- `aicodexml-web/src/app/webapp-common/experiments-compare/services/model-details-reverter.service.ts` - locale-aware model compare formatting
- Representative card/table/dashboard templates - migrated from Angular built-in `date` and `number` usage to shared `smDate` and `smNumber`

## Decisions Made

- Treated representative high-leverage surfaces as the Phase 2 support baseline while keeping full copy localization for later phases.
- Preferred pipe-based migration for templates and service-based migration for TypeScript call sites to keep the rollout consistent and incremental.

## Deviations from Plan

None.

## Issues Encountered

- The frontend repository already contains unrelated style-budget warnings during production build. They remained unchanged and do not block Phase 2.

## User Setup Required

None.

## Next Phase Readiness

- Shared shell and auth localization can build on stable locale persistence and consistent formatting behavior.
- Phase 3 can focus on user-visible text coverage rather than locale mechanics.

---
*Phase: 02-locale-persistence-and-formatting*
*Completed: 2026-03-12*
