# AI Code XML Web Multilingual

## What This Is

This project adds full bilingual support to the existing AI Code XML web application, prioritizing Simplified Chinese and English across all user-visible UI text. The web frontend lives in the local `aicodexml-web` submodule and is built into this server repository's Docker image, so the work spans frontend internationalization, runtime configuration, and preference persistence integration.

The goal is not to build a new web app. It is to retrofit a mature Angular-based ClearML-derived UI so Chinese and English users can reliably use the same product without language friction.

## Core Value

Users can switch between Chinese and English and consistently see the entire web UI in their chosen language without breaking existing workflows.

## Requirements

### Validated

- ✓ Existing ClearML-derived web UI is already integrated into this repository via the `aicodexml-web` submodule and packaged by `docker/build/Dockerfile` — existing
- ✓ Existing backend and web runtime configuration flow can inject web settings into the deployed UI via `configuration.json` and environment-based startup scripts — existing
- ✓ Existing application already supports authenticated user state and user preferences loading on startup, creating a viable place to persist language preference — existing

### Active

- [ ] All current user-visible web UI text supports Simplified Chinese and English
- [ ] Users can switch language intentionally from the web application without needing redeploys or manual config edits
- [ ] Logged-in users keep their language preference across sessions, with browser language and local state as fallback before account preference is known
- [ ] Shared UI components, navigation, settings, forms, dialogs, notifications, and empty states use a consistent translation mechanism instead of ad hoc string handling
- [ ] The Dockerized server/web build continues to package the localized frontend from the local `aicodexml-web` submodule

### Out of Scope

- Additional locales beyond Simplified Chinese and English — keep v1 focused on the two languages the team explicitly needs
- Translation of user-generated content, experiment names, project names, logs, or external data payloads — those are content, not product UI chrome
- Rewriting unrelated frontend architecture or redesigning the product UI — this initiative is localization-first, not a general frontend overhaul
- Backend API message localization for every server response — only support server-side changes required to persist and expose locale preference cleanly

## Context

This is a brownfield project on top of an existing ClearML Server fork. The server repository contains Python API and file services plus Docker build/deployment logic. The actual frontend source now lives locally in the `aicodexml-web` git submodule on the `dev` branch, and the server build consumes it directly instead of cloning a remote repo during image build.

The frontend is an Angular 20 application with a large existing feature surface. The codebase already uses `LOCALE_ID` in a few places and Angular build tooling exposes `extract-i18n`, but there is no established end-to-end product i18n system or language switching flow yet. Because the user selected "all UI" scope, this effort needs inventorying and migration of many existing hard-coded strings, not just a small shell-level translation pass.

The app already loads user and preference data during startup. That makes user preference persistence the preferred source of truth after login, while browser language or local cache can provide a pre-login and first-load fallback.

## Constraints

- **Tech stack**: Must fit the existing Angular 20 frontend and current server/runtime packaging flow — avoid introducing a localization approach that fights the current build system
- **Compatibility**: Must preserve existing login, navigation, dashboards, settings, and feature behavior while replacing visible strings — regression risk is high in a mature UI
- **Scope**: v1 focuses on Simplified Chinese and English only — broad locale expansion would delay delivery and increase translation maintenance cost
- **Deployment**: The frontend is built through this repository's Docker pipeline from the `aicodexml-web` submodule — localized assets and configuration must work in that packaging model
- **Persistence**: Preferred locale should persist at the user level when authenticated, with browser-based fallback before preference sync — language should feel stable across sessions and devices

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Use `aicodexml-web` as a local git submodule | Frontend code must be versioned alongside server packaging work and available locally for i18n implementation | — Pending |
| Prioritize Simplified Chinese and English only | Matches the stated business need and keeps the first multilingual rollout bounded | — Pending |
| Treat "all current user-visible text" as v1 target | User explicitly chose full UI coverage rather than a shell-only or core-flow-only rollout | — Pending |
| Persist locale through user preference with browser fallback | Gives stable cross-session behavior for signed-in users without blocking pre-login rendering | — Pending |

---
*Last updated: 2026-03-12 after initialization*
