---
layout: default
title: "Tech"
---

<h1 class="page-heading">Tech</h1>

I've been a hobbyist programmer since I was a kid—my first few languages were Logo, HyperCard, and Basic. Now I mostly use Ruby and Racket.

### Projects

#### Russell

I spoke at RubyConf on logic programming, and wrote a demo framework based on [cKanren](http://scheme2011.ucombinator.org/papers/Alvis2011.pdf) [PDF].

<div style="position:relative;height:0;margin-top:25px;padding-bottom:56.25%"><iframe src="https://www.youtube.com/embed/f5Bi6_GOIB8?ecver=2" width="640" height="360" frameborder="0" style="position:absolute;width:100%;height:100%;left:0" allowfullscreen></iframe></div>

You can check out my code [here](https://gitlab.com/gavinmcg/russell).

<h1>Posts about Tech</h1>

<ul class="post-list">
{% for post in site.posts %}
{% if post.tags contains "tech" %}
  <li>
    <h2>
      <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
    </h2>
    <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>
  </li>
{% endif %}
{% endfor %}
</ul>