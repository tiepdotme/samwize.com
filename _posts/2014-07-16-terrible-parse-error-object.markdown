---
layout: post
title: "Terrible Parse Error Object"
date: 2014-07-16 11:00:00 +0800
comments: true
categories: [Parse]
---

`NSError` is a very common and easy to use object in iOS.

But Parse has shown us how not to use it.

I faced the [same problem](https://www.parse.com/questions/localized-error-description) as Jonathan with using the `NSError` emitted from Parse library.

<!-- more -->

Usually, a `[error localizedDescription]` will print a nice message.

It is especially useful if the error is to do with "The Internet appears to be offline".

However, Parse `[error localizedDescription]` is a generic "The operation cannot be completed (Error 100)".

Firstly, if you look under their [error codes](http://parse.com/docs/dotnet/api/html/T_Parse_ParseException_ErrorCode.htm), error 100 is Connection Failed. But in my case, it is specifically the Internet is offline. 

If you dig around to find other usable error message, you will stumble upon `[[error userInfo] objectForKey:@"error"]`, which is a long debug string that looks like this:

```
Error Domain=NSURLErrorDomain Code=-1009 "The Internet connection appears to be offline." UserInfo=0x1700fc300 {NSUnderlyingError=0x178254cd0 "The Internet connection appears to be offline.", NSErrorFailingURLStringKey=https://api.parse.com/2/client_function, NSErrorFailingURLKey=https://api.parse.com/2/client_function, NSLocalizedDescription=The Internet connection appears to be offline.}
```

How terrible is their design, once [again](/2014/05/30/parse-error-141-uncaught-userid-must-be-a-string/), and [again](/2014/06/30/parse-custom-error-not-possible/), and [again](/2014/07/15/pitfall-with-using-anonymous-user-in-parse/).
