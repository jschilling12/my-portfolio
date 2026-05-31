---
layout: post
title: "Implementing Observability Across API, Worker, and Gateway"
author: "Jordan Schilling"
date: 2026-05-30
category: game-services-reliability-platform
description: "Adding Prometheus metrics, Grafana dashboards, Jaeger tracing, structured JSON logs, and correlation fields across the API, worker, Redis payloads, and NGINX gateway."
tags: [observability, prometheus, grafana, jaeger, opentelemetry, logging]
---

# Devlog - 2026-05-30

## Overview

Implemented the foundation for **Prometheus** health checks, **Grafana** dashboard visibility, and telemetry through the **Jaeger UI**.

The first step was adding the Python observability dependencies inside `requirements.txt`. From there, metrics were added for the exposed API on `:8000` and the worker metrics endpoint on `:9091`.

## Metrics Added

The current metrics include:

* Latency histogram
* Error count and error rate
* Queue depth
* Worker consumed count
* Worker failures
* Worker processing latency

The gateway now includes an NGINX `stub_status` endpoint plus `nginx/nginx-prometheus-exporter:1.5.1` running on `:9113`.

## Structured Logs and Correlation

Added structured JSON logs so the API creates a `match_request_id` for each join request.

API and worker logs now include these fields when available:

* `request_id`
* `match_request_id`
* `player_id`
* `match_id`

The Redis payload changed from a raw match ID string to JSON containing the same correlation fields. OpenTelemetry context is injected into the Redis JSON payload by the API and extracted by the worker.

## Manual Spans

Created manual spans for the core request lifecycle:

* `join_queue`
* `db.queue_insert`
* `db.match_create`
* `redis.queue_push`
* `worker.consume_match`
* `db.match_ready`

This gives the project a traceable path across the API, database work, Redis queueing, worker processing, and the final database update.

## Grafana Dashboard

Created the `week2-observability.json` dashboard structure with these panels:

* **Request count**: `sum(rate(matchmaking_http_requests_total[5m]))`
* **Error rate**: `sum(rate(matchmaking_http_requests_total{status=~"5.."}[5m])) / sum(rate(matchmaking_http_requests_total[5m]))`
* **p50 latency**: `histogram_quantile(0.50, sum(rate(matchmaking_http_request_duration_seconds_bucket[5m])) by (le))`
* **p95 latency**: `histogram_quantile(0.95, sum(rate(matchmaking_http_request_duration_seconds_bucket[5m])) by (le))`
* **Queue depth**: `max(matchmaking_queue_depth)`
* **Worker throughput**: `sum(rate(worker_matches_consumed_total[5m]))`

## Documentation

Added a Markdown documentation file for observability operations. It includes:

* Symptoms
* Grafana panels
* Prometheus queries
* Jaeger lookup by `request_id`
* Docker log commands
* Common causes
* Recovery steps

## Pass Criteria

This observability milestone is considered complete when:

* `curl -i http://localhost/api/health` returns `200`
* Prometheus shows `prometheus`, `matchmaking-api`, `worker`, and `gateway` as `UP`
* Two join requests create one match
* API and worker logs share the same correlation fields
* Jaeger shows one request across API, DB, Redis, worker, and DB update spans
* Grafana auto-loads Week 2 Observability without manual panel creation
