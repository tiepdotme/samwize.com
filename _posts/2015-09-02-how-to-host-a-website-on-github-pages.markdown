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

In your root, have a `CNAME` file with the line `example.com` (or your subdomain).

Commit and push.

## GitHub Pages

Create a new repository on GitHub for the project.

    git remote add origin https://github.com/yourusername/yourproject.git
    git push -u origin gh-pages

If you have an existing repository, create a new `gh-pages` branch, and make sure it is your default branch in your repos settings.

## Configure Domain Name

GitHub does provide an [article](https://help.github.com/articles/tips-for-configuring-an-a-record-with-your-dns-provider/) on how you can do it, but it is very confusing.

As an illustration, this is my configuration using [namecheap](http://www.namecheap.com/?aff=68466):

    @       185.199.108.153              A (Address)
    @       185.199.109.153              A (Address)
    @       185.199.110.153              A (Address)
    @       185.199.111.153              A (Address)
    www     yourusername.github.io.     CNAME (Alias)

For my configuration, I want both `example.com` and `www.example.com` to go to the website.

Note: The **CNAME (Alias)** record is for the subdomain **www**. It can be omitted if you are using only the root domain.

The CNAME is pointing to your github username, not the project name. GitHub will determine which project it is for via the CNAME file.

In the case where you are configuring for only a subdomain eg. blog.example.com, then in your `CNAME` file you will have `blog.example.com`, and you only need to have the CNAME Alias (no need A Address).

Wait for the DNS changes to propagate.

Or check with `dig example.com +nostats +nocomments +nocmd` if changes are propagated.

That's it. With GitHub Pages, you can host as many projects/websites as you want, for free!
