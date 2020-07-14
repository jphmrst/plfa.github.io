---
title     : "Acknowledgements"
layout    : page
prev      : /Untyped/
permalink : /Acknowledgements/
next      : /Fonts/
---

Thank you to:

* The inventors of Agda, for a new playground.
* The authors of Software Foundations, for inspiration.


A special thank you, for inventing ideas on which
this book is based, and for hand-holding:

<ul class="list-of-contributors">
<li>Andreas Abel</li>
<li>Catarina Coquand</li>
<li>Thierry Coquand</li>
<li>David Darais</li>
<li>Per Martin-Löf</li>
<li>Lena Magnusson</li>
<li>Conor McBride</li>
<li>James McKinna</li>
<li>Ulf Norell</li>
</ul>

{% if site.contributors or site.extra_contributors %}
For pull requests big and small, and for answering questions on the Agda mailing list:

<ul class="list-of-contributors">
{% for contributor in site.contributors %}
  <li><a href="https://github.com/{{ contributor.github_username }}">{{ contributor.name }}</a></li>
{% endfor %}
{% for contributor in site.extra_contributors %}
  {% if contributor.name and contributor.github_username %}
  <li><a href="https://github.com/{{ contributor.github_username }}">{{ contributor.name }}</a></li>
  {% else %}
  {% if contributor.name %}
  <li>{{ contributor.name }}</li>
  {% endif %}
  {% if contributor.github_username %}
  <li><a href="https://github.com/{{ contributor.github_username }}">{{ contributor.github_username }}</a></li>
  {% endif %}
  {% endif %}
{% endfor %}
<li>[Your name goes here]</li>
</ul>
{% else %}
{% endif %}


For support:

* EPSRC Programme Grant EP/K034413/1
* NSF Grant No. 1814460
* Foundation Sciences Mathematiques de Paris (FSMP)
  Distinguised Professor Fellowship
---

*This page is derived from Wadler et al.; for more information see the [sources and authorship]({{ site.baseurl }}/Sources/) page.*