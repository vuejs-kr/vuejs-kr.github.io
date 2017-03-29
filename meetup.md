---
layout: page
title: 한국어 사용자 모임 기록
permalink: /meetup/
---

### 이 곳은..
Vue.js 한국어 사용자 모임을 기록하는 곳 입니다.

{% for post in site.categories.meetup %}
  * [{{post.title}}]({{ post.url | prepend: site.baseurl }}) <span class="post-meta">{{ post.date | date: site.date_format }}</span>
{% endfor %}