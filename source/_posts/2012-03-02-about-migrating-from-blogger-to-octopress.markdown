---
layout: post
title: "About migrating from Blogger to Octopress"
date: 2012-03-02 12:57
comments: true
categories: 
 - meta
 - octopress
---

Another way Octopress (and Jekyll) differs from Blogger is that we now have each post in its own directory. I can't say I fully understand this, though it does look cleaner. It would make sense if there were also an easy way to have images for each post live in that same directory. Regardlss, this breaks all inbound links. Here's the 404 page I created to solve this problem. If the original URL ended with `.html`, it suggests a link to the possible new URL.

Try it out [here](2010/07/tracking-request-queue-time-on-new-relic-rpm-with-varnish.html).

{% gist 1960042 %}

To complete the picture, you must also structure your permalinks like Blogger does.

``` yml _config.yml
permalink: /:year/:month/:title/
```

