---
layout: default
---

# Project Updates

{% for post in site.categories.portfolio-dev %}
* {{ post.date | date: "%b %d, %Y" }} - [{{ post.title }}]({{ site.baseurl }}{{ post.url }})
{% endfor %}