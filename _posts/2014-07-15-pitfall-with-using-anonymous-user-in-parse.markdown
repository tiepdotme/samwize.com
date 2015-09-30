---
layout: post
title: "Pitfall with using Anonymous User in Parse"
date: 2014-07-15 22:12:40 +0800
comments: true
categories: [Parse]
---

[Once](/2014/05/30/parse-error-141-uncaught-userid-must-be-a-string/) [again](/2014/06/30/parse-custom-error-not-possible/), Parse disappointed me with their developer support.

Their [Anonymous User feature](https://www.parse.com/docs/ios_guide#users-anonymous/iOS) is a great feature, but it lacks the last 10% to be awesome.

When I was using the feature and did a query, it crashed. 

<!-- more -->

```
*** Terminating app due to uncaught exception 'NSInternalInconsistencyException', reason: 'Tried to save an object with a new, unsaved child.'
```

I found the [same incident](https://www.parse.com/questions/crash-on-query-with-new-anonymous-user) on Parse forum.

The reply from a Parse official rep is again disappointing.

Let me explain the pitfalls and shortcoming of Parse:

- Parse will automatically save an Anonymous User when an a `PFUser` (as an object or as a relation) is saved

- However, calling `PFQuery`, it will trigger the error because no object was automatically saved (probably due to your logic flow)

- Yet, strangely it seems, a `PFQuery` does not handle an anonymous user situation at all, and caused the crash

I felt this is a bug, just like the 2 other developers in the forum. Unfortunately, the forum, yet again, is closed for comments.

Parse, please improve your library.

## Workaround

As suggested, one could check to make certain `PFUser` is saved by checking the objectId. If it isn't, save it, then perform the query:

```objc
if ([PFUser currentUser].objectId == nil)
  [[PFUser currentUser] saveInBackgroundWithBlock:^(BOOL succeeded, NSError *error) {
    // Perform the query fetch in block
  }];
else
  // Perform the query fetch directly
```



