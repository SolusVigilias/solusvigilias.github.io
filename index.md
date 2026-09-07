---
layout: default
title: Home
---

# MirrorVerse
Welcome. This is the home of **MirrorVerse** — a living philosophy of infinite degrees of freedom and possibility.

- 📖 [Download the Book (PDF)](/files/Mirrorverse_Book.pdf)
- 💻 [MirrorVerse on GitHub](https://github.com/solusvigilias/mirrorverse)
- ✨ [MirrorVerse Chat (coming soon)](/chat)

<section class="lab-teaser" aria-labelledby="lab-title">
  <div class="lab-teaser-heading">
    <h2 id="lab-title">MirrorVerse Loop Lab</h2>
    <span class="lab-label">Experimental</span>
  </div>
  <p>Explore when a simplified model can predict what happens next, where it fails, and what information helps repair it.</p>
  <a class="lab-link" href="/lab/mirrorverse/">Try the Lab</a>
</section>

## Latest Posts
{% for post in site.posts limit:6 %}
- **[{{ post.title }}]({{ post.url }})** <small>— {{ post.date | date: "%b %d, %Y" }}</small>  
  {{ post.excerpt | strip_html | truncate: 140 }}
{% endfor %}
