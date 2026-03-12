---
layout: default
title: "Python Playwright Dev Log | Automation Framework Build Notes"
description: "Development log tracking a Python Playwright automation framework using OOP and the Page Object Model pattern."
permalink: /python-playwright
---

# Python Playwright Dev Log

Build notes, technical decisions, and iterative progress on a Python x Playwright automation framework using OOP and the Page Object Model (POM) pattern.

{% for post in site.categories.python-playwright %}
* {{ post.date | date: "%b %d, %Y" }} - [{{ post.title }}]({{ site.baseurl }}{{ post.url }})
{% endfor %}