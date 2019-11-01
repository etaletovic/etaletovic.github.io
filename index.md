---
layout: default
---
  {%- assign post = site.posts.first -%}
  {%- assign content = post.content -%}
  {%- assign custom-title = post.title -%}
  {%- assign custom-date = post.date -%}
  {%- assign custom-author = post.author -%}
  {%- include post-detail.html -%}
