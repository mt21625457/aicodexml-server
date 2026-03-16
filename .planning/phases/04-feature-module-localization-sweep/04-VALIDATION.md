---
phase: 4
slug: feature-module-localization-sweep
status: draft
nyquist_compliant: true
wave_0_complete: false
created: 2026-03-13
---

# Phase 4 - Validation Strategy

> Per-phase validation contract for feedback sampling during execution.

---

## Test Infrastructure

| Property | Value |
|----------|-------|
| **Framework** | Angular CLI build with existing Karma infrastructure |
| **Config file** | `aicodexml-web/angular.json`, `aicodexml-web/karma.conf.js` |
| **Quick run command** | `cd aicodexml-web && npm run build` |
| **Supplemental checks** | `rg -n "MANAGE WORKERS AND QUEUES|RECENT PROJECTS|NEW DATASET|NO DATASETS TO SHOW|NEW PIPELINE|NO PIPELINES TO SHOW"` in touched files |
| **Estimated runtime** | ~120 seconds |

---

## Sampling Rate

- **After each substantial localization slice:** run focused `rg` checks for remaining hard-coded Phase 4 strings in touched files
- **After every plan completion:** run `cd aicodexml-web && npm run build`
- **Before Phase verification:** confirm touched feature surfaces no longer depend on the known English literals targeted by the plan
- **Max feedback latency:** 120 seconds

---

## Per-Task Verification Map

| Task ID | Plan | Wave | Requirement | Test Type | Automated Command | File Exists | Status |
|---------|------|------|-------------|-----------|-------------------|-------------|--------|
| 4-01-01 | 01 | 1 | FEAT-01 | build/static | `cd aicodexml-web && npm run build` | ❌ W0 | ⬜ pending |
| 4-01-02 | 01 | 1 | FEAT-01 | static/manual | `cd aicodexml-web && npm run build` | ❌ W0 | ⬜ pending |
| 4-02-01 | 02 | 2 | FEAT-01 | build/static | `cd aicodexml-web && npm run build` | ❌ W0 | ⬜ pending |
| 4-02-02 | 02 | 2 | FEAT-01 | static/manual | `cd aicodexml-web && npm run build` | ❌ W0 | ⬜ pending |
| 4-03-01 | 03 | 3 | FEAT-01 | build/static | `cd aicodexml-web && npm run build` | ❌ W0 | ⬜ pending |
| 4-03-02 | 03 | 3 | FEAT-01 | static/manual | `cd aicodexml-web && npm run build` | ❌ W0 | ⬜ pending |

*Status: ⬜ pending · ✅ green · ❌ red · ⚠️ flaky*

---

## Wave 0 Requirements

- [ ] Existing infrastructure covers Angular build verification
- [ ] Add explicit static checks for known hard-coded feature literals after each plan
- [ ] Capture at least one bilingual spot-check note per completed plan, even if a live browser run remains manual follow-up

---

## Manual-Only Verifications

| Behavior | Requirement | Why Manual | Test Instructions |
|----------|-------------|------------|-------------------|
| Dashboard and project landing surfaces switch cleanly | FEAT-01 | Requires visual confirmation of headers, cards, and CTAs | Switch between `en` and `zh-CN`, then inspect dashboard sections and project list sorting/actions |
| Dataset and pipeline empty states and toggles switch cleanly | FEAT-01 | Requires rendering multiple feature layouts | Visit datasets and pipelines in both locales and confirm toggles, create buttons, empty states, and counters follow the active language |
| Later experiment/model/report and serving/settings slices remain free of targeted English | FEAT-01 | Requires feature navigation after each plan | Open the touched routes for the active plan and confirm the targeted literals no longer appear |

---

## Validation Sign-Off

- [x] All tasks have automated build verification
- [x] Sampling continuity preserved
- [x] No watch-mode commands
- [x] Feedback latency < 120s
- [x] `nyquist_compliant: true` set in frontmatter

**Approval:** pending
