---
layout: post
title: "316 of 316: what a stable validator artifact pipeline looks like"
author: "Jordan Schilling"
date: 2026-04-05
category: multi-language-code-evaluation-pipeline
description: "Reaching a clean 316/316 milestone and documenting the operational model that keeps a multi-language validator artifact pipeline stable."
tags: [validator, artifacts, operations, ci, stability]
---

# Devlog - 2026-04-05

## Overview

Today we hit a clean milestone: **316 of 316**. More important than the number is the operating model behind it. The pipeline is now stable because each layer has a clear contract and artifacts are treated as the canonical record.

## UML System Design

```mermaid
flowchart LR
	D[Developer / CI] -->|trigger run| VR[Validator Runner (CLI/Job)]

	subgraph VCP[Validation Control Plane]
		VR
		STG[Starter Template Gate]
		ESF[Eligibility + Scope Filter]
		PTCG[Problem/Test Catalog Gateway]
		GC[Guardrail Comparator]
		CR[Contract Registry]
		ML[Metrics + Logs]
		AW[Artifact Writer]
	end

	subgraph EP[Execution Plane]
		EG[Execution Gateway]
		BCP[Batch Client + Poller]
		CRPA[Compile/Run Provider Adapter]
		ECRP[External Compile/Run Provider (Redacted)]
	end

	VR -->|preflight starter checks| STG
	VR -->|apply scope| ESF
	VR -->|load problems/tests| PTCG
	VR -->|evaluate outputs| GC
	VR -->|summary + metadata| AW
	VR --> ML
	VR -->|execute candidate code| EG

	GC -->|resolve rules| CR
	GC -->|pass/fail + diagnostics| AW
	GC --> ML

	EG <--> |submit + poll| BCP
	BCP <--> |normalize provider IO| CRPA
	CRPA -->|compile/run| ECRP
	ECRP -->|status/stdout/stderr| CRPA

	EG --> ML

	AW -->|persist reports| VA[(Validation Artifacts (JSON))]
```

## What Stable Looks Like

The architecture now behaves predictably across both control and execution planes:

* validator runner orchestrates one deterministic run path
* scope and starter gates run before expensive execution
* execution gateway and provider adapter normalize compile/run behavior
* guardrail comparison resolves pass/fail consistently
* artifact writer persists summary, metadata, and diagnostics as JSON

## Operational Model

To keep this stable over time, we now run with explicit rules:

* always derive status from fresh artifacts
* avoid mixing counters from side channels
* keep language scoreboard generation centralized
* sync validator status back into UI metadata after validation
* log metrics and diagnostics for every run

## Why This Milestone Matters

A stable validator is not only about passing tests. It is about producing outputs that teams can trust for product decisions, language availability, and release confidence.

316 of 316 is the result, but repeatability is the real win.
