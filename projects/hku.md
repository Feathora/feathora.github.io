---
title: HKU
layout: projects
permalink: /hku/
---

<div class="card">
    {% for exp in site.author_work_experiences %} {% if exp.company_name == 'Utrecht University of the Arts'
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
          <p><a href="{{exp.company_url}}">{{ exp.company_url}}</a></p>
        </div>
      </div>
    {% endif %} {% endfor %}
</div>

<div class="row">
    {% for post in site.categories.hku %}
        {%- include project_card.html -%}
    {% endfor %}
</div>