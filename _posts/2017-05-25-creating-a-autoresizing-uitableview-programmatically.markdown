---
layout: post
title: "Creating a Autoresizing UITableView Programmatically"
date: 2017-05-25T09:27:42+08:00
published: false
categories: [iOS]
---

_This is an incomplete guide because `estimatedRowHeight` SUCKS. Should NOT use it._

---

This is a common scenario we will use as an example: Creating a table view that display images of different sizes, loading the images from the network and resizing the table view cells automatically.

We will do this programmatically (no storyboard), in Swift, and also make use of 2 helpful libraries:

1. [Reusable](https://github.com/AliSoftware/Reusable) - Instead of messing with cell identifiers (_Strings!_) for `UITableViewCell`/etc, you can safely and conveniently use this mixin.
2. [SDWebImage](https://github.com/rs/SDWebImage) - Asynchrously downloads images with caching


## Setup

In `viewDidLoad`, setup the table view. 

```swift
private func setupTableView() {
    tableView.register(cellType: MyTableViewCell.self)
    tableView.rowHeight = UITableViewAutomaticDimension
    tableView.estimatedRowHeight = 100 // Just an estimated value for calculating scroll indicator
}
```

The cell class is `MyTableViewCell`, and behold the beauty of registering without a cell identifier string. This is brought to you by Reusable.

The topic of auto adjusting `UITableViewCell` height with autolayout is not new. I have previously written [the steps](/2016/05/10/auto-adjust-uitableviewcell-height/) to doing that. And in the setup, I have done that by setting the `rowHeight` and `estimatedRowHeight`.

## The Table View Cell

`MyTableViewCell` looks like this.

```swift
class MyTableViewCell: UITableViewCell, NibReusable {
  private var urlString: String?
  @IBOutlet weak var theImageView: UIImageView!
  @IBOutlet weak var theImageViewHeightConstraint: NSLayoutConstraint!
}
```

Remember we said we will be using `Reusable`? How it works is simply extending the cell with `NibReusable`. That's all.

Okay, there is a Xib that goes along with this. The cell is not fully programmatically created. Because it is easier to use autolayout in the xib. 

What is important is that there is a `theImageViewHeightConstraint`, which will will change when the image is downloaded. 

## The method to set the image

The cell provides a method to set the image URL string.

```swift
func setImage(withUrlString urlString: String, completion: @escaping () -> Void) {
    // Need to store the URL because cells will be reused. The check is in adjustBannerHeightToFitImage.
    self.urlString = urlString  
    
    // Flush first. Or placeholder if you have.
    bannerImageView.image = nil 
    
    guard let url = URL(string: bannerUrlString) else { return }
    
    // Loads the image asynchronously
    bannerImageView.sd_setImage(with: url) { [weak self] (image, error, cacheType, url) in
        self?.adjustHeightToFitImage(image: image, url: url, completion: completion)
    }
}
```    

We split the code up with `adjustHeightToFitImage`, which calculate the image aspect ratio, and adjust the height (while occupying full/fixed width in the cell).

```swift
private func adjustHeightToFitImage(image: UIImage?, url: URL?, completion: @escaping () -> Void) {
    guard let bannerUrlString = bannerUrlString, bannerUrlString == url?.absoluteString else { return }
    guard let image = image else { return }
    
    let aspectRatio = image.size.width / image.size.height
    let bannerHeightToFit = bannerImageView.bounds.size.width / aspectRatio
    
    if bannerImageViewHeightConstraint.constant != bannerHeightToFit {
        bannerImageViewHeightConstraint.constant = bannerHeightToFit
        completion()
    }
}
```

The `completion` is necessary for the table view to know that the image is downloaded, and the height is adjusted.

## Configuring the cell 

We will omit the unnecessary code in the table view. What is most important is in the cell configuration.

```swift
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    // Dequeue without cell identifer!
    let cell: MyTableViewCell = tableView.dequeueReusableCell(for: indexPath)
    
    cell.setImage(withUrlString: imageUrlString, completion: { [weak self] in
        // A trick. The begin/end calls will reload just the height.
        self?.tableView.beginUpdates()
        self?.tableView.endUpdates()
    })
    
    return cell
}
```

In the `completion` handler, we did a trick to reload just the heights for the table view. This is more efficient than `reloadData` or `reloadRows(at:with:)`.

## The Problem with `estimatedRowHeight`

There is a [major bug](http://www.openradar.me/20829131) with using `estimatedRowHeight` (a feature of iOS 7, yet still not fixed after 2 years).

It does not `scrollToRow` correctly.

Hence, if you want to use `scrollToRow`, then go use the archaic approach since the beginning of iOS - implement `heightForRowAt`.

## Bonus: Table Header/Footer

Read [this post](/2015/11/06/guide-to-customizing-uitableview-section-header-footer/) on adding header/footer. 

With Resuable, register the view.

```
tableView.register(headerFooterViewType: GroupHeaderView.self)
```