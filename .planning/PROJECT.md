# AI Code XML Web Multilingual

## What This Is

This project adds full bilingual support to the existing AI Code XML web application, prioritizing Simplified Chinese and English across all user-visible UI text. The web frontend lives in the local `aicodexml-web` submodule and is built into this server repository's Docker image, so the work spans frontend internationalization, runtime configuration, and preference persistence integration.

The goal is not to build a new web app. It is to retrofit a mature Angular-based ClearML-derived UI so Chinese and English users can reliably use the same product without language friction.

## Core Value

Users can switch between Chinese and English and consistently see the entire web UI in their chosen language without breaking existing workflows.

## Current State

Milestone `v1.0` shipped on 2026-03-16 and archived the first bilingual rollout for the web application.

- Runtime English and Simplified Chinese support is implemented through `ngx-translate` JSON catalogs plus a centralized `LocaleService`.
- Language switching is exposed in the header menu and persists through the existing user preference path `views.language`.
- Locale-sensitive formatting now follows the active language through shared date/number/percent helpers and locale-aware service formatting.
- The rollout includes localized shared shell/auth/common surfaces, route-level feature pages, and a local `npm run i18n:guardrail` for future regressions.

## Next Milestone Goals

- Run authenticated bilingual walkthroughs across post-login feature routes when stable QA credentials or a dedicated environment are available.
- Reduce the i18n guardrail baseline by converting more legacy hard-coded UI strings into translation keys.
- Decide whether the next milestone expands language scope, improves localization operations, or focuses on adjacent product work.

## Requirements

### Validated

- ✓ Existing ClearML-derived web UI is already integrated into this repository via the `aicodexml-web` submodule and packaged by `docker/build/Dockerfile` — existing
- ✓ Existing backend and web runtime configuration flow can inject web settings into the deployed UI via `configuration.json` and environment-based startup scripts — existing
- ✓ Existing application already supports authenticated user state and user preferences loading on startup, creating a viable place to persist language preference — existing
- ✓ All current user-visible web UI text supports Simplified Chinese and English in the implemented release scope — v1.0
- ✓ Users can switch language intentionally from the web application without needing redeploys or manual config edits — v1.0
- ✓ Logged-in users keep their language preference across sessions, with browser language and local state as fallback before account preference is known — v1.0
- ✓ Shared UI components, navigation, settings, forms, dialogs, notifications, and empty states use a consistent translation mechanism instead of ad hoc string handling — v1.0
- ✓ The Dockerized server/web build continues to package the localized frontend from the local `aicodexml-web` submodule — v1.0

### Active

- [ ] Complete authenticated end-to-end bilingual verification across representative post-login routes
- [ ] Shrink the i18n guardrail baseline by removing accepted legacy hard-coded strings
- [ ] Decide whether to backfill missing phase-level `VALIDATION.md` files or replace that coverage with a newer milestone-level validation standard

### Out of Scope

- Additional locales beyond Simplified Chinese and English — keep v1 focused on the two languages the team explicitly needs
- Translation of user-generated content, experiment names, project names, logs, or external data payloads — those are content, not product UI chrome
- Rewriting unrelated frontend architecture or redesigning the product UI — this initiative is localization-first, not a general frontend overhaul
- Backend API message localization for every server response — only support server-side changes required to persist and expose locale preference cleanly

## Context

This is a brownfield project on top of an existing ClearML Server fork. The server repository contains Python API and file services plus Docker build/deployment logic. The actual frontend source now lives locally in the `aicodexml-web` git submodule on the `dev` branch, and the server build consumes it directly instead of cloning a remote repo during image build.

The frontend is still an Angular 20 application with a large existing feature surface, but the multilingual foundation is no longer hypothetical. The codebase now has a working runtime i18n path, translated `en` and `zh-CN` catalogs, persisted locale selection, locale-aware formatting helpers, and a baseline-backed guardrail for newly introduced hard-coded UI strings.

The remaining concerns are mostly cleanup and verification depth rather than architectural uncertainty: authenticated route QA coverage is incomplete, the initial guardrail baseline is intentionally broad, and legacy lint/style-budget debt still exists outside the milestone scope.

## Constraints

- **Tech stack**: Must fit the existing Angular 20 frontend and current server/runtime packaging flow — avoid introducing a localization approach that fights the current build system
- **Compatibility**: Must preserve existing login, navigation, dashboards, settings, and feature behavior while replacing visible strings — regression risk is high in a mature UI
- **Scope**: v1 focuses on Simplified Chinese and English only — broad locale expansion would delay delivery and increase translation maintenance cost
- **Deployment**: The frontend is built through this repository's Docker pipeline from the `aicodexml-web` submodule — localized assets and configuration must work in that packaging model
- **Persistence**: Preferred locale should persist at the user level when authenticated, with browser-based fallback before preference sync — language should feel stable across sessions and devices

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Use `aicodexml-web` as a local git submodule | Frontend code must be versioned alongside server packaging work and available locally for i18n implementation | Implemented in v1.0 packaging flow |
| Prioritize Simplified Chinese and English only | Matches the stated business need and keeps the first multilingual rollout bounded | Implemented in v1.0 scope |
| Treat "all current user-visible text" as v1 target | User explicitly chose full UI coverage rather than a shell-only or core-flow-only rollout | Implemented in v1.0 release scope |
| Persist locale through user preference with browser fallback | Gives stable cross-session behavior for signed-in users without blocking pre-login rendering | Implemented in Phase 2 |
| Keep runtime i18n on `ngx-translate` JSON catalogs with a centralized `LocaleService` | Fits the existing Angular app without requiring a compile-time extraction migration and keeps language switching dynamic | Implemented in Phase 1 |
| Avoid eager locale-service resolution in store/bootstrap providers | `UserPreferences` is instantiated during store meta-reducer setup; eager locale/config DI can loop back into Store and break startup | `UserPreferences` now resolves `LocaleService` lazily and `LOCALE_ID` bootstrap reads persisted local state without DI |
| Enforce future multilingual hygiene with a baseline-backed local guardrail | The repo still contains historical hard-coded strings, so guardrails must catch new regressions without blocking the team on a one-shot cleanup | Implemented in Phase 5 through `npm run i18n:guardrail` and a checked-in baseline |

---
*Last updated: 2026-03-16 after v1.0 milestone archival*
