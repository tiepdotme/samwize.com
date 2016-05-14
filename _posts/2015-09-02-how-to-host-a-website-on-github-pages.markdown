---
layout: post
title: "How to host a website on Github Pages"
date: 2015-09-02 22:45:32 +0800
comments: true
categories: [Website, Github]
---

[GitHub Pages](https://pages.github.com) provides FREE hosting of your (static) website.

This post is a guide on hosting your "project" repository with a custom domain.

<!-- more -->

## Your Local Repository

Instead of a `master` branch, rename it to `gh-pages`.

In your root, have a `CNAME` file with the line `example.com`.

Commit and push.


## GitHub Pages

Create a new repository on GitHub for the project.

    git remote add origin https://github.com/yourusername/yourproject.git
    git push -u origin gh-pages

If you have an existing respository, create a new `gh-pages` branch, and make sure it is your default branch in your repos settings.


## Configure Domain Name

GitHub does provide an [article](https://help.github.com/articles/tips-for-configuring-an-a-record-with-your-dns-provider/) on how you can do it, but it is very confusing.

As an illustration, this is my configuration using [namecheap](http://www.namecheap.com/?aff=68466):

    @       192.30.252.153              A (Address)
    @       192.30.252.154              A (Address)
    www     yourusername.github.io.     CNAME (Alias)

Note: The **CNAME (Alias)** record is for the subdomain **www**, if you are configuring for a subdomain. If you configuring for a root doamin, then you can omit it. The CNAME is pointing to your github username, not the project name. GitHub will determine which project it is for via the CNAME file.

Wait for the DNS changes to propagate.

Or check with `dig example.com +nostats +nocomments +nocmd` if changes are propagated.
 
That's it. With GitHub Pages, you can host as many projects/websites as you want, for free!
