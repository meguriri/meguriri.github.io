---
layout: page
title: Books
permalink: /books/
---

This is my recommended list of books to read:

<div class="book-collection">
{% for book in site.data.books %}
  <hr>
  <div class="book-summary">
    <div class="image-thumbnail">
      <img src="{{ book.thumbnail }}" />
    </div>
    <div class="book-info">
      <h2>{{ book.title }}</h2>
      <h3>{{ book.subtitle }}</h3>
      <div class="book-metadata">
        By {{ book.author}}
      </div>
      <div class="book-notes">
      {{ book.notes}}
      </div>
      {%- if book.tags.size > 0-%}
      <div class="post-meta">
        <ul class="post-tags">
          {%- for tag in book.tags -%}
          <li>{{ tag | downcase }}</li>
          {%- endfor -%}
        </ul>
      </div>
      {%- endif -%}
    </div>

  </div>
{% endfor %}
</div>