---
layout: post
title: "Why we rebuilt our validator around consistency, not speed"
author: "Jordan Schilling"
date: 2026-03-20
category: multi-language-code-evaluation-pipeline
description: "Resetting a multi-language validator after scope drift and establishing fresh JSON artifacts as the single source of truth."
tags: [validator, multi-language, artifacts, reliability, ci]
---

# Devlog - 2026-03-20

## Overview

Today was the reset point for the validator work. The priority was not speed. The priority was trust. We were moving fast, but the output set was drifting and I could not reliably tell what was passing now versus what had passed in a previous run.

## Scope Drift Discovery (343 vs 316)

The biggest signal was scope drift:

* One view of the run showed **343** items in scope
* Another view showed **316** as the actual active set

That mismatch made every summary suspect. A fast pipeline that cannot produce a stable count is not a reliable pipeline.

## What We Changed

I started the validator reset with one hard rule: only trust **freshly generated JSON artifacts** from the current run.

That shifted the operating model:

* Historical snapshots are context, not truth
* Current-run artifacts are the source of truth
* Summary counts are derived from artifacts, not side logs

## Why This Matters

Once the source of truth is unambiguous, everything else becomes easier:

* scoreboards stop oscillating
* language-level status is reproducible
* follow-up automation can safely consume outputs

This set up the next phase: recovering failing languages on top of stable run accounting.
