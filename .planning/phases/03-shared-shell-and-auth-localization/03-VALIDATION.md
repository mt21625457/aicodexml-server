---
phase: 3
slug: shared-shell-and-auth-localization
status: draft
nyquist_compliant: true
wave_0_complete: false
created: 2026-03-12
---

# Phase 3 - Validation Strategy

> Per-phase validation contract for feedback sampling during execution.

---

## Test Infrastructure

| Property | Value |
|----------|-------|
| **Framework** | Angular CLI build with existing Karma infrastructure |
| **Config file** | `aicodexml-web/angular.json`, `aicodexml-web/karma.conf.js` |
| **Quick run command** | `cd aicodexml-web && npm run build` |
| **Supplemental checks** | `rg -n "WORKERS & QUEUES|USER PREFERENCES|Invalid User/Password combination|Don't show this message again|New version available"` in touched files |
| **Estimated runtime** | ~120 seconds |

---

## Sampling Rate

- **After each substantial localization slice:** run focused `rg` checks for remaining hard-coded Phase 3 strings in touched files
- **After every plan completion:** run `cd aicodexml-web && npm run build`
- **Before Phase verification:** confirm touched shell/auth/shared surfaces no longer depend on the known English literals targeted by this phase
- **Max feedback latency:** 120 seconds

---

## Per-Task Verification Map

| Task ID | Plan | Wave | Requirement | Test Type | Automated Command | File Exists | Status |
|---------|------|------|-------------|-----------|-------------------|-------------|--------|
| 3-01-01 | 01 | 1 | SHELL-01 | build/static | `cd aicodexml-web && npm run build` | ❌ W0 | ⬜ pending |
| 3-01-02 | 01 | 1 | SHELL-01 | static/manual | `cd aicodexml-web && npm run build` | ❌ W0 | ⬜ pending |
| 3-02-01 | 02 | 1 | AUTH-01 | build/static | `cd aicodexml-web && npm run build` | ❌ W0 | ⬜ pending |
| 3-02-02 | 02 | 1 | AUTH-01 | build/manual | `cd aicodexml-web && npm run build` | ❌ W0 | ⬜ pending |
| 3-03-01 | 03 | 2 | COMM-01 | build/static | `cd aicodexml-web && npm run build` | ❌ W0 | ⬜ pending |
| 3-03-02 | 03 | 2 | COMM-01 | build/manual | `cd aicodexml-web && npm run build` | ❌ W0 | ⬜ pending |

*Status: ⬜ pending · ✅ green · ❌ red · ⚠️ flaky*

---

## Wave 0 Requirements

- [ ] Existing infrastructure covers Angular build verification
- [ ] Add explicit static checks for remaining hard-coded Phase 3 shell/auth/shared literals in touched files
- [ ] Capture at least one bilingual spot-check for login and shared shell behavior in the execution summary, even if a live browser run remains manual follow-up

---

## Manual-Only Verifications

| Behavior | Requirement | Why Manual | Test Instructions |
|----------|-------------|------------|-------------------|
| Shared shell labels change with active language | SHELL-01 | Visual shell navigation and breadcrumb inspection is easiest manually | Switch between `en` and `zh-CN`, then inspect side nav tooltips, settings drawer labels, breadcrumb share UI, and user-preferences shell |
| Login flow text follows selected language | AUTH-01 | Requires rendering the login page under both locales | Visit login route in both languages and confirm page title, login labels, buttons, and notice/support text match the selected language |
| Shared dialog and toast text follows selected language | COMM-01 | Requires opening modals and triggering notifications | Open touched dialogs or trigger touched toasts/empty states in both languages and confirm no targeted English literals remain |

---

## Validation Sign-Off

- [x] All tasks have automated build verification
- [x] Sampling continuity preserved
- [x] No watch-mode commands
- [x] Feedback latency < 120s
- [x] `nyquist_compliant: true` set in frontmatter

**Approval:** pending
