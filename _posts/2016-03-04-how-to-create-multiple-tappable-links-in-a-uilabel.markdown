---
layout: post
title: "How to Create Multiple Tappable Links in a UILabel"
date: 2016-03-04T14:41:56+08:00
categories: [iOS]
---

A good example is a `UILabel` that looks like the following below the Facebook button.

{%img center /images/tapped-links-in-uilabel.png %}

There are 2 tasks at hand - first using `NSAttributedString` to create the underlined texts, then detecting if the underlined text is tapped on.

## Underline part of a text

Using `NSAttributedString`, we can style the text.

```swift
myLabel.text = "By signing up you agree to our Terms & Conditions and Privacy Policy"
let text = (myLabel.text)!
let underlineAttriString = NSMutableAttributedString(string: text)
let range1 = (text as NSString).rangeOfString("Terms & Conditions")
underlineAttriString.addAttribute(NSUnderlineStyleAttributeName, value: NSUnderlineStyle.StyleSingle.rawValue, range: range1)
let range2 = (text as NSString).rangeOfString("Privacy Policy")
underlineAttriString.addAttribute(NSUnderlineStyleAttributeName, value: NSUnderlineStyle.StyleSingle.rawValue, range: range2)
myLabel.attributedText = underlineAttriString
```

## Extending UITapGestureRecognizer

We extend `UITapGestureRecognizer` to provide a convenient function to detect if a certain range (`NSRange`) is tapped on in a `UILabel`.

```swift
extension UITapGestureRecognizer {

    func didTapAttributedTextInLabel(label: UILabel, inRange targetRange: NSRange) -> Bool {
        // Create instances of NSLayoutManager, NSTextContainer and NSTextStorage
        let layoutManager = NSLayoutManager()
        let textContainer = NSTextContainer(size: CGSize.zero)
        let textStorage = NSTextStorage(attributedString: label.attributedText!)

        // Configure layoutManager and textStorage
        layoutManager.addTextContainer(textContainer)
        textStorage.addLayoutManager(layoutManager)

        // Configure textContainer
        textContainer.lineFragmentPadding = 0.0
        textContainer.lineBreakMode = label.lineBreakMode
        textContainer.maximumNumberOfLines = label.numberOfLines
        let labelSize = label.bounds.size
        textContainer.size = labelSize

        // Find the tapped character location and compare it to the specified range
        let locationOfTouchInLabel = self.locationInView(label)
        let textBoundingBox = layoutManager.usedRectForTextContainer(textContainer)
        let textContainerOffset = CGPointMake((labelSize.width - textBoundingBox.size.width) * 0.5 - textBoundingBox.origin.x,
            (labelSize.height - textBoundingBox.size.height) * 0.5 - textBoundingBox.origin.y);
        let locationOfTouchInTextContainer = CGPointMake(locationOfTouchInLabel.x - textContainerOffset.x,
            locationOfTouchInLabel.y - textContainerOffset.y);
        let indexOfCharacter = layoutManager.characterIndexForPoint(locationOfTouchInTextContainer, inTextContainer: textContainer, fractionOfDistanceBetweenInsertionPoints: nil)

        return NSLocationInRange(indexOfCharacter, targetRange)
    }

}
```

Note: `textContainer` configuration should match that of `UILabel` eg. `lineBreakMode`


## On Tap Event

Have your `UITapGestureRecognizer` send action to `tapLabel:`, and detect using the extension method `didTapAttributedTextInLabel:inRange:`.

```swift
@IBAction func tapLabel(gesture: UITapGestureRecognizer) {
    let text = (myLabel.text)!
    let termsRange = (text as NSString).rangeOfString("Terms & Conditions")
    let privacyRange = (text as NSString).rangeOfString("Privacy Policy")

    if gesture.didTapAttributedTextInLabel(myLabel, inRange: termsRange) {
        print("Tapped terms")
    } else if gesture.didTapAttributedTextInLabel(myLabel, inRange: privacyRange) {
        print("Tapped privacy")
    } else {
        print("Tapped none")
    }
}
```
