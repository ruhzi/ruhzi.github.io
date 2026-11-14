---
layout: default
title: Ruhaan Chopra
---
<div class="home">

<p class="bio">I'm Ruhaan Chopra. I work on evaluation and reinforcement-learning post-training for language models. Most of it is about research taste: whether a model can predict how an experiment will turn out before running it, and how to train that.</p>

<p class="links">{% include social-links.html %}</p>

<h2 id="now">Now</h2>
<p>Using frontier models as judges that write the reward inside an RL loop, and measuring what that does to the small model being trained.</p>

<h2 id="work">Papers and writing</h2>
<ul class="post-list">
{% assign papers = site.pages | where: "layout", "pdf-redirect" | sort: "date" | reverse %}
{% for p in papers %}
  <li>
    <time datetime="{{ p.date | date_to_xmlschema }}">{{ p.date | date: "%b %Y" }}</time>
    <div>
      <a href="{{ p.pdf | relative_url }}">{{ p.title }}</a>
      <p class="desc">Paper{% for l in p.links %} <span class="sep">·</span> <a class="paper-extra" href="{{ l.url }}">{{ l.name }}</a>{% endfor %}</p>
    </div>
  </li>
{% endfor %}
{% for p in site.data.papers %}
  <li>
    <time datetime="{{ p.date | date_to_xmlschema }}">{{ p.date | date: "%b %Y" }}</time>
    <div>
      <a href="{{ p.url }}">{{ p.title }}</a>
      <p class="desc">Paper{% if p.authors %} <span class="sep">·</span> {{ p.authors }}{% endif %} <span class="sep">·</span> {{ p.venue }}</p>
    </div>
  </li>
{% endfor %}
{% for post in site.posts %}
  <li>
    <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%b %Y" }}</time>
    <div>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      <p class="desc">Post{% if post.description %} <span class="sep">·</span> {{ post.description }}{% endif %}</p>
    </div>
  </li>
{% endfor %}
</ul>

</div>
