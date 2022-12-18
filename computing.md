---
layout: default
title: computing
---
# computing projects
i'm just learning jekyll so this'll be a collection of software / hardware related posts, describing in-progress and completed projects. hopefully this isn't too redundant.


## software:
<ul>
    {% for post in site.posts %}
        {% if post.category == "cs" %}
        <li>
        <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
        {{ post.excerpt }}
        </li>
        {% endif %}
    {% endfor %}
</ul>

## hardware / embedded