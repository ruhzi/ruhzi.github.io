---
layout: default
title: Papers
permalink: /papers/
---
<ul class="post-list">
{% assign papers = site.pages | where: "layout", "pdf-redirect" | sort: "date" | reverse %}
{% for p in papers %}
  <li>
    <time datetime="{{ p.date | date_to_xmlschema }}">{{ p.date | date: "%b %Y" }}</time>
    <div>
      <a href="{{ p.pdf | relative_url }}">{{ p.title }}</a>
      <p class="desc">{{ p.authors | join: ", " }}{% for l in p.links %} <span class="sep">·</span> <a class="paper-extra" href="{{ l.url }}">{{ l.name }}</a>{% endfor %}</p>
    </div>
  </li>
{% endfor %}
</ul>
