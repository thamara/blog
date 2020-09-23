---
layout: page
title: Archive
---

<div class="page">
    <br>
    <ul>
    {% for post in site.posts %}
        <li>
        <span class="post-date">{{ post.date | date_to_string }} | <a href="{{ post.url }}">{{ post.title }}</a></span>
        </li>
    {% endfor %}
    </ul>
</div>
