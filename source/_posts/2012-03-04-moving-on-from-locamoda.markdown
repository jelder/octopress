---
layout: post
title: "Moving on from LocaModa"
date: 2012-03-04 09:09
comments: true
categories: 
---

{% blockquote Paul Valery, Charmes. Le Cimetière Marin %}
In the eyes of those lovers of perfection, a work is never finished&mdash;a word that for them has no sense&mdash;but abandoned; and this abandonment, whether to the flames or to the public (and which is the result of weariness or an obligation to deliver) is a kind of an accident to them, like the breaking off of a reflection, which fatigue, irritation, or something similar has made worthless.
{% endblockquote %}

What's the right amount of time to stay at a startup? My first startup ([Livewire Mobile](http://www.livewiremobile.com/), née Groove Mobile) was the fastest and most eye-opening 18 months of my life. Before that was six years of running IT for [The Real Estate Company](http://www.capecodvacation.com/), which was at the time the largest independently owned real estate agency in southeastern Massachusetts. That company was founded the year I was born and is still operating today. In my memory, Groove and T.R.E.C. occupy about the same amount of space.

Ultimately my four years at LocaModa was just the right amount of time. I am immensely proud of what I built while there. Some of it is open source (on [my github](https://www.github.com/jelder) account, with more to be released later on theirs). In a later post I may write about some of the cool work we did with Solr, Chef, ActiveMQ, Varnish, HAProxy, and Nagios. There's a lot to be excited about happening over there, but that will be for others to ship, not me.

Below is the Thank You card I left my team. 

<iframe src="http://thankyou.locamoda.com/" width="100%" height="600"></iframe>

## About the Mosaic ##

The mosaic was assembled from a corpus of every image ever submitted to the LocaModa platorm by our users. Every profile picture, every Wiffiti background image, every image attached to a message. There are even a few Poster backgrounds in there. Everything. I didn't even bother excluding the stuff which had been flagged as inappropriate by our moderators. It's a lot of data.

It only took [metapixel](http://www.complang.tuwien.ac.at/schani/metapixel/) about 20 minutes to assemble those into the target image: [a reasonable approximation of] a candid portrait of me by the affable [Dan Norton](https://plus.google.com/116527656433325548832/posts).

The final product is wrapped in a Google Maps-style pan/zoom interface provided by [PanoJS3](http://www.dimin.net/software/panojs/) and lives, hopefully forever, on Amazon S3 at [http://thankyou.locamoda.com](http://thankyou.locamoda.com).
