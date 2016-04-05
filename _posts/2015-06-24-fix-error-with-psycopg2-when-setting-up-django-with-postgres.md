---
layout: post
status: publish
published: true
title: Fix error with psycopg2 when setting up django with postgres
author:
  display_name: hisabimbola
  login: hisabimbola
  email: admin@blog.hisabimbola.com
  url: ''
author_login: hisabimbola
author_email: admin@blog.hisabimbola.com
wordpress_id: 117
wordpress_url: http://www.hisabimbola.com/?p=117
date: '2015-06-24 11:52:24 +0100'
date_gmt: '2015-06-24 11:52:24 +0100'
categories:
- programming
tags: []
comments: []
permalink: fix-error-with-psycopg2-when-setting-up-django-with-postgres/
---

Recently, I wanted to start a Django project, and this time I decided to use PostgreSQL as my database instead of SQLite, and I ran into this error when I run

```bash
python -c "import psycopg2"
```

to confirm if Postgres has be set up correctly.

[![Screen Shot 2015-06-24 at 12.05.49 PM](/assets/django-psycopg-screenshot.png)](/assets/django-psycopg-screenshot.png)

After several searches and trying out several solutions, saw this [post on stackOverflow](http://stackoverflow.com/a/29605817/4151025) where I was told to run this command

{% highlight bash %}
sudo mv /usr/lib/libpq.5.dylib /usr/lib/libpq.5.dylib.old

sudo ln -s Applications/Postgres.app/Contents/Versions/9.4/lib/libpq.5.dylib /usr/lib
{% endhighlight %}

So I did that, and it fixed the issue.

I hope this post helps someone and save you of hours of debugging and thanks to my colleagues [Ifedapo](https://ng.linkedin.com/pub/ifedapo-olarewaju/82/b5/86) and [Eniola](https://ng.linkedin.com/pub/arinde-eniola/32/b03/763) that helped me in solving this and other issues I had when setting up. Gracias

{% include twitter_plug.html %}
