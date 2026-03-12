# Requirements: AI Code XML Web Multilingual

**Defined:** 2026-03-12
**Core Value:** Users can switch between Chinese and English and consistently see the entire web UI in their chosen language without breaking existing workflows.

## v1 Requirements

### I18n Foundation

- [ ] **I18N-01**: User can load the web application with a runtime translation system active in a single deployed build
- [ ] **I18N-02**: User can switch the web UI language between English and Simplified Chinese without redeploying the application
- [ ] **I18N-03**: User sees a safe fallback language instead of raw translation keys or blank labels when a translation is missing

### Locale Preference

- [ ] **PREF-01**: Signed-in user keeps their selected language across refreshes and later sessions
- [ ] **PREF-02**: User who has no saved language preference starts with browser or deployment default language and the app reconciles cleanly after login

### Locale Formatting

- [ ] **FMT-01**: User sees dates and times formatted according to the active language locale in supported UI areas
- [ ] **FMT-02**: User sees locale-sensitive numeric values such as numbers, percentages, and similar formatted output according to the active language locale in supported UI areas

### Shared UI

- [ ] **SHELL-01**: User sees header, side navigation, breadcrumbs, user menu, and settings shell text in the selected language
- [ ] **AUTH-01**: User sees login, signup, and related authentication screen text in the selected language
- [ ] **COMM-01**: User sees common dialogs, toasts, empty states, validation messages, placeholders, and reusable shared component text in the selected language

### Feature Coverage

- [ ] **FEAT-01**: User sees existing feature-page UI text in the selected language across the current web application, including dashboards, project/task/model flows, reports, serving, and settings areas

### Delivery And Verification

- [ ] **BUILD-01**: User receives the localized frontend through the existing server Docker packaging flow using the local `aicodexml-web` submodule
- [ ] **QA-01**: User does not encounter known mixed-language critical workflows in the released English and Simplified Chinese UI

## v2 Requirements

### Locale Expansion

- **LOCL-01**: User can use additional locales beyond English and Simplified Chinese
- **LOCL-02**: Administrator can define tenant- or deployment-specific default locale behavior with finer-grained runtime controls

### Localization Operations

- **L10N-01**: Team has CI or lint automation that blocks newly introduced hard-coded user-visible strings
- **L10N-02**: Team has structured tooling for translation coverage reporting and catalog maintenance

### Server Localization

- **SRV-01**: User sees selected backend-originated system messages localized when those messages are surfaced directly by the product

## Out of Scope

| Feature | Reason |
|---------|--------|
| Additional languages beyond `en` and `zh-CN` | Keep v1 bounded around the explicitly requested business need |
| Automatic translation of user-generated content | Not reliable and not part of product chrome localization |
| Broad frontend redesign or unrelated refactor | This project is for multilingual support, not visual or architectural overhaul |
| Full localization of every backend/API error string | Only changes necessary for preference persistence and clean UX are in scope for v1 |

## Traceability

| Requirement | Phase | Status |
|-------------|-------|--------|
| I18N-01 | Unmapped | Pending |
| I18N-02 | Unmapped | Pending |
| I18N-03 | Unmapped | Pending |
| PREF-01 | Unmapped | Pending |
| PREF-02 | Unmapped | Pending |
| FMT-01 | Unmapped | Pending |
| FMT-02 | Unmapped | Pending |
| SHELL-01 | Unmapped | Pending |
| AUTH-01 | Unmapped | Pending |
| COMM-01 | Unmapped | Pending |
| FEAT-01 | Unmapped | Pending |
| BUILD-01 | Unmapped | Pending |
| QA-01 | Unmapped | Pending |

**Coverage:**
- v1 requirements: 13 total
- Mapped to phases: 0
- Unmapped: 13 ⚠️

---
*Requirements defined: 2026-03-12*
*Last updated: 2026-03-12 after initial definition*
