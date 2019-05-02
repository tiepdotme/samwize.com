---
layout: post
title: "Pitfall on App ID Prefixes (Legacy)"
date: 2018-09-20T09:46:27+08:00
categories: [Certs]
---

I stumbled on a [similar Keychain issue](https://github.com/kishikawakatsumi/KeychainAccess/issues/52) with error code `-34018`, which is `missingEntitlement`, and the message is:

> Internal error when a required entitlement isn't present, client has neither application-identifier nor keychain-access-groups entitlements.

But my problem is not because of a missing entitlement file.

My problem is that my app ID prefix is wrong.

## What is App ID Prefix?

In [Technical Note TN2311](https://developer.apple.com/library/archive/technotes/tn2311/_index.html), you would learn of this "feature".

> Every keychain item in iOS contains an attribute called the keychain access group. An iOS app can only access those keychain items it has permission to. This permission comes from the code signing entitlements stamped into the app when it is signed (using your current App ID prefix).

Until 2011, developers may create different App ID Prefix, which group a number of apps together so as to share keychain and `UIPasteboard`. This is way for "cross-app communication".

Then in 2011, Apple took away this pretty needless feature, and use the default team ID that is associated with an Apple team account. This is simpler.

_UPDATE: I have another [post on App Group, and explaining the different IDs](/2018/09/23/app-group-for-sharing-between-apps-and-extensions/)._

## I have multiple prefixes

I have been creating apps for a [long time](/2018/06/01/evolution-of-my-code-in-last-10-years/), and I ended up having a non-team ID prefix for an app.

It took me a while to realize the issue.

The error was misleading to start with.

And you can only find out your app ID prefix by logging into [Apple developer member portal](https://developer.apple.com/), and look under App IDs.

## How to see my entitlement and keychain groups?

To verify that prefix ID is correct, check the actual entitlements in the app.

You can [inspect them](https://developer.apple.com/library/archive/qa/qa1798/_index.html) when you upload the app via Xcode, or validate an existing archive.

Or if you have an IPA, you can inspect the content and look for "embedded.mobileprovision".

## Can I migrate to team ID prefix?

It is possible, but you will lose keychain data, because 1 version of an app cannot have multiple keychain access groups.

1. Go to developer member centre
2. Create a new App ID with the team ID prefix
3. You may leave the old App ID with non-team ID prefix
4. Re-create all the provisioning profiles with the new App ID

Note: The "new App ID" can have the same app identifier string eg. "com.just2us.app". You won't want to change this because if an app identifier is changed, it is a new app on App Store!

I didn't migrate, and stick to using my old App ID, since it is not critical for me app to share keychain with my other apps. If I need to share data, there are other ways such as [app access group](https://developer.apple.com/documentation/security/keychain_services/keychain_items/sharing_access_to_keychain_items_among_a_collection_of_apps).

If I really want to migrate, I will need 1 update to migrate out of the keychain (to `UserDefaults` or persist on file), wait for a few years to make sure most users did migrate.. then another update to migrate to the new keychain.
