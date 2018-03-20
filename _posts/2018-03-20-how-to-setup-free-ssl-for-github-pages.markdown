---
layout: post
title: "How to Setup Free SSL for Github Pages"
date: 2018-03-20T17:58:01+08:00
categories: [Website, Github]
---

SSL/HTTPS is now [almost mandatory](https://searchengineland.com/effective-july-2018-googles-chrome-browser-will-mark-non-https-sites-as-not-secure-291623).

Good news is, we can easily setup SSL with [Cloudflare](https://www.Cloudflare.com), and it's free!

## Requirements

Make sure you already have a website hosted on Github Pages, and serving via your own domain name.

If not, [get your website running](/2015/09/02/how-to-host-a-website-on-github-pages/) first.

Then register a [Cloudflare](https://www.Cloudflare.com) account.

## Setup on Cloudflare

1. In Cloudflare > "Add Site" > Enter your website domain name

2. Cloudflare automatically copy your DNS records, but you should double check them

3. In your domain registrar, change the nameservers to that provided by Cloudflare -- eg. `mia.ns.Cloudflare.com` and `yichun.ns.Cloudflare.com`

That's it!

## The Thing About Page Rules

Cloudflare's Page Rules is like nginx configuration. You can rewrite URLs, configure cache settings, redirects, etc.

For simple redirection/forwarding, you [need 1 page rule](https://support.cloudflare.com/hc/en-us/articles/200168306-Is-there-a-tutorial-for-Page-Rules-#redirects) for every redirect.

Cloudflare free plan provides only 3 page rules.

Argh, that's the caveat of being free.
