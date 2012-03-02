---
layout: post
title: "Octopress and Amazon CloudFront"
date: 2012-03-01 21:42
comments: true
categories:
  - meta
  - aws
  - octopress
---

As it turns out, getting your Octopress-hosted blog up and running on CloudFront is pretty easy. The only problem I ran into is that Octopress wants every post and page to be a directory with an `index.html`, but CloudFront only recognizes default objects at the root, not for subdirectories. The solution to that is to create a CloudFront distribution using a Custom Origin, which points to the `example.s3-website-us-east-1.amazonaws.com` URL rather than the S3 bucket itself. This way you end up using S3's much more functional default object support. As of this writing, this blog is served by CloudFront.

I'm still looking for the right way to get `Cache-control` and `Expire` headers from `s3cmd sync`. Without that, the best YSlow score you can get on a default Octopress blog is about a C (76). CloudFront doesn't do any transparent compression for you (no `Accept-Encoding` support) so that's a no-go as well. It would also be nice if Amazon would acknowledge that initcwnd of 2 is nonsensical on today's Internet. Nearly all other CDNs use something higher, with Akamai using up to 6, and Google using _10_.

See [Initcwnd settings of major CDN providers](http://www.cdnplanet.com/blog/initcwnd-settings-major-cdn-providers/) for a comparison.
