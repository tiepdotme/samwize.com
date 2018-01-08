---
layout: post
title: "UI Testing With BrowserStack Cloud Devices"
date: 2017-12-29T16:53:56+08:00
categories: []
published: false
---

https://www.browserstack.com/app-automate/appium-webdriverio#integration-with-browserstack

https://www.browserstack.com/app-automate/capabilities


## All about Appium

http://appium.io/docs/en/about-appium/intro/?lang=en

Know the goals, and how they achieve it.

- You test on the same compiled app you ship to App Store
- Appium use vendor supplied framework eg XCUITest under the hood
- WebDriver API wraps the hood, providing a REST + JSON API to drive the tests
- WebDriver is the same protocol as Selenium
- Extend WebDriver for mobile automation
- Appium is open source

Appium has a server-client architecture.

BrowserStack runs the server part.

## WebDriver.io as client

You need to use a client and write the tests.

Travis could be running the client, driving the tests, which runs on the server -- BrowserStack or Sauce Lab or etc -- with real devices or simulator.

<diagram>

https://github.com/webdriverio/webdriverio/tree/master/examples - how to use the client

https://github.com/webdriverio/webdriverio/issues/138
Discussion of the difference VS selenium-webdriverjs VS WD.js

## Finding elements

http://webdriver.io/guide/usage/selectors.html#iOS-UIAutomation

The WebDriver JS API can select element ~~using XCUITest API~~ like this:

```js
    var selector = 'UIATarget.localTarget().frontMostApp().mainWindow().buttons()[0]'
    browser.click('ios=' + selector);
```

> -ios uiautomation: a string corresponding to a recursive element search using the UIAutomation library (iOS 9.3 and below only)

They are using the old UIAutomation library, not the XCUITest..

Appium says they support and migrated, with [caveats](https://github.com/appium/appium/blob/master/docs/en/advanced-concepts/migrating-to-xcuitest.md).

Using [predicate can too](https://github.com/appium/appium/blob/master/docs/en/writing-running-appium/ios/ios-predicate.md)!


http://appium.io/docs/en/writing-running-appium/finding-elements/


https://github.com/appium/sample-code/tree/master/sample-code/examples/node


https://github.com/appium/appium-desktop

http://appium.io/docs/en/writing-running-appium/caps/index.html