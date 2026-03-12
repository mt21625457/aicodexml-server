# Phase 1: I18n Foundation - Context

**Gathered:** 2026-03-12
**Status:** Ready for planning
**Source:** Derived from PROJECT.md, research artifacts, and user scope decisions during initialization

<domain>
## Phase Boundary

Phase 1 establishes the runtime multilingual foundation for the existing Angular web app. It must choose and wire one i18n architecture, support in-app switching between English and Simplified Chinese, provide safe fallback behavior for missing translations, and keep Docker packaging aligned with the local `aicodexml-web` submodule.

This phase does not attempt full feature-page translation or durable cross-session server-backed locale persistence yet. It creates the infrastructure that later phases will build on.

</domain>

<decisions>
## Implementation Decisions

### Locked Decisions
- Use a single deployed frontend build with runtime language switching.
- Support only `en` and `zh-CN` in v1.
- Keep Angular 20 as-is; do not combine i18n work with framework migration.
- Use the local `aicodexml-web` submodule as the frontend source of truth.
- Keep Docker/server packaging working from this repository.
- Prefer browser/local fallback before authenticated preference persistence is implemented in Phase 2.

### Claude's Discretion
- Exact service/file naming for locale infrastructure inside the Angular app.
- Exact location of the first language switcher entry point if the header menu needs light UI scaffolding.
- Whether to add deployment-facing locale config now or stub it behind existing runtime configuration plumbing.

</decisions>

<specifics>
## Specific Ideas

- Existing integration points already exist in `aicodexml-web/src/app/core/app-init.ts`, `aicodexml-web/src/app/webapp-common/user-preferences.ts`, and `apiserver/services/users.py`.
- `aicodexml-web/angular.json` already includes `extract-i18n`, but Angular build-time localized variants are not the primary strategy for this project.
- `aicodexml-web/src/assets` already ships static assets, making `src/assets/i18n/` a natural initial catalog location.
- `aicodexml-web/src/app/layout/header/header-user-menu-actions/` is a likely first hook for the language switcher.

</specifics>

<deferred>
## Deferred Ideas

- Server-backed locale persistence and reconciliation details move to Phase 2.
- Full shared UI and feature-module translation move to Phases 3 and 4.
- Additional locales, CI guardrails, and deeper localization operations move to later phases.

</deferred>

---

*Phase: 01-i18n-foundation*
*Context gathered: 2026-03-12 via initialization*
