---
phase: 2
slug: locale-persistence-and-formatting
status: draft
nyquist_compliant: true
wave_0_complete: false
created: 2026-03-12
---

# Phase 2 - Validation Strategy

> Per-phase validation contract for feedback sampling during execution.

---

## Test Infrastructure

| Property | Value |
|----------|-------|
| **Framework** | Angular CLI build with existing Karma infrastructure |
| **Config file** | `aicodexml-web/angular.json`, `aicodexml-web/karma.conf.js` |
| **Quick run command** | `cd aicodexml-web && npm run build` |
| **Supplemental checks** | `rg -n "en-US|enGB|MAT_DATE_LOCALE"` in touched files |
| **Estimated runtime** | ~120 seconds |

---

## Sampling Rate

- **After each substantial formatting or persistence slice:** run focused `rg` checks for static locale pins in touched files
- **After every plan completion:** run `cd aicodexml-web && npm run build`
- **Before Phase verification:** confirm no remaining Phase 2 locale pins in modified foundational files
- **Max feedback latency:** 120 seconds

---

## Per-Task Verification Map

| Task ID | Plan | Wave | Requirement | Test Type | Automated Command | File Exists | Status |
|---------|------|------|-------------|-----------|-------------------|-------------|--------|
| 2-01-01 | 01 | 1 | PREF-01 | build/static | `cd aicodexml-web && npm run build` | ❌ W0 | ⬜ pending |
| 2-01-02 | 01 | 1 | PREF-02 | build/manual | `cd aicodexml-web && npm run build` | ❌ W0 | ⬜ pending |
| 2-02-01 | 02 | 1 | FMT-01 | build/static | `cd aicodexml-web && npm run build` | ❌ W0 | ⬜ pending |
| 2-02-02 | 02 | 1 | FMT-02 | build/static | `cd aicodexml-web && npm run build` | ❌ W0 | ⬜ pending |
| 2-03-01 | 03 | 2 | PREF-01 | build/manual | `cd aicodexml-web && npm run build` | ❌ W0 | ⬜ pending |
| 2-03-02 | 03 | 2 | FMT-01, FMT-02 | build/manual | `cd aicodexml-web && npm run build` | ❌ W0 | ⬜ pending |

*Status: ⬜ pending · ✅ green · ❌ red · ⚠️ flaky*

---

## Wave 0 Requirements

- [ ] Existing infrastructure covers Angular build verification
- [ ] Add explicit static checks for remaining hard-coded Phase 2 locale pins in touched files
- [ ] Capture at least one authenticated persistence flow in the execution summary, even if live UI login remains a manual follow-up

---

## Manual-Only Verifications

| Behavior | Requirement | Why Manual | Test Instructions |
|----------|-------------|------------|-------------------|
| Saved language wins after later login | PREF-01 | Requires authenticated session lifecycle | Log in, change language, reload and re-login, confirm the chosen language remains active |
| No saved preference reconciles cleanly after login | PREF-02 | Depends on account state | Use an account without saved locale, confirm browser/deployment-selected language remains active after login |
| Representative date and number surfaces follow active locale | FMT-01, FMT-02 | Visual comparison across languages is easiest manually | Switch between `en` and `zh-CN`, visit touched tables/cards/date pickers, confirm formatting changes accordingly |

---

## Validation Sign-Off

- [x] All tasks have automated build verification
- [x] Sampling continuity preserved
- [x] No watch-mode commands
- [x] Feedback latency < 120s
- [x] `nyquist_compliant: true` set in frontmatter

**Approval:** pending
