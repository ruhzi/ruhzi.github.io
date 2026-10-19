---
layout: default
title: Papers
permalink: /papers/
---
<ul class="post-list">
{% assign papers = site.pages | where: "layout", "paper" | sort: "date" | reverse %}
{% for p in papers %}
  <li>
    <time datetime="{{ p.date | date_to_xmlschema }}">{{ p.date | date: "%b %Y" }}</time>
    <div>
      <a href="{{ p.url | relative_url }}">{{ p.title }}</a>
      <p class="desc">{{ p.authors | join: ", " }}</p>
    </div>
  </li>
{% endfor %}
</ul>
