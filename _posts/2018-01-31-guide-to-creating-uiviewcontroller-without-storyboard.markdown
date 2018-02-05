---
layout: post
title: "Guide to Creating UIViewController Without Storyboard"
date: 2018-01-31T16:43:53+08:00
categories: []
---

This is a guide on creating your custom `UIViewController` with code, without any storyboard/nibs/xibs. You may also be interested in reading [guide to creating custom `UIView`](/2017/11/01/guide-to-creating-custom-uiview/).

## Initializer

The view controller's initializer can be bare minimal, but it must use the [designated initializer `init(nibName:bundle:)`](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621359-init).

```swift
init() {
    super.init(nibName: nil, bundle: nil)
}
```

As silly as it sound, you will find the documentation stating:

> If you subclass UIViewController, you must call the super implementation of this method, even if you aren't using a NIB... and specify nil for both ...

Yup, that's proof that Apple prefers storyboard, but we know what's good for ourselves.

If you use MVVM, or you require any dependency injection to the view controller, then it will look like this:

```swift
init(viewModel: MyViewModel, someDependency: SomeDependency, ...) {
    super.init(nibName: nil, bundle: nil)
    self.viewModel = viewModel
    self.someDependency = someDependency
}
```

You may setup lightweight initialization in `init`, but you should never setup your views (read on where to do that later). 

## No Need for `init(coder:)`

`init(coder:)` is called only when you create your views from storyboard. It will never be invoked since we are going with no-storyboard so we can safely fatal out.

```swift
required init?(coder aDecoder: NSCoder) {
    fatalError("Never will happen")
}
```

## Create your view in `viewDidLoad`

Take a look at the [life cycle diagram](https://rdkw.wordpress.com/2013/02/24/ios-uiviewcontroller-lifecycle/).

Importantly, the diagram is saying that _view will be unloaded/deallocated when memory is low_.

If your view is then asked to appear again, it needs to re-loaded. 

`viewDidLoad` is where you should create your view, or more specifically, create all your subviews in `self.view`.

I break the creation of views into 2 stages, in 2 `private func`s.

```swift
override func viewDidLoad() {
    super.viewDidLoad()
    setupViews()
    bindViews()
}
```

### 1. `setupViews()`

Add each subview with `view.addSubview(someSubview)`, then setup the layout constraints (we use [Cartography](https://github.com/robb/Cartography), a autolayout helper).

```swift
private func setupViews() {
    view.addSubview(subview1)
    constrain(view, subview1) { view, subview1 in
        subview1.edges == view.edges
    }
    
    // Similarly for subview2, subview3, ...
}
```

`subview1` is being initialized via lazy loading.

```swift
private lazy var subview1: UIView = {
    let view = UIView()
    view.backgroundColor = .red
    return view
}()
```

This is equivalent to how you configure a view in Storyboard, but in code, and within the lazy load code block. It is the initial configuration. Afterwhich, you may of course programmatically change any of the properties.

### 2. `bindViews()`

This 2nd stage is to bind the views with the model. It sets the actual content of the views.

In a very simple example, we set the transparency level `subview1.alpha` with a view model.

```swift
private func bindViews() {
    subview1.alpha = viewModel.alpha
}
```

Note that we are "binding" without using any frameworks (such as RxSwift), so this binding is one-time only. If subsequently `viewModel.alpha` is changed, the function `bindViews()` must be called again to update the view.

## The Reactive Way

Contrast this with using [RxSwift](https://github.com/ReactiveX/RxSwift):

```swift
private func bindViews() {
    viewModel.alpha
        .bind(to: subview1.rx.alpha)
        .disposed(by: disposeBag)
}
```

Using RxSwift, `viewModel.alpha` is an observable, and whenever it observes a new value of alpha, the binding `subview1` will be updated automatically.
