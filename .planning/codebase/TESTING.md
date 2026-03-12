# Testing Patterns (Quality Focus)

## Test Locations and Framework
- Automated tests are located under `apiserver/tests/automated/` and are standard `unittest` test cases.
- The base test harness is `TestService` in `apiserver/tests/automated/__init__.py`, which provides setup, teardown, and helper utilities.
- `apiserver/tests/requirements.txt` lists `pynose`, suggesting historical compatibility with nose-based runners.

## API Test Harness Behavior
- Tests are integration-style and call the running API service via `APIClient` in `apiserver/tests/api_client.py`.
- `TestService.setUp()` constructs `APIClient(base_url="http://localhost:8008/v{version}")`, so a local API server must be running and reachable.
- `APIClient` reads credentials from env (`SM_API_KEY`, `SM_API_SECRET`, `SM_API_URL`) or from config defaults like `apiserver/config/default/secure.conf`.
- The context manager `APIClient.raises(...)` in `apiserver/tests/api_client.py` is used to assert error codes.

## Common Test Patterns
- Tests use `self.api.<service>.<endpoint>(...)` dynamic helpers to invoke endpoints (see `apiserver/tests/automated/test_models.py`).
- Resource cleanup is handled through `self.defer(...)` in `apiserver/tests/automated/__init__.py`, which executes in `tearDown()`.
- Convenience helpers like `create_temp(...)` generate temporary resources and schedule cleanup automatically.

## Safe Multilingual Change Verification
- If a change spans multiple layers (Python + schema + config), keep them consistent across `apiserver/services/`, `apiserver/apimodels/`, and `apiserver/schema/services/*.conf`.
- When modifying schema files, run the schema validator in `apiserver/schema/meta/validate.py` to catch JSON schema and ASCII-only violations.
- When changing error behavior or adding new error codes, update `apiserver/apierrors/errors.conf` and regenerate via `apiserver/apierrors_generator/__main__.py`.
- For database model changes in `apiserver/database/model/`, add or update migration scripts in `apiserver/mongo/migrations/` and validate indexes defined in `meta` blocks.
- For ElasticSearch mapping changes, review and update `apiserver/elastic/apply_mappings.py` or `apiserver/elastic/initialize.py` as appropriate.
- For fileserver behavior changes, validate the `fileserver/` service independently (entrypoint `fileserver/fileserver.py`) and ensure auth/config compatibility with `fileserver/config/`.
- Run targeted automated tests from `apiserver/tests/automated/` that cover the modified endpoint(s) and data flows, and ensure the API server is running with valid credentials.
- If API versions are affected, update documentation in `apiserver/documentation/api_versions.md` and verify version-gated logic in `apiserver/services/*.py` and schema version blocks.
