---
layout: post
title: "How we recovered multiple languages from failing to 100%"
author: "Jordan Schilling"
date: 2026-03-21
category: multi-language-code-evaluation-pipeline
description: "A major recovery day for Java, Rust, and JavaScript driven by pipeline behavior fixes and scoreboard consolidation."
tags: [validator, java, rust, javascript, pipeline, reliability]
---

# Devlog - 2026-03-21

## Overview

Big recovery day. Java, Rust, and JavaScript all moved from unstable outcomes to full pass coverage. The key point is this was not just a code-fix day at the solution layer. Most of the gains came from pipeline behavior changes.

## Recovery Results

Recovered to 100% for:

* **Java**
* **Rust**
* **JavaScript**

At the same time, I consolidated the scoreboard output so all language summaries were being produced from one artifact model instead of mixed counters.

## What Changed in Pipeline Behavior

The recovery came from tightening execution behavior, not hand-tuning individual problems:

* standardized preflight checks before compile/run dispatch
* normalized provider IO so status, stdout, and stderr were interpreted the same way per language
* stabilized submit/poll behavior in batch execution to reduce false timeouts
* enforced one summary path for scoreboard generation from fresh JSON artifacts

## Why This Was Different

Before this, a language could fail for reasons that had nothing to do with the actual solver path. After these changes, a failure was much more likely to represent a real issue rather than pipeline ambiguity.

That is what made the 100% recovery meaningful.
