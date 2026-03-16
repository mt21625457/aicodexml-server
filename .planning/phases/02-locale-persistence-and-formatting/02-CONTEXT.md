# Phase 2: Locale Persistence And Formatting - Context

**Gathered:** 2026-03-12
**Status:** Ready for planning
**Source:** Derived from Phase 1 outputs, current frontend code inspection, and user requirement focus on English and Simplified Chinese

<domain>
## Phase Boundary

Phase 2 makes the active language durable for signed-in users and aligns locale-sensitive formatting with the active application language. It builds directly on the runtime translation service from Phase 1 and reuses the existing user preference API instead of introducing a new backend surface.

This phase does not attempt broad UI copy translation. It focuses on preference persistence, startup reconciliation, locale-data registration, formatting helpers, and the first representative formatting surfaces that later localization phases depend on.

</domain>

<decisions>
## Implementation Decisions

### Locked Decisions
- Keep the Phase 1 runtime translation architecture and local `aicodexml-web` submodule.
- Support only `en` and `zh-CN` in v1.
- Reuse existing `users.get_preferences` / `users.set_preferences` persistence rather than adding a dedicated locale API.
- Use one app-level locale source of truth that maps the active app language to Angular locale formatting, `Intl` behavior, and Material date adapter locale behavior.

### Claude's Discretion
- Exact preference key path for persisted locale inside existing user preferences.
- Whether locale formatting should be standardized through shared pipes, services, provider wiring, or a mix of these.
- Which existing formatting call sites count as the minimum high-leverage support surface for this phase.

</decisions>

<specifics>
## Specific Ideas

- `aicodexml-web/src/app/shared/services/locale.service.ts` already owns language selection and local/browser/config fallback.
- `aicodexml-web/src/app/webapp-common/user-preferences.ts` and `apiserver/services/users.py` already persist arbitrary nested user preferences.
- `aicodexml-web/src/app/core/app-init.ts` initializes locale before authenticated preference loading, so login-time reconciliation must happen after preferences load.
- Existing formatting currently mixes Angular `DatePipe`, `DecimalPipe`, `formatDate`, `Intl.Collator`, `toLocaleString`, and two Material date adapter providers pinned to `enGB`.

</specifics>

<deferred>
## Deferred Ideas

- Translating settings labels, auth screens, and shared shell copy remains Phase 3 work.
- Full feature-page localization remains Phase 4 work.
- Guardrails and broad bilingual regression remain Phase 5 work.

</deferred>

---

*Phase: 02-locale-persistence-and-formatting*
*Context gathered: 2026-03-12 via Phase 2 planning*
