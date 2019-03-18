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


<div class="contentGrid">

<div class="gridItem">

Max / Max For Live Fundamentals
<ul>{% for doc in site.docs %}
{% if doc.category == "Max" %}
<li><a href="{{ doc.url }}">{{ doc.title }}</a></li>
          {% endif %}
        {% endfor %}</ul>
</div>

<div class="gridItem">

Audio Effects

<ul>{% for doc in site.docs %}
{% if doc.category == "asp_lessons" %}
<li><a href="{{ doc.url }}">{{ doc.title }}</a></li>
          {% endif %}
        {% endfor %}</ul>

</div>


<div class="gridItem">
Instruments

<ul>{% for doc in site.docs %}
{% if doc.category == "aasp_lessons" %}
<li><a href="{{ doc.url }}">{{ doc.title }}</a></li>
          {% endif %}
      {% endfor %}</ul>
</div>
</div>
