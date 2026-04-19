# max-live-test

demo of fast golden path

This service follows the shared Primetime Python API, Docker, GitHub Actions,
and Kubernetes conventions.

## Local Commands

- Bootstrap dependencies: `make bootstrap`
- Lint: `make lint`
- Run tests: `make test`
- Build the package: `make build`
- Start the API: `PYTHONPATH=src uv run --frozen python -m max_live_test.main`
- Build the image: `docker build -t max-live-test:dev .`
- Render shared chart: `helm template max-live-test oci://ghcr.io/primetime-dev/charts/flask-service --version 0.1.0 -f deploy/values.yaml --set image.repository=ghcr.io/primetime-dev/max-live-test --set image.tag=dev`

## Quick Start

Start the service directly:

```bash
make bootstrap
PYTHONPATH=src uv run --frozen python -m max_live_test.main
```

In another terminal, verify it is serving traffic:

```bash
curl http://127.0.0.1:8080/
curl http://127.0.0.1:8080/healthz
curl http://127.0.0.1:8080/readyz
```

Build and run the container:

```bash
docker build -t max-live-test:dev .
docker run --rm -p 8080:8080 max-live-test:dev
```

Then verify the containerized service:

```bash
curl http://127.0.0.1:8080/
curl http://127.0.0.1:8080/healthz
curl http://127.0.0.1:8080/readyz
```

## Endpoints

- `GET /`
- `GET /healthz`
- `GET /readyz`

## Pipeline

This repo consumes the reusable workflows from the org `.github` repository for:

- pull request CI
- post-merge Docker build and push to `ghcr.io/primetime-dev`
- Helm-based deployment via the shared `flask-service` chart

The generated repo stores chart overrides in `deploy/values.yaml`.
