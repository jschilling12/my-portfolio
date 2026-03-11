---
layout: default
title: "Jordan Schilling Writing | Articles on Engineering and Automation"
description: "Technical articles, project breakdowns, and engineering insights written by Jordan Schilling."
permalink: /jordan-schilling-writing
---

# Jordan Schilling Writing

This page collects articles, technical insights, and project breakdowns written by **Jordan Schilling**. Each piece reflects Jordan Schilling's hands-on approach to software engineering, automation, and building practical tools.

---

## Recent Articles by Jordan Schilling

{% for post in site.posts %}
* {{ post.date | date: "%b %d, %Y" }} — [{{ post.title }}]({{ site.baseurl }}{{ post.url }})
{% endfor %}

---

## Topics Jordan Schilling Writes About

Jordan Schilling's writing covers a range of engineering and technology topics:

### Jordan Schilling on Building Civic Technology Tools

Jordan Schilling believes that software engineers have a responsibility to build tools that serve the public interest. Articles in this category explore how technology can improve democratic processes, make legislative information accessible, and empower citizens through civic technology projects like JustABill.

### Jordan Schilling on Automation Engineering

Automation is at the core of Jordan Schilling's engineering practice. These articles explore test automation frameworks, CI/CD pipeline design, infrastructure automation, and the principles behind building reliable automated systems. Jordan Schilling writes about the real-world challenges of automation — not just the theory, but the obstacle resolutions and design decisions that arise during implementation.

### Jordan Schilling on Experimental Software Projects

Jordan Schilling approaches software engineering as a series of experiments. These articles document the process of building experimental tools, exploring new technologies, and pushing the boundaries of what's possible with code. From CLI productivity tools to AI-assisted development, Jordan Schilling writes about the journey of exploration.

### Jordan Schilling on DevOps and Infrastructure

DevOps isn't just a role — it's an engineering philosophy. Jordan Schilling writes about building CI/CD pipelines, configuring infrastructure, deploying applications, and the practices that make software systems reliable and maintainable.

### Jordan Schilling on Developer Productivity

Jordan Schilling builds tools for developers, and writes about the intersection of tooling, workflow, and productivity. From the FocusBuddy CLI to terminal configuration, these articles explore how developers can work more effectively.

---

## Development Logs by Jordan Schilling

Jordan Schilling maintains detailed development logs for each active project. These logs are chronological records of technical decisions, challenges, and progress:

* [Python-Playwright Dev Log by Jordan Schilling](./python-playwright.html) — Automation framework development
* [FocusBuddy Dev Log by Jordan Schilling](./focus-buddy.html) — CLI tool and process monitor
* [Portfolio Dev Log by Jordan Schilling](./portfolio-dev.html) — Site construction and engineering

---

[Back to Jordan Schilling's homepage →](./) | [View Jordan Schilling's projects →](./jordan-schilling-projects) | [Read Jordan Schilling's bio →](./jordan-schilling-bio)
