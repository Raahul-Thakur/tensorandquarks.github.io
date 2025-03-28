---
layout: default
title: Home
---

<div class="hero">
  <h1>Welcome to Tensors & Quarks</h1>
  <p>Exploring the cosmos of Physics & the depths of Machine Learning.</p>
</div>

<h2>Latest Posts</h2>

<ul class="post-list">
  {% for post in paginator.posts %}
    <li>
      <h3><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3>
      <p>{{ post.date | date: "%B %-d, %Y" }}</p>
      <p>{{ post.excerpt }}</p>
    </li>
  {% endfor %}
</ul>
