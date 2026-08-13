---
layout: default
title: Writing
description: Writing archive from Mithun Shanbhag covering Azure, software architecture, developer productivity, and product exploration.
permalink: /writing/
---
<h1>Writing archive</h1>

<p>The writing archive is organized around practical topics: Azure and cloud architecture, software architecture, developer productivity, and product exploration.</p>

<h2>Topic hubs</h2>

<div class="feature-grid">
  <article class="feature-card">
    <h3><a href="{{ "/topics/azure/" | relative_url }}">Azure and cloud architecture</a></h3>
    <p>High-availability patterns, Azure resources, and platform guidance.</p>
  </article>
  <article class="feature-card">
    <h3><a href="{{ "/topics/software-architecture/" | relative_url }}">Software architecture</a></h3>
    <p>Architecture notes from product building and implementation tradeoffs.</p>
  </article>
  <article class="feature-card">
    <h3><a href="{{ "/topics/developer-productivity/" | relative_url }}">Developer productivity</a></h3>
    <p>Tooling, systems, and setup notes for shipping consistently.</p>
  </article>
  <article class="feature-card">
    <h3><a href="{{ "/topics/ai-and-comics/" | relative_url }}">AI and comic product ideas</a></h3>
    <p>Early ideas that combine AI workflows with storytelling and communication.</p>
  </article>
</div>

<h2>Latest posts</h2>

<ul class="post-list">
  {% for post in site.posts %}
    {% unless post.unlisted %}
    <li>
      <article>
        <time datetime="{{ post.date | date: "%Y-%m-%d" }}">{{ post.date | date: "%d %b %Y" }}</time>
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      </article>
    </li>
    {% endunless %}
  {% endfor %}
</ul>
