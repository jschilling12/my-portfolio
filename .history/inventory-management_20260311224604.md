---
layout: default
title: "Inventory Management Dev Log | Web App Build Notes"
description: "Development log for the inventory management web application — backend architecture, deployment, and feature development."
permalink: /inventory-management
---

# Inventory Management Dev Log

Build notes and progress on the inventory management web application — backend architecture, deployment, and feature development.

{% for post in site.categories.inventory-management %}
* {{ post.date | date: "%b %d, %Y" }} - [{{ post.title }}]({{ site.baseurl }}{{ post.url }})
{% endfor %}