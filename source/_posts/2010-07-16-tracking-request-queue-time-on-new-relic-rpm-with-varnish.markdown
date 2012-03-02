---           
layout: post
title: Tracking request queue time on New Relic RPM with Varnish
date: "2010-07-16 21:48:00 UTC"
comments: true
categories:
 - newrelic
 - varnish
---

<p>The nice folks at New Relic have added an under-hyped <a href="https://newrelic.com/docs/features/tracking-front-end-time">feature</a> to RPM which allows for the tracking of the time a given request spent in the server's work queue before processing began. This information in crucial in determining when you need to add more workers. It only requires that your front-end add an <code>X-Request-Start</code> header containing the epoch time in microseconds when the request was received. They offer a patch for NginX and a one-line config change for Apache.</p>

<p>But what about the new hotness, Varnish?</p>

{% gist 494265 %}

<p><em>That's</em> what. Save this to <code>/etc/varnish/newrelic.h</code> and include it in your <code>vcl_recv</code> declaration.</p>
