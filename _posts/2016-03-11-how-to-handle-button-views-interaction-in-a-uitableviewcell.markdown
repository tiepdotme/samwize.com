---
layout: post
title: "How to Handle Button/views Interaction in a UITableViewCell"
date: 2016-03-11T20:47:15+08:00
categories: [iOS]
---

We all know a cell is tapped wehn `UITableViewDelegate` method - `tableView:didSelectRowAtIndexPath:` - is called.

But what if you have a `UIButton` in the cell, and the user tap on the button? Or it could be any `UIView` that can be tapped on in the cell.

In those cases, the buttons or views themselves will handle the touches, and `tableView:didSelectRowAtIndexPath:` will NOT be called.

## Interacting with a `UIButton`

To handle the interaction with a button, you can create the `IBAction` in your view controller as per normal (eg. drag the button action selector to your view controller).

The trick is to get the `superview` of the button, which will be the cell, and then using `tableView.indexPathForCell(cell)` to get the index path.

```swift
@IBAction func tapOnButton(sender: UIButton) {
    let cell = sender.superview as! UITableViewCell
    let indexPath = tableView.indexPathForCell(cell)
}
```

Note: This is assuming the button is contained directly in the cell. If the button is contained in another view, then the cell could be `sender.superview.superview`.


## Alternate solutions

This problem is very common, and the ["accepted answer"](http://stackoverflow.com/a/20655223/242682) in StackOverflow is one that set the tag of the button during `cellForRowAtIndexPath:`.

```swift
button.tag = indexPath.row
```

This "misuse" of a view tag means you may not use `viewWithTag:`, and secondly it could have bug when you delete/move cells (because `cellForRowAtIndexPath:` will not be called).

Our solution above is simpler, except that you must make sure you know where is `UITableViewCell` as you travel upwards via `superview`.

But the [best answer](http://stackoverflow.com/a/24720704/242682) is to create your custom `UITableViewCell`, then have your `UIButton` delegate the call to your view controller. This requires more code, and a custom class, but is the most correct.


## Interacting with any `UIView`

If you are tapping on any other `UIView`, you could handle similarly.

But in this case, you cannot add the `UIGestureRecognizer` to the view on storyboard.

You have to create the gesture with code.

```swift
func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {
    ...
    let tapGesture = UITapGestureRecognizer(target: viewController, action: "tapThis:")
    myView.addGestureRecognizer(tapGesture)
```

To access the view in the selector action `tapThis:`, use `sender.view`. Again, make use of `superview` to find the cell.
