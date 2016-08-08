---
layout: post
title: "iOS Data Storage Guidelines - and how to resolve iTunes Connect Reject"
date: 2013-06-28 01:10
comments: true
categories: [iOS]
published: true
---

Since the introduction of [iCloud Backup](https://www.apple.com/support/icloud/backup/), developers have increasingly faced the following reason for rejected binary:

>	2.23: Apps must follow the iOS Data Storage Guidelines or they will be rejected

>	The iOS Data Storage Guidelines indicate that only content that the user creates using your app, e.g., documents, new files, edits, etc., should be backed up by iCloud. 

This all boils down to understanding [iOS Data Storage Guidelines](https://developer.apple.com/library/ios/qa/qa1719/_index.html).

<!-- more -->

# How App Backup and Update

Apps are backup via iCloud/iTunes, but they do NOT backup the following directories:

1. `<Application_Home>/AppName.app`
2. `<Application_Home>/Library/Caches`
3. `<Application_Home>/tmp`

In addition, if a file has an attribute that says "exclude from backup", it will NOT be backup too.

When you update an application, data in these 2 directories will be copied over:

1. `<Application_Home>/Documents`
2. `<Application_Home>/Library`

Although files in other user directories may also be moved over, you should not rely on them being present after an update.



# Type of data - Critical, Support, Cached, Temporary

Before we move on, these are the definitions for the 4 types of data:

1. **Critical**: Data that cannot be recreated by your app such as user generated content. These need to be backup.

2. **Support**: Needed files which the app can re-generate or download. They should be excluded from backup.

3. **Cached**: Downloaded content such as images, map data, magazine. System could delete them to free up disk space.

4. **Temporary**: Data that are needed for a short while such as during processing. You should delete them after use.

As you see, only critical data should be backup.



# Different iOS Version

There are 3 versions that you [need to beware of](https://developer.apple.com/library/ios/#qa/qa1719/_index.html) when you are using iCloud Backup. It is a fragmentation issue.

Actually it deals with what files you do NOT want to backup. Because if you simply backup every files (including non critical files), Apple will reject your app.

1. 5.1 and later: You can add a file attribute `NSURLIsExcludedFromBackupKey` to specify that you don't want to backup the file. 

2. 5.0.1: A slightly different method that uses `setxattr`

3. 5.0: No method to specify a file not to backup. The only way is to store them in `<Application_Home>/Library/Caches`



# Summary

A guideline for your files.

- For **critcal** data (the only type that need to be backup), store them in:

	- `<Application_Home>/Documents`

- For **support** data, they should be excluded from backup. It is a little complicated due to the different iOS version.

	- For iOS 5.0.1 and later, store in `<Application_Home>/Library/Application Support` and add the file attribute as described in the previous section.

	- If you need to support iOS 5.0, then store in `<Application_Home>/Library/Caches` and still use the file attributes for 5.0.1 and later

	- Note: Support files might not exist when an app backup or update, so you should check if it exists first before use.

- For **cache**, store in 

	- `<Application_Home>/Library/Caches`

- For **temp** files, store in 

	- `<Application_Home>/tmp`


# Code to fix your app

It is your **support** data in `<Application_Home>/Library/Application Support` that should be excluded from backup. 

You can add the following code in `application:didFinishLaunchingWithOptions:` to exclude all files and directories:

```objc
NSArray *paths = NSSearchPathForDirectoriesInDomains(NSApplicationSupportDirectory, NSUserDomainMask, YES);
NSString *basePath = ([paths count] > 0) ? [paths objectAtIndex:0] : nil;
NSArray *documents = [[NSFileManager defaultManager] contentsOfDirectoryAtPath:basePath error:nil];
NSURL *URL;
NSString *completeFilePath;
for (NSString *file in documents) {
    completeFilePath = [NSString stringWithFormat:@"%@/%@", basePath, file];
    URL = [NSURL fileURLWithPath:completeFilePath];
    NSDictionary *values = [URL resourceValuesForKeys:@[NSURLIsExcludedFromBackupKey] error:nil];
    if ([values[NSURLIsExcludedFromBackupKey] boolValue] == NO) {
        NSLog(@"To exclude from iCloud backup: %@", completeFilePath);
        [URL setResourceValue:@(YES) forKey:NSURLIsExcludedFromBackupKey error:nil];
    }
}
```
