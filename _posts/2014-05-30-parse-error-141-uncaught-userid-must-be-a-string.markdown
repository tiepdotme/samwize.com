---
layout: post
title: "Parse error 141: Uncaught userId must be a string"
date: 2014-05-30 18:05:22 +0800
comments: true
categories: [Parse]
---

[Parse](https://www.parse.com) default ACL is - everyone can read and everyone can write.

Even for their User class.

That is a rather bad design.

<!-- more -->

## The 'Solution'

Parse official rep [answered](https://parse.com/questions/default-acl-for-pfuser) that you may use cloud code to apply an ACL with a beforeSave trigger.

Nice.

Until you get error the error:

    Parse error 141 - Uncaught userId must be a string

And in your cloud code log, you clearly see an `Uncaught userId must be a string`


## The Correct Solution

The [correct way](https://www.parse.com/questions/errors-when-trying-to-set-acls-in-user-beforesave) is to use the **afterSave** trigger (not **beforeSave**).

```js
Parse.Cloud.afterSave(Parse.User, function(request) {
  var user = request.user;
  if (!user.existed()) {
    // ACL - only user can read and write
    user.setACL(new Parse.ACL(user));
    user.save();
  }
});
```

I wasted quite some time as I was misled by a flawed solution by a Parse rep.

And, I believed there are others who were misled, yet no one can reply to the [flawed solution](https://parse.com/questions/default-acl-for-pfuser) because comments are closed..
