---
title: V++
layout: projects
permalink: /vplusplus/
---

<div class="card">
    {% for exp in site.author_work_experiences %} {% if exp.company_name == 'V++'
      %}
      <div class="row">
        <div class="col-md-2">
          <img
            src="{{site.url}}{{site.baseurl}}/assets/img/{{ exp.company_logo }}"
            class="company-logo"
          />
        </div>
        <div class="col-md-6">
          <h4 class="experience-title">{{ exp.designation }}</h4>
          <h6 class="experience-info">{{ exp.company_name}} ({{ exp.duration }})</h6>
          <p class="experience-desc">{{ exp.description }}</p>
        </div>
      </div>
    {% endif %} {% endfor %}
</div>

<div class="row">
    {% for post in site.categories.vplusplus %}
        {%- include project_card.html -%}
    {% endfor %}
</div>