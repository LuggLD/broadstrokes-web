---
layout: archive
title: "Free Games"
author_profile: false
paginate: false
permalink: /games/free/
---

{% include base_path %}

<p><a href="{{ base_path }}/games/" class="meta">Back to Games</a></p>

<div class="grid__wrapper">
  {% for post in site.free reversed %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>
