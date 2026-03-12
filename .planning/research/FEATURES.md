# Feature Research

**Domain:** Enterprise Angular web app multilingual retrofit
**Researched:** 2026-03-12
**Confidence:** HIGH

## Feature Landscape

### Table Stakes (Users Expect These)

| Feature | Why Expected | Complexity | Notes |
|---------|--------------|------------|-------|
| Language switcher | Users expect to change product language explicitly | MEDIUM | Usually exposed in user menu or settings and applied immediately |
| Persistent language preference | Enterprise users expect the app to remember their choice | MEDIUM | This codebase already has user preference APIs and frontend preference plumbing |
| Full coverage of navigation, forms, dialogs, errors, empty states | Partial localization feels broken in admin products | HIGH | The user explicitly chose full visible UI scope, not shell-only coverage |
| Locale-aware date/number formatting | Bilingual UI without localized formatting feels inconsistent | MEDIUM | Existing code already injects `LOCALE_ID` in some places |
| Fallback language and missing-key safety | Production apps cannot show broken keys or blank labels | MEDIUM | Default English fallback and missing translation reporting should be part of v1 |

### Differentiators (Competitive Advantage)

| Feature | Value Proposition | Complexity | Notes |
|---------|-------------------|------------|-------|
| Translation coverage auditing | Makes regression control tractable in a large legacy UI | MEDIUM | Useful because the app has 300+ HTML templates and many TS-defined strings |
| Shared translation key taxonomy by feature/module | Keeps catalogs maintainable as the product evolves | MEDIUM | Important in a mature, multi-feature application |
| Admin/runtime-configurable default locale | Helps deployments where Chinese should be default for a tenant or environment | MEDIUM | Can build on existing runtime configuration injection |
| CI checks for untranslated or newly hard-coded strings | Reduces future localization debt | MEDIUM | Valuable once the initial migration lands |

### Anti-Features (Commonly Requested, Often Problematic)

| Feature | Why Requested | Why Problematic | Alternative |
|---------|---------------|-----------------|-------------|
| Machine-translate everything and ship | Fast initial output | Produces inconsistent enterprise terminology and low trust | Human-reviewed catalogs with limited auto-translation assistance |
| Localize user-generated content automatically | Seems “fully multilingual” | Incorrect semantics, high ambiguity, and data ownership concerns | Keep user-generated content raw; localize only UI chrome |
| Separate app builds per language for this admin surface | Matches Angular built-in i18n docs | Conflicts with per-user runtime switching and multiplies deployment complexity | Single deployment with runtime translation files |

## Feature Dependencies

```text
Language switcher
    └──requires──> Runtime translation service
                          └──requires──> Translation catalogs

Persistent locale preference
    └──requires──> User preference API integration

Locale-aware formatting
    └──requires──> Locale data registration + active locale state

Full-UI translation coverage
    └──requires──> String inventory + migration plan + regression checks
```

### Dependency Notes

- **Language switcher requires runtime translation service:** Otherwise language changes require full rebuild or redeploy.
- **Persistent locale preference requires API integration:** Browser-only storage is not enough for cross-session and cross-device consistency.
- **Full coverage requires inventory and enforcement:** A mature app will miss strings unless the migration is systematic.

## MVP Definition

### Launch With (v1)

- [ ] Runtime translation framework integrated into the existing Angular app
- [ ] English and Simplified Chinese catalogs for all current user-visible UI text
- [ ] Language switcher in an obvious user-facing location
- [ ] Authenticated preference persistence plus browser fallback
- [ ] Locale-aware formatting for dates, numbers, percentages, and similar visible values
- [ ] Missing-translation fallback and QA verification path

### Add After Validation (v1.x)

- [ ] Runtime-configurable default locale via deployment configuration
- [ ] Translation coverage scripts or linting
- [ ] Structured process/tooling for catalog maintenance

### Future Consideration (v2+)

- [ ] More locales beyond `en` and `zh-CN`
- [ ] Terminology glossary integration for translators
- [ ] Server-side localization of API-originated system messages where needed

## Feature Prioritization Matrix

| Feature | User Value | Implementation Cost | Priority |
|---------|------------|---------------------|----------|
| Runtime translation layer | HIGH | MEDIUM | P1 |
| Language switcher | HIGH | MEDIUM | P1 |
| Locale persistence | HIGH | MEDIUM | P1 |
| Full catalog migration | HIGH | HIGH | P1 |
| Locale-aware formatting | HIGH | MEDIUM | P1 |
| Missing-key reporting | MEDIUM | LOW | P2 |
| Runtime-configurable default locale | MEDIUM | MEDIUM | P2 |
| CI guardrails | MEDIUM | MEDIUM | P2 |

## Competitor Feature Analysis

| Feature | Competitor A | Competitor B | Our Approach |
|---------|--------------|--------------|--------------|
| Language selection | Usually profile menu or settings | Usually profile menu | Put it where current users already manage preferences |
| Persistence | Account-scoped preference | Account or browser | Account-scoped with browser fallback |
| Coverage | Mature products localize shared shell and main workflows first | Better products localize the full visible UI | v1 targets full visible UI because the user explicitly requested it |

## Sources

- https://angular.dev/guide/i18n
- https://angular.dev/guide/i18n/deploy
- https://ngx-translate.org/getting-started/translation-files/
- Local codebase analysis of `aicodexml-web` and existing user preference plumbing

---
*Features research for: web multilingual support*
*Researched: 2026-03-12*
