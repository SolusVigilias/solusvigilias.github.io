---
layout: default
title: Home
---

# MirrorVerse
Welcome. This is the home of **MirrorVerse** — a living philosophy of infinite degrees of freedom and possibility.

- 📖 [Download the Book (PDF)](/files/Mirrorverse_Book.pdf)
- 💻 [MirrorVerse on GitHub](https://github.com/solusvigilias/mirrorverse)
- ✨ [MirrorVerse Chat (coming soon)](/chat)

## Latest Posts
{% for post in site.posts limit:6 %}
- **[{{ post.title }}]({{ post.url }})** <small>— {{ post.date | date: "%b %d, %Y" }}</small>  
  {{ post.excerpt | strip_html | truncate: 140 }}
{% endfor %}
