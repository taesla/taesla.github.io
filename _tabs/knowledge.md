---
layout: page
title: Knowledge
icon: fas fa-book
order: 3
permalink: /knowledge/
---

## 📚 공학 지식 정리

자율주행, 로보틱스, 제어공학 관련 지식을 체계적으로 정리합니다.

---

{% assign knowledge_posts = site.posts | where_exp: "post", "post.categories contains 'Knowledge'" %}

### 🚗 자율주행 (Autonomous Driving)
{% assign ad_posts = knowledge_posts | where_exp: "post", "post.categories contains 'Autonomous Driving'" %}
{% if ad_posts.size > 0 %}
<ul>
{% for post in ad_posts %}
  <li><a href="{{ post.url }}">{{ post.title }}</a> <small>({{ post.date | date: "%Y.%m.%d" }})</small></li>
{% endfor %}
</ul>
{% else %}
<p><em>아직 작성된 글이 없습니다.</em></p>
{% endif %}

---

### 🤖 로보틱스 (Robotics)
{% assign robotics_posts = knowledge_posts | where_exp: "post", "post.categories contains 'Robotics'" %}
{% if robotics_posts.size > 0 %}
<ul>
{% for post in robotics_posts %}
  <li><a href="{{ post.url }}">{{ post.title }}</a> <small>({{ post.date | date: "%Y.%m.%d" }})</small></li>
{% endfor %}
</ul>
{% else %}
<p><em>아직 작성된 글이 없습니다.</em></p>
{% endif %}

---

### 🎛️ 제어공학 (Control Engineering)
{% assign control_posts = knowledge_posts | where_exp: "post", "post.categories contains 'Control Engineering'" %}
{% if control_posts.size > 0 %}
<ul>
{% for post in control_posts %}
  <li><a href="{{ post.url }}">{{ post.title }}</a> <small>({{ post.date | date: "%Y.%m.%d" }})</small></li>
{% endfor %}
</ul>
{% else %}
<p><em>아직 작성된 글이 없습니다.</em></p>
{% endif %}

---

### 🧠 AI / Machine Learning
{% assign ai_posts = knowledge_posts | where_exp: "post", "post.categories contains 'AI'" %}
{% if ai_posts.size > 0 %}
<ul>
{% for post in ai_posts %}
  <li><a href="{{ post.url }}">{{ post.title }}</a> <small>({{ post.date | date: "%Y.%m.%d" }})</small></li>
{% endfor %}
</ul>
{% else %}
<p><em>아직 작성된 글이 없습니다.</em></p>
{% endif %}

---

### 💻 Software Engineering
{% assign sw_posts = knowledge_posts | where_exp: "post", "post.categories contains 'Software'" %}
{% if sw_posts.size > 0 %}
<ul>
{% for post in sw_posts %}
  <li><a href="{{ post.url }}">{{ post.title }}</a> <small>({{ post.date | date: "%Y.%m.%d" }})</small></li>
{% endfor %}
</ul>
{% else %}
<p><em>아직 작성된 글이 없습니다.</em></p>
{% endif %}
