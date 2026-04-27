---
layout: page
title: Courses
permalink: /courses/
description: Lecture series I follow on YouTube. Less rigorous than the books page — I won't pretend to take notes for all of these, so this is a resource gallery with a bit of commentary.
nav: true
nav_order: 5
display_statuses:
  - slug: currently-watching
    title: Currently watching
  - slug: watched
    title: Watched and followed
  - slug: plan-to-watch
    title: Plan to watch
---

<div class="projects">
{% for status in page.display_statuses %}
  {% assign courses_in_status = site.courses | where: "status", status.slug %}
  {% if courses_in_status.size > 0 %}
    <a id="{{ status.slug }}" href="#{{ status.slug }}">
      <h2 class="category">{{ status.title }}</h2>
    </a>
    <div class="row row-cols-1 row-cols-md-2">
      {% for course in courses_in_status %}
        {%- comment -%} Resolve a thumbnail from any of: explicit thumbnail, youtube_video_id, or v=<id> embedded in playlist_url. {%- endcomment -%}
        {% assign vid = course.youtube_video_id %}
        {% if vid == blank and course.playlist_url contains "v=" %}
          {% assign after_v = course.playlist_url | split: "v=" | last %}
          {% assign vid = after_v | split: "&" | first %}
        {% endif %}
        {% if course.thumbnail %}
          {% assign thumb = course.thumbnail %}
        {% elsif vid != blank %}
          {% capture thumb %}https://img.youtube.com/vi/{{ vid }}/hqdefault.jpg{% endcapture %}
        {% else %}
          {% assign thumb = "" %}
        {% endif %}

        <div class="col mb-4">
          <a href="{{ course.url | relative_url }}">
            <div class="card h-100 hoverable course-card">
              {% if thumb != "" %}
                <div class="card-img-wrapper course-thumb">
                  <img src="{{ thumb }}" alt="{{ course.title }} preview" loading="lazy" />
                  <span class="course-thumb-play">
                    <i class="fa-brands fa-youtube"></i>
                  </span>
                </div>
              {% else %}
                <div class="card-img-wrapper course-thumb course-thumb-empty">
                  <i class="fa-brands fa-youtube"></i>
                </div>
              {% endif %}
              <div class="card-body">
                <h2 class="card-title">{{ course.title }}</h2>
                <p class="card-text text-muted mb-2">
                  {% if course.instructor %}{{ course.instructor }}{% endif %}
                  {% if course.institution %}&middot; {{ course.institution }}{% endif %}
                  {% if course.year %}&middot; {{ course.year }}{% endif %}
                </p>
                {% if course.lectures_total %}
                  <p class="card-text mb-2">
                    <span class="badge badge-pill badge-light">
                      {{ course.lectures_watched | default: 0 }} / {{ course.lectures_total }} lectures
                    </span>
                  </p>
                {% endif %}
                {% if course.description %}
                  <p class="card-text">{{ course.description }}</p>
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

<style>
  .course-thumb {
    position: relative;
    aspect-ratio: 16 / 9;
    overflow: hidden;
  }
  .course-thumb img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
  }
  .course-thumb-play {
    position: absolute;
    inset: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 3rem;
    color: rgba(255, 255, 255, 0.9);
    text-shadow: 0 2px 8px rgba(0, 0, 0, 0.6);
    opacity: 0.85;
    transition: opacity 0.2s ease;
    pointer-events: none;
  }
  .course-card:hover .course-thumb-play {
    opacity: 1;
  }
  .course-thumb-empty {
    background: linear-gradient(135deg, var(--global-divider-color), var(--global-card-bg-color));
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 3rem;
    color: var(--global-text-color-light);
  }
</style>
