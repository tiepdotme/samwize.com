---
layout: post
title: "Android use-feature element is case sensitive"
date: 2013-05-09 22:12
comments: true
categories: [Android]
---

In the previous post, I discussed about how to [Google Play manage compatibility](http://samwize.com/2013/05/07/why-an-android-app-is-not-supported-for-a-particular-device/) between your app and devices.

The key is to make `use-feature` optional.

Recently, I have encountered another problem which I want to share.

I have this in my manifest file:

```xml
<uses-feature android:name="android.hardware.TELEPHONY" android:required="false" />

<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

On first look, it seems ok. Making TELEPHONY optional will make the app compatible with tablet (a common solution).

However, the above is WRONG! Because it should be `android.hardware.telephony` (lower case)!

I made a mistake.

But I also blame Android, because for permission, it is upper case! `android.permission.INTERNET` is correct.

Weirdedness of Android..