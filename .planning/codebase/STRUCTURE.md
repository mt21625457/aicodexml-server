# Directory Structure

## Top Level
- `apiserver/`: Python Flask API service, schema validation, business logic, and database access.
- `fileserver/`: Python Flask file storage service (upload/download/delete).
- `docker/`: Dockerfiles, entrypoints, compose files, and Nginx templates for deployment.
- `docs/`: Product diagrams and documentation assets.
- `upgrade/`: Upgrade helper scripts and notes.
- `openspec/`: OpenSpec change artifacts (workflow inputs/outputs).
- `.planning/`: Planning artifacts created by automated mapping and GSD workflows.
- `README.md`: System overview and deployment instructions.

## API Service (`apiserver/`)
- `apiserver/server.py`: Flask app entrypoint for API server.
- `apiserver/server_init/app_sequence.py`: Startup init for config, DBs, endpoint loading, and background workers.
- `apiserver/server_init/request_handlers.py`: Request parsing, auth, schema validation, and response formatting.
- `apiserver/services/`: Endpoint implementations grouped by service domain.
- `apiserver/bll/`: Business logic layer used by endpoints (events, tasks, projects, queues, etc.).
- `apiserver/apimodels/`: Request/response data models (jsonmodels).
- `apiserver/schema/services/`: HOCON API schemas describing endpoint inputs/outputs and auth rules.
- `apiserver/schema/schema_reader.py`: Loads and interprets the HOCON schema files.
- `apiserver/database/`: MongoEngine setup, base model helpers, query utilities.
- `apiserver/database/model/`: MongoDB document models (users, tasks, projects, etc.).
- `apiserver/elastic/`: Elasticsearch initialization and mappings.
- `apiserver/es_factory.py`: Elasticsearch cluster config and client factory.
- `apiserver/redis_manager.py`: Redis connection manager.
- `apiserver/jobs/`: Background jobs (async deletion and maintenance tasks).
- `apiserver/mongo/`: MongoDB init and migrations.
- `apiserver/utilities/`: Shared helpers (env, json, dict utils, etc.).

## File Service (`fileserver/`)
- `fileserver/fileserver.py`: Flask app with upload/download/delete endpoints.
- `fileserver/auth.py`: Auth validation for file requests.
- `fileserver/config/`: File server config loading and defaults.
- `fileserver/redis_manager.py`: Redis support for file service workflows.
- `fileserver/utils.py`: Utility helpers used by the file service.

## Docker And Deployment (`docker/`)
- `docker/build/Dockerfile`: Builds API, file server, and webserver image, cloning the UI repo.
- `docker/build/internal_files/entrypoint.sh`: Selects runtime mode and wires Nginx config.
- `docker/build/internal_files/update_from_env.py`: Injects env values into web UI `configuration.json`.
- `docker/build/internal_files/clearml.conf.template`: Nginx config for serving UI and proxying API/files.
- `docker/build/internal_files/clearml_subpath.conf.template`: Nginx subpath rewrite rules.
- `docker/compose.yaml`: Main docker-compose with API, web, files, Mongo, Redis, Elasticsearch.
- `docker/docker-compose.yml`: Alternate compose variant (older name/path).

## Docs And Assets (`docs/`)
- `docs/ClearML_Server_Diagram.png`: Architecture diagram referenced in the README.
- `docs/clearml_server_logo.png`: Project logo.

## Specs And Workflow (`openspec/`)
- `openspec/`: OpenSpec change artifacts (when using the experimental workflow).
