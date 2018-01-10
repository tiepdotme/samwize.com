---
layout: post
title: "UI Testing With BrowserStack Cloud Devices"
date: 2018-01-09T16:53:56+08:00
categories: [Testing]
---

This guide is on using BrowserStack to run UI Tests on cloud devices.

I don't recommend using BrowserStack, unless you need:

- UI Testing with cloud devices
- Want the same stack for testing across all platforms (Android, iOS, Web)
- Probably a dedicated QA engineer

## If you are an indie..

If you don't need all of the above, and is just an indie, focusing on iOS, then keep it simple.

For UI Testing, use `XCUITest` framework and add test cases to the project in Xcode. Make use of the UI recorder.

Run on as many simulators.

Integrate to your CI tool eg. travis. 

## Ok, so you want BrowserStack..

But if you are still here, ok, I have _some guide_ for you, because the ecosystem is just too complicated. Many things are never explained.

[BrowserStack documentation](https://www.browserstack.com/app-automate/appium-webdriverio#integration-with-browserstack) is terrible. They will guide you on how to set up with Appium, that's all.

BrowserStack main contribution is their cloud devices, aka the [capabilities](https://www.browserstack.com/app-automate/capabilities) you can use.

## It's all about Appium

You need to [understand Appium](http://appium.io/docs/en/about-appium/intro/?lang=en), know the goals, and how they achieve it.

- You test on the same compiled app you ship to App Store
- Appium use vendor supplied framework eg XCUITest under the hood
- WebDriver API wraps the hood, providing a REST + JSON API to drive the tests
- WebDriver is the same protocol as Selenium
- Extend WebDriver for mobile automation
- Appium is open source

Appium has a **server-client architecture**.

BrowserStack simply runs the server part.

## WebDriver.io as client

You need to use a client and write the tests, and WebDriver.io is one such client. There are [others](https://github.com/webdriverio/webdriverio/issues/138).

Let's look at the JS code that uses webdriver.

```javascript
it('should show do something', () => {
return browser
  .waitForVisible('//XCUIElementTypeOther[@name="container-foo"]/XCUIElementTypeCollectionView/XCUIElementTypeCell[1]/')
  .then(visible => {
    expect(visible).to.be.true;
  })
  .click('~some-accessibility-id');
});
```

Each of the API called from `browser` is [WebDriver's API](http://webdriver.io/api.html). 

## Selector

In the code, we illustrated 2 ways you can refer to UI elements.

Using **accessibility identifer** such as `~some-accessibility-id` is the preferred way.

Because **Xpath** is not stable on Appium!

Note: Don't use the [old UIAutomation framework selector](https://github.com/appium/appium/blob/master/docs/en/advanced-concepts/migrating-to-xcuitest.md).

## Appium Inspector

You can easily find the Xpath by using the [Appium Desktop Inspector app](https://github.com/appium/appium-desktop).

1. Start Server
2. Start Inspector Session
3. Create and save a capability
4. One of the capability you must add is `app` which points to your IPA file eg `/Users/junda/Library/Developer/Xcode/DerivedData/MyApp-avrefqcpyqccstgkxgaottezlrvt/Build/Products/Debug-iphonesimulator/MyAPp.app`
5. Start Session
6. Xcode simulator will run and you can inspect!
