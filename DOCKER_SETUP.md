# Docker Build and Push Setup

This repository is configured to automatically build and push Docker images to `docker.io/mhmgad/dockge-plus` using GitHub Actions.

## Prerequisites

- Docker Hub account (mhmgad)
- GitHub repository with Actions enabled

## GitHub Secrets Configuration

To enable the automated Docker build and push workflow, you need to configure the following GitHub secret:

### Required Secret

**DOCKER_PASSWORD**
- Navigate to your repository settings: `Settings` → `Secrets and variables` → `Actions`
- Click `New repository secret`
- Name: `DOCKER_PASSWORD`
- Value: Your Docker Hub password or access token

#### Getting a Docker Hub Access Token (Recommended)

Using an access token is more secure than using your password:

1. Log in to [Docker Hub](https://hub.docker.com/)
2. Go to `Account Settings` → `Security` → `Access Tokens`
3. Click `New Access Token`
4. Give it a descriptive name (e.g., "GitHub Actions - dockge-plus")
5. Set permissions to `Read, Write, Delete` (or `Read & Write` minimum)
6. Copy the generated token and save it as the `DOCKER_PASSWORD` secret

## Workflow Overview

The workflow (`.github/workflows/docker-build-push.yml`) consists of three jobs:

### 1. Build Base Image (`build-base`)
- Builds the base Node.js image with Docker CLI
- Tags: `mhmgad/dockge-plus:base`
- Platforms: `linux/amd64`, `linux/arm64`, `linux/arm/v7`

### 2. Build Healthcheck Image (`build-healthcheck`)
- Builds the healthcheck binary using Go
- Tags: `mhmgad/dockge-plus:build-healthcheck`
- Platforms: `linux/amd64`, `linux/arm64`, `linux/arm/v7`

### 3. Build Main Application Image (`build-main`)
- Builds the main Dockge application
- Tags: `mhmgad/dockge-plus:latest`, `mhmgad/dockge-plus:<version>`, and more
- Platforms: `linux/amd64`, `linux/arm64`, `linux/arm/v7`
- Depends on: base and healthcheck images

## Triggering the Workflow

The workflow runs automatically on:
- **Push to master branch**: Builds and pushes all images
- **Pull requests**: Builds images without pushing (for validation)
- **Manual trigger**: Use the "Run workflow" button in GitHub Actions
- **Version tags**: When you push tags like `v1.0.0`, it creates versioned images

## Docker Images Produced

After a successful workflow run, the following images will be available on Docker Hub:

- `mhmgad/dockge-plus:base` - Base image with Node.js and Docker CLI
- `mhmgad/dockge-plus:build-healthcheck` - Healthcheck binary
- `mhmgad/dockge-plus:latest` - Latest main application (from master branch)
- `mhmgad/dockge-plus:<version>` - Specific version (e.g., 1.4.2)
- `mhmgad/dockge-plus:master` - Latest from master branch

## Using the Docker Images

### With Docker Compose

The `compose.yaml` file in this repository is configured to use the new image:

```yaml
services:
  dockge:
    image: mhmgad/dockge-plus:latest
    # ... rest of configuration
```

Run with:
```bash
docker compose up -d
```

### Standalone Docker Run

```bash
docker run -d \
  -p 5001:5001 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v ./data:/app/data \
  -v /opt/stacks:/opt/stacks \
  -e DOCKGE_STACKS_DIR=/opt/stacks \
  --name dockge \
  mhmgad/dockge-plus:latest
```

## Troubleshooting

### Workflow Fails with Authentication Error

- Verify the `DOCKER_PASSWORD` secret is correctly set
- If using Docker Hub password, consider switching to an access token
- Check that the Docker Hub account `mhmgad` has permissions to push to `dockge-plus` repository

### Image Not Found When Running Compose

- Ensure the workflow has completed successfully
- Check Docker Hub to verify images were pushed: https://hub.docker.com/r/mhmgad/dockge-plus
- Try pulling manually: `docker pull mhmgad/dockge-plus:latest`

### Build Fails for Specific Platform

- Check the workflow logs in GitHub Actions
- Verify QEMU and Docker Buildx are properly configured (handled automatically in workflow)

## Notes

- The workflow uses GitHub Actions cache to speed up subsequent builds
- Pull requests will build but not push images to Docker Hub
- Multi-platform builds may take 10-20 minutes depending on the changes
