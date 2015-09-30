---
layout: post
title: "Long Standing Bug With Using UIScreenEdgePanGestureRecognizer Created with Xcode Interface Builder"
date: 2015-04-08 16:05:55 +0800
comments: true
categories: [iOS]
---

Encountered the [same bug](http://stackoverflow.com/q/26256985/242682) when creating a `UIScreenEdgePanGestureRecognizer` via Interface Builder.

It just didn't work as expected: the selector is never called.

Other gesture recognizers work. Except panning from edge.

<!-- more -->

We can only fallback to manually creating it

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    UIScreenEdgePanGestureRecognizer *leftEdgeGesture = [[UIScreenEdgePanGestureRecognizer alloc] initWithTarget:self action:@selector(handleLeftEdgeGesture:)];
    leftEdgeGesture.edges = UIRectEdgeLeft;
    leftEdgeGesture.delegate = self;
    [self.view addGestureRecognizer:leftEdgeGesture];
}

- (IBAction)handleLeftEdgeGesture:(UIScreenEdgePanGestureRecognizer *)gesture {
    NSLog(@"panned");
}

- (BOOL)gestureRecognizer:(UIGestureRecognizer *)gestureRecognizer shouldRecognizeSimultaneouslyWithGestureRecognizer:(UIGestureRecognizer *)otherGestureRecognizer {
    return YES;
}
```

Not impressed with a bug that is not fixed for over 6 months.
