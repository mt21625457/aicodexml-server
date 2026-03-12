# Integrations

## Core Infrastructure Services
- MongoDB is the primary persistence store; connection setup lives in `apiserver/database/__init__.py`, and the docker service is defined in `docker/docker-compose.yml`.
- Redis is used for caching, locking, and background coordination in both API and fileserver (`apiserver/redis_manager.py`, `fileserver/redis_manager.py`).
- Elasticsearch is used for event/metrics indexing and queries (`apiserver/bll/event/event_bll.py`, `apiserver/database/errors.py`).

## Object Storage Providers
- AWS S3 support via `boto3` with storage credential configuration in `apiserver/config/default/services/storage_credentials.conf` and usage in `apiserver/jobs/async_urls_delete.py`.
- Azure Blob Storage support via `azure-storage-blob`, with storage settings models in `apiserver/apimodels/storage.py` and delete flow in `apiserver/jobs/async_urls_delete.py`.
- Google Cloud Storage support via `google-cloud-storage`, wired in `apiserver/bll/storage/__init__.py` and `apiserver/jobs/async_urls_delete.py`.
- Storage settings persisted in Mongo via `apiserver/database/model/storage_settings.py`.

## Web UI Build Source (External Repo)
- The web UI is built by cloning an external Git repository during image build (`docker/build/Dockerfile` uses `CLEARML_WEB_GIT_URL` to clone `clearml-web`).
- There is no local UI source tree in this repo; only the build hook exists (`docker/build/internal_files/build_webapp.sh`).

## ClearML Agent Services (Optional)
- Docker Compose includes a separate `clearml/clearml-agent-services:latest` container for running service tasks (`docker/docker-compose.yml`).
- Agent services integrate with Docker by mounting the host Docker socket (`docker/docker-compose.yml`).
- Agent services expose environment hooks for AWS/Azure/GCP credentials and ClearML access keys (`docker/docker-compose.yml`).

## Filesystem Integration
- The file server reads/writes artifacts to a host-mounted filesystem path (default `/mnt/fileserver`) as configured in `fileserver/fileserver.py` and mapped in `docker/docker-compose.yml`.

## Authentication / Authorization (Internal)
- Auth is stored in MongoDB; auth DB host appears in `apiserver/config/default/hosts.conf` and cookie settings in `apiserver/config/default/apiserver.conf`.
- No external SSO/OAuth providers are referenced in the codebase; auth looks self-contained within ClearML Server.
