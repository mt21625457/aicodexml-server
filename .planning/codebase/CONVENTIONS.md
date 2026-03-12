# Code Conventions (Quality Focus)

## Structure and Roles
- The primary Python service lives in `apiserver/` and is structured into layers: service endpoints in `apiserver/services/`, business logic in `apiserver/bll/`, request/response models in `apiserver/apimodels/`, and persistence models in `apiserver/database/model/`.
- The file service is a separate Python service under `fileserver/` with its own entrypoint in `fileserver/fileserver.py` and config in `fileserver/config/`.
- Shared utilities and helpers are concentrated under `apiserver/utilities/` and are imported broadly across layers.

## API Endpoint Pattern
- Endpoints are defined as functions decorated with `@endpoint(...)` in `apiserver/services/*.py`, which binds request/response models and versioning to a service call.
- Request/response models are declared in `apiserver/apimodels/*.py` and imported into service modules (example: `apiserver/services/tasks.py`).
- API schema definitions live in HOCON `.conf` files under `apiserver/schema/services/` and are versioned by API version blocks (see `apiserver/schema/services/tasks.conf`).
- API version lineage is tracked in `apiserver/documentation/api_versions.md` and should stay in sync with schema and service behavior.

## Error Definitions
- Error codes and messages are defined in `apiserver/apierrors/errors.conf` and generate Python error classes used as `errors.bad_request.*` and similar imports.
- The generator entrypoint is `apiserver/apierrors_generator/__main__.py` and templates live under `apiserver/apierrors_generator/templates/`.

## Configuration and Logging
- Configuration is loaded via `apiserver/config/basic.py` from `apiserver/config/default/*.conf` plus optional extra config paths and env overrides.
- Logging is typically initialized via `config.logger(__file__)` from `apiserver/config_repo.py` (see usage in `apiserver/tests/automated/__init__.py`).

## Data Model and Storage
- MongoDB models use `mongoengine` and are defined in `apiserver/database/model/` with indexes declared in `meta` (example: `apiserver/database/model/task/task.py`).
- Mongo data migrations are versioned Python scripts under `apiserver/mongo/migrations/`.
- ElasticSearch mappings and initialization helpers live under `apiserver/elastic/`.

## Schema Authoring Rules
- `apiserver/schema/services/README.md` documents whitespace rules for schema descriptions; follow those rules to keep generated docs consistent.
- `apiserver/schema/meta/validate.py` enforces ASCII-only schema files and JSON schema validation; this impacts any changes in `apiserver/schema/**/*.conf`.

## Code Style Observations
- Python files use explicit imports, module-level constants, and class-based enums (e.g., `TaskStatus` in `apiserver/database/model/task/task.py`).
- Type hints are used in many modules but are not universally strict; follow existing module style for consistency.
- Shared helper functions tend to be in `apiserver/bll/util.py` and `apiserver/utilities/*.py` instead of inline duplication.
