---
layout: post
status: publish
published: true
title: Fix Error - No Selenium Server Jar found with Protractor
author:
  display_name: hisabimbola
  login: hisabimbola
  email: admin@blog.hisabimbola.com
  url: ''
author_login: hisabimbola
author_email: admin@blog.hisabimbola.com
wordpress_id: 6
wordpress_url: http://www.hisabimbola.com/?p=6
date: '2015-03-10 12:28:00 +0100'
date_gmt: '2015-03-10 12:28:00 +0100'
categories:
- BDD
- Angular
- Selenium
- TDD
tags: []
comments: []
permalink: fix-error-no-selenium-server-jar-found-with-protractor
---

End to end testing with protractor is very important for developers doing [BDD](http://en.wikipedia.org/wiki/Behavior-driven_development "BDD") or [TDD](http://en.wikipedia.org/wiki/Test-driven_development "TDD") and using Angular

To install protractor there is a common error encountered of

```bash
"Warning: there's no selenium server jar at the specified **location**".
```

Though there are several solutions online, and some include you specifying the selenium jar folder in your protractor configuration file, I disagree with this solution because if you are in a team and have other developers contributing to the project, this solution may not work on their machine

So the solution that I did that fixed this was that, instead of running

```bash
webdriver-manager update
```

as specified in protractor site, you should run this command instead

```bash
./node_modules/protractor/bin/webdriver-manager update
```

This would update the webdriver that your app is using

and voila, the error should be gone, and you should see a selenium folder in the protractor folder in your app

You can drop a comment if you have any other question

{% include twitter_plug.html %}
