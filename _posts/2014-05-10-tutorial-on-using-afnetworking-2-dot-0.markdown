---
layout: post
title: "Tutorial on using AFNetworking 2.0"
date: 2014-05-10 09:42:16 +0800
comments: true
categories: [iOS]
---

This is a follow-up post to the [tutorial on using AFNetworking 1.0](/2012/10/25/simple-get-post-afnetworking/).

With the upgrade of [AFNetworking](https://github.com/AFNetworking/AFNetworking) to 2.0, the code for 1.0 becomes obsolete. It is not backward compatible at all. So here is the new guide.

<!-- more -->

## GET

```objc
    AFHTTPRequestOperationManager *manager = [AFHTTPRequestOperationManager manager];
    [manager GET:@"http://samwize.com/api/poos/"
      parameters:nil
         success:^(AFHTTPRequestOperation *operation, id responseObject) {
        NSLog(@"JSON: %@", responseObject);
    } failure:^(AFHTTPRequestOperation *operation, NSError *error) {
        NSLog(@"Error: %@", error);
    }];
```


## POST

```objc
    AFHTTPRequestOperationManager *manager = [AFHTTPRequestOperationManager manager];
    [manager POST:@"http://samwize.com/api/poo/"
       parameters:@{@"color": @"green"}
          success:^(AFHTTPRequestOperation *operation, id responseObject) {
        NSLog(@"JSON: %@", responseObject);
    } failure:^(AFHTTPRequestOperation *operation, NSError *error) {
        NSLog(@"Error: %@", error);
    }];
```


## POST Multi-Part

Similar to POST, but with data (in this case an image) in the body in multi-parts.

```objc
    AFHTTPRequestOperationManager *manager = [AFHTTPRequestOperationManager manager];
    NSURL *filePath = [NSURL fileURLWithPath:@"file://path/to/image.png"];
    [manager POST:@"http://samwize.com/api/poo/"
       parameters:@{@"color": @"green"}
       constructingBodyWithBlock:^(id<AFMultipartFormData> formData) {
        [formData appendPartWithFileURL:filePath name:@"image" error:nil];
    } success:^(AFHTTPRequestOperation *operation, id responseObject) {
        NSLog(@"Success: %@", responseObject);
    } failure:^(AFHTTPRequestOperation *operation, NSError *error) {
        NSLog(@"Error: %@", error);
    }];
```


## Batch of Operations

In the above, `AFHTTPRequestOperationManager` is used, and that is usually good enough.

But if you requires batch operations, then you have to use `AFHTTPRequestSerializer`, `AFHTTPRequestOperation` and `NSOperationQueue`.

The [sample code](https://github.com/AFNetworking/AFNetworking#afhttprequestoperation) below is directly from AFNetworking:

```objc
    NSMutableArray *mutableOperations = [NSMutableArray array];
    for (NSURL *fileURL in filesToUpload) {
        NSURLRequest *request = [[AFHTTPRequestSerializer serializer] multipartFormRequestWithMethod:@"POST" URLString:@"http://example.com/upload" parameters:nil constructingBodyWithBlock:^(id<AFMultipartFormData> formData) {
            [formData appendPartWithFileURL:fileURL name:@"images[]" error:nil];
        }];

        AFHTTPRequestOperation *operation = [[AFHTTPRequestOperation alloc] initWithRequest:request];

        [mutableOperations addObject:operation];
    }

    NSArray *operations = [AFURLConnectionOperation batchOfRequestOperations:@[...] progressBlock:^(NSUInteger numberOfFinishedOperations, NSUInteger totalNumberOfOperations) {
        NSLog(@"%lu of %lu complete", numberOfFinishedOperations, totalNumberOfOperations);
    } completionBlock:^(NSArray *operations) {
        NSLog(@"All operations in batch complete");
    }];
    [[NSOperationQueue mainQueue] addOperations:operations waitUntilFinished:NO];
```

---

## More Tips

The above examples use the default `AFJSONResponseSerializer` - which means it expect the response to be a valid JSON.

If you are handling XML or data, then you want to use a [different serializer](https://github.com/AFNetworking/AFNetworking#serialization).

```objc
    // To handle XML
    AFHTTPRequestOperationManager *manager = [AFHTTPRequestOperationManager manager];
    manager.responseSerializer = [AFXMLParserResponseSerializer new];
    // Or data
    manager.responseSerializer = [AFHTTPResponseSerializer new];
```

AFNetworking is strict with the `Content-Type`.

So if your response is a "text/plain" when you are using a JSON serializer, you can do this:

```objc
    // Tip by dStudios
    manager.responseSerializer.acceptableContentTypes = [NSSet setWithObject:@"text/plain"];
```
