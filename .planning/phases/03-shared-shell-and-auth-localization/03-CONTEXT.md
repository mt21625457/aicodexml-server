# Phase 3: Shared Shell And Auth Localization - Context

**Gathered:** 2026-03-12
**Status:** Ready for planning
**Source:** Derived from Phase 2 outputs, current frontend code inspection, and the user requirement to prioritize English and Simplified Chinese

<domain>
## Phase Boundary

Phase 3 localizes the highest-leverage shared UI copy so users stop encountering mixed-language chrome in the most common flows. The work centers on shared shell navigation, settings entry points, authentication screens, and reusable dialogs and messages that appear across multiple modules.

This phase does not attempt full route-level feature localization. It focuses on common product chrome and reusable components that many later feature pages already depend on.

</domain>

<decisions>
## Implementation Decisions

### Locked Decisions
- Keep the runtime translation architecture established in Phase 1 and the locale persistence/formatting behavior delivered in Phase 2.
- Support only `en` and `zh-CN` in v1.
- Work inside the local `aicodexml-web` submodule on branch `dev`.
- Prioritize shared shell, auth, and common reusable UI surfaces before broad feature-module translation.

### Claude's Discretion
- Which shared shell surfaces count as the minimum high-leverage baseline for `SHELL-01`.
- Whether reusable components should translate via template pipes, `TranslateService`, or a mixed approach.
- How breadcrumb localization should distinguish between translation keys and user-generated names.

</decisions>

<specifics>
## Specific Ideas

- `src/app/layout/side-nav/side-nav.component.html` still contains hard-coded navigation tooltips.
- `src/app/features/settings/settings.component.html` and `settings-routing.module.ts` still hard-code settings shell labels and breadcrumb names.
- `src/app/webapp-common/login/login/login.component.{ts,html}` still hard-code the main login copy, button captions, page title, and invite title.
- `src/app/webapp-common/shared/services/login.service.ts` still hard-codes the server-unavailable dialog copy.
- Several shared reusable surfaces still contain hard-coded copy:
  - breadcrumbs share/archive UI
  - update dialog
  - appearance dialog
  - profile preferences toggles and tooltips
  - grouped filter/search empty states
  - confirm dialogs, JSON editor validation, copy-to-clipboard toast, share dialog text, and credential-label dialog fields
- Existing catalogs in `src/assets/i18n/en.json` and `src/assets/i18n/zh-CN.json` are still minimal, so Phase 3 must expand the translation key taxonomy substantially.

</specifics>

<deferred>
## Deferred Ideas

- Full localization of dashboards, projects, experiments, reports, serving, and other route-level feature pages remains Phase 4 work.
- Guardrails, coverage checks, and broad bilingual regression verification remain Phase 5 work.
- Additional languages beyond English and Simplified Chinese remain out of scope for v1.

</deferred>

---

*Phase: 03-shared-shell-and-auth-localization*
*Context gathered: 2026-03-12 via Phase 3 planning*
