---
layout: post
title: "Embedding YouTube on Website and Loading Lazily"
date: 2020-06-19T22:57:18+08:00
categories: [Website]
---

If you have ever embed YouTube using their iframe code, this post would optimize your webpage loading speed tremendously.

## The Problem

Embedding multiple YouTube videos will slow down a webpage loading speed.

I tested [my website](https://dualgram.com) which contains just 4 YouTube videos, and it gave a very bad score on PageSpeed.

![A desktop score of 44/100](/images/pagespeed-dualgram-web-old.jpg){:.border}

## The Solution

It turns out loading iframe will use a lot of resources, and multiple times for every video! A big waste on the time spent to download unnecessary scripts and stuff.

There are a few lazy loading solutions, involving JS.

The latest I found is a [genius way](https://css-tricks.com/lazy-load-embedded-youtube-videos/) using `srcdoc`, which would fall back to `src` in case unsupported.

The iframe will look like this:

```html
{% raw %}
<iframe width="800" height="450"
src="https://www.youtube.com/embed/zuY4KtE9w-E?start=0"
srcdoc="<style>*{padding:0;margin:0;overflow:hidden}html,body{height:100%}img,span{position:absolute;width:100%;top:0;bottom:0;margin:auto}span{height:1.5em;text-align:center;font:48px/1.5 sans-serif;color:white;text-shadow:0 0 0.5em black}.title{font:24px/1.5 sans-serif;color:white;position:relative;left:20px;top:10px;}</style><a href=https://www.youtube.com/embed/zuY4KtE9w-E?autoplay=1&amp;start=0><img src=https://img.youtube.com/vi/zuY4KtE9w-E/hqdefault.jpg alt='Review by Phones and stuff'><span>▶</span></a><span class='title'>Review by Phones and stuff</span>"
frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen=""></iframe>
{% endraw %}
```

If you can read, the addition is the `srcdoc`, which is a long line of CSS and HTML. I also modified the original css to include a title at the top.

When the iframe is embedded it looks like this:

<iframe width="800" height="450"
src="https://www.youtube.com/embed/zuY4KtE9w-E?start=0"
srcdoc="<style>*{padding:0;margin:0;overflow:hidden}html,body{height:100%}img,span{position:absolute;width:100%;top:0;bottom:0;margin:auto}span{height:1.5em;text-align:center;font:48px/1.5 sans-serif;color:white;text-shadow:0 0 0.5em black}.title{font:24px/1.5 sans-serif;color:white;position:relative;left:20px;top:10px;}</style><a href=https://www.youtube.com/embed/zuY4KtE9w-E?autoplay=1&amp;start=0><img src=https://img.youtube.com/vi/zuY4KtE9w-E/hqdefault.jpg alt='Review by Phones and stuff'><span>▶</span></a><span class='title'>Review by Phones and stuff</span>"
frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen=""></iframe>

<br />

The above is different from _the real thing_ from YouTube, but it loads much much faster.

With lazy loading, my PageSpeed increased to nearly 100 😁 IMO it's worth the tradeoff.

![A desktop score of 98/100](/images/pagespeed-dualgram-web-lazy-loaded.jpg){:.border}

## My Jekyll Solution

This website is powered by Jekyll, so I will use some Liquid Template to embed easily.

The usage will look like this:

```
{% raw %}
{% include youtube.html width="800" height="450" id="f1aBlyYax6o" start="12" title="Top 10 iOS Apps of October 2019" %}
{% endraw %}
```

The usage is much simpler, because the included **youtube.html** does the hard work:

```html
{% raw %}
<iframe width="{{ include.width }}" height="{{ include.height }}"
src="https://www.youtube.com/embed/{{ include.id }}?start={{ include.start | default: 0 }}"
srcdoc="<style>*{padding:0;margin:0;overflow:hidden}html,body{height:100%}img,span{position:absolute;width:100%;top:0;bottom:0;margin:auto}span{height:1.5em;text-align:center;font:48px/1.5 sans-serif;color:white;text-shadow:0 0 0.5em black}.title{font:24px/1.5 sans-serif;color:white;position:relative;left:20px;top:10px;}</style><a href=https://www.youtube.com/embed/{{ include.id }}?autoplay=1&start={{ include.start | default: 0 }}><img src=https://img.youtube.com/vi/{{ include.id }}/hqdefault.jpg alt='{{ include.title | default: 'video' }}'><span>▶</span></a>{% if include.title %}<span class='title'>{{ include.title }}</span>{% endif %}"
frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
{% endraw %}
```
