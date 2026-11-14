---
layout: default
title: Ruhaan Chopra
---
<div class="home">

<p class="bio">Working on evals and RL envs for unverifiable domains like research taste.</p>

<p class="links">{% include social-links.html %}</p>


<h2 id="work">Papers and writing</h2>
<ul class="post-list">
{% assign papers = site.pages | where: "layout", "pdf-redirect" | sort: "date" | reverse %}
{% for p in papers %}
  <li>
    <time datetime="{{ p.date | date_to_xmlschema }}">{{ p.date | date: "%b %Y" }}</time>
    <div>
      <a href="{{ p.pdf | relative_url }}">{{ p.title }}</a>
      {% if p.note or p.links %}<p class="desc">{{ p.note }}{% for l in p.links %}{% if p.note or forloop.first == false %} <span class="sep">·</span> {% endif %}<a class="paper-extra" href="{{ l.url }}">{{ l.name }}</a>{% endfor %}</p>{% endif %}
    </div>
  </li>
{% endfor %}
{% for p in site.data.papers %}
  <li>
    <time datetime="{{ p.date | date_to_xmlschema }}">{{ p.date | date: "%b %Y" }}</time>
    <div>
      <a href="{{ p.url }}">{{ p.title }}</a>
      {% if p.note %}<p class="desc">{{ p.note }}</p>{% endif %}
    </div>
  </li>
{% endfor %}
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

</div>
