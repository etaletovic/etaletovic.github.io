---
layout: post
---

  {% assign post = site.posts.first %}
  {% assign content = post.content %}
  {& assign title = post.title &}
  
  {% include post-detail.html %}
