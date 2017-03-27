---
id: 62
title: Photos
date: 2017-03-27
author: Jan
layout: photos
permalink: /photos/
---

{% include base_path %}

<div class="gridphotos">
{% for image in site.static_files %}
  {%if image.path contains '/photos/' %}
    <div class="gridphotos__item"><a href="{{ site.baseurl }}{{ image.path }}"><img src="{% thumbnail image.path 160x160< %}" alt="image" style="max-height: 160px; float: left; margin: 0.1em"/></a></div>
  {% endif %}
{% endfor %}
</div>
