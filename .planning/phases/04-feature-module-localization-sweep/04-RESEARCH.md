# Phase 4 Research: Feature Module Localization Sweep

**Phase:** 4
**Name:** Feature Module Localization Sweep
**Researched:** 2026-03-13
**Confidence:** HIGH

## What This Phase Needs To Prove

Phase 4 must prove that the selected locale now controls route-level feature pages, not only shared shell and dialog surfaces. Users should be able to move through major product areas without hitting obvious English-only headers, actions, empty states, sort labels, and feature breadcrumbs.

## Recommended Approach

### 1. Execute the phase as three bounded feature slices

- `04-01`: dashboard, projects, datasets, pipelines
- `04-02`: experiments/tasks, compare flows, models, reports
- `04-03`: serving, workers/queues, remaining settings/admin feature pages

This split matches the roadmap and keeps each implementation pass buildable and reviewable.

### 2. Expand catalogs in lockstep with each slice

- Add only the namespaces needed for the current plan before editing templates.
- Keep keys grouped by feature area so future maintenance stays predictable.
- Reuse previously introduced shared keys where copy already exists to avoid duplicate translations.

### 3. Treat view toggles, counters, and breadcrumb labels as part of feature UX

- Static feature labels like `DATASETS`, `PIPELINES`, `RUNNING`, `FAILED`, `List view`, and `Project view` are user-visible product chrome and should resolve from catalogs.
- Breadcrumb feature entries should pass translation keys with `translate: true` instead of hard-coded names.
- Inputs such as button-toggle option arrays and computed sort headers often require TypeScript translation via `TranslateService`.

### 4. Prefer the least invasive translation path per component

- Use `TranslatePipe` in templates when the string is rendered directly.
- Use `TranslateService.instant(...)` for computed labels, array literals, dialog configs, and menu headers assembled in TypeScript.
- Keep entity names, project names, and user-generated content literal.

## Implementation Notes

### 04-01 Targets

- Dashboard: recent section titles, CTA buttons, and dashboard entry labels.
- Projects: create button, sort menu labels, and computed sort header.
- Datasets: create buttons, empty states, docs/help copy, tab labels, view toggles, and feature breadcrumb labels.
- Pipelines: create buttons, empty states, docs/help copy, status counters, view toggles, and feature breadcrumb labels.

### 04-02 Targets

- Experiment details, compare flows, clone/queue dialogs, model detail panels, report list/detail/dialog flows.
- Expect more TypeScript-side translation because table actions and dialogs build labels programmatically.

### 04-03 Targets

- Serving tables and empty states, workers/queues detail panels and queue actions, storage credential flows, remaining admin/settings copy.
- Expect some overlap with Phase 3 shared settings keys, but add route-specific namespaces for admin feature pages instead of overloading shared groups.

## Risks

| Risk | Why It Matters | Mitigation |
|------|----------------|------------|
| Feature pages use many standalone components without `TranslatePipe` imported | Template changes will fail to compile if imports are missed | Add `TranslatePipe` explicitly in every touched standalone component |
| TypeScript arrays and computed labels remain English | Mixed-language UI persists even after template migration | Use `TranslateService.instant(...)` for option arrays, menu headers, and runtime strings |
| Breadcrumb translation leaks into entity names | Dynamic project names may render raw keys or be altered incorrectly | Only mark static feature breadcrumbs with `translate: true` |
| Large scope creates noisy catalog churn | Later plans become hard to review and maintain | Keep catalog additions grouped per feature slice and only add keys needed by the plan |

## Validation Architecture

### Tooling

- Primary build command: `cd aicodexml-web && npm run build`
- Supplemental static validation:
  - `rg` checks for known Phase 4 literals in touched files after each plan
  - catalog sanity checks in `src/assets/i18n/en.json` and `src/assets/i18n/zh-CN.json`
- Primary manual check:
  - switch language between English and Simplified Chinese and inspect touched feature pages

### Minimum Verification For This Phase

1. Each completed Phase 4 plan builds successfully.
2. Touched feature pages no longer depend on the targeted hard-coded English literals.
3. Static feature breadcrumbs, view toggles, empty states, and page actions follow the active locale.
4. Any remaining English in touched areas is documented as a tracked exception rather than left implicit.

## Sources

- Local code: `aicodexml-web/src/app/features/dashboard/dashboard.component.{ts,html}`
- Local code: `aicodexml-web/src/app/webapp-common/projects/containers/projects-page/projects-page.component.{ts,html}`
- Local code: `aicodexml-web/src/app/webapp-common/projects/dumb/projects-header/projects-header.component.{ts,html}`
- Local code: `aicodexml-web/src/app/features/datasets/nested-datasets-page/nested-datasets-page.component.{ts,html}`
- Local code: `aicodexml-web/src/app/webapp-common/datasets/open-datasets/open-datasets.component.{ts,html}`
- Local code: `aicodexml-web/src/app/webapp-common/datasets/dataset-empty/dataset-empty.component.{ts,html}`
- Local code: `aicodexml-web/src/app/webapp-common/pipelines/pipelines-page/pipelines-page.component.{ts,html}`
- Local code: `aicodexml-web/src/app/webapp-common/pipelines/nested-pipeline-page/nested-pipeline-page.component.{ts,html}`
- Local code: `aicodexml-web/src/app/webapp-common/pipelines/pipeline-card/pipeline-card.component.{ts,html}`
- Local code: `aicodexml-web/src/assets/i18n/en.json`
- Local code: `aicodexml-web/src/assets/i18n/zh-CN.json`

---
*Phase 4 research complete*
