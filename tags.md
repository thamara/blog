---
layout: page
title: Tags
---

<div class="page">
	{% assign sorted_tags = site.data.tags | sort_by :"name" %}
	{% for tag in sorted_tags %}
		{% include tag_link_formatter.html tag=tag tag_separator='<br />' %}
	{% endfor %}
</div>
