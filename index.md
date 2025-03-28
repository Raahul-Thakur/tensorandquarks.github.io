---
layout: default
title: Home
---

<div class="hero">
  <h1>🧠🔬 Welcome to Tensors & Quarks</h1>
  <p>Exploring the cosmos of Physics & the depths of Machine Learning.</p>
  <div class="topics">
    <span>⚛️</span>
    <span>🧮</span>
    <span>🤖</span>
    <span>🌌</span>
  </div>
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

<div class="pagination">
  {% if paginator.previous_page %}
    <a href="{{ paginator.previous_page_path | relative_url }}">&larr; Newer</a>
  {% endif %}
  {% if paginator.next_page %}
    <a href="{{ paginator.next_page_path | relative_url }}">Older &rarr;</a>
  {% endif %}
</div>
