---
phase: 05-verification-and-guardrails
plan: 02
subsystem: qa
tags: [i18n, guardrail, docs, tooling]
requires:
  - phase: 05-01
    provides: Verified bilingual runtime and concrete knowledge of the remaining hard-coded UI risk
provides:
  - A repeatable local guardrail for new hard-coded UI strings under `src/app`
  - Contributor-facing multilingual development rules for templates and runtime-generated copy
  - A baseline mechanism that blocks newly introduced findings without forcing a full legacy cleanup first
affects: [phase-5, verification, tooling, docs]
tech-stack:
  added: [node-script, npm-script]
  patterns: [baseline-backed guardrail, translation-pipe guidance, translate-service runtime guidance]
key-files:
  created:
    - .planning/phases/05-verification-and-guardrails/05-02-SUMMARY.md
    - aicodexml-web/scripts/i18n-guardrail.mjs
    - aicodexml-web/scripts/i18n-guardrail-baseline.json
  modified:
    - aicodexml-web/package.json
    - aicodexml-web/README.md
key-decisions:
  - "The guardrail should be baseline-backed so it fails only on newly introduced hard-coded UI strings, not on the entire historical repository at once."
  - "Contributor guidance should explicitly separate template translation (`TranslatePipe`) from runtime-generated copy (`TranslateService.instant(...)`) to preserve the architecture chosen earlier in the rollout."
patterns-established:
  - "Run `npm run i18n:guardrail` before merging UI changes that touch templates, dialogs, menus, tables, or runtime labels/messages."
  - "Update the guardrail baseline only for intentional safe exceptions or when deliberately accepting the repository-wide finding set."
requirements-completed: [QA-01]
duration: 35min
completed: 2026-03-14
---

# Phase 5: Verification And Guardrails 05-02 Summary

**05-02 added a repo-native localization guardrail and contributor rules so future UI work is less likely to regress into hard-coded or mixed-language copy.**

## Accomplishments

- Added `aicodexml-web/scripts/i18n-guardrail.mjs`, a lightweight scan that inspects likely user-visible strings in `src/app/**/*.{ts,html}`.
- Added baseline-backed enforcement through `aicodexml-web/scripts/i18n-guardrail-baseline.json`, allowing the repo to block new findings without requiring an immediate full cleanup of all historical hard-coded strings.
- Wired the guardrail into npm as `npm run i18n:guardrail` and `npm run i18n:guardrail:update-baseline`.
- Documented contributor rules in `aicodexml-web/README.md` for when to use `TranslatePipe`, when to use `TranslateService.instant(...)`, and when baseline updates are appropriate.

## Verification

- `cd aicodexml-web && npm run i18n:guardrail` passed on 2026-03-14 after generating the initial baseline.
- `cd aicodexml-web && npm run build` passed on 2026-03-14 after adding the guardrail and documentation.
- Existing `ngx-markdown-editor` side-effect and stylesheet budget warnings remained unchanged and were not introduced by this work.

## Task Commits

No commit was created in this execution slice. Changes remain in the working tree.

## Issues Encountered

- The initial repository-wide baseline is intentionally large because the guardrail is designed to catch regressions incrementally rather than block all legacy findings in one phase.

## Outcome

- Phase 5 is now complete: the multilingual rollout has both verification evidence and a local guardrail against future hard-coded UI regressions.

---
*Phase: 05-verification-and-guardrails*
*Completed: 2026-03-14*
