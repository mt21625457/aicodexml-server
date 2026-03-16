# Phase 5: Verification And Guardrails - Context

**Gathered:** 2026-03-13
**Status:** Ready for planning
**Source:** Derived from completed Phases 1-4, current frontend scripts/config, and the remaining requirement to verify bilingual behavior and prevent regression

<domain>
## Phase Boundary

Phase 5 validates that the English and Simplified Chinese rollout is usable across critical workflows, then adds lightweight guardrails so future UI changes stay inside the translation system.

This phase does not add new languages, redesign existing screens, or attempt a broad backend localization project. It closes `QA-01` and the maintainability portion of the multilingual rollout.

</domain>

<decisions>
## Implementation Decisions

### Locked Decisions
- Keep the runtime translation architecture, locale persistence, and formatting behavior delivered in Phases 1-4.
- Support only `en` and `zh-CN` in v1.
- Work inside the local `aicodexml-web` submodule on branch `dev`.
- Treat production build success plus targeted static scans as the minimum automated verification baseline.
- Treat live browser walkthroughs as the preferred evidence for bilingual route-level UX whenever a working proxied API/runtime is available.

### Claude's Discretion
- How much of `05-01` can be completed from local build/static/browser tooling alone versus documented as blocked on a live authenticated environment.
- What form the Phase 5 guardrail should take in `05-02` as long as it is lightweight, repeatable, and fits the existing Angular workspace.
- Which residual mixed-language exceptions are safe to document versus needing immediate fixes during verification.

</decisions>

<specifics>
## Specific Ideas

- `05-01` should verify the highest-value bilingual workflows across:
  - login/auth entry
  - locale switch and persistence
  - dashboard/projects/datasets/pipelines
  - experiments/models/reports
  - serving/workers/queues
  - settings/admin
- `05-01` should combine:
  - `npm run build`
  - targeted `rg` checks for known hard-coded English literals
  - browser spot checks where a runnable local environment exists
  - explicit documentation of runtime blockers if authenticated flows cannot be exercised locally
- `05-02` should add a maintainable regression guard such as:
  - a focused hard-coded-string scan script
  - an npm script entry
  - developer guidance on `TranslatePipe` vs `TranslateService.instant(...)`
  - documented exception patterns like `data-id`, code snippets, and user-generated content

</specifics>

<deferred>
## Deferred Ideas

- Additional locales beyond English and Simplified Chinese remain out of scope for v1.
- Full localization of backend-originated error payloads remains out of scope unless a critical user-facing blocker is discovered during verification.
- Heavy CI integration beyond a lightweight repo-native guardrail remains optional follow-up work after v1 completion.

</deferred>

---

*Phase: 05-verification-and-guardrails*
*Context gathered: 2026-03-13 via Phase 5 planning*
