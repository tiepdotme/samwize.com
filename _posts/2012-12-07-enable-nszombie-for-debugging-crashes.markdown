---
layout: post
title: "Enable NSZombie for Debugging Crashes"
date: 2012-12-07 23:50
comments: true
categories: iOS
---

I posted a tip before on how you can [find the real cause of exception in Xcode](http://blog.just2us.com/2012/02/find-the-real-exception-in-xcode-debugger/).

There is another feature which you can enable to help in debugging.

`NSZombie` is an option that helps to trace overreleasing of objects.

<!-- more -->

Sometimes you might encounter exceptions `objc_msgSend`, which no matter how many times you Continue, an exception is thrown. That is a good sign get help from the zombie.

To enable, go to Product > Edit Scheme > Diagnostics, and check **Enable Zombie Objects**.

Run and debug again, and more meaningful info of what was deallocated wrongly will be shown.