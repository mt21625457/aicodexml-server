---
phase: 05-verification-and-guardrails
plan: 01
subsystem: qa
tags: [i18n, qa, login, runtime, regression]
requires:
  - phase: 04-03
    provides: Bilingual feature-module coverage across serving, workers/queues, and settings-admin
provides:
  - Automated bilingual regression evidence from build and targeted literal scans
  - Browser verification evidence for the reachable login flow in `en` and `zh-CN`
  - Documented blocker scope for authenticated-route verification without a local signed-in session
affects: [phase-5, verification, login, runtime, i18n]
tech-stack:
  added: []
  patterns: [targeted literal regression scans, browser locale spot checks, lazy service resolution during store bootstrap]
key-files:
  created:
    - .planning/phases/05-verification-and-guardrails/05-01-SUMMARY.md
  modified:
    - aicodexml-web/src/app/app.module.ts
    - aicodexml-web/src/app/webapp-common/user-preferences.ts
    - aicodexml-web/src/app/webapp-common/models/dumbs/model-header/model-header.component.html
    - aicodexml-web/src/app/webapp-common/experiments/dumb/experiment-header/experiment-header.component.ts
    - aicodexml-web/src/app/webapp-common/shared/queue-create-dialog/queue-create-dialog.component.html
    - aicodexml-web/src/assets/i18n/en.json
    - aicodexml-web/src/assets/i18n/zh-CN.json
key-decisions:
  - "UserPreferences must resolve LocaleService lazily because the service is instantiated as part of store meta-reducer setup; eager LocaleService injection causes a bootstrap DI loop back into Store."
  - "The app-level LOCALE_ID provider should derive its initial value from persisted local state instead of injecting LocaleService during provider creation."
patterns-established:
  - "Phase 5 verification should combine production build, targeted literal scans, and browser reload checks in both `en` and `zh-CN`."
  - "When authenticated workflows are not reachable locally, the exact blocker must be recorded explicitly rather than inferred away."
requirements-completed: []
duration: 50min
completed: 2026-03-13
---

# Phase 5: Verification And Guardrails 05-01 Summary

**05-01 completed the bilingual regression verification pass for the locally reachable login flow, closed the remaining low-risk mixed-language misses, and removed a startup DI cycle that had been blocking browser QA.**

## Accomplishments

- Replaced the remaining tracked literal misses in the model header, experiment header, and queue-create dialog with translation keys and catalog entries.
- Re-ran the production build and targeted literal scans; the tracked route-level English leftovers from the 05-01 scan no longer remain in application code.
- Diagnosed the browser blocker `NG0200: Circular dependency detected for ReducerObservable` and fixed it by removing eager locale-service resolution from bootstrap-sensitive paths.
- Verified the reachable `/login` flow in both `en` and `zh-CN` by toggling the persisted app language and reloading the page locally.

## Verification

- `cd aicodexml-web && npm run build` passed on 2026-03-13 after the final verification fixes. Existing `ngx-markdown-editor` side-effect and stylesheet budget warnings remained unchanged.
- Targeted literal scan for `NEW QUEUE`, `UPDATE QUEUE`, `Table view`, `Details view`, and `Compare view` no longer found the previously identified application-code regressions.
- Browser verification at `http://127.0.0.1:4200/login` succeeded after the runtime fix:
  - `zh-CN`: login title rendered as `ClearML - 登录` with localized labels such as `姓名` and `开始`
  - `en`: login title rendered as `ClearML - Login` with localized labels such as `Full Name` and `Start`
- The proxied API target at `http://localhost:8008` remained reachable during this pass, including a successful `login.supported_modes` response.

## Task Commits

No commit was created in this execution slice. Changes remain in the working tree.

## Issues Encountered

- Browser QA was initially blocked by a startup dependency cycle introduced by eager locale-service injection during store bootstrap. The cycle is now resolved.
- Authenticated route verification is still limited by environment, not by the i18n implementation: no local signed-in session or dedicated test credentials were available for verifying post-login feature flows in this pass.

## Next Step

- Execute `05-02` to add maintainability guardrails that keep future UI text inside the translation system and reduce the chance of mixed-language regressions returning.

---
*Phase: 05-verification-and-guardrails*
*Completed: 2026-03-13*
