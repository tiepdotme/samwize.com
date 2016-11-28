---
layout: post
title: "UISearchController Development Guide"
date: 2016-11-27T10:12:44+08:00
categories: [iOS]
---

[UISearchController](https://developer.apple.com/reference/uikit/uisearchcontroller) is a new controller in iOS 8 to handle search.

Prior to iOS 8, we have [UISearchDisplayController](https://developer.apple.com/reference/uikit/uisearchdisplaycontroller), which is deprecated. `UISearchController` more than replaced it, with a architectural change. 


## The Architecture

There are two View Controllers (VC) involved in search. Let's call them:
  
1. **Presenting VC** - where the search is triggered
2. **Results VC** - where the results are displayed in

You may also have the presenting VC display the results. In that case, they are the same VC.

`UISearchController` also provides a `UISearchBar` object, because all search requires the search bar for input.


## Triggering the search

There are 2 ways.

### 1. Add Search Bar

Add the `UISearchBar` to your presenting VC.

This must be done programatically, because there is no library object for `UISearchController` in Xcode. And you have to use it's `searchBar` object.

If you are using a `UITableView`, a search bar can be added to the header easily:

```swift
tableView.tableHeaderView = searchController.searchBar
```

### 2. Add Search Button 

Another way is to have a button to trigger the search, instead of adding the whole search bar in like in (1).

Create the `IBAction` for the button:

```swift
@IBAction func tapSearch(sender: AnyObject) {
    presentViewController(searchController, animated: true, completion: nil)
}
```

_Remember: `searchController` is a `UIViewController` (read in later section) so it can be called with `presentViewController`._

When the presenting VC has a navigation bar, you will need to configure search controller:

```swift
searchController.hidesNavigationBarDuringPresentation = false
```


## The Delegates

### Delegate #1 - UISearchResultsUpdating

[`UISearchResultsUpdating`](https://developer.apple.com/reference/uikit/uisearchresultsupdating) protocol has a callback when the user enters into the search bar.

Set `searchResultsUpdater`. Typically, the results VC will implement the protocol, so that it will update the results accordingly.

```swift
searchController.searchResultsUpdater = resultsViewController
```

Then in `resultsViewController`, implement the method:

```swift
func updateSearchResultsForSearchController(searchController: UISearchController) {
        let searchTerm = searchController.searchBar.text
        // Update your results
}
```


### Delegate #2 - UISearchBarDelegate

[`UISearchBarDelegate`](https://developer.apple.com/reference/uikit/uisearchbardelegate) protocol provides more events:

- text changed
- should text change
- should/begin/end editing
- tap on cancel button/etc

Set `searchBar.delegate`. Typically, the results VC.

```swift
searchController.searchBar.delegate = resultsViewController
```

You might be thinking we have `searchResultsUpdater`. Isn't that enough? Usually so, unless you want to know when buttons such as [scopes button](https://developer.apple.com/reference/uikit/uisearchbar/1624292-scopebuttontitles) are tapped on.


### Delegate #3 - UISearchControllerDelegate

[`UISearchControllerDelegate`](https://developer.apple.com/reference/uikit/uisearchcontrollerdelegate) protocol provides events when:

- the search controller is presented or dismissed 

Set `delegate` to the view controller that handles the calls, typically the presenting VC.

```swift
searchController.delegate = self
```


## UISearchController is a UIViewController

`UISearchController` inherits from `UIViewController`.

You can present it modally with `presentViewController`.

BUT, you should never push to navigation controller or use it as a child etc. If you want that, you can use [UISearchContainerViewController](https://developer.apple.com/reference/uikit/uisearchcontainerviewcontroller) to wrap it first.


## Display Results Instead of Dimming

The default behaviour dims the presenting VC when search is triggered.

User has to type 1 character, then the results VC will be shown.

It is common UX to display an intial set of results once search is triggered. Who knows, our smart filtering might already show up a good match?

Firstly, we prevent the dim with:

```swift
searchController.dimsBackgroundDuringPresentation = false
```

To show the results VC, a [little hack](http://stackoverflow.com/a/30814194/242682) is needed in the results VC:

```swift
class ResultsViewController: UIViewController {

  var context = 0

  override func viewDidLoad() {
      super.viewDidLoad()
      setupToPreventHiddenBehaviour()
  }

  func setupToPreventHiddenBehaviour() {
      view.addObserver(self, forKeyPath: "hidden", options: [ .New, .Old ], context: &context)
  }
  
  deinit {
      view.removeObserver(self, forKeyPath: "hidden")
  }
  
  override func observeValueForKeyPath(keyPath: String?, ofObject object: AnyObject?, change: [String : AnyObject]?, context: UnsafeMutablePointer<Void>) {
    guard context == &self.context else {
        super.observeValueForKeyPath(keyPath, ofObject: object, change: change, context: context)
        return
    }

    if change?[NSKeyValueChangeNewKey] as? Bool == true {
        view.hidden = false
    }
  }
    
}
```

It hacks around by observing for the view's `hidden` property, forcing it to never hide. Even when you clear the search bar, it gets back to this initial state.
