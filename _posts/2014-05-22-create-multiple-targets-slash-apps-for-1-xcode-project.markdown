---
layout: post
title: "Create Multiple Targets/Apps for 1 Xcode Project"
date: 2014-05-22 22:50:32 +0800
comments: true
categories: [iOS]
---

This guide is for building muliple targets/apps for a single project, follow-up of an [old post](http://blog.just2us.com/2009/07/tutorial-creating-multiple-targets-for-xcode-iphone-projects/) in 2009.

If you have 2 or more versions of the same app eg. a lite and a pro version, or many versions of the same map app but for different country, then this guide is for you.

Let's use the scenario of an **"Awesome"** app, which I want to create **"Awesome Lite"**.

<!-- more -->

# 1. Create New Target

Go to Project > Targets > Select the original target "Awesome" > Right click > Duplicate.

When duplicating a target, the default generates "Awesome copy" .app, .plist, etc. We want to change that.

Rename the new target (select and press enter to edit) to "Awesome Lite".

Under Build Settings, search for "Awesome copy". You need to rename **Product Name** to "Awesome Lite", and rename **Info.plist** to "Awesome Lite-info.plist".

In your project, find "Awesome copy-info.plist", and rename to "Awesome Lite-info.plist". After renaming, you would need to delete the "missing one" in your project navigator, and drag the new one in, and select the correct target (Awesome Lite) to copy for.

Pitfall: Take note of the path for the plist. If you place it in the root folder of the project, then you can just specify the name (no need the path). If not, specify the full path `$(PROJECT_DIR)/path/to/Info.plist`.


# 2. Edit Scheme

Go to Product > Scheme > Edit Scheme > change the scheme "Awesome copy" to "Awesome Lite".

At this point, you should now be able to build and run the new scheme (even though it is exactly the same as the original target). We will modify the lite version from this point onwards.


# 3. Edit Info.plist

Go to the new target > Info > edit these accordingly:

- Bundle Display Name
- Bundle Name
- Bundle Identifier


# 4. Writing Preprocessor Codes

Preprocessor codes are used to determine which code would be used during compile time. You may want different targets to run different section of the codes using preprocessor codes.

For example:

```objc
#if defined(TARGET_LITE)
    NSLog(@"Lite version");
#else
    NSLog(@"Original version");
#endif
```

Select "Awesome Lite" target > Build Settings > Preprocessing > Preprocessor Macros > Add `TARGET_LITE` to each of the configuration (eg both Debug and Release configurations).

Warning: You must change for all configurations. The default is Debug and Release, but let's say you created a "Beta", then you have to change that too.


# 5. Resources, Images and Assets Catalog

For any such resources (except Assets Catalog), the trick here is to specify their targets.

Select the resource > File Inspector > Target Membership > check the targets intended.

For Assets Catalog, you have to create for each target because you cannot specify individual target membership for each of the images in it.

You can add "New App Icon" in the new assets catalog, and simply delete the app icon in the old one.


# 6. If you are using Pods

If you are using CocoaPods, and assets doesn't work correctly, read this [pod issue](https://github.com/CocoaPods/CocoaPods/issues/1546).

I added the following to my `Podfile` and it works:

```ruby
# Append to Podfile
post_install do |installer|
    installer.project.targets.each do |target|
        %x~ if [ ! -f Pods/#{target.name}-resources.sh.bak ]; then cp Pods/#{target.name}-resources.sh Pods/#{target.name}-resources.sh.bak; fi ~

        %x~ sed '/WRAPPER_EXTENSION/,/fi\\n/d' Pods/#{target.name}-resources.sh > Pods/#{target.name}-resources.sh.temp ~
        %x~ sed '/*.xcassets)/,/;;/d' Pods/#{target.name}-resources.sh.temp > Pods/#{target.name}-resources.sh ~
        %x~ rm Pods/#{target.name}-resources.sh.temp ~
    end
end
```


