---
layout: default
title: computing
---
# computing projects
this is a list of my software / hardware related posts, describing in-progress and completed projects.


## software:
<ul>
    {% for post in site.posts %}
        {% if post.category == "cs" %}
        <li>
            {% if post.completed != "in progress"%}
                <h3><a href="{{ post.url }}">{{ post.title | downcase}} (completed {{post.completed | date_to_string}})</a></h3>
            {% else %}
                <h3><a href="{{ post.url }}">{{ post.title | downcase}} (in progress)</a></h3>
            {% endif %}
        </li>
        {% endif %}
    {% endfor %}
</ul>

## hardware / embedded

<ul>
    {% for post in site.posts %}
        {% if post.category == "hw" %}
        <li>
            {% if post.completed != "in progress"%}
                <h3><a href="{{ post.url }}">{{ post.title | downcase}} (completed {{post.completed | date_to_string}})</a></h3>
            {% else %}
                <h3><a href="{{ post.url }}">{{ post.title | downcase}} (in progress)</a></h3>
            {% endif %}
        </li>
        {% endif %}
    {% endfor %}
</ul>