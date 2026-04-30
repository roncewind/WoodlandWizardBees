---
layout: page
title: Welcome to Amberfold Bees
---

![Amberfold Bees]({{ "/assets/images/logo-main.png" | relative_url }})

Just my hive notes and other bee buzz.

## Colonies

### Active

{% assign active = site.colonies | where: "status", "active" | sort: "established" %}
{% for c in active %}- [{{ c.title }}]({{ c.url | relative_url }}){% if c.location %} — {{ c.location }}{% endif %}{% if c.origin %} ({{ c.origin }}){% endif %}
{% endfor %}

### Historical

{% assign historical = site.colonies | where_exp: "c", "c.status != 'active'" | sort: "established" %}
{% for c in historical %}- [{{ c.title }}]({{ c.url | relative_url }}){% if c.status %} — {{ c.status }}{% endif %}{% if c.end_date %} {{ c.end_date | date: "%Y-%m-%d" }}{% endif %}
{% endfor %}

Keeping an eye on the sticky boards to see what the current mite loads are:

![Mite load]({{ "/assets/images/mite-count.jpg" | relative_url }})
