# Phase 4: Feature Module Localization Sweep - Context

**Gathered:** 2026-03-13
**Status:** Ready for planning
**Source:** Derived from Phase 3 outputs, current frontend code inspection, and the user requirement to prioritize English and Simplified Chinese

<domain>
## Phase Boundary

Phase 4 localizes route-level product surfaces that still expose hard-coded English after the shared shell work in Phase 3. The focus is on feature modules and page-specific product copy across dashboard, projects, datasets, pipelines, experiments, models, reports, serving, workers/queues, and remaining settings/admin flows.

This phase does not introduce new locales, redesign the UI, or add broad automation guardrails. It completes the user-visible bilingual sweep needed for `FEAT-01`.

</domain>

<decisions>
## Implementation Decisions

### Locked Decisions
- Keep the runtime translation architecture established in Phase 1 and the locale persistence/formatting behavior delivered in Phase 2.
- Support only `en` and `zh-CN` in v1.
- Work inside the local `aicodexml-web` submodule on branch `dev`.
- Reuse the Phase 3 breadcrumb `translate` flag for static feature breadcrumb labels.
- Prefer `TranslatePipe` for stable template copy and `TranslateService.instant(...)` for runtime-generated labels in TypeScript.

### Claude's Discretion
- How to group Phase 4 catalog namespaces so they stay readable as coverage expands.
- Which remaining English strings should be fixed now versus documented as tracked follow-up exceptions.
- Whether a targeted slice can be completed safely in one pass or should stop after the first verified plan.

</decisions>

<specifics>
## Specific Ideas

- `04-01` should cover dashboard, projects, datasets, and pipelines because these pages still contain obvious hard-coded English headers, empty states, view toggles, and breadcrumb labels.
- `04-02` should cover experiment/task/model/report surfaces because they contain many labels split between templates and TypeScript-generated config.
- `04-03` should cover serving, workers/queues, storage credentials, and remaining settings/admin pages with route-level product copy still left in English.
- Existing catalogs in `src/assets/i18n/en.json` and `src/assets/i18n/zh-CN.json` should add structured Phase 4 namespaces such as:
  - `dashboard.*`
  - `projects.*`
  - `datasets.*`
  - `pipelines.*`
  - `reports.*`
  - `models.*`
  - `experiments.*`
  - `serving.*`
  - `workers.*`
  - `settingsAdmin.*`

</specifics>

<deferred>
## Deferred Ideas

- Broad bilingual regression walkthroughs and localization guardrails remain Phase 5 work.
- Additional languages beyond English and Simplified Chinese remain out of scope for v1.
- Server-originated message localization remains out of scope unless directly needed for clean feature UX.

</deferred>

---

*Phase: 04-feature-module-localization-sweep*
*Context gathered: 2026-03-13 via Phase 4 planning*
