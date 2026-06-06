# Infrastructure Notes

This repo now has the basic DevOps setup needed to run the full stack locally and build Docker images in CI.

## What I added

- A multi-stage Dockerfile for the backend
- A Dockerfile for the frontend
- A Docker-based reverse proxy for local use
- A `docker-compose.yml` file that starts the database, backend, frontend, and proxy together
- A GitHub Actions workflow that builds the Docker images on every push to `main`
- An `.env.example` file for local environment variables

## How to run it locally

1. Install Docker Desktop.
2. In the project root, create a local env file from the example file.
3. If you named it `.env_eample`, rename it to `.env` before running the app.
4. Start the stack with:

```powershell
docker compose up --build
```

5. Open `http://localhost:8080`.

## Go version

The backend `go.mod` requires `Go 1.25.7`.

If you want to run the backend outside Docker, install that version or newer.

## GitHub Actions

The workflow is located at:

`.github/workflows/docker-ci.yml`

It builds the backend and frontend images when code is pushed to `main`.

If Docker Hub secrets are added in GitHub repository settings, the workflow can also push the images.

## Docker Hub

For Docker Hub, create repositories for the images you want to publish. The simplest setup is to publish the backend and frontend images.

## Submission checklist

- Docker Hub image link
- GitHub Actions workflow URL
- Deployed application URL

## Practical note

The reverse proxy is there so the app can be opened from one public URL while the backend stays behind it. For local development, the whole stack is started through Compose.
