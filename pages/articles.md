---
title: "All Articles"
layout: page
permalink: /articles/
---

<p>{{ site.posts.size }} articles ∙ {{ site.categories.size }} categories</p>

<hr>

<ul class="tag-cloud">
{% for tag in site.categories %}
  <li style="font-size: {{ tag | last | size | times: 200 | divided_by: site.categories.size | plus: 100 }}%">
    <a href="#{{ tag | first | slugize }}">
      {{ tag | first }}
    </a>
  </li>
{% endfor %}
</ul>

<hr>

<div id="archives">
{% for tag in site.categories %}
    {% capture tag_name %}{{ tag | first }}{% endcapture %}
    <h3 id="#{{ tag_name | slugize }}">{{ tag_name }}</h3>
    <a name="{{ tag_name | slugize }}"></a>
    <ul>
    {% for post in site.categories[tag_name] %}
    <li><a href="{{ post.url | prepend: site.baseurl }}">{{post.title}}</a></li>
    {% endfor %}
    </ul>
{% endfor %}
</div>
