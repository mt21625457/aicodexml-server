# Stack

## Overview
- The repo implements the ClearML Server backend services (API + file server) and builds a separate web UI into the Docker image.
- Core runtime services are Python-based: `apiserver/server.py` and `fileserver/fileserver.py`.
- The web UI is built from an external repo and served by Nginx inside the same container image as a `webserver` entrypoint.

## Languages & Runtimes
- Python 3.11 is the primary runtime for API and file services (Docker base in `docker/build/Dockerfile`).
- Node.js 20 is used only at image build time to compile the web UI (see `docker/build/Dockerfile`).
- Shell scripts drive image build and entrypoint flow (`docker/build/internal_files/build_webapp.sh`, `docker/build/internal_files/entrypoint.sh`).

## Backend Frameworks & Servers
- Flask powers both API and file services (`apiserver/server.py`, `fileserver/fileserver.py`).
- Optional Gunicorn production server for API and fileserver (`docker/build/internal_files/entrypoint.sh`).
- Nginx serves the web UI in the `webserver` mode (`docker/build/internal_files/entrypoint.sh`).
- CORS and response compression via Flask extensions (`apiserver/server_init/app_sequence.py`, `fileserver/fileserver.py`).

## Data & Storage Layers
- MongoDB is the primary database using MongoEngine + PyMongo (`apiserver/database/__init__.py`, `apiserver/requirements.txt`).
- Redis is used for caching/locks (`apiserver/redis_manager.py`, `fileserver/redis_manager.py`).
- Elasticsearch is used for event/metrics indexing (`apiserver/bll/event/event_bll.py`).
- Local filesystem storage is used for artifact files in the file server (`fileserver/fileserver.py`).

## Config & Serialization
- Configuration files are HOCON-style `.conf` read via pyhocon (`apiserver/config/default/*.conf`, `apiserver/requirements.txt`).
- JSON schema validation via `jsonschema` and `fastjsonschema` (`apiserver/requirements.txt`).
- API error code generation uses Jinja2 templates (`apiserver/apierrors_generator/generator.py`).

## Build, Packaging, Deployment
- Docker multi-stage build compiles web UI and packages Python services (`docker/build/Dockerfile`).
- Docker Compose defines all services for local/prod deployment (`docker/docker-compose.yml`).
- The same image (`clearml/server:latest`) is used for apiserver, fileserver, and webserver roles.

## Web/Frontend Presence
- No frontend source code is stored in this repo; the UI is pulled from `https://github.com/allegroai/clearml-web.git` during image build (`docker/build/Dockerfile`).
- The only UI-related artifact here is the build script used inside the Docker image (`docker/build/internal_files/build_webapp.sh`).
- The runtime webserver is Nginx serving the compiled SPA assets (`docker/build/internal_files/entrypoint.sh`).

## Testing & Utilities
- Test dependencies include `pynose` (`apiserver/tests/requirements.txt`).
- CLI utilities and init scripts live under `apiserver/server_init` and `apiserver/utilities`.
