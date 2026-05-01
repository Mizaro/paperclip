---
title: Docker
summary: Docker Compose quickstart
---

Run Paperclip in Docker without installing Node or pnpm locally.

## Compose Quickstart (Recommended)

```sh
docker compose -f docker/docker-compose.quickstart.yml up -d
```

Open [http://localhost:3100](http://localhost:3100).

Defaults:

- Host port: `3100`
- Image: `ghcr.io/mizaro/paperclip:latest`
- Data volume: `paperclip-data`

Override with environment variables:

```sh
PAPERCLIP_IMAGE=ghcr.io/mizaro/paperclip:latest PAPERCLIP_PORT=3200 \
  docker compose -f docker/docker-compose.quickstart.yml up -d
```

This compose file is registry-driven so platforms like Coolify can deploy it directly without building from source. Configure registry credentials in Coolify if the image is private.

## Manual Docker Build

```sh
docker build -t paperclip-local .
docker run --name paperclip \
  -p 3100:3100 \
  -e HOST=0.0.0.0 \
  -e PAPERCLIP_HOME=/paperclip \
  -v "$(pwd)/data/docker-paperclip:/paperclip" \
  paperclip-local
```

## Data Persistence

All data is persisted under the Docker volume mounted at `/paperclip`:

- Embedded PostgreSQL data
- Uploaded assets
- Local secrets key
- Agent workspace data

## Claude and Codex Adapters in Docker

The Docker image pre-installs:

- `claude` (Anthropic Claude Code CLI)
- `codex` (OpenAI Codex CLI)

Pass API keys to enable local adapter runs inside the container:

```sh
docker run --name paperclip \
  -p 3100:3100 \
  -e HOST=0.0.0.0 \
  -e PAPERCLIP_HOME=/paperclip \
  -e OPENAI_API_KEY=sk-... \
  -e ANTHROPIC_API_KEY=sk-... \
  -v "$(pwd)/data/docker-paperclip:/paperclip" \
  paperclip-local
```

Without API keys, the app runs normally — adapter environment checks will surface missing prerequisites.
