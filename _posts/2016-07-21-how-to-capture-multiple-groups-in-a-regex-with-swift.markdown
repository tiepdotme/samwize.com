---
layout: post
title: "How to Capture Multiple Groups in a Regex With Swift"
date: 2016-07-21T15:07:17+08:00
categories: [iOS]
---

Swift does not provide regex support in the language (hopefully in Swift 4!?).  For now we still have to rely on [`NSRegularExpression`](https://developer.apple.com/library/mac/documentation/Foundation/Reference/NSRegularExpression_Class/).

Here is a `String` extension that extract the captured groups with a regex pattern. 

```swift
extension String {
    func capturedGroups(withRegex pattern: String) -> [String] {
        var results = [String]()

        var regex: NSRegularExpression
        do {
            regex = try NSRegularExpression(pattern: pattern, options: [])
        } catch {
            return results
        }
        
        let matches = regex.matchesInString(self, options: [], range: NSRange(location:0, length: self.characters.count))
        
        guard let match = matches.first else { return results }
        
        let lastRangeIndex = match.numberOfRanges - 1
        guard lastRangeIndex >= 1 else { return results }
        
        for i in 1...lastRangeIndex {
            let capturedGroupIndex = match.rangeAtIndex(i)
            let matchedString = (self as NSString).substringWithRange(capturedGroupIndex)
            results.append(matchedString)
        }
        
        return results
    }
}
```

To use:

```swift
// Will match "bcde"
"abcdefg".capturedGroups(withRegex: "a(.*)f")
```

## Capture Groups vs Matches

There is a difference.

The code `regex.matchesInString(...)` will return 0 or more **matches**. For example, "hello hello" will match the regex pattern "hello" twice.

We did not bother with multiple matches, and instead only handle for the first match.

[Capture groups](http://www.regular-expressions.info/brackets.html) are regex pattern which you define that you want to capture using brackets eg. `id=(\\d*)` will capture a number after a "id=".
