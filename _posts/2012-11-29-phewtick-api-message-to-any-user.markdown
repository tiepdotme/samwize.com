---
layout: post
title: "Phewtick API - Message to any user you want"
date: 2012-11-29 22:18
comments: true
categories: Python
---

I wrote a [Phewtick Hack](https://github.com/samwize/phewtick-hack) a few days ago.

{%img http://www.phewtick.com/img/home_main.png Phewtick %}

It started out as a hack via their RESTful API to automatically scan QR code and get Phewtick points ($$$).

Today, Phewtick released a new [version 3.2.0](https://itunes.apple.com/sg/app/phewtick/id487158487?mt=8), which includes 2 new features: timeline and messaging.

I took a look at their API (again)..

<!-- more -->

Shockingly,

- The timeline shows not only the friends I met with, but also the friends that my friends met with! 

- The messaging API allows me to send to anyone!

This [python code](https://github.com/samwize/phewtick-hack) will allow you to spam Phewtick user id 123456.

```python
from phew import *
message(0, 123456, "Meow~")
```

I could easily spam everyone in Phewtick with 2 lines of code.

```python
for id in range(0,1000000):
	message(0, id, "Meow~")
```

