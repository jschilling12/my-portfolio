---
layout: post
title: "Closing the loop between validator and UI metadata"
author: "Jordan Schilling"
date: 2026-03-23
category: multi-language-code-evaluation-pipeline
description: "Building a validator-to-UI metadata sync utility so validated language status is accurately reflected in product behavior."
tags: [validator, ui, metadata, automation, sync]
---

# Devlog - 2026-03-23

## Overview

Today was about closing a gap that caused product confusion: validator status and UI language metadata were not always aligned. A language could be validated in backend artifacts but still appear incorrectly in UI behavior.

## Problem

Two systems were moving at different speeds:

* validator outputs were updated per run
* UI metadata could lag behind and expose stale availability

That created mismatches in what users could select versus what the validator had actually confirmed.

## Utility Added

I added a validator-to-UI sync utility that reads current validation artifacts and updates language metadata in one pass.

Core behavior:

* consumes fresh validation artifact JSON
* maps language status into UI availability flags
* updates metadata deterministically from current run state
* logs each change for traceability

## Product Impact

This removed a whole class of inconsistent behavior:

* validated languages are correctly marked available
* failing or unverified languages are clearly handled
* release behavior now reflects validator truth, not stale metadata

With this in place, the validator and product surface finally operate as one system.
