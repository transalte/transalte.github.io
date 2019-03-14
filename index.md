---
title: transalte~
layout: default
category: none
order: 1
permalink:
summary: Front Page
lastupdate: 20-12-2018
---


## About These Notes...

If you are new to Max/M4L, or needing a refresh, it is recommended that you read through the Max Fundamentals section. For L08 (ASP) and L09 (AASP). 

<details><summary>Max / Max For Live Fundamentals</summary><ul>{% for doc in site.docs %}
{% if doc.category == "Max" %}
<li><a href="{{ doc.url }}">{{ doc.title }}</a></li>
          {% endif %}
        {% endfor %}</ul></details>


<details><summary>Audio Effects</summary><ul>{% for doc in site.docs %}
{% if doc.category == "asp_lessons" %}
<li><a href="{{ doc.url }}">{{ doc.title }}</a></li>
          {% endif %}
        {% endfor %}</ul></details>

<details><summary>Instruments</summary><ul>{% for doc in site.docs %}
{% if doc.category == "aasp_lessons" %}
<li><a href="{{ doc.url }}">{{ doc.title }}</a></li>
          {% endif %}
      {% endfor %}</ul></details>
