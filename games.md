---
id: 62
title: Games
date: 2014-09-29T14:08:45+00:00
author: Jan
layout: archive
permalink: /games/
guid: http://www.broad-strokes.com/?page_id=62
---

{% include base_path %}

<div class="grid__wrapper">
  {% for post in site.games reversed %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>

<h2>Gamejams</h2>

<div class="grid__wrapper">
  {% for post in site.gamejams reversed %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>