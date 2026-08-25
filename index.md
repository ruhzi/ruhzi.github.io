---
layout: default
title: Writing
---
<p class="intro">{{ site.description }}</p>

<ul class="post-list">
{% for post in site.posts %}
  <li>
    <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%b %Y" }}</time>
    <div>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      {% if post.description %}<p class="desc">{{ post.description }}</p>{% endif %}
    </div>
  </li>
{% endfor %}
</ul>
