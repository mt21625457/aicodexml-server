# Phase 1 Research: I18n Foundation

**Phase:** 1
**Name:** I18n Foundation
**Researched:** 2026-03-12
**Confidence:** HIGH

## What This Phase Needs To Prove

Phase 1 must prove that the existing Angular 20 application can run one deployed build with runtime language switching, safe fallback behavior, and packaging support from the server repository. The most important decision is architectural: choose a runtime translation layer that fits a single deployed admin app and avoid drifting into Angular’s multi-build-per-locale model.

## Recommended Approach

### 1. Use Runtime Translation For UI Copy

- Angular’s official built-in i18n flow is centered on localized build outputs and separate deployment paths per locale.
- This project needs a single deployed app where signed-in users switch languages at runtime.
- Therefore Phase 1 should integrate a runtime translation layer, load `en` and `zh-CN` catalogs, and expose a single locale service for the app shell.

### 2. Keep Formatting And Persistence Separable

- Formatting localization and server-backed persistence are important, but they do not have to block the first foundation phase.
- Phase 1 should provide the active locale state and basic browser/local fallback shape so later phases can plug in persistence and formatting without reworking the architecture.

### 3. Use Existing App Hooks

The current codebase already exposes the right anchors:

- `aicodexml-web/src/app/app.module.ts` for provider wiring
- `aicodexml-web/src/app/core/app-init.ts` for startup precedence
- `aicodexml-web/src/app/layout/header/header-user-menu-actions/` for an initial switcher surface
- `aicodexml-web/src/assets/` for shipping translation catalogs
- `docker/build/Dockerfile` and runtime web config plumbing for packaging alignment

## Implementation Notes

### Translation Infrastructure

- Add runtime translation dependencies compatible with Angular 20.
- Create a locale service that owns:
  - supported languages
  - current language
  - fallback language
  - browser/default resolution
  - document `<html lang>` updates
- Seed initial `en.json` and `zh-CN.json` catalogs with shared shell keys only in Phase 1.

### Switching Surface

- Prefer a lightweight switcher in the header user menu or a nearby preference entry point.
- It is acceptable for Phase 1 switching to use session/local storage while Phase 2 formalizes server persistence.

### Build And Packaging

- Because `src/assets` is already copied by the Angular build, translation JSON under `src/assets/i18n/` should ship without extra asset plumbing unless a different catalog location is chosen.
- Server-side runtime config can later expose `defaultLanguage` or `supportedLanguages`, but Phase 1 only needs enough structure to keep packaging compatible and ready for that extension.

## Risks

| Risk | Why It Matters | Mitigation |
|------|----------------|------------|
| Choosing the wrong i18n architecture | Would force rework in every later phase | Lock runtime translation approach in Phase 1 |
| Header switcher insertion point is too limited | Could slow visible progress | Allow temporary switcher surface if current menu container is minimal |
| Fallback behavior leaks keys | Undermines trust immediately | Make fallback a first-class deliverable, not a later enhancement |

## Validation Architecture

### Tooling

- Frontend package manager: `npm` via `aicodexml-web/package-lock.json`
- Build command: `cd aicodexml-web && npm run build`
- Lint command: `cd aicodexml-web && npm run lint`
- Primary manual check: run the app locally and confirm language switching in the shell

### Minimum Verification For This Phase

1. Build succeeds after translation infrastructure is added.
2. Two language catalogs are bundled and loadable.
3. Switching language updates visible shell text in a live session.
4. Missing keys do not surface as raw broken labels in the core shell path.
5. Docker packaging still consumes the localized frontend from `aicodexml-web`.

## Sources

- https://angular.dev/guide/i18n
- https://angular.dev/guide/i18n/merge
- https://angular.dev/guide/i18n/deploy
- https://ngx-translate.org/getting-started/installation/
- https://ngx-translate.org/getting-started/translation-files/
- Local code: `aicodexml-web/angular.json`, `aicodexml-web/src/app/app.module.ts`, `aicodexml-web/src/app/core/app-init.ts`, `docker/build/Dockerfile`

---
*Phase 1 research complete*
