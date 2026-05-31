---
layout: post
title: "Tracing the Request Path Through the Matchmaking Stack"
author: "Jordan Schilling"
date: 2026-05-06
category: game-services-reliability-platform
description: "Documenting the current request path from NGINX through Uvicorn and FastAPI into PostgreSQL and Redis, plus the planned role of the worker service."
tags: [nginx, fastapi, uvicorn, postgresql, redis, architecture]
---

# Devlog - 2026-05-06

## Overview

The current request path is now clear: client traffic first goes through **NGINX**, which acts as the gateway and reverse proxy. It receives external requests on port `80` and forwards `/api/` traffic to the FastAPI matchmaking API running on Uvicorn.

NGINX is not truly load balancing yet because it only points to one matchmaking API container, but it is already positioned as the gateway layer where load balancing can be added later.

## API and Server Layer

The matchmaking API is built with **FastAPI** and exposes endpoints such as:

* `POST /queue/join`
* `GET /match/{match_id}`
* `GET /health`
* `GET /ready`

**Uvicorn** is the ASGI server that runs the FastAPI app. It starts the app through `uvicorn app:app`, imports the `app.py` file, finds the `app = FastAPI(...)` object, and serves it using the ASGI interface.

Conceptually, Uvicorn and ASGI are useful for async I/O-heavy APIs because an async endpoint can pause while waiting on Redis, PostgreSQL, or another external service, allowing the server to continue handling other requests. That is the async event loop model: while one player request is waiting on I/O, another player request can make progress.

In the current codebase, however, the route handlers and Redis/PostgreSQL clients are synchronous, so the app is not yet using explicit `await`-based async I/O. Uvicorn still runs the FastAPI app, but the current concurrency behavior comes more from FastAPI and Uvicorn's synchronous handler execution model than from async Redis or async PostgreSQL calls.

## Matchmaking Flow

When a player joins matchmaking, the API writes that player into the PostgreSQL queue table. It then searches PostgreSQL for another compatible queued player based on rank.

If there are not enough compatible players, the player remains queued in PostgreSQL. If two compatible players are found, the API removes both players from the queue table, creates a new match record in PostgreSQL, and pushes the new match ID into Redis under `matches:pending`.

Right now, PostgreSQL is the source of truth for queued players and created matches. Redis is being used as a lightweight job queue for match IDs that need background processing.

## Worker Service Direction

The worker service is intended to consume Redis jobs later and perform background tasks such as:

* Updating match status
* Processing match lifecycle work
* Writing additional state to the database
* Supporting observability or telemetry workflows

That worker logic has not been implemented yet.

## Current System Shape

The current system can be summarized as:

```text
NGINX gateway -> Uvicorn ASGI server -> FastAPI matchmaking API -> PostgreSQL durable state -> Redis pending match jobs -> future worker processing
```
