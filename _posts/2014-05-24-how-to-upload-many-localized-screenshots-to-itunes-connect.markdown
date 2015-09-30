---
layout: post
title: "How to upload (many) localized screenshots to iTunes Connect"
date: 2014-05-24 11:34:49 +0800
comments: true
categories: [iOS, Localization]
---

_UPDATE (Feb 2015): I have [replaced these tools](/2015/01/29/fastlane-replacing-ui-screen-shooter-and-screenshot-uploader/) with Fastlane._

---

If you [localize your app](/2014/04/10/everything-about-ios-localization/), you should be also localizing your screenshots.

It sounds like a nightmare if you have 10 supported languages. Multiply that be 3 device types - iPhone 3.5 inch, iPhone 4 inch, and iPad - and you will have 150 screenshots to capture, and finally upload!

Fortunately, iOS development has matured so much that there are tools and scripts to help in these processes.

<!-- more -->

# Automate screenshot capturing

[ui-screen-shooter](https://github.com/jonathanpenn/ui-screen-shooter) is some scripts to automatically build and run simulator on different languages, and automatically take screenshots.

This is how you use:

1. [Download](https://github.com/jonathanpenn/ui-screen-shooter/archive/master.zip) and unzip

2. You only need to copy these 4 files to your project root: shoot_the_screens.js, ui-screen-shooter.sh, capture.js, unix_instruments.sh

3. Edit `ui-screen-shooter.sh` for the `languages` you have localized. You may edit `simulators` eg if you don't need iPad.

4. Edit `shoot_the_screens.js` for the actual UIAutomation. Tip: Run Instruments, and record your actions, and copy the javascript code.

5. Run `./ui-screen-shooter.sh ~/Desktop/screenshots` on your project

Go drink a coffee while the screenshots are being automatically captured!


# Alias itms

iTMSTransporter is the command line tool provided by Apple for the geekies, as an alternative to iTunes Connect website.

For convenience, I [added an alias](http://stackoverflow.com/a/17786822/242682) so that I can run fine with just `itms`:

You can add this to your .bash_profile, or just run in on your bash:

    alias itms='`xcode-select --print-path`/../Applications/Application\ Loader.app/Contents/MacOS/itms/bin/iTMSTransporter'

For myself, I use [dotfiles](http://samwize.com/2014/01/12/getting-started-with-dotfiles/), hence I added the alias to my `~/dotfiles/.aliases`, and make sure my zsh [work nicely](http://samwize.com/2014/01/19/load-dotfiles-aliases-in-zsh/) with it.


# Screenshot Uploader

[itc-localized-screenshot-uploader](https://github.com/fulldecent/itc-localized-screenshot-uploader) is a helper to prepare your screenshots when using iTMSTransporter. Note: I am using a forked version, which is better and clearer.

Specifically, the PHP script generates the `metadata.xml` with the screenshots and their checksum.

This is how you use:

1. `~/Desktop/screenshots` - Firstly, this is where all your screenshots must be in (the default if using ui-screen-shooter)

2. Change the ui-screen-shooter format to the required format. Run this command in `~/Desktop/screenshots`:

        for a in */*; do d=$(dirname $a); f=$(basename $a); mv $a ${d}___${f}; done; rmdir */

3. Download your app metadata package:

    ITMSUSER=YourItunesUsername
    ITMSPASS=YourItunesPassword
    ITMSSKU=YourAppSKU
    itms -m lookupMetadata -u $ITMSUSER -p $ITMSPASS -vendor_id $ITMSSKU -destination ~/Desktop

    Edit your iTunes Connect username, password and your app sku. Note that the destionation must be ~/Desktop.

4. [Download](https://github.com/fulldecent/itc-localized-screenshot-uploader/archive/d72b891a3af4e5a31654de2678ce30a999b9b2bb.zip), unzip, then run `php screenshots.php`

5. `metadata.xml` should be successfully generated in `~/Desktop/screenshots`. Copy this and the screenshots to the itms package.

    cp ~/Desktop/screenshots/* ~/Desktop/*.itmsp

6. Verify

    itms -m verify -u $ITMSUSER -p $ITMSPASS -vendor_id $ITMSSKU -f ~/Desktop/*.itmsp

7. Upload

    itms -m upload -u $ITMSUSER -p $ITMSPASS -vendor_id $ITMSSKU -f ~/Desktop/*.itmsp


# Subsequent Versions

The first time performing this automation could be daunting. But if you follow the steps above closely, it will save you much precious time.

Automating it subsequently is much easier:

    # Fetch the latest itmsp
    ITMSUSER=YourItunesUsername
    ITMSPASS=YourItunesPassword
    ITMSSKU=YourAppSKU
    itms -m lookupMetadata -u $ITMSUSER -p $ITMSPASS -vendor_id $ITMSSKU -destination ~/Desktop

    # Edit shoot_the_screens.js if UI has changes
    # Automate the shooting, then rename them
    ./ui-screen-shooter.sh ~/Desktop/screenshots
    cd ~/Desktop/screenshots
    for a in */*; do d=$(dirname $a); f=$(basename $a); mv $a ${d}___${f}; done; rmdir */

    # Generate the new metadata.xml
    php screenshots.php

    # Copy all files to itmsp and upload
    cp ~/Desktop/screenshots/* ~/Desktop/*.itmsp
    itms -m upload -u $ITMSUSER -p $ITMSPASS -vendor_id $ITMSSKU -f ~/Desktop/*.itmsp


# Common Pitfalls

### Locale identifiers

The locale identifiers used in iTMSTransporter is strict. Instead of 'en', it is 'en-US'. Instead of 'zh-Hans', it is 'cmn-Hans'.

After itms `lookup`, you can find out the locale identifiers with

    grep locale ~/Desktop/*.itmsp/metadata.xml  | grep name | sort -u

You must use these for the languages.

Then re-run `./ui-screen-shooter.sh ~/Desktop/screenshots`. Then re-upload again.


### Screenshot filenames

If you didn't modify `capture.js`, then the filenames should be suitable for uploading with itms.

The files names should be like `iOS-3.5-in___portrait___screen1.png`. The "iOS-3.5-in" is used in the metadata.xml as display_target.

You should also delete all screenshots starting with "UIATarget-" which are automatically generated.


### Create the new locale first

Go to iTunes Connect, and manually add in the new language (if you haven't). Just use the default screenshots first. Note: You can only add new language for new version of your app.


### Error: Screenshots cannot be edited in the current state

If itms upload gives an error `software_screenshots cannot be edited in the current state`, just try again 1 hour later.

You have probably edited your app status ("Waiting for Binary Upload"), yet iTunes Connect seems to be lagged in knowing your state.


