---
layout: default
---

# Project Updates

{% for post in site.categories.python-playwright %}
* {{ post.date | date: "%b %d, %Y" }} - [{{ post.title }}]({{ site.baseurl }}{{ post.url }})
{% endfor %}