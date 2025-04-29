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
  {% for post in site.posts %}
    <li class="post-card">
      <h2>{{ post.title }}</h2>

      <p class="post-meta">
        {{ post.date | date: "%B %-d, %Y" }}
        {% if post.tag %}
          <span class="inline-tag">{{ post.tag }}</span>
        {% endif %}
      </p>

      <p>{{ post.excerpt }}</p>

      <a href="{{ post.url | relative_url }}" class="read-more">Read more →</a>
    </li>
  {% endfor %}
</ul>
