---
layout: post
title: "Everything about iOS Localization"
date: 2014-04-10 16:23:03 +0800
comments: true
categories: [iOS, Localization]
---

## Why localize your app

Because it can [increase your app download by 128%](http://www.distimo.com/blog/2012_10_publication-the-impact-of-app-translations/), according to Distimo report.

It is a fact: [English is only the third largest language](http://exploredia.com/how-many-people-in-the-world-speak-english-2013/) (after Chinese and Spanish). And there is around 1 billion people who speaks English.

Don't ignore others.

<!-- more -->

## Start using NSLocalizedString

If you are using a string that will be displayed to your user, always use the macro `NSLocalizedString`.

```objc
    NSLocalizedString(@"some string", nil)
```

## Then genstring

Thereafter, you can easily extract all the string to a `.strings` file.

    find . -name "*.m" | xargs genstrings -o tmp

In your project, under Info > Localizations, add a new language.

Replace with the strings file generated.


## Plural Support

New in [Foundation](https://developer.apple.com/library/Mac/releasenotes/Foundation/RN-Foundation/index.html), you can have a `.stringsdict` file alongside the `.strings`.

Refer to the section on **Localized Property List File**.

The dict looks like this:

```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
    <plist version="1.0">
    <dict>
        <key>%d files are selected</key>
        <dict>
            <key>NSStringLocalizedFormatKey</key>
            <string>%#@num_files_are@ selected</string>
            <key>num_files_are</key>
            <dict>
                <key>NSStringFormatSpecTypeKey</key>
                <string>NSStringPluralRuleType</string>
                <key>NSStringFormatValueTypeKey</key>
                <string>d</string>
                <key>zero</key>
                <string>No file is</string>
                <key>one</key>
                <string>A file is</string>
                <key>other</key>
                <string>%d files are</string>
            </dict>
        </dict>
    </dict>
    </plist>
```


## Upper/Lower Case

In NSString, you should use `uppercaseStringWithLocale:` and `lowercaseStringWithLocale:` methods instead.


## Formatting Dates

In NSDateFormatter, you should use `dateFormatFromTemplate:options:locale:` instead.

Refer to the [documentation](https://developer.apple.com/library/mac/documentation/cocoa/Reference/Foundation/Classes/NSDateFormatter_Class/Reference/Reference.html#//apple_ref/occ/clm/NSDateFormatter/dateFormatFromTemplate:options:locale:). Displaying dates is much harder than you think, with the punctuation and ordering all different.

This is the string formats: [TR35-31](http://www.unicode.org/reports/tr35/tr35-31/tr35-dates.html#Date_Format_Patterns)


## The right locale

The current system locale is `[NSLocale currentLocale]`.

But sometimes your app only is supporting certain locale, and you want the presentation to be consistent. In that case, you should use:

```objc
    NSString *localization = [NSBundle mainBundle].preferredLocalizations.firstObject;
    NSLocale *locale = [[NSLocale alloc] initWithLocaleIdentifier:localization];
```

You might also want to know when the locale has changed.

```objc
    [[NSNotificationCenter defaultCenter] addObserver:self
                                             selector:@selector(localeDidChange)
                                                 name:NSCurrentLocaleDidChangeNotification
                                               object:nil];
```

## Test on Simulator

To test, you can go to Settings app in simulator to change the language.

Or if you prefer faster way, you can set the launch arguments in Scheme > Run > Arguments, and add the following:

    -AppleLanguages (zh-Hans)

The above will start the simulator in Chinese Simplified. Edit the language accordingly such as es, ko, de


## Third Party Services

There are mnay services that provides translations. Apple even has a [list of vendors](https://developer.apple.com/internationalization/).

I recommend: [OneSky](http://www.oneskyapp.com)

This is because:

- they have a free management tool that is the best (can even add screenshots)

- it's free to to use their crowdsource up to 5 collaborators

- their charge of 0.10 USD/word is comparable to others

I would use OneSky to ask my friends and users to help in translation.


## Google API

It's usually terrible to translate using Google API. They probably can only get 50% right.

The other 50% is either grammatically wrong, or assumed wrong context.

But if you still want to, you can use the [REST API](https://developers.google.com/translate/v2/getting_started) at $20 per 1 million characters.

Or use [poeditor](https://poeditor.com) which comes with free 10,000 character.

Or manually use the [Google Translate web](http://translate.google.com) for free.

## Fallback to base language

Sometimes, you have incomplete translation, and would need the app to fallback to the base language. Read [this](/2018/01/23/localization-fall-back-to-base-language/).
