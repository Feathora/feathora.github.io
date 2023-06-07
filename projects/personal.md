---
title: Personal
layout: projects
permalink: /personal/
---

<div class="row">
    {% for post in site.categories.personal %}
        {%- include project_card.html -%}
    {% endfor %}
</div>