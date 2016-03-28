---
layout: post
title: Migrating from Wordpress to Jekyll
permalink: migrating-to-jekyll
---

As a developer, I need a portfolio/blog to showcase myself and document my experiences in programming, but solutions I have used have not been satisfying.

Wordpress was too heavy-weight and caused me a lot of time every time I try to tweak something on the site; in fact, I spent more time trying out themes that would satisfy me. I also had issues with disk storage because my backup was as massive as the site and regularly consumed my disk storage.

It was painful, and I decided to go for something straightforward and elegant.

Although I had this issue lingering for a long time, I decided to work on it this Easter break. I searched online for several minimalistic solutions, and I discovered that most people like me unanimously voted for Jekyll and after reading about it, I knew it was the solution I needed.

I googled about guides to do migrate from Wordpress and I saw this outstanding [tutorial](http://joshualande.com/jekyll-github-pages-poole/) by [Joshua Lande](http://joshualande.com/about/). His tutorial was especially helpful and within minutes, I was able to have my blog up and hosted at Github pages.

The other major task was migrating posts from my WordPress host to Jekyll. Jekyll has support for importing from several popular blog CMS, and I used the WordPress Importer to import from my Wordpress DB rather seamlessly.

While importing Jekyll normalized all symbols, hence the HTML tags were not closing, so the posts were not being rendered correctly. I merely ran a _"find and replace"_ command to replace all, and it corrected the tags. Another issue was that Jekyll did not import images and assets; my posts were not many, so I manually saved all of them and Presto that was all.

Voila, my portfolio is set up.

I love the feel thus far, and I hope it will inspire me to write more regularly.

Comment below if you have any question or suggestions

{% include twitter_plug.html %}
