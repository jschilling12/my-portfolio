---
layout: default
---

# Project Updates

{% for post in site.categories.inventory-management %}
* {{ post.date | date: "%b %d, %Y" }} - [{{ post.title }}]({{ site.baseurl }}{{ post.url }})
{% endfor %}