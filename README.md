# Docker Claude CLI

Run [Claude Code](https://github.com/anthropics/claude-code) inside a Docker container, with the container mounting the host working directory so Claude can read and edit your project files.

## Pattern

This pattern is useful when you want to run Claude CLI against a codebase without installing Node.js locally, or when you need a reproducible, isolated environment for AI-assisted development.

The container mounts the directory from which you launch it (or a directory you specify) at `/workspace`, making all files visible to Claude.

## Prerequisites

- Docker and Docker Compose
- An Anthropic account (free or paid) — authentication is done via browser on first run

## Setup

Build the image:

```bash
docker compose build
```

## Usage

Run Claude interactively from within a project directory:

```bash
# From your project directory:
HOST_DIR=$(pwd) docker compose -f /path/to/this/repo/docker-compose.yml run --rm claude

# Or copy the compose file into your project and run:
docker compose run --rm claude
```

On first run, Claude will open a browser window to authenticate with your Anthropic account. After that, credentials are persisted in a Docker named volume (`claude-config`) so you won't be prompted again.

Pass a one-shot prompt directly:

```bash
HOST_DIR=$(pwd) docker compose run --rm claude -p "Explain this codebase"
```

### Overriding the mounted directory

By default the current directory is mounted. Set `HOST_DIR` to mount a different path:

```bash
HOST_DIR=/path/to/project docker compose run --rm claude
```

## File structure

```
.
├── Dockerfile          # Node 22 Alpine image with claude-code installed globally
├── docker-compose.yml  # Mounts HOST_DIR and persists Claude credentials via named volume
├── .gitignore
└── .dockerignore
```
