---
layout: post
title: "Adding Health Checks and Matchmaking Endpoints"
author: "Jordan Schilling"
date: 2026-04-27
category: game-services-reliability-platform
description: "Adding CLI health checks, standing up the Docker runtime, and building the first FastAPI matchmaking endpoints for queueing and match inspection."
tags: [fastapi, docker, health-checks, api, redis, postgresql]
---

# Devlog - 2026-04-27

## Overview

Added a CLI health check so each Docker startup can be verified across the running instances. This gives the project a quick way to confirm whether the local platform is actually ready before moving into deeper API or observability work.

## Initial FastAPI App

Built a minimal FastAPI application with the first matchmaking endpoints:

* `POST /queue/join`
* `GET /match/{id}`
* `GET /health`
* `GET /ready`

## Docker Runtime

The Docker container setup is now standing up with health checks across the core packages, including Redis and PostgreSQL. This is the first version of the platform where the services can be brought up together and checked as a system instead of inspected one container at a time.

## References

* [DevOps Foundations: Your First Project](https://www.linkedin.com/learning/devops-foundations-your-first-project-24355651/your-first-project-devopsified-24080523?u=2045532)
* [DevOps Foundations: Containers](https://www.linkedin.com/learning/devops-foundations-containers-14207858?u=2045532)
* [DevOps Foundations](https://www.linkedin.com/learning/devops-foundations-23454205?u=2045532)
* [Introduction to Docker](https://platform.qa.com/course/introduction-to-docker-2/course-intro-1/)
* [DevOps Playbook Part 1](https://platform.qa.com/course/devops-playbook-part1/devops-adoption-playbook-part-1-intro/)
