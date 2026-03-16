# Phase 3 Research: Shared Shell And Auth Localization

**Phase:** 3
**Name:** Shared Shell And Auth Localization
**Researched:** 2026-03-12
**Confidence:** HIGH

## What This Phase Needs To Prove

Phase 3 must prove that the selected language now governs the most visible shared UI surfaces: app shell navigation, settings shell copy, authentication flows, and reusable dialogs/messages that appear across the application.

## Recommended Approach

### 1. Expand the translation catalog before touching templates

- The current i18n catalog is intentionally small and only covers the header language switcher labels.
- Phase 3 should introduce a structured key taxonomy for:
  - shell/navigation/settings
  - auth/login
  - dialogs/notifications/common empty states
  - credentials/admin auth-adjacent UI
- Keeping the catalogs organized now reduces churn in Phase 4 when feature modules start consuming the same shared keys.

### 2. Use template translation for stable chrome, service translation for runtime messages

- Static UI copy in standalone components should use `TranslatePipe`.
- Dynamic toasts, dialog config objects, page titles, and computed strings should use `TranslateService.instant(...)`.
- This matches the existing Angular architecture: templates already accept standalone imports, while runtime notifications are dispatched from TypeScript services and components.

### 3. Add an explicit breadcrumb translation mechanism

- Breadcrumb names are a mix of translation-worthy shell labels and user/project names that must remain literal.
- The cleanest Phase 3 implementation is to extend breadcrumb link metadata with a `translate` flag and render only flagged names through the translation pipe.
- This keeps route-driven static settings/login breadcrumbs localizable without risking accidental translation of entity names.

### 4. Treat shared component defaults as part of the localization surface

- Reusable inputs such as search and copy-to-clipboard still expose English defaults.
- Localizing only specific call sites would leave mixed-language shared UI when components are used without explicit labels.
- Phase 3 should update high-traffic reusable defaults where the risk is low and the value is broad.

## Implementation Notes

### Shared Shell Targets

- Side nav tooltips for workers, endpoints, datasets, projects, pipelines, enterprise, and Slack support
- Settings drawer labels and settings-route breadcrumbs
- User preferences page title
- Breadcrumb share/archive UI
- Header accessibility/fallback labels that still bypass translation

### Auth Targets

- Login template labels, validation copy, legal/support text, and marketing/community links
- Login component title, invite title, action button caption, document title, breadcrumb label, and login popup confirm action
- Server-unavailable dialog in `BaseLoginService`
- Auth-adjacent credential creation/edit flows that still expose hard-coded copy

### Common Reusable UI Targets

- Confirm dialog checkbox and button defaults
- UI update dialog
- Appearance dialog
- Profile preference toggles, info tooltips, and action links
- Search/filter/empty-state defaults
- JSON validation toast, code copy toast, share dialog toast/subtitles, credential label dialog labels

## Risks

| Risk | Why It Matters | Mitigation |
|------|----------------|------------|
| Breadcrumb translation catches entity names | Could render raw keys or incorrectly translate user data | Add a per-crumb translation flag instead of translating every breadcrumb blindly |
| Shared dialog text is split between templates and TS | Partial migration would leave mixed-language modal flows | Standardize static text in templates and use `TranslateService.instant(...)` for runtime dialog/toast strings |
| Reusable default labels remain English | Users still see mixed-language placeholders in many flows | Localize a small set of shared defaults with low compatibility risk |
| Catalog growth becomes inconsistent | Later phases become harder to maintain | Introduce grouped translation namespaces now (`shell`, `auth`, `shared`, `settings`, `credentials`, `errors`) |

## Validation Architecture

### Tooling

- Primary build command: `cd aicodexml-web && npm run build`
- Supplemental static validation:
  - `rg` checks for known hard-coded Phase 3 strings in touched files
  - catalog key sanity checks in `src/assets/i18n/en.json` and `src/assets/i18n/zh-CN.json`
- Primary manual check:
  - switch language between English and Simplified Chinese, then inspect login, settings, shell nav, and shared dialogs

### Minimum Verification For This Phase

1. Build succeeds after template and service translation changes.
2. Shared shell tooltips, settings entry labels, and breadcrumb share/settings/login labels render from translation catalogs.
3. Login copy, login titles, and auth error dialogs no longer rely on hard-coded English strings in the touched flows.
4. Shared dialogs, toasts, placeholders, and empty states touched in this phase resolve through the translation system.

## Sources

- Local code: `aicodexml-web/src/app/layout/side-nav/side-nav.component.html`
- Local code: `aicodexml-web/src/app/features/settings/settings.component.html`
- Local code: `aicodexml-web/src/app/features/settings/settings-routing.module.ts`
- Local code: `aicodexml-web/src/app/webapp-common/layout/breadcrumbs/breadcrumbs.component.{ts,html}`
- Local code: `aicodexml-web/src/app/webapp-common/login/login/login.component.{ts,html}`
- Local code: `aicodexml-web/src/app/webapp-common/shared/services/login.service.ts`
- Local code: `aicodexml-web/src/app/webapp-common/settings/admin/profile-preferences/profile-preferences.component.html`
- Local code: `aicodexml-web/src/app/webapp-common/shared/ui-components/overlay/confirm-dialog/confirm-dialog.component.html`
- Local code: `aicodexml-web/src/assets/i18n/en.json`
- Local code: `aicodexml-web/src/assets/i18n/zh-CN.json`

---
*Phase 3 research complete*
