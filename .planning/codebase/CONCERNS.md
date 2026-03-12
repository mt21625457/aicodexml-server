# Concerns Mapping

## Multilingual Support Gaps
- User-facing error strings are hard-coded in config and code, which blocks easy localization. Examples: `apiserver/apierrors/errors.conf` and replacement messages in `apiserver/services/events.py`.
- API responses carry a single `result_msg` string with no locale metadata; defaults are English (e.g., "OK") and error messages are set as raw strings in `apiserver/service_repo/apicall.py`.
- No locale negotiation is implemented in request handling: `apiserver/service_repo/apicall.py` only manages ClearML-specific headers and there is no `Accept-Language` parsing or locale selection in `apiserver/service_repo/service_repo.py`.

## Search/Index Locale Assumptions
- Mongo text indexes are pinned to English for core entities, which affects search relevance for non-English content. See `apiserver/database/model/project.py`, `apiserver/database/model/task/task.py`, and `apiserver/database/model/model.py` (`default_language: "english"`).
- Collation is fixed to `en_US` in `apiserver/database/model/base.py` and applied across models like `apiserver/database/model/task/task.py`, `apiserver/database/model/model.py`, `apiserver/database/model/queue.py`, and `apiserver/database/model/url_to_delete.py`. This risks incorrect sort order for non-English locales.
- Aggregations explicitly rely on the same numeric collation in `apiserver/bll/project/project_queries.py`, so locale changes would require careful query/index updates and likely reindexing.

## Preference/Settings Gaps
- User preferences are stored as a JSON string in a `DynamicField` with no schema or enforced locale key in `apiserver/database/model/user.py`; updates happen via `apiserver/services/users.py` using `dpath` and raw JSON dumps/loads.
- Company-level defaults do not include any locale or language configuration in `apiserver/database/model/company.py`, which limits centralized locale policy for web clients.

## Encoding/Response Handling
- JSON serialization defaults to ASCII-escaped output unless callers set `ensure_ascii=False` in `apiserver/service_repo/apicall.py`, even though `apiserver/utilities/json.py` supports non-ASCII encoding. This can complicate clients expecting UTF-8 literals.

## Operational Debt/Risks
- Error messages are split between generated error definitions (`apiserver/apierrors/errors.conf`) and ad-hoc replacements in services (e.g., `apiserver/services/events.py`), making a full i18n audit and migration non-trivial.
