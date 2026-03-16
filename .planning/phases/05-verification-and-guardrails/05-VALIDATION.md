---
phase: 5
slug: verification-and-guardrails
status: draft
nyquist_compliant: true
wave_0_complete: false
created: 2026-03-13
---

# Phase 5 - Validation Strategy

> Per-phase validation contract for verification and regression prevention work.

---

## Test Infrastructure

| Property | Value |
|----------|-------|
| **Framework** | Angular CLI build, optional local dev server, targeted static scans |
| **Config file** | `aicodexml-web/angular.json`, `aicodexml-web/proxy.config.mjs` |
| **Quick run command** | `cd aicodexml-web && npm run build` |
| **Runtime command** | `cd aicodexml-web && npm run start` |
| **Supplemental checks** | `rg` scans for known English literals and future hard-coded UI strings |
| **Estimated runtime** | ~2 min for build, longer if local browser/runtime verification is available |

---

## Sampling Rate

- **After each verification or guardrail change:** run focused `rg` checks in touched files
- **After every plan completion:** run `cd aicodexml-web && npm run build`
- **Before Phase 5 sign-off:** confirm either:
  - bilingual browser/runtime evidence exists for the targeted workflows, or
  - the exact runtime blocker is documented
- **Max feedback latency:** 120 seconds for automated checks

---

## Per-Task Verification Map

| Task ID | Plan | Wave | Requirement | Test Type | Automated Command | File Exists | Status |
|---------|------|------|-------------|-----------|-------------------|-------------|--------|
| 5-01-01 | 01 | 4 | QA-01 | build/static | `cd aicodexml-web && npm run build` | ❌ W0 | ⬜ pending |
| 5-01-02 | 01 | 4 | QA-01 | static/manual | `rg` bilingual spot-check scans + runtime notes | ❌ W0 | ⬜ pending |
| 5-02-01 | 02 | 5 | QA-01 | build/static | `cd aicodexml-web && npm run build` | ❌ W0 | ⬜ pending |
| 5-02-02 | 02 | 5 | QA-01 | static | guardrail script dry-run against `src/app` | ❌ W0 | ⬜ pending |

*Status: ⬜ pending · ✅ green · ❌ red · ⚠️ flaky*

---

## Wave 0 Requirements

- [ ] Existing build verification is confirmed for the final multilingual slice
- [ ] Verification matrix and blocker-reporting format are defined before runtime claims are made
- [ ] Guardrail approach is specific enough to run locally without custom infrastructure

---

## Manual-Only Verifications

| Behavior | Requirement | Why Manual | Test Instructions |
|----------|-------------|------------|-------------------|
| Language switcher updates visible UI and persists across refresh/login | QA-01 | Requires real application state and browser interaction | Use the app header locale control, switch between `en` and `zh-CN`, refresh, and confirm the same locale persists |
| Critical authenticated feature routes remain bilingual end-to-end | QA-01 | Requires a reachable API and authenticated navigation | Visit dashboard, projects, experiments/models/reports, serving, workers/queues, and settings in both locales and note any mixed-language blockers |
| Login/auth entry remains bilingual | QA-01 | Requires rendered route verification | Open `/login` in both locales and inspect all user-visible copy and actions |

---

## Validation Sign-Off

- [x] All tasks have automated build verification
- [x] Sampling continuity preserved
- [x] No watch-mode commands required for automated sign-off
- [x] Feedback latency < 120s for automated checks
- [x] `nyquist_compliant: true` set in frontmatter

**Approval:** pending
