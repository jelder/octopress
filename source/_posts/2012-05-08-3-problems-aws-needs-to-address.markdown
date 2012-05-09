---
layout: post
title: "3 Problems AWS Needs to Address"
date: 2012-05-08 23:03
comments: true
categories: aws s3 cloudfront 
---

A few days ago, in a fit of pre-launch, late-night, frustration, I issued the following 140 missive.

{% tweet https://twitter.com/#!/jelder/status/199295708635987968 align=center %}

To my surprise, this actually got a response. Someone monitoring the @awscloud account opened a trouble ticket to my email address asking for clarification. The exchange was friendly and hopefully, and I think it's worth sharing here.

<!-- more -->

It's a pretty compelling situation: cloud service offerings and web browser technology have advanced to the point where S3 and CloudFront should be all one needs to deliver an incredibly performant and cost-effective user experience, letting small startups compete in the time-to-first-render game on an even playing field with the likes of Google and Yahoo. Instead, developers are forced to settle for ugly workarounds and outright hacks due to a few crucial shortcomings. 

My team at [Boundless](http://boundless.com) is has been working on a cutting edge single-page HTML5 app. We are hosting it on S3 and CloudFront, and its underlying API lives on EC2. Without getting into too much detail, the architecture is a lot like #newtwitter.

## 1. S3 Restricts Response Headers 

Despite initial appearences, and without much justification from Amazon, the S3 API severely restricts which headers can be attached to an object.

* `Cache-Control`
* `Expires`
* `Content-Disposition`
* `Content-Type`
* `Content-Language`
* `Content-Encoding`

Users can apply their own metadata, but it will always be prefixed with `x-amz-meta`. CSS3 brings the ability to embed arbitrary fonts on the web. Fonts are the clothes words wear, and CSS3 is why the web is looking so sharp lately. The difficulty is that W3 puts fonts under a [same-origin restriction](http://www.w3.org/TR/css3-fonts/#same-origin-restriction). Thus, embedding these fonts requires these additional headers:

* `Access-Control-Allow-Headers`
* `Access-Control-Allow-Origin`

And the complete [CORS](http://enable-cors.org/) specification has yet more headers to contend with:

* `Access-Control-Max-Age`
* `Access-Control-Allow-Methods`
* `Access-Control-Allow-Credentials`

This leaves CloudFront users who wish to embed fonts with a handful of undesirable options. 

1. **Serve the entire domain through CloudFront.** This is fine unless there's anything on your domain which shouldn't be cached, and I'm sure things get even more complicated if you throw SSL into the mix.
3. **Skip S3 and serve everything from EC2.** S3 has _eleven_ nines of durability. Go ahead, reproduce that with a couple of NginX boxes. 
4. **Insert some proxy servers to add the headers.** I think what you mean is, add yet another hop in your network while increasing your attack footprint _and_ your EC2 bill.
2. **Mix fonts into stylesheets using `data:` URIs.** Now every time you adjust a `<div>` tag, your visitors have to download your fonts again. You could break your CSS into multiple files, but this is in direct opposition to one of the tenants of website optimization: minimize the number of HTTP requests. Also, 7-bit encoding means your fonts are now 37% fatter on the wire.

Here is a [forum post from 2009](https://forums.aws.amazon.com/thread.jspa?threadID=34281) bringing this to Amazon's highly dismissive attention. What really irks me about this is that Amazon chose to bless a few headers instead of letting the end-user decide what is best for our customers.

## 2. S3 and CloudFront Won't Compress Anything 

RFC2616 allows that a client may suggest to a server that it would like to have the response encoded as something other than raw bytes before transmission. One common encoding is `gzip`, and lots of HTTP traffic includes a header like `Accept-Encoding: gzip`. Most web servers will comply with this suggestion, reducing plain text like HTML, CSS, and JavaScript by well over 50%.

Two notable exceptions to "most web servers" are S3 and CloudFront. A possible workaround involves `Content-Encoding` being among the allowed HTTP headers for S3 objects. The image below has been compressed with `gzip -9` before uploading, and has `Content-Encoding: gzip` set in S3.

![Success Kid](http://s3-gzip-hack.jacobelder.com/success-kid.png)

If you can see Success Kid, this hack will work on your browser.

This exploits the fact that most browsers usually send `Accept-Encoding: gzip`, or they will handle `Content-Encoding: gzip` in the response even if they didn't request it. Users of IE7 and previous versions will see a broken image icon. `wget` or `curl` will also result in corruption unless those tools are explicitly configured to always use compression. This is really a quasi-violation of [RFC2616 Section 14.3](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.3), but it does sort of work.

If you want to be compliant, you most choose between S3 and compression. CloudFront, at least, will cache both compressed and raw versions of each object depending on the clients which have requested it. 

## 3. CloudFront's TCP Stack Lacks Tuning

I have harped on this issue before, but Amazon CloudFront exhibits one of the smallest initial TCP congestion windows in the CDN marketplace. They're at 2. Consensus is growing that it should be closer to 10. Rather than making an argument for it here, I'll let some Googlers do it for me.

{% blockquote %}

An Argument for Increasing TCP's Initial Congestion Window&#8221;, <a href="author39115.html">Nandita Dukkipati</a>, <a href="author38914.html">Tiziana Refice</a>, <a href="author27276.html">Yuchung Cheng</a>, <a href="author39277.html">Jerry Chu</a>, Tom Herbert, Amit Agarwal, <a href="author35943.html">Arvind Jain</a>, Natalia Sutin, <i>ACM SIGCOMM Computer Communications Review</i>, vol. 40 (2010), pp. 27-33.<br/> [<a href="http://ccr.sigcomm.org/drupal/?q=node/621"><strong>ccr.sigcomm.org</strong></a>] [<a href="archive/36640.pdf"><strong>pdf</strong></a>] [<a href="http://www.google.com/search?lr=&amp;ie=UTF-8&amp;oe=UTF-8&amp;q=An+Argument+for+Increasing+TCP%27s+Initial+Congestion+Window+Dukkipati+Refice+Cheng+Chu+Herbert+Agarwal+Jain+Sutin"><strong>search</strong></a>]

{% endblockquote %}

The @awscloud guys are apparently considering my request. If I get any answers back, I will be sure to post them here.

