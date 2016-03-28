---
layout: post
status: publish
published: true
title: Steps to setup Postgres with Django on mac
author:
  display_name: hisabimbola
  login: hisabimbola
  email: admin@blog.hisabimbola.com
  url: ''
author_login: hisabimbola
author_email: admin@blog.hisabimbola.com
wordpress_id: 123
wordpress_url: http://www.hisabimbola.com/?p=123
date: '2015-06-24 23:00:28 +0100'
date_gmt: '2015-06-24 23:00:28 +0100'
categories:
- programming
tags:
- django
- postgres
- setup
- mac
comments: []
permalink: steps-to-setup-postgres-with-django-on-mac
---

Recently I started a Django project and for this project, I decided to use PostgreSQL inside of the default SQLite that comes with Django on a mac.

I ran into some issues that cost me a lot of billable hours, but I was finally able to fix it, hence this post so that nobody would waste lot of billable hours to setup PostgreSQL with Django.

There are some issues I encountered that may be peculiar to me, but also could be helpful to someone, so I would highlight that too.

## Install Postgres

* * *

Django supports PostgreSQL 9.0 and higher.

In case you do not have Postgres on your system already, I would recommend you downloading and installing it from the package app on their [site](http://postgresapp.com/).

If you are like me that I forgot the password I used for Postgres on my machine, there are several ways to change your password, but I had nothing to lose so I just uninstalled the app. Double-click the `uninstall-postgresql.app` app in this directory

```bash
/Library/PostgreSQL/9.4/uninstall-postgresql.app/
```

That would do a total clean-up for you, then you can install Postgres from the app you downloaded.

## Install Psycopg2

* * *

Install this inside virtual environment to connect Django with Postgresql database server

```bash
pip install psycopg2
```

Confirm that `psycopy2` is working by running this command

```bash
python -c "import psycopg2"
```

If you get any error with that command, check out my post on [psycopg2 error and how I solved it](http://www.hisabimbola.com/fix-error-with-psycopg2-when-setting-up-django-with-postgres/).

## Install Django

You should install Django by running this command

```bash
pip install django
```

## Create a database

* * *

Before Django can use Postgres it would need a database, you can create it from the GUI or run this command

```sql
CREATE DATABASE  OWNER
CREATE DATABASE

```

## Set Postgres as your default database

* * *

The next thing to do is to update `<projectname>/settings.py</projectname>`

{% highlight python %}
DATABASES = {

    "default": {

        "ENGINE": "django.db.backends.postgresql_psycopg2",

        "NAME": "<your database='' name=''>",

        "USER": "<the user=''>",

        "PASSWORD": "<your password=''>",

        "HOST": "localhost",

        "PORT": "5432",

    }

}
{% endhighlight %}

## Run your migration

* * *

Run your migration to create tables in the database

```bash
python manage.py migrate
```

If your see something like this

[![Django Migration successful](assets/django-migration-terminal-screenshot.png)](assets/django-migration-terminal-screenshot.png)
Then you have successfully setup Postgres with your Django app, if you encounter any challenges, please drop comments and let's fix it then we update this post, to avoid others going through that also.

Great thanks to my colleagues [Ifedapo](https://ng.linkedin.com/pub/ifedapo-olarewaju/82/b5/86) and [Eniola](https://ng.linkedin.com/pub/arinde-eniola/32/b03/763) that helped me in solving this and other issues I had when setting up. Gracias

{% include twitter_plug.html %}
