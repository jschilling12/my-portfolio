---
layout: post
title: "Planning the Game Services Reliability Platform"
author: "Jordan Schilling"
date: 2026-04-23
category: game-services-reliability-platform
description: "Planning a production-style backend platform for simulated game services, with DevOps, DevSecOps, observability, and reliability engineering as the core goals."
tags: [backend, devops, devsecops, reliability-engineering, docker]
---

# Devlog - 2026-04-23

## Overview

Developed the plan and framework that the entire project would be built behind: a production-style backend platform designed to demonstrate **DevOps, DevSecOps, and reliability engineering** through a small distributed system.

This project is focused on operating and securing backend services, not building a playable game. It simulates a real service environment where traffic can be routed, jobs processed, data stored, deployments automated, and failures observed.

## Core Architecture

The initial platform design includes:

* API service
* Worker service
* PostgreSQL database
* Redis queue
* Monitoring and logging
* CI/CD pipeline

## Planning Artifacts

The project started with a structured plan covering class diagrams, project scaffolding, a Mermaid-style architecture diagram, a roadmap, a TODO list, and source notes.

The intended shape is a single repo that is:

* Docker-driven
* Local-development friendly
* Kubernetes-enabled later
* Designed for secrets management through Vault
* Designed for policy checks through OPA
* Built around continuously manipulated topology data

Docker Compose was selected for the first version because it supports multi-container applications, health checks, named volumes, and multiple Compose files that can be merged or overridden by file order. That makes it practical for quick development and staging spin-ups under a strict timeline.

## Terraform Environment Note

HashiCorp's Terraform documentation warns that CLI workspaces are separate state instances, not a true environment-separation model when different environment variables, IAM credentials, and isolation boundaries are required. That note will shape how later infrastructure work is structured.

## Recommended v1 Stack

The first concrete stack target is:

* **FastAPI** for API services and OpenAPI-based endpoint development
* **Redis** as the queue, using documented reliable queue patterns with blocking operations
* **PostgreSQL** for durable relational state
* **NGINX** as the gateway, traffic switch, and future load-balancing layer
* **Docker Compose** for local orchestration
* **Prometheus + Grafana** for metrics, dashboards, and alerting visibility
* **OpenTelemetry + Jaeger** for distributed traces
* **GitHub Actions** for CI/CD
* **Trivy + Syft** for image scanning and SBOM generation
* **Cosign** as an optional artifact signing stretch goal if the security work finishes early
* **tc/netem** for latency injection and failure simulation

## References

* [Terraform CLI workspaces](https://developer.hashicorp.com/terraform/cli/workspaces)
