---
layout: page
title: Index
permalink: /index/
---
<style>
#container {
    display: grid;
    grid-template-rows: repeat(1, 45vh); 
    grid-template-columns: repeat(2, 48%);
    column-gap: 4%;
}

h3:first-letter {
  text-transform: uppercase;
}

#tags, #categories {
    overflow-y:auto;
}


</style>
<div id ="container">
<div id ="categories">
<h2>Categories</h2>
{% for category in site.categories %}
  <h3>{{ category[0] }}</h3>
  <ul>
    {% for post in category[1] %}
      <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
{% endfor %}
</div>
<div id ="tags">
<h2>Tags</h2>
{% for tag in site.tags %}
  <h3>{{ tag[0] }}</h3>
  <ul>
    {% for post in tag[1] %}
      <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
{% endfor %}
</div>
</div>