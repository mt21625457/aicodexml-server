# Pitfalls Research

**Domain:** Brownfield Angular multilingual retrofit
**Researched:** 2026-03-12
**Confidence:** HIGH

## Critical Pitfalls

| Pitfall | Warning Signs | Prevention Strategy | Phase |
|---------|---------------|---------------------|-------|
| Choosing Angular compile-time localized builds for a runtime-switching product | One build per locale, routing/subpath complexity, cannot cleanly switch language per signed-in user | Use runtime translation catalogs for UI copy; keep Angular locale APIs for formatting only | Phase 1 |
| Underestimating string inventory size | Team localizes shell pages but many dialogs, tooltips, empty states, snackbars, and TS-constructed strings remain untranslated | Start with inventory and shared-component migration before feature sweep | Phase 1-2 |
| Forgetting formatting localization | Copy changes to Chinese but dates, numbers, percentages, and plural output remain English-centric | Register locale data and audit formatting call sites as a first-class requirement | Phase 1-3 |
| Saving locale only in browser storage | User logs in from another device and language resets unexpectedly | Persist locale in user preferences and reconcile during bootstrap | Phase 2 |
| Missing-key behavior leaking to production | Users see raw translation keys or blank UI labels | Enforce fallback language, log/report missing keys, and add QA checks | Phase 2-4 |

## Technical Debt Traps

| Shortcut | Why It’s Tempting | Long-Term Cost | Better Approach |
|----------|-------------------|----------------|-----------------|
| Leave TS string literals for “later” | Templates are easier to grep than action labels or notifications | Persistent mixed-language UX and missed regressions | Audit both HTML and TS-created UI strings |
| Translate page by page without shared key taxonomy | Fast for the current file | Future maintenance becomes chaotic | Define feature/module key convention first |
| Hard-code locale checks inside components | Feels easy in the moment | Duplicated logic and impossible future expansion | Central locale service and formatting helpers |

## Performance Traps

| Trap | Why It Happens | How to Avoid | Scale Threshold |
|------|----------------|--------------|-----------------|
| Bundling all translations statically into the main app | Simplest setup for a small demo | Use loader-based catalogs for a large production app | Immediate concern in this codebase size |
| Excessive synchronous language flips during bootstrap | Locale decision made too late or in multiple places | Resolve initial locale centrally before most UI renders | Immediate concern |

## UX Pitfalls

| Pitfall | User Impact | Better Approach |
|---------|-------------|-----------------|
| Partial Chinese UI with English leftovers | Product feels unfinished and low trust | Track coverage by shared shell, then feature modules, and verify visibly |
| Inconsistent terminology between pages | Users struggle to map the same concept across flows | Maintain reviewable bilingual terminology in catalogs |
| Hidden language setting | Users assume the product only supports one language | Place switcher in an obvious preference or header location |

## "Looks Done But Isn't" Checklist

- [ ] **Language switcher:** verify dialogs, overlays, menus, toasts, and lazy-loaded modules also update
- [ ] **Chinese support:** verify date, number, and plural formatting also switch, not just labels
- [ ] **Preference persistence:** verify browser fallback, first login, logout, and new device behavior
- [ ] **Full coverage:** verify table headers, placeholders, form validation, empty states, and TS-built notifications
- [ ] **Deployment:** verify Docker build still packages translation assets from `aicodexml-web`

## Recovery Strategies

| Pitfall | Recovery Cost | Recovery Steps |
|---------|---------------|----------------|
| Wrong architecture choice (build-per-locale) | HIGH | Stop before broad migration, move to runtime catalogs, simplify deployment assumptions |
| Missed strings after rollout | MEDIUM | Add coverage audit, patch missing keys by feature, add regression checklist |
| Locale persistence bugs | MEDIUM | Reconcile browser, cached, and server preference precedence in one service |

## Pitfall-to-Phase Mapping

| Pitfall | Prevention Phase | Verification |
|---------|------------------|--------------|
| Wrong i18n architecture | Phase 1 | One documented translation approach chosen and wired into bootstrap |
| Incomplete coverage | Phase 2-4 | Feature-by-feature visible string audits pass |
| Formatting mismatch | Phase 2-3 | Representative pages show localized dates/numbers in both languages |
| Preference inconsistency | Phase 2 | Login, refresh, logout, and cross-session checks pass |

## Sources

- https://angular.dev/guide/i18n/merge
- https://angular.dev/guide/i18n/deploy
- https://ngx-translate.org/getting-started/translation-files/
- Local codebase analysis of current app size, bootstrap flow, and preference persistence

---
*Pitfalls research for: web multilingual support*
*Researched: 2026-03-12*
