# Phase 2 Research: Locale Persistence And Formatting

**Phase:** 2
**Name:** Locale Persistence And Formatting
**Researched:** 2026-03-12
**Confidence:** HIGH

## What This Phase Needs To Prove

Phase 2 must prove that language choice survives refresh and later authenticated sessions, and that locale-sensitive formatting follows the active language in the foundational UI surfaces already touched by the runtime i18n rollout.

## Recommended Approach

### 1. Reuse Existing User Preference Persistence

- The frontend already loads user preferences through `UserPreferences.loadPreferences()`.
- The server already accepts arbitrary nested preference updates through `users.set_preferences`.
- The cleanest implementation is to store locale under the existing preference document and reconcile it immediately after preferences load.

### 2. Keep Startup Precedence Predictable

- Before authentication, the app should continue using the Phase 1 resolution order: local language, deployment default, then browser language.
- After authenticated preferences load:
  - if the server already has a saved language, it becomes the authoritative language
  - otherwise the current local/browser-resolved language should be kept and saved to the server preference document

### 3. Standardize Formatting Through Shared Locale Helpers

- Angular locale formatting needs explicit locale-data registration and an app-level `LOCALE_ID` source.
- Template formatting is currently scattered across Angular built-in `date` and `number` pipes, while TS code uses `formatDate`, `DatePipe`, `Intl.Collator`, and `toLocaleString`.
- Phase 2 should create shared locale-aware formatting helpers or pipes and replace the hard-coded or static foundational call sites first.

## Implementation Notes

### Preference Path

- Use a stable nested preference path under the existing user-preference document rather than a one-off local-storage-only key.
- `views.language` fits existing preference naming patterns and keeps locale with other presentation-level user settings.

### Formatting Targets

Prioritize these high-leverage areas:

- app-level locale provider wiring in `app.module.ts`
- Material date adapters currently pinned to English
- programmatic date formatting in model and experiment compare detail builders
- representative template date/number outputs across tables, cards, and dashboard surfaces
- locale-aware human sorting using `Intl.Collator`

### Runtime Behavior

- Switching language should continue to update translation catalogs immediately.
- Persisted preference writes should be best-effort and reuse existing debounced `UserPreferences.setPreferences(...)` behavior.

## Risks

| Risk | Why It Matters | Mitigation |
|------|----------------|------------|
| Server preference and local choice disagree | Could cause confusing language jumps after login | Define explicit precedence and reconcile in one place |
| Angular formatting remains pinned to English | Users would still see mixed-language locale output | Register locale data, provide app locale mapping, and replace static formatting call sites |
| Material date pickers diverge from app locale | Settings and filters would remain inconsistent | Drive `DateAdapter` locale from the locale service |

## Validation Architecture

### Tooling

- Primary build command: `cd aicodexml-web && npm run build`
- Supplemental static validation:
  - `rg` checks for remaining Phase 2 hard-coded `en-US` / `enGB` locale pins in touched areas
- Primary manual check:
  - switch language, refresh, and re-login to confirm reconciliation and formatting behavior

### Minimum Verification For This Phase

1. Build succeeds with locale provider and formatting changes.
2. Authenticated preference loading reconciles language cleanly.
3. Signed-in language changes enqueue persistence through the existing user preference API.
4. Representative date and number surfaces use locale-aware formatting instead of static English defaults.
5. Date-picker surfaces no longer hard-code English-only adapter locales.

## Sources

- Local code: `aicodexml-web/src/app/shared/services/locale.service.ts`
- Local code: `aicodexml-web/src/app/webapp-common/user-preferences.ts`
- Local code: `aicodexml-web/src/app/webapp-common/shared/services/login.service.ts`
- Local code: `aicodexml-web/src/app/app.module.ts`
- Local code: `aicodexml-web/src/app/webapp-common/shared/components/period-selector/period-selector.component.ts`
- Local code: `aicodexml-web/src/app/webapp-common/project-workloads/workloads-page/workloads-page.component.ts`
- Local code: `apiserver/services/users.py`

---
*Phase 2 research complete*
