# Roadmap: AI Code XML Web Multilingual

## Overview

This roadmap retrofits full English and Simplified Chinese support into the existing `aicodexml-web` Angular application without breaking current delivery and packaging through the server repository. The work starts by establishing a single runtime i18n architecture, then adds locale persistence and formatting, localizes the shared shell and authentication surfaces, completes a feature-module sweep across the current product, and finishes with verification and maintainability guardrails.

## Phases

- [ ] **Phase 1: I18n Foundation** - Establish one runtime translation architecture, language switching, fallback behavior, and build integration
- [ ] **Phase 2: Locale Persistence And Formatting** - Persist locale preference and align date/number formatting with active language
- [ ] **Phase 3: Shared Shell And Auth Localization** - Localize the highest-leverage shared UI, auth flows, and common interaction surfaces
- [ ] **Phase 4: Feature Module Localization Sweep** - Translate the current feature pages and route-level product surface
- [ ] **Phase 5: Verification And Guardrails** - Validate bilingual coverage and add guardrails to prevent regression

## Phase Details

### Phase 1: I18n Foundation
**Goal**: Deliver a working runtime translation baseline in the existing Angular app and keep Docker packaging aligned with the local `aicodexml-web` submodule.
**Depends on**: Nothing (first phase)
**Requirements**: [I18N-01, I18N-02, I18N-03, BUILD-01]
**Success Criteria** (what must be TRUE):
  1. User can load one deployed app build that has runtime translation infrastructure available.
  2. User can switch between English and Simplified Chinese from the UI without redeploying.
  3. Missing translations fall back safely instead of exposing raw keys in core flows.
  4. Docker build and server packaging include translation assets from `aicodexml-web`.
**Plans**: 3 plans

Plans:
- [ ] 01-01: Integrate translation runtime, catalogs, and app-wide locale service
- [ ] 01-02: Wire initial language switcher, bootstrap precedence, and fallback behavior
- [ ] 01-03: Ensure build/package flow includes localized frontend assets from the submodule

### Phase 2: Locale Persistence And Formatting
**Goal**: Make locale choice stable across sessions and ensure locale-sensitive values follow the active language.
**Depends on**: Phase 1
**Requirements**: [PREF-01, PREF-02, FMT-01, FMT-02]
**Success Criteria** (what must be TRUE):
  1. Signed-in user keeps selected language after refresh and later login sessions.
  2. User without saved preference starts from browser or deployment default and sees clean reconciliation after login.
  3. Supported dates and times render according to active locale.
  4. Supported numbers and percentages render according to active locale.
**Plans**: 3 plans

Plans:
- [ ] 02-01: Persist locale via existing user preference APIs and reconcile startup precedence
- [ ] 02-02: Register locale data and standardize formatting helpers/pipes
- [ ] 02-03: Verify locale stability across login, refresh, logout, and relaunch flows

### Phase 3: Shared Shell And Auth Localization
**Goal**: Localize the most visible and reusable UI surfaces so users experience consistent bilingual behavior across common flows.
**Depends on**: Phase 2
**Requirements**: [SHELL-01, AUTH-01, COMM-01]
**Success Criteria** (what must be TRUE):
  1. Header, side nav, breadcrumbs, menus, and settings shell text follow the selected language.
  2. Login and signup flows display selected-language UI text.
  3. Shared dialogs, validation, placeholders, empty states, and toasts display selected-language UI text.
**Plans**: 3 plans

Plans:
- [ ] 03-01: Localize shared layout and preference entry points
- [ ] 03-02: Localize authentication flows and related reusable screens
- [ ] 03-03: Localize shared dialogs, notifications, validation, and empty states

### Phase 4: Feature Module Localization Sweep
**Goal**: Complete bilingual coverage of current route-level feature modules and page-specific product UI text.
**Depends on**: Phase 3
**Requirements**: [FEAT-01]
**Success Criteria** (what must be TRUE):
  1. Dashboard, projects, tasks/experiments, models, reports, serving, and settings feature pages show selected-language UI text.
  2. Users can navigate between translated feature areas without mixed-language critical flows.
  3. Remaining hard-coded user-visible text is reduced to explicitly tracked exceptions only.
**Plans**: 3 plans

Plans:
- [ ] 04-01: Localize dashboard, projects, and core navigation-driven pages
- [ ] 04-02: Localize experiment/task/model/report feature surfaces and shared subflows
- [ ] 04-03: Localize serving, settings, and remaining route-level product surfaces

### Phase 5: Verification And Guardrails
**Goal**: Prove the bilingual rollout is usable and prevent future regression toward hard-coded or mixed-language UI.
**Depends on**: Phase 4
**Requirements**: [QA-01]
**Success Criteria** (what must be TRUE):
  1. English and Simplified Chinese critical workflows pass manual verification without known mixed-language blockers.
  2. Missing translation behavior is visible during QA and safe in release behavior.
  3. Team has documented rules or checks to keep new UI strings within the translation system.
**Plans**: 2 plans

Plans:
- [ ] 05-01: Execute bilingual regression verification across critical workflows
- [ ] 05-02: Add maintainability guardrails for future localized development

## Progress

| Phase | Plans Complete | Status | Completed |
|-------|----------------|--------|-----------|
| 1. I18n Foundation | 0/3 | Not started | - |
| 2. Locale Persistence And Formatting | 0/3 | Not started | - |
| 3. Shared Shell And Auth Localization | 0/3 | Not started | - |
| 4. Feature Module Localization Sweep | 0/3 | Not started | - |
| 5. Verification And Guardrails | 0/2 | Not started | - |
