---
layout: post
title: "SenTestingKit does not support wait for blocks"
date: 2012-10-03 00:21
comments: true
categories: iOS, Testing
---

I was using [SenTestingKit](http://developer.apple.com/library/mac/#documentation/developertools/Conceptual/UnitTesting/00-About_Unit_Testing/about.html), the default unit testing framework from Apple, when I found out that it does not support tests that involve asynchronous methods, or blocks.

That's a waste. 

<!-- more -->

Though there is [a workaround](https://gist.github.com/2254570) using semaphore.

``` objc
- (void)testBlockMethod {
    dispatch_semaphore_t semaphore = dispatch_semaphore_create(0);    
    
    // Your block method eg. AFNetworking
    NSURL *url = [NSURL URLWithString:@"http://httpbin.org/ip"];
    NSURLRequest *request = [NSURLRequest requestWithURL:url];
    AFJSONRequestOperation *operation = [AFJSONRequestOperation JSONRequestOperationWithRequest:request success:^(NSURLRequest *request, NSHTTPURLResponse *response, id JSON) {
        NSLog(@"IP Address: %@", [JSON valueForKeyPath:@"origin"]);
        STAssertNotNil(JSON, @"JSON not loaded");
        // Signal that block has completed
        dispatch_semaphore_signal(semaphore);        
    } failure:nil];
    [operation start];
    
    // Run loop
    while (dispatch_semaphore_wait(semaphore, DISPATCH_TIME_NOW))
        [[NSRunLoop currentRunLoop] runMode:NSDefaultRunLoopMode
                                 beforeDate:[NSDate dateWithTimeIntervalSinceNow:10]];
    dispatch_release(semaphore);    
}
```

But if you mind, there are lots of [other testing frameworks](http://stackoverflow.com/questions/4114083/ios-tests-specs-tdd-bdd-and-integration-acceptance-testing), eg [GHUnit](https://github.com/gabriel/gh-unit) supports asynchronous testing.