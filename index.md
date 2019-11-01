---
layout: post
---

  {% assign post = site.posts.first %}
  {% assign content = post.content %}
  
  {% include_relative _layouts/post.html %}
