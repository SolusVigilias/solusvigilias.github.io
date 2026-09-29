---
layout: default
title: Home
---

# MirrorVerse
Welcome. This is the home of **MirrorVerse** — a living philosophy of infinite degrees of freedom and possibility.

- 📖 [Download the Book (PDF)](/files/Mirrorverse_Book.pdf)
- 💻 [MirrorVerse on GitHub](https://github.com/solusvigilias/mirrorverse)
- ✨ [MirrorVerse Chat (coming soon)](/chat)

<section class="labs-section" aria-labelledby="labs-title">
  <h2 id="labs-title">Labs &amp; Experiments</h2>
  <p>Interactive spaces to explore models, test ideas, and inspect decisions.</p>
  {% include lab-cards.html heading_level=3 %}
  <p><a class="lab-link" href="{{ '/lab/' | relative_url }}">Explore all labs &rarr;</a></p>
</section>

## Latest Posts
{% for post in site.posts limit:6 %}
- **[{{ post.title }}]({{ post.url }})** <small>— {{ post.date | date: "%b %d, %Y" }}</small>  
  {{ post.excerpt | strip_html | truncate: 140 }}
{% endfor %}
