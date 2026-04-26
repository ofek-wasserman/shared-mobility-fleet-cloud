# Vehicle Sharing API — Advanced Programming Final Project + Cloud Deployment Extension

This repository is a personal continuation of an academic project originally developed as part of Tel Aviv University’s Advanced Programming course.

The original backend system was developed as a team project. The cloud deployment extension was later completed as a smaller academic extension, including Docker-based packaging, GitHub Actions workflows, Google Artifact Registry publishing, and Google Cloud Run deployment.

This repository is now maintained as my personal version of the project, allowing me to continue improving, documenting, and extending it independently.

## Overview

A backend system for a vehicle-sharing service, implementing ride lifecycle management including user registration, ride handling, vehicle allocation, degraded-vehicle reporting, maintenance flows, billing logic, and state persistence.

The system simulates a shared mobility fleet with bicycles, e-bikes, and scooters distributed across stations.

## Tech Stack

- Python 3.12
- FastAPI
- Pytest
- Docker
- GitHub Actions (CI/CD)
- Google Artifact Registry
- Google Cloud Run
- Google Cloud Platform
- Jira for planning and traceability

## Repository Structure

- `src/` → application code
- `tests/` → test suite
- `data/` → input CSV files
- `docs/` → documentation
- `.github/` → CI/CD workflows
- `Dockerfile` → container image definition

## Local Setup

Run the project locally with Python 3.12.

1. Create and activate a virtual environment:

```bash
python3.12 -m venv .venv
source .venv/bin/activate
```

2. Install dependencies:

```bash
python -m pip install -r requirements.txt
```

3. Run tests:

```bash
python -m pytest -q
```

4. Start the API server:

```bash
uvicorn src.main:app --reload
```

5. Open the API documentation in your browser:

```text
http://127.0.0.1:8000/docs
```

## API Usage

After starting the server, the main endpoints are available through the interactive API documentation page at `/docs`.

Main endpoints:

- `POST /register`
- `POST /ride/start`
- `POST /ride/end`
- `POST /vehicle/treat`
- `POST /vehicle/report-degraded`
- `GET /stations/nearest`
- `GET /rides/active-users`

### Example Requests

Register a user:

```http
POST /register
```

```json
{
  "payment_token": "token_123"
}
```

Start a ride:

```http
POST /ride/start
```

```json
{
  "user_id": 1,
  "lat": 32.0853,
  "lon": 34.7818
}
```

## Supported Features

The system supports the following core functionality:

- User registration with a unique payment token
- Starting a ride with deterministic vehicle selection
- Ending a ride with automatic station selection and billing
- Reporting a vehicle as degraded during a ride
- Vehicle treatment and reintegration into stations
- Retrieval of the nearest station based on location
- Retrieval of currently active users
- Runtime state persistence using `state.json`

The API uses structured error handling with appropriate HTTP status codes such as `400`, `404`, and `409`.

## Architecture

The system follows a layered architecture:

### API Layer

Handles HTTP requests, validation, and response mapping using FastAPI.

### Service Layer

Contains business logic and orchestrates workflows such as ride start, ride end, pricing, degraded-vehicle handling, and vehicle treatment.

### Domain Layer

Defines core entities such as Vehicle, Ride, User, and Station, and helps enforce system invariants.

### Data Layer

Responsible for CSV loading and runtime state persistence.

## Original Development Workflow

The original academic project was developed using a structured GitHub workflow:

- Branch per Jira ticket
- Pull requests into `main`
- At least one reviewer approval
- CI validation before merging
- Jira-based task tracking and traceability

This history is intentionally preserved in the repository because it reflects the collaborative development process used during the academic project.

## Current Personal Continuation

This repository now serves as my personal continuation of the project.

Future improvements may be developed independently, using smaller commits, feature branches, pull requests when useful, and CI validation. Since this is now maintained as a personal repository, reviewer approval is no longer required for every change.

## Cloud Deployment Extension

The cloud extension adds deployment capabilities to the original backend system.

It includes:

1. Docker containerization  
   - The application is packaged as a Docker image.

2. GitHub Actions CI/CD workflows  
   - Workflows validate the Docker image build.
   - Additional workflows support publishing images and deploying to Cloud Run.

3. Google Artifact Registry  
   - Docker images can be pushed to Artifact Registry.

4. Google Cloud Run  
   - The API can be deployed as a public cloud service.

## Cloud Extension Workflows

The cloud extension includes a multi-stage CI/CD flow:

1. Validate Docker image build  
   - Workflow: `.github/workflows/docker-validate.yml`

2. Publish image to Artifact Registry  
   - Workflow: `.github/workflows/image-publish.yml`

3. Deploy image to Cloud Run  
   - Workflow: `.github/workflows/cloudrun-deploy.yml`

WIF-based alternatives for cloud authentication are also included:

- `.github/workflows/eden-wif-gcp-deployment.yml`
- `.github/workflows/eden-wif-deploy-cloudrun.yml`

These workflows were part of the academic cloud deployment extension. Future cleanup may consolidate the deployment flow into a single preferred workflow.

The preferred deployment method going forward is a single GitHub Actions workflow using Google Cloud Workload Identity Federation (WIF):

- Workflow: `.github/workflows/deploy-cloud-run.yml`
- Purpose: authenticate with WIF (no JSON key), build/push image to Artifact Registry, and deploy to Cloud Run
- Trigger: `workflow_dispatch`

Legacy cloud deployment workflows are currently kept in the repository for reference and migration safety, and can be removed after migration is complete.

## Deployment Notes

- The deployment region is set to `us-central1`.
- Preferred auth is WIF via `google-github-actions/auth` without service account JSON keys.
- Non-sensitive deployment settings are passed via GitHub Variables.
- WIF identity binding values are recommended to be stored in GitHub Secrets.
- The deployment workflow is manual-only and does not auto-run on push.
- The deployed service is intended to expose the API through a public Cloud Run endpoint.
- Cloud behavior is expected to match local behavior.

## Public Deployment

The system was deployed to Google Cloud Run as part of the cloud deployment extension.

Public API endpoint:

```text
https://fleet-api-otq3yt33kq-uc.a.run.app
```

Interactive API documentation:

```text
https://fleet-api-otq3yt33kq-uc.a.run.app/docs
```

## Notes

- Initial system data is loaded from CSV files in the `data/` directory.
- Runtime state is persisted in `state.json`.
- Some deployment workflows reflect alternative approaches explored during the cloud extension and may be consolidated in future personal development.
