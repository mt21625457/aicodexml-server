---
phase: 01-i18n-foundation
plan: 03
subsystem: infra
tags: [docker, nginx, runtime-config, configuration-json]
requires:
  - phase: 01-01
    provides: Frontend runtime locale fields expected by the Angular app
provides:
  - Local frontend packaging from the submodule
  - Runtime locale config aliases and camelCase config mapping
  - No-cache delivery for configuration.json
affects: [phase-2, deployment, runtime-config]
tech-stack:
  added: []
  patterns: [camelCase env-to-config mapping, runtime locale aliases, non-cached configuration delivery]
key-files:
  created: []
  modified:
    - .gitmodules
    - docker/build/Dockerfile
    - docker/build/internal_files/update_from_env.py
    - docker/build/internal_files/entrypoint.sh
    - docker/build/internal_files/clearml.conf.template
key-decisions:
  - "Use the local aicodexml-web submodule as the build source instead of cloning the frontend during Docker build."
  - "Expose locale runtime config through configuration.json rather than hard-coding deployment defaults into the frontend."
patterns-established:
  - "WEBSERVER/CLEARML_WEB env vars can hydrate frontend locale metadata via configuration.json."
requirements-completed: [BUILD-01]
duration: 6min
completed: 2026-03-12
---

# Phase 1: I18n Foundation Summary

**Local submodule-based frontend packaging with runtime locale config aliases and non-cached configuration delivery**

## Performance

- **Duration:** 6 min
- **Started:** 2026-03-12T16:49:00+0800
- **Completed:** 2026-03-12T16:55:00+0800
- **Tasks:** 2
- **Files modified:** 5

## Accomplishments
- Switched server packaging to consume the local `aicodexml-web` submodule rather than cloning a remote frontend during image build.
- Added locale-friendly runtime config aliases and camelCase env-to-JSON mapping for `defaultLanguage` and `supportedLanguages`.
- Set `configuration.json` to no-cache so runtime locale config changes are not masked by stale browser caching.

## Task Commits

Each task was committed atomically:

1. **Task 1: Confirm translation assets survive the frontend build and server packaging path** - `0b1ff27` (`feat(web): package local multilingual frontend`)
2. **Task 2: Prepare runtime web configuration for locale metadata** - `0b1ff27` (`feat(web): package local multilingual frontend`)

## Files Created/Modified
- `.gitmodules` - tracks the local `aicodexml-web` submodule
- `docker/build/Dockerfile` - copies the local submodule into the frontend build stage
- `docker/build/internal_files/update_from_env.py` - exact-prefix slicing and camelCase config key normalization
- `docker/build/internal_files/entrypoint.sh` - locale env aliases exported into web configuration generation
- `docker/build/internal_files/clearml.conf.template` - `configuration.json` served with `Cache-Control: no-cache`

## Decisions Made

- Normalized new env-backed config keys to camelCase because the frontend reads `defaultLanguage` and `supportedLanguages`.
- Added friendly locale env aliases so deployments do not need to know the internal config key casing.

## Deviations from Plan

None - the plan stayed within packaging and runtime-config scope.

## Issues Encountered

- `update_from_env.py` had a latent bug from using `lstrip(prefix)` for prefix removal; this broke newly added keys like `SUPPORTED_LANGUAGES`. The script was corrected to slice the exact prefix length before final validation.

## User Setup Required

None - no external service configuration required.

## Next Phase Readiness

- Deployments can now pass locale defaults and supported languages into the frontend runtime config cleanly.
- Phase 2 can consume these config values together with user preference persistence.

---
*Phase: 01-i18n-foundation*
*Completed: 2026-03-12*
