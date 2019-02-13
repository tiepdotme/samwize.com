---
layout: page
title: "KIV Libraries"
permalink: /libs/kiv/
---

I have yet to try these, but they look awesome.

- iOS 10 drawer UI - [Pulley](https://github.com/52inc/Pulley) and [SPStorkController](https://github.com/IvanVorobei/SPStorkController)
- [Permission](https://github.com/IvanVorobei/SPPermission) request
- [Spectrum](https://github.com/facebookincubator/spectrum) - Lossless resize, rotate operations on image
- [R](https://github.com/mac-cain13/R.swift) - This is like Android's `R`, generating a file with constants to the strings
- [Ensemble](http://www.ensembles.io/) - sync Core Data to iCloud or Dropbox ($299)
- [Deep Belief](https://github.com/jetpacapp/DeepBeliefSDK) - Image recognition
- [Layout](https://github.com/schibsted/layout) - Supports live reloading and creating view hierarchy with XML (kind of like Android). [Nick Lockwood](http://bytes.schibsted.com/layout-declarative-ui-framework-ios/) builds the higher level UI system.
- [Expression](https://github.com/nicklockwood/Expression) - Also by Nick Lockwood. Evaluate simple expression in String such as "1 + 2".

## Segmented Controls

- [BetterSegmentedControl](https://github.com/gmarm/BetterSegmentedControl) - Swift 3, with just 1 file cleverly creates a segmented control with selected text being masked
- [Segmentio](https://github.com/Yalantis/Segmentio) - Animated segmented control
- [HMSegmentedControl](https://github.com/HeshamMegid/HMSegmentedControl) - Google Currents like segments
- [DZNSegmentedControl](https://github.com/dzenbot/DZNSegmentedControl) - Segmented Control views

## Others

- KVOController - by Facebook for observing key-value easily
- ViewDeck - Slide a view over like Path app
- JLRoutes - URL schemes
- Kiwi - BDD testing
- SCWaveformView - from audio/video
- SHUIKitBlocks - UIKit replacement - block instead of delegates
- Tweaks - a settings for on/off debug stuff, by Facebook
- LocationManager - grab once easily

---

# UIKit

## UITextField

- [GCPlaceholderTextView](https://github.com/gcamp/GCPlaceholderTextView) - also for UITextView
- [MLPAutoCompleteTextField](https://github.com/EddyBorja/MLPAutoCompleteTextField.git) - Auto complete. Demo with countries.
- [SlackTextViewController](https://github.com/slackhq/SlackTextViewController)
- [MPGTextField](https://github.com/gaurvw/MPGTextField) - auto complete dropdown

## UIButton/Buttons

- [ZFRippleButton](https://github.com/zoonooz/ZFRippleButton) - nice ripple effect when touch button
- [FoldingTabBar.iOS](https://github.com/Yalantis/FoldingTabBar.iOS) - expand circular plus button to a "tab bar"

## Alert Sheet/View/Popup Modal

- [SHActionSheetBlocks](https://github.com/seivan/SHActionSheetBlocks)
    + More block based libs at [SHUIKitBlocks](https://github.com/seivan/SHUIKitBlocks)
- UIActionSheet-Blocks - easier using category
- UIAlertView-Blocks - with blocks
- [AMSmoothAlert](https://github.com/mtonio91/AMSmoothAlert) - catchy dropdown effect for 3 types: info, error, success
    + Author also has a [Login VC](https://github.com/mtonio91/AMLoginViewController/tree/master/AMLoginViewController) which plays a video for the background
- [MZFormSheetPresentationController](https://github.com/m1entus/MZFormSheetPresentationController) - Can use storyboard to design, and it will simply fit it into a dialog/form
- [DropdownMenu](https://github.com/nmattisson/DropdownMenu) - Drop menu from a button
- [CWStatusBarNotification](https://github.com/cezarywojcik/CWStatusBarNotification) - very smart way of showing in status bar (or navigation bar). It uses snapshot, a fake image view temporarily.

## Pull to refresh

- ODRefreshControl - native didn't even work
- [DGElasticPullToRefresh](https://github.com/gontovnik/DGElasticPullToRefresh): With animation using UIBezierPath

## Slide to reveal menu

- SWRevealViewController
- PHAirViewController - like AirBnb
- iOS-Slide-Menu
- RW tutorial
- MMDrawerController
- [RESideMenu](https://github.com/romaonthego/RESideMenu)

## Chart

- JBChartView

## Progress Bar

- YLProgressBar - nice, and without images

## Image View/UIImage

- [FastImageCache](https://github.com/path/FastImageCache) - Load image at higher FPS, less memory usage, by sacrificing disk usage
- JTSImageViewController - full screen, pan, zoom, flick away - like lightbox
- SDWebImage - Web view and cache and gif. But they say AFNetworking provides the same
- PAAImageView - Rounded, using AFNetworking and lightly cached
- ZDStickerView - edit with 1 finger
- UzysImageCropper
- [TDImageColors](https://github.com/timominous/TDImageColors): Detect most used color
- [UIImageViewModeScaleAspect](https://github.com/VivienCormier/UIImageViewModeScaleAspect) - Animate between 2 contentMode

## Graphics

- [NXDrawKit](https://github.com/Nicejinux/NXDrawKit) - draw on view, written in Swift
- [jot](https://github.com/IFTTT/jot) - Add touch-controlled drawings, by IFTTT

## Map View

- Clustering
- FBAnnotationClustering
- CCHMapClusterController

## Activity

- [NVActivityIndicatorView](https://github.com/ninjaprox/NVActivityIndicatorView) - Collection of nice loading animations

## Tags/Tokens

- [JSTokenField](https://github.com/jasarien/JSTokenField) - similar to SMS recipients
- [TLTagsControl](https://github.com/ali312/TLTagsControl) - with mode editable

## Effects

- Shimmer - A shimmering effect to show progress
- DEInfiniteTileMarqueeView - marquee image
- UIImage-BlurredFrame - apply blur
- RQShineLabel - UILabel random char fade in
- FXBlurView
- [YIInnerShadowView](https://github.com/inamiy/YIInnerShadowView) - Drop inner shadow

## Other Views

- [10Clock](https://github.com/joedaniels29/10Clock) - Circular sleep to wake period
- [Eureka](https://github.com/xmartlabs/Eureka) - Create forms eg section `+++` row
- [SZTextView](https://github.com/glaszig/SZTextView) - UITextView with placeholder
- [Flat UI](https://github.com/Grouper/FlatUIKit)
- MDCSwipeToChoose
- [RAMReel](https://github.com/Ramotion/reel-search) - a text field and list selector, that animates beautifully
- [RPSlidingMenu](https://github.com/RobotsAndPencils/RPSlidingMenu) - a parallax effect as you drag in the collection view
- Chameleon - Auto color scheme

---

## VoIP/SMS/Calls

[Sinch](https://www.sinch.com) - rebtel using? Seems mature. Talked about PushKit.

## Multiple libs-in-1

twotoasters (pod subspecs! but not popular lib)
DerpKit - KVO+blocks and others

## Swift Web Frameworks

This is not iOS, but Swift. And for sure many web frameworks will be created, just like JS.

- https://github.com/qutheory/vapor
  - Inspired by Laravel. Pretty comprehensive on how to deploy to DO and Heroku.
- https://github.com/PerfectlySoft/Perfect
- https://github.com/izqui/Taylor

## Apple Watch

- [JBWatchActivityIndicator](https://github.com/mikeswanson/JBWatchActivityIndicator) - create activity indicator and output each frame

## Markdown

- AttributedMarkdown - customise the font style for PARA, EMPH, etc
- MXMarkdownKeyboard - using inputAccessoryView
- GHMarkdownParser - GitHub flavour
- Cocoanetics/DTMarkdownParser
- MMMarkdown - popular, but not yet supporting mmd
- [bypass](https://github.com/Uncodin/bypass) - Render in UITextView (not html)
- [/NimbusKit/markdown](https://github.com/NimbusKit/markdown): NSAttributedString Parser (can use with TTTAttributedLabel)
- [Markingbird](https://github.com/kristopherjohnson/Markingbird) - In swift

## Gestures

- [DBPathRecognizer](https://github.com/didierbrun/DBPathRecognizer): Recognizer with combinations of 8 strokes. Easily 'A', 'B', etc.
