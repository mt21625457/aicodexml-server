---
phase: 02-locale-persistence-and-formatting
plan: 01
subsystem: ui
tags: [locale, preferences, persistence, login-reconciliation]
requires: []
provides:
  - Durable locale preference under existing user preferences
  - Login-time locale reconciliation and backfill
  - Persisted writes from the existing header language switcher
affects: [phase-3, phase-4, user-preferences, login-flow]
tech-stack:
  added: []
  patterns: [views.language preference path, preference backfill, post-auth locale reconciliation]
key-files:
  created: []
  modified:
    - aicodexml-web/src/app/shared/services/locale.service.ts
    - aicodexml-web/src/app/webapp-common/user-preferences.ts
    - aicodexml-web/src/app/layout/header/header-user-menu-actions/header-user-menu-actions.component.ts
key-decisions:
  - "Persist locale under `views.language` to match existing presentation-level preference organization."
  - "When no server preference exists, keep the already resolved language and backfill it to the server."
patterns-established:
  - "UserPreferences.loadPreferences() is now the reconciliation point between anonymous locale fallback and authenticated preference authority."
requirements-completed: [PREF-01, PREF-02]
duration: 8min
completed: 2026-03-12
---

# Phase 2: Locale Persistence And Formatting Summary

**Server-backed locale persistence and login-time reconciliation using the existing user preference pipeline**

## Performance

- **Duration:** 8 min
- **Started:** 2026-03-12T17:06:00+0800
- **Completed:** 2026-03-12T17:14:00+0800
- **Tasks:** 3
- **Files modified:** 3

## Accomplishments

- Added a stable locale preference path at `views.language`.
- Reconciled authenticated user preferences after load so saved server locale wins over local/browser fallback.
- Backfilled the current active locale when a signed-in user had no saved locale yet.
- Updated the existing header language switcher to persist locale changes once preferences are ready.

## Task Commits

No commit was created in this execution slice. Changes remain in the working tree.

## Files Created/Modified

- `aicodexml-web/src/app/shared/services/locale.service.ts` - locale preference path, persisted-language lookup, and reconciliation helpers
- `aicodexml-web/src/app/webapp-common/user-preferences.ts` - post-load locale reconciliation and backfill into existing preference persistence
- `aicodexml-web/src/app/layout/header/header-user-menu-actions/header-user-menu-actions.component.ts` - persisted writes from the existing header language switcher

## Decisions Made

- Reused the current user preference API rather than adding a new locale-specific backend surface.
- Chose `views.language` so locale sits alongside other user-facing presentation preferences like theme and shell toggles.

## Deviations from Plan

None. The persistence work stayed within the existing preference and header-switcher boundaries.

## Issues Encountered

- Preference reconciliation had to happen after authenticated preferences load rather than during app bootstrap because anonymous fallback still resolves before login state is known.

## User Setup Required

None.

## Next Phase Readiness

- Active language is now durable for signed-in users.
- Formatting infrastructure can safely use the active locale without being limited to session-only switching.

---
*Phase: 02-locale-persistence-and-formatting*
*Completed: 2026-03-12*
