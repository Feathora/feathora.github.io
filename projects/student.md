---
title: Student
layout: projects
permalink: /student/
---

<div class="row">
    {% for post in site.categories.student %}
        {%- include project_card.html -%}
    {% endfor %}
</div>