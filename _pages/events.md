---
layout: page
permalink: /events/
title: events
description: Shared tasks, workshops, conference roles, and BoF sessions organized over the years.
nav: true
nav_order: 5
---

{% assign event_sections = site.data.events.sections %}
{% assign total_events = 0 %}
{% for section in event_sections %}
  {% assign total_events = total_events | plus: section.items.size %}
{% endfor %}

<div class="event-layout">
  <div class="event-overview">
    {% for section in event_sections %}
      <a class="event-section-link" href="#{{ section.id }}">
        <strong class="event-section-link__title">{{ section.title }}</strong>
        <span class="event-section-link__meta">{{ section.items.size }} entries curated by event type</span>
      </a>
    {% endfor %}
  </div>
</div>

{% for section in event_sections %}
  <section id="{{ section.id }}" class="event-section">
    <div class="event-section__header">
      <div>
        <h2>{{ section.title }}</h2>
        <p class="event-section__copy">{{ section.description }}</p>
      </div>

      <div class="event-section__actions">
        {% if section.home_url %}
          <a class="event-chip" href="{{ section.home_url }}" target="_blank" rel="noopener noreferrer">{{ section.home_label }}</a>
        {% endif %}
        <span class="event-chip event-chip--muted">{{ section.items.size }} entries</span>
      </div>
    </div>

    {% assign section_years = section.items | map: "year" | uniq %}
    <div class="event-year-list">
      {% for year in section_years %}
        {% assign year_items = section.items | where: "year", year %}
        <section class="event-year-group">
          <div class="event-year-group__header">
            <div class="event-year-group__year">{{ year }}</div>
            <div class="event-year-group__count">
              {{ year_items.size }}
              {% if year_items.size == 1 %}
                event
              {% else %}
                events
              {% endif %}
            </div>
          </div>

          <div class="event-entry-list">
            {% for item in year_items %}
              <article class="event-entry">
                <div class="event-entry__header">
                  <h3 class="event-entry__title">
                    <a href="{{ item.url }}" target="_blank" rel="noopener noreferrer">{{ item.title }}</a>
                  </h3>
                  <a class="event-link" href="{{ item.url }}" target="_blank" rel="noopener noreferrer">Visit event site</a>
                </div>

                <div class="event-entry__meta">
                  <span class="event-pill">{{ section.item_label }}</span>
                  {% if item.venue %}
                    <span>{{ item.venue }}</span>
                  {% endif %}
                  {% if item.when %}
                    <span>{{ item.when }}</span>
                  {% endif %}
                  {% if item.location %}
                    <span>{{ item.location }}</span>
                  {% endif %}
                </div>

                <p class="event-entry__summary">{{ item.summary }}</p>
              </article>
            {% endfor %}
          </div>
        </section>
      {% endfor %}
    </div>
  </section>
{% endfor %}
