# Phase 5 Research: Verification And Guardrails

**Phase:** 5
**Name:** Verification And Guardrails
**Researched:** 2026-03-13
**Confidence:** HIGH

## What This Phase Needs To Prove

Phase 5 must prove two things:

1. The multilingual rollout is actually usable in English and Simplified Chinese across the highest-value workflows, not just buildable.
2. The repo now has a practical way to catch future hard-coded UI strings before they silently regress the bilingual experience.

## Recommended Approach

### 1. Split the phase into one verification plan and one guardrail plan

- `05-01`: execute bilingual regression verification across critical workflows and record evidence, blockers, and any gap items.
- `05-02`: add a lightweight automated guardrail plus contributor guidance so new UI copy follows the established translation patterns.

This keeps Phase 5 aligned with the roadmap while preserving a clean handoff between evidence gathering and prevention work.

### 2. Use the strongest available verification signal first

- Always run `cd aicodexml-web && npm run build`.
- Add targeted `rg` checks for the kinds of literals that were removed in Phase 4.
- If a runnable proxied environment is available, use browser automation or manual spot checks for route-level bilingual behavior.
- If the authenticated runtime is not available locally, record that explicitly instead of overstating verification coverage.

### 3. Keep guardrails lightweight and repo-native

- Prefer a small string-scan script plus an npm entry over a large lint-plugin investment.
- Focus the scan on user-visible Angular templates and TS config surfaces where raw copy tends to appear.
- Exclude known safe patterns such as `data-id`, telemetry strings, code snippets, and user-generated content.

## Local Constraints Discovered

### Available frontend commands

From `aicodexml-web/package.json`:

- `npm run start` -> `npx ng serve`
- `npm run build` -> production Angular build
- `npm run test` -> Karma test runner
- `npm run lint` -> Angular lint task

### Local runtime prerequisites

- `aicodexml-web/README.md` states the dev server requires a working API target configured in `proxy.config.mjs`.
- This means authenticated Phase 5 browser verification depends on a reachable API server, not just the frontend workspace alone.

### Existing multilingual architecture to validate

- `LocaleService` remains the single source of truth for locale selection and persistence.
- Language persistence lives under `views.language`.
- Templates use `TranslatePipe`; runtime-generated labels/messages increasingly use `TranslateService.instant(...)`.

## Verification Strategy Recommendation

### 05-01

- Build a bilingual verification matrix covering core routes and flows.
- Run production build and targeted static scans.
- Attempt local browser verification with the best available runtime setup.
- If authenticated verification is blocked, document the blocker and still capture evidence for all reachable surfaces.

### 05-02

- Add a hard-coded-string scan script under `aicodexml-web/scripts/` or equivalent.
- Wire it through `package.json`.
- Document localization rules for future contributors, including:
  - when to use `TranslatePipe`
  - when to use `TranslateService.instant(...)`
  - how to handle shared reusable components
  - what exceptions are allowed

## Risks

| Risk | Why It Matters | Mitigation |
|------|----------------|------------|
| No local authenticated runtime is available | Phase 5 could falsely appear complete without meaningful UX verification | Treat runtime access as an explicit verification gate and record blockers clearly |
| Static scans are too noisy | Guardrails become ignored and Phase 5 loses value | Scope scans to app templates/TS UI config and document allowed exceptions |
| Guardrail misses runtime-generated labels | Mixed-language regressions can return through TS arrays and dialog config | Include TS-side scan targets and contributor guidance for `TranslateService.instant(...)` |
| Browser-only verification is too manual | Repeatability drops after v1 | Pair browser notes with a saved verification matrix and automated baseline commands |

## Validation Architecture

### Tooling

- Primary build command: `cd aicodexml-web && npm run build`
- Runtime command: `cd aicodexml-web && npm run start` with a valid proxy target
- Static validation:
  - `rg` checks for known hard-coded English literals in touched files
  - translation-catalog sanity checks for `en.json` and `zh-CN.json`
- Browser validation:
  - local browser automation if the proxied runtime is available

### Minimum Verification For This Phase

1. The app still builds after the full multilingual sweep.
2. The highest-value flows have explicit bilingual verification evidence or a documented runtime blocker.
3. Any remaining mixed-language issues are either fixed or tracked as clear follow-up gaps.
4. The repo gains at least one repeatable guardrail against new hard-coded UI strings.

## Sources

- Local code: `aicodexml-web/package.json`
- Local docs: `aicodexml-web/README.md`
- Local code: `aicodexml-web/src/app/shared/services/locale.service.ts`
- Local code: `aicodexml-web/src/app/core/app-init.ts`
- Local code: `aicodexml-web/src/app/webapp-common/login/login/login.component.{ts,html}`
- Local docs: `.planning/ROADMAP.md`
- Local docs: `.planning/REQUIREMENTS.md`
- Local docs: `.planning/phases/04-feature-module-localization-sweep/04-03-SUMMARY.md`

---
*Phase 5 research complete*
