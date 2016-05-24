---
layout: archive
title: "Past Gamejams"
author_profile: false
paginate: false
permalink: /games/gamejams/
---

{% include base_path %}

<p><a href="{{ base_path }}/games/" class="meta">Back to Games</a></p>

<div class="grid__wrapper">
  {% for post in site.gamejams reversed %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>