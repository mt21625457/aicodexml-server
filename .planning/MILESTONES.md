# Milestones

## v1.0 AI Code XML Web Multilingual (Shipped: 2026-03-16)

**Phases completed:** 5 phases, 14 plans, 26 tasks

**Key accomplishments:**
- Established one runtime bilingual architecture with `ngx-translate`, centralized locale bootstrap, and packaged translation assets through the server build.
- Persisted locale under `views.language`, reconciled server preference after login, and aligned date, number, percent, and sorting behavior with the active locale.
- Localized the shared shell, authentication flows, dialogs, toasts, placeholders, and reusable component defaults so common UX surfaces follow the selected language.
- Completed a feature-module sweep across dashboard, projects, datasets, pipelines, experiments, models, reports, serving, workers/queues, and settings/admin surfaces.
- Added bilingual regression evidence for the reachable login flow, removed the startup DI cycle that blocked browser QA, and introduced the baseline-backed `npm run i18n:guardrail`.

---
