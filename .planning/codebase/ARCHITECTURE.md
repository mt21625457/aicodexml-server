# Architecture Map

## Overview
ClearML Server is a multi-service backend that provides a REST API, a static web UI, and a file storage service. The Python API service and file service are packaged in the same Docker image, while the web UI is built from a separate repo and served by Nginx.

## Runtime Components
- `apiserver/server.py`: Flask entrypoint for the API service.
- `apiserver/server_init/app_sequence.py`: Startup sequence that configures middleware, initializes Mongo/Elasticsearch, loads endpoints, and starts background workers.
- `apiserver/server_init/request_handlers.py`: Central request router that parses paths, validates auth/schema, dispatches to ServiceRepo, and formats responses.
- `apiserver/service_repo/`: Endpoint registry, auth, validation, and API call lifecycle.
- `apiserver/services/*.py`: Endpoint implementations grouped by domain (tasks, projects, users, etc.).
- `apiserver/bll/`: Business logic layer used by services (queue metrics, events, models, tasks, etc.).
- `fileserver/fileserver.py`: Flask app for upload/download/delete of artifacts.
- `docker/build/internal_files/entrypoint.sh`: Runtime selector for `apiserver`, `webserver`, or `fileserver` modes.
- `docker/compose.yaml`: Canonical service graph (API, web, files, Mongo, Elasticsearch, Redis, async worker).

## Request Flow (API)
1. HTTP request hits `apiserver/server.py` Flask app.
2. `RequestHandlers.before_request` in `apiserver/server_init/request_handlers.py` parses the URL into an endpoint name/version.
3. `ServiceRepo` in `apiserver/service_repo/service_repo.py` resolves the endpoint and performs auth and schema validation.
4. Endpoint handlers in `apiserver/services/*.py` call into `apiserver/bll/*` and `apiserver/database/model/*`.
5. Response payload and errors are normalized back in `RequestHandlers`.

## Data Stores And Infrastructure
- MongoDB: Primary persistence via MongoEngine in `apiserver/database/` and `apiserver/database/model/*`.
- Elasticsearch: Event/log indexing and search via `apiserver/elastic/` and `apiserver/es_factory.py`.
- Redis: Caches/queues/locks via `apiserver/redis_manager.py` and BLL helpers.
- File storage: Local filesystem behind `fileserver/fileserver.py` with auth enforced in `fileserver/auth.py`.

## Configuration And Deployment
- Config loading uses HOCON via `apiserver/config/basic.py` and `apiserver/config/default/*.conf`.
- Docker builds clone the UI from a separate repo in `docker/build/Dockerfile` and serve assets from `/usr/share/nginx/html`.
- Nginx config templates live in `docker/build/internal_files/clearml.conf.template` and `docker/build/internal_files/clearml_subpath.conf.template`.

## Where Web i18n Would Likely Attach
- Frontend code is not in this repo. The web UI is cloned in `docker/build/Dockerfile` from the `CLEARML_WEB_GIT_URL` (defaults to `https://github.com/allegroai/clearml-web.git`) and built into static assets.
- Locale assets and i18n initialization should live in that external UI repo, with the resulting bundles placed under `/usr/share/nginx/html` by the web build step.
- Runtime UI configuration is injected into `/usr/share/nginx/html/configuration.json` in `docker/build/internal_files/entrypoint.sh`, via `docker/build/internal_files/update_from_env.py` using `WEBSERVER__...` env vars and `/mnt/external_files/configs/configuration.json` if present. This is the natural place to pass default locale and available languages to the UI.
- Subpath hosting rewrites `env.js` and `index.html` in `docker/build/internal_files/entrypoint.sh`. Any i18n asset pathing should honor `CLEARML_SERVER_SUB_PATH` to keep locale bundles resolvable.
- If per-user locale needs to be stored server-side, the `preferences` field on `apiserver/database/model/user.py` plus `users.get_preferences` and `users.set_preferences` in `apiserver/services/users.py` are the likely persistence and API touchpoints.
