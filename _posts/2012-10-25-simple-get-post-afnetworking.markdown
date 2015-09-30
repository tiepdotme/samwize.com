---
layout: post
title: "Simple GET/POST AFNetworking"
date: 2012-10-25 00:32
comments: true
categories: iOS
---

***UPDATE**: For AFNetworking 2.0, refer to [this tutorial](/2014/05/10/tutorial-on-using-afnetworking-2-dot-0/) instead.*

[AFNetworking](https://github.com/AFNetworking/AFNetworking) is the choice for iOS/Mac developers when it comes to choosing a HTTP library.

[ASIHTTPRequest](http://allseeing-i.com/ASIHTTPRequest/) used to be the choice, until 2011 when it became inactive.

I am one of the many who is forced to switch camp.

<!-- more -->

In many ways, it seems AFNetworking would be better. It uses blocks!

However, I find the documentation lacking. It has an [overview](https://github.com/AFNetworking/AFNetworking), [getting started](https://github.com/AFNetworking/AFNetworking/wiki/Getting-Started-with-AFNetworking), [introduction](https://github.com/AFNetworking/AFNetworking/wiki/Introduction-to-AFNetworking), [complete reference](http://afnetworking.github.com/AFNetworking/), ... But yet, it didn't provide example on how you make a simple HTTP GET or POST.

Here is how you do it:

## GET ##

```objc
    AFHTTPClient *httpClient = [[AFHTTPClient alloc] initWithBaseURL:[NSURL URLWithString:@"http://samwize.com/"]];
    NSMutableURLRequest *request = [httpClient requestWithMethod:@"GET"
                                                            path:@"http://samwize.com/api/pigs/"
                                                      parameters:nil];
    AFHTTPRequestOperation *operation = [[AFHTTPRequestOperation alloc] initWithRequest:request];
    [httpClient registerHTTPOperationClass:[AFHTTPRequestOperation class]];
    [operation setCompletionBlockWithSuccess:^(AFHTTPRequestOperation *operation, id responseObject) {
        // Print the response body in text
        NSLog(@"Response: %@", [[NSString alloc] initWithData:responseObject encoding:NSUTF8StringEncoding]);
    } failure:^(AFHTTPRequestOperation *operation, NSError *error) {
        NSLog(@"Error: %@", error);
    }];
    [operation start];
```

## POST ##

POST a urlencoded form `name=piggy` in the http body.

```objc
    AFHTTPClient *httpClient = [[AFHTTPClient alloc] initWithBaseURL:[NSURL URLWithString:@"http://samwize.com/"]];
    [httpClient setParameterEncoding:AFFormURLParameterEncoding];
    NSMutableURLRequest *request = [httpClient requestWithMethod:@"POST"
                                                            path:@"http://samwize.com/api/pig/"
                                                      parameters:@{@"name":@"piggy"}];

    // Similar to GET code ...
```

If you want to POST a json such as `{"name":"piggy"}`, you change the encoding:

```objc
    [httpClient setParameterEncoding:AFJSONParameterEncoding];
```

If you want to do a multi-part POST of an image, you do this:

```objc
NSMutableURLRequest *request = [httpClient multipartFormRequestWithMethod:@"POST" path:@"http://samwize.com/api/pig/photo" parameters:nil constructingBodyWithBlock: ^(id <AFMultipartFormData>formData) {
    [formData appendPartWithFileData:imageData name:@"avatar" fileName:@"avatar.jpg" mimeType:@"image/jpeg"];
}];
```

This simple guide has been helped by [this](http://stackoverflow.com/questions/9927945/afnetworking-post-to-rest-webservice), [this](http://stackoverflow.com/questions/9275333/afnetworking-post-request-with-application-x-www-form-urlencoded) and [this](http://stackoverflow.com/questions/8468065/is-there-an-example-of-afhttpclient-posting-json-with-afnetworking).


## Pitfalls ##

Do not use `[[AFHTTPClient alloc] init]`, as that does not initialize a lot of stuff. Use `initWithBaseURL` instead.

For instance, if in the above example (POST JSON) you had used init, the Content-Type will be

    application/json; charset=(null)

Charset should be `utf-8`, not `null`.


