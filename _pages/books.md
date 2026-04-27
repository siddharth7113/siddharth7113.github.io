---
layout: page
title: Books
permalink: /books/
description: Books I am reading, have read, or have set down for now. Technical ones get notes; the rest just live here as covers I'd recommend (or wouldn't).
nav: true
nav_order: 4
display_categories:
  - slug: technical
    title: Technical
  - slug: mathematics
    title: Mathematics
  - slug: non-fiction
    title: Non-fiction
---

<div class="projects">
{% assign all_books = site.books | where_exp: "b", "b.is_book" %}

{% for category in page.display_categories %}
  {% assign books_in_cat = all_books | where: "category", category.slug %}
  {% if books_in_cat.size > 0 %}
    <a id="{{ category.slug }}" href="#{{ category.slug }}">
      <h2 class="category">{{ category.title }}</h2>
    </a>
    <div class="row row-cols-1 row-cols-md-3">
      {% for book in books_in_cat %}
        <div class="col mb-4">
          <a href="{{ book.url | relative_url }}">
            <div class="card h-100 hoverable">
              {% if book.cover %}
                <div class="card-img-wrapper">
                  <img src="{{ book.cover | relative_url }}" alt="{{ book.title }} cover" />
                </div>
              {% endif %}
              <div class="card-body text-center">
                <h2 class="card-title">{{ book.title }}</h2>
                {% if book.author %}
                  <p class="card-text text-muted mb-1">{{ book.author }}</p>
                {% endif %}
                {% if book.status %}
                  <p class="card-text mb-2">
                    <span class="badge badge-pill badge-secondary">{{ book.status }}</span>
                  </p>
                {% endif %}
                {% if book.description %}
                  <p class="card-text">{{ book.description }}</p>
                {% endif %}
              </div>
            </div>
          </a>
        </div>
      {% endfor %}
    </div>
  {% endif %}
{% endfor %}
</div>
