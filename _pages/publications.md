---
layout: page
permalink: /research/
title: research
description: a list of my pre-prints and publications, in reverse chronological order.
nav: true
nav_order: 1
---

also see [Google Scholar](https://scholar.google.com/citations?user=89nZKJgAAAAJ).
<!-- _pages/publications.md -->
<div class="publications">

{% bibliography -f {{ site.scholar.bibliography }} %}
{% assign publications = 'your generated publication list here' %}
{% for publication in publications %}
  {% assign modified_publication = publication | replace: 'Kushal Thaman', '<strong>Kushal Thaman</strong>' %}
  {{ modified_publication }}
{% endfor %}

</div>
