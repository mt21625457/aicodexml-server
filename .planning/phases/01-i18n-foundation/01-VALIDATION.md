---
phase: 1
slug: i18n-foundation
status: draft
nyquist_compliant: true
wave_0_complete: false
created: 2026-03-12
---

# Phase 1 — Validation Strategy

> Per-phase validation contract for feedback sampling during execution.

---

## Test Infrastructure

| Property | Value |
|----------|-------|
| **Framework** | Angular CLI build + lint, Karma test infrastructure present |
| **Config file** | `aicodexml-web/angular.json`, `aicodexml-web/karma.conf.js` |
| **Quick run command** | `cd aicodexml-web && npm run lint` |
| **Full suite command** | `cd aicodexml-web && npm run build` |
| **Estimated runtime** | ~120 seconds |

---

## Sampling Rate

- **After every task commit:** Run `cd aicodexml-web && npm run lint`
- **After every plan wave:** Run `cd aicodexml-web && npm run build`
- **Before `$gsd-verify-work`:** Full suite must be green
- **Max feedback latency:** 120 seconds

---

## Per-Task Verification Map

| Task ID | Plan | Wave | Requirement | Test Type | Automated Command | File Exists | Status |
|---------|------|------|-------------|-----------|-------------------|-------------|--------|
| 1-01-01 | 01 | 1 | I18N-01 | build/lint | `cd aicodexml-web && npm run lint && npm run build` | ❌ W0 | ⬜ pending |
| 1-01-02 | 01 | 1 | I18N-03 | build/manual | `cd aicodexml-web && npm run build` | ❌ W0 | ⬜ pending |
| 1-01-03 | 01 | 1 | I18N-03 | manual | `cd aicodexml-web && npm run build` | ❌ W0 | ⬜ pending |
| 1-02-01 | 02 | 2 | I18N-02 | build/manual | `cd aicodexml-web && npm run build` | ❌ W0 | ⬜ pending |
| 1-02-02 | 02 | 2 | I18N-02 | build/manual | `cd aicodexml-web && npm run build` | ❌ W0 | ⬜ pending |
| 1-03-01 | 03 | 2 | BUILD-01 | build | `cd aicodexml-web && npm run build` | ❌ W0 | ⬜ pending |
| 1-03-02 | 03 | 2 | BUILD-01 | build/manual | `cd aicodexml-web && npm run build` | ❌ W0 | ⬜ pending |

*Status: ⬜ pending · ✅ green · ❌ red · ⚠️ flaky*

---

## Wave 0 Requirements

- [ ] Existing infrastructure covers lint and build, but no dedicated i18n smoke checks exist yet
- [ ] Add at least one lightweight verification path for translation asset presence and language-switch smoke behavior during execution summaries

---

## Manual-Only Verifications

| Behavior | Requirement | Why Manual | Test Instructions |
|----------|-------------|------------|-------------------|
| Header or first-entry switcher visibly changes shell language | I18N-02 | Visual state change is hard to assert from existing test setup | Run app locally, switch between `en` and `zh-CN`, verify visible shell text changes |
| Missing-key fallback does not leak raw keys in core flow | I18N-03 | Best validated with a live UI sanity pass | Trigger a route that uses shared shell text and confirm fallback labels render safely |

---

## Validation Sign-Off

- [x] All tasks have `<automated>` verify or Wave 0 dependencies
- [x] Sampling continuity: no 3 consecutive tasks without automated verify
- [x] Wave 0 covers all MISSING references
- [x] No watch-mode flags
- [x] Feedback latency < 120s
- [x] `nyquist_compliant: true` set in frontmatter

**Approval:** pending
