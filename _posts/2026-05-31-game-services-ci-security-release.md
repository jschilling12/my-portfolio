---
layout: post
title: "Green-Gated Releases and Blue-Green Deployment Planning"
author: "Jordan Schilling"
date: 2026-05-31
category: game-services-reliability-platform
description: "Defining GitHub Actions workflows for CI, security scanning, SBOM generation, release publishing, and the first blue-green deployment structure."
tags: [github-actions, ci-cd, trivy, syft, sbom, cosign, ghcr, blue-green-deployment, docker-compose, nginx]
---

# Devlog - 2026-05-31

## Overview

Started shaping the GitHub Actions model into one green-gated flow: lint, test, build image, scan image, generate SBOM, upload SBOM, and push SHA-tagged images only if checks pass.

I also developed the first **blue-green deployment** structure for the platform. The deployment layers are now visible in the framework: `dev`, `staging`, `blue`, `green`, and `rollback`.

The workflow strategy is split into three files, each answering a different operational question.

## Workflow Responsibilities

| Workflow | Covers | Does Not Cover |
|---|---|---|
| `ci.yml` | Does the code build? | Deep security, publishing, releases |
| `security.yml` | Is the code or image vulnerable? | Normal app correctness beyond building images |
| `release.yml` | Can we publish a real version? | PR validation or nightly scanning |

## CI Workflow

CI answers: **Does the app build?**

The current CI installs dependencies, runs a syntax check, and builds Docker images. It does not try to own the full security or release-publishing process.

`ci.yml` is designed for fast PR checks and runs on pushes or PRs to normal branches.

It currently covers:

* Checking out code
* Setting up Python
* Installing service dependencies
* Running a Python syntax check with `compileall`
* Building Docker images for `matchmaking-api` and `worker`

## Release Workflow

Release answers: **Are we publishing a real version?**

This workflow only runs on version tags, such as:

```bash
git tag v1.0.0
git push origin v1.0.0
```

It builds images, pushes them to GitHub Container Registry, and signs them with Cosign.

## Security Workflow

Security scan answers: **Are there vulnerabilities or supply-chain issues?**

`security.yml` runs deeper security scanning on pushes and PRs to `main`, plus nightly. It uses Trivy for filesystem and image scans, uploads SARIF results to GitHub Security, and generates SBOM artifacts with Syft.

It currently covers:

* Trivy filesystem scan of the repo
* Trivy Docker image scan for `matchmaking-api`
* Trivy Docker image scan for `worker`
* SARIF upload to the GitHub Security tab
* SBOM generation with Syft
* SBOM artifact upload

## Why This Matters

Through GitHub Actions, the framework can now support multiple automation stages during version control. Fast CI stays focused on build confidence, deeper security checks focus on vulnerabilities and supply chain visibility, and release automation handles the path to real versioned packages and signed container images.

## Blue-Green Deployment Structure

The key deployment idea is that `compose.yml` no longer chooses the API version directly. Dev and staging choose the API version, while NGINX chooses where traffic goes.

The current tree shape:

```text
infra/compose/
  compose.yml
  compose.dev.yml
  compose.staging.yml
  env/
    dev.env
    staging.env
gateway/
  nginx.conf
  upstreams/
    blue.conf
    green.conf
    canary.conf
docs/runbooks/
  rollback.md
```

## Compose Responsibilities

The base Compose file keeps only shared behavior.

`compose.dev.yml` adds `build:` so development can rebuild local services quickly.

`compose.staging.yml` adds `image:` so staging can run tagged images instead of local build output.

The next step is setting up health checks and canary checks to decide whether the blue or green deployment is stable and ready for traffic. If the new color is not healthy, the rollback path should be triggered.

## Run Dev

From the repo root:

```bash
docker compose \
  --env-file infra/compose/env/dev.env \
  -p game-dev \
  -f infra/compose/compose.yml \
  -f infra/compose/compose.dev.yml \
  up -d --build
```

Check it:

```bash
docker compose -p game-dev ps
curl http://localhost/api/health
```

Expected response:

```json
{"status":"ok","match_flow":"v1","deploy_color":"blue"}
```

## Run Staging

After real GHCR image tags are available in `staging.env`:

```bash
docker compose \
  --env-file infra/compose/env/staging.env \
  -p game-staging \
  -f infra/compose/compose.yml \
  -f infra/compose/compose.staging.yml \
  up -d
```

Check it:

```bash
docker compose -p game-staging ps
curl http://localhost:8080/api/health
```

## Switch Blue to Green

Edit `infra/compose/env/staging.env`:

```text
NGINX_UPSTREAM_FILE=green.conf
```

Then recreate only NGINX:

```bash
docker compose \
  --env-file infra/compose/env/staging.env \
  -p game-staging \
  -f infra/compose/compose.yml \
  -f infra/compose/compose.staging.yml \
  up -d --force-recreate gateway
```

Verify:

```bash
curl http://localhost:8080/api/health
```

Expected response:

```json
{"status":"ok","match_flow":"v2","deploy_color":"green"}
```

## Rollback

Set the upstream file back:

```text
NGINX_UPSTREAM_FILE=blue.conf
```

Then run the same gateway recreate command. That becomes the less-than-five-minute rollback path.

## Deployment Principle

The core separation is now:

* `compose.yml` owns shared service behavior
* Dev and staging override how versions are selected
* NGINX controls which color receives traffic
* Rollback becomes an upstream switch instead of a full environment rebuild
