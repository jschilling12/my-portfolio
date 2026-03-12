---
layout: default
title: "Jordan Schilling Writing | Articles on Engineering and Automation"
description: "Technical articles, project breakdowns, and engineering insights written by Jordan Schilling."
permalink: /jordan-schilling-writing
---

# Writing

This page collects articles, technical insights, and project breakdowns. Each piece reflects a hands-on approach to software engineering, automation, and building practical tools.

---

## Recent Articles

{% for post in site.posts %}
* {{ post.date | date: "%b %d, %Y" }} — [{{ post.title }}]({{ site.baseurl }}{{ post.url }})
{% endfor %}

---

## Topics I Write About

### Automation Engineering

Automation is at the core of my engineering practice. These articles explore test automation frameworks, CI/CD pipeline design, infrastructure automation, and the principles behind building reliable automated systems — not just the theory, but the obstacle resolutions and design decisions that arise during implementation.

### Experimental Software Projects

I approach software engineering as a series of experiments. These articles document the process of building experimental tools, exploring new technologies, and pushing the boundaries of what's possible with code. From CLI productivity tools to AI-assisted development, writing about the journey of exploration.

### DevOps and Infrastructure

DevOps isn't just a role — it's an engineering philosophy. These articles cover building CI/CD pipelines, configuring infrastructure, deploying applications, and the practices that make software systems reliable and maintainable.

### Developer Productivity

I build tools for developers and write about the intersection of tooling, workflow, and productivity. From the FocusBuddy CLI to terminal configuration, these articles explore how developers can work more effectively.

---

## Development Logs

I maintain detailed development logs for each active project. These logs are chronological records of technical decisions, challenges, and progress:

* [Python-Playwright Dev Log](./python-playwright.html) — Automation framework development
* [FocusBuddy Dev Log](./focus-buddy.html) — CLI tool and process monitor
* [Portfolio Dev Log](./portfolio-dev.html) — Site construction and engineering

---

[Back to homepage →](./) | [View projects →](./)
