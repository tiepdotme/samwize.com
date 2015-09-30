---
layout: post
title: "Where to store CoreData sqlite file - and avoid Apple App review rejection"
date: 2013-09-03 15:41
comments: true
published: true
categories: [iOS]
---

I seems to be getting Apple app review [rejection](/2013/06/28/ios-data-storage-guidelines-and-how-to-resolve-itunes-connect-reject/) every time I sumbit an app update. This time, it is about my use of `UIFileSharingEnabled`.

The app review team said:

> Your app has the UIFileSharingEnabled key set to true in the Info.plist, but this feature is not functional.
>
> When file sharing is enabled, the entire Documents folder is used for file sharing. Files that are not intended for user access via the file sharing feature should be stored in another part of your application's bundle. If your application does not require the file sharing feature, the UIFileSharingEnabled key in the Info.plist should not be set to true.

<!-- more -->

The problem with my app is in the 2nd paragraph, which says *files in Documents folder should only be files that user need to access*.

I have a `myapp.sqlite` in `/Documents` (hence a violation).

There wasn't a problem 5 years ago when iOS first started, and in fact I am sure a lot of code around the Internet set up the CoreData persistant document in Documents folder.

However, things evolved, and there are [guidelines](http://samwize.com/2013/06/28/ios-data-storage-guidelines-and-how-to-resolve-itunes-connect-reject/) to how you use the file system.

To cut things short, the **best place** to put your sqlite file is in `/Library`.

- It will not be exposed in iTunes File Sharing

- It will still be backup automatically
