---
layout: post
title: "Queue HTTP Operations with AFNetworking"
date: 2013-03-02 22:55
comments: true
categories: iOS
---

In a previous post, I wrote about a [simple usage of AFNetworking](http://samwize.com/2012/10/25/simple-get-post-afnetworking/), the de facto HTTP library for iOS.

This post, I will show how you can use AFNetworking to queue multiple HTTP operations. They could be running concurrently, or have dependencies. 

Adding dependencies to HTTP operations is especially useful. For example, you can make sure that you fetch a list of resources, then fetch image1, then image2, ...

<!-- more -->

Adding operations to a queue is rather simple. 

You will be using `NSOperationQueue`, which is part of Apple's [Foundation framework](http://developer.apple.com/library/ios/#documentation/cocoa/Reference/NSOperationQueue_class/Reference/Reference.html).

You will be adding `NSOperation` to the queue. Not surprisingly, classes such as `AFHTTPRequestOperation` subclass `NSOperation`. 

## Adding an operation to a queue ##

```objc
// Create a http operation
NSURL *url = [NSURL URLWithString:@"http://samwize.com/api/cars/"];
NSURLRequest *request = [NSURLRequest requestWithURL:url];
AFHTTPRequestOperation *operation = [[AFHTTPRequestOperation alloc] initWithRequest:request];
[httpClient registerHTTPOperationClass:[AFHTTPRequestOperation class]];
[operation setCompletionBlockWithSuccess:^(AFHTTPRequestOperation *operation, id responseObject) {
    // Print the response body in text
    NSLog(@"Response: %@", [[NSString alloc] initWithData:responseObject encoding:NSUTF8StringEncoding]);
} failure:^(AFHTTPRequestOperation *operation, NSError *error) {
    NSLog(@"Error: %@", error);
}];

// Add the operation to a queue
// It will start once added
NSOperationQueue *operationQueue = [[NSOperationQueue alloc] init];
[operationQueue addOperation:operation];
```

As you can see, it is just 2 more lines of code.


## Adding multiple operations and run them concurrently

```objc
NSOperationQueue *operationQueue = [[NSOperationQueue alloc] init];
// Set the max number of concurrent operations (threads)
[operationQueue setMaxConcurrentOperationCount:3];
[operationQueue addOperations:@[operation1, operation2, operation3] waitUntilFinished:NO];
```

## Adding Dependencies ##

Let's say we have 2 operations, and we want `operation2` to start only after `operation1` has finish.

```objc
NSOperationQueue *operationQueue = [[NSOperationQueue alloc] init];
// Make operation2 depend on operation1
[operation2 addDependency:operation1];
[operationQueue addOperations:@[operation1, operation2, operation3] waitUntilFinished:NO];

```