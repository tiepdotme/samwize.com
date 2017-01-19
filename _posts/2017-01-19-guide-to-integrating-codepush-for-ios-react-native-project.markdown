---
layout: post
title: "Guide to Integrating CodePush for iOS React Native Project"
date: 2017-01-19T16:55:28+08:00
categories: [react-native]
---

[CodePush](https://microsoft.github.io/code-push/) is a wonderful technology to make INSTANT changes to your app.

The problem with CodePush documentation is that it is toooo long, because it has to cover for multiple platforms. I ran into a few pitfalls, and missed some steps, because of that.

So this post will only cover for an iOS, React native app.

## Install and Setup the CLI

    # Install the CLI
    npm install -g code-push-cli

    # Register for an account via github oauth
    code-push register

    # Register your app. We call it AwesomeApp.
    code-push app add AwesomeApp

## Setup React Native Project

`cd` to your React Native project and install the "plugin".

    npm install --save react-native-code-push@latest

Next, setup the `sync` for your component. There are more [options](https://github.com/Microsoft/react-native-code-push#codepush) on the syncing of the code eg. frequency, install etc

```javascript
class AwesomeApp extends Component {
  ...
}

let codePushOptions = { 
  checkFrequency: codePush.CheckFrequency.ON_APP_RESUME, 
  installMode: codePush.InstallMode.ON_NEXT_RESUME 
}
AwesomeApp = codePush(codePushOptions)(AwesomeApp);

module.exports = AwesomeApp
```

My configuration for above is to check for updates when app goes into foreground, and then install when it goes into foreground **again**. I belive that's good enough.

Pitfall: I made the mistake of writing `export default class AwesomeApp` at first. Because `codePush` wraps the component, you have to export it after that.


## Setup iOS in Xcode

Next, you need to setup the project in Xcode.

Add the pod to `Podfile`, and then `pod install`.

    # Change your path accordingly. My React Native source is in ./react
    pod 'CodePush', :path => './react/node_modules/react-native-code-push'

Then in the code where you use the `NSURL`, change it like this:

```swift
import CodePush

func sourceURLForBridge(bridge: RCTBridge!) -> NSURL! {
    #if DEBUG
        return NSURL(string: "http://localhost:8081/index.ios.bundle?platform=ios")
    #else
        return CodePush.bundleURL()
    #endif
}
```

I have a `sourceURLForBridge` function, which will return the appropriate `NSURL` to `RCTRootView`.

Using preprocessor/macros, the code will use the packager running on `localhost:8081` in `DEBUG` build configuration, and using CodePush URL for `RELEASE` (or otherwise).

## Setup CodePush Keys in Xcode

When you setup code-push CLI, in `code-push app add AwesomeApp`, you were given 2 keys - Staging and Production.

You have to add these keys to your iOS project.

1. Add to `Info.plist` with the key `CodePushDeploymentKey`, and the value `$(CODEPUSH_KEY)`
2. In project Build Settings, "Add User-Defined Settings" with the key `CODEPUSH_KEY`
3. Expand `CODEPUSH_KEY`, and under **Release**, set to the CodePush **Production** key

The above is setup to only use CodePush production deployment.

If you want to use a Staging deployment, you will have to add another build configuration. Go to Project > Info > Configurations and duplicate Release, and rename it to **Staging**. 

Then similarly like you did for Production key in Build Settings, set `CODEPUSH_KEY` for **Staging** to CodePush staging key.

To recap, you have now 3 build configurations:

1. Debug - Use packager localhost:8081
2. Release - Use CodePush Production
3. Staging - Use COdePush Staging


## Push Live Code

At last, we are ready to push live code! Let's demo how you do for production.

Build the iOS app with Release build configuration (check your scheme), and run in Simulator or device.

While you have not yet push any code to Production, the app still runs fine, because for the first run, it will use the local main.jsbundle.

Make some changes to `AwesomeApp` now. Perhaps change a `<Text>`? Or a color?

Then push it:

    code-push release-react AwesomeApp ios -d Production --plistFile "../AwesomeApp/Info.plist"

By default, -d is Staging, and plist is in ./ios. You may change as necessary.

Bring the app to foreground, and it should still be the same (though it has now fetched the update).

Bring the app to background, then foreground again, and viola! It will install the new code and reflect the changes!


## Pitfall: Not Getting Updates

There are quite some [pitfalls](https://microsoft.github.io/code-push/docs/react-native.html#link-13), and the long documentation doesn't help.

I hope this guide has been more concise (:

If you are not getting the update,

- make sure the version of the iOS app (in `Info.plist`), matches that when you [code push with targetBinaryVersion](https://github.com/Microsoft/code-push/blob/master/cli/README.md#releasing-updates-general)
- make sure the app is using `CodePush.bundleURL()`
- make sure the keys are correct, and you deploy production/staging correctly
- debug with `code-push debug ios` and run the simulator
