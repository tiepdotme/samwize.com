---
layout: post
title: "Guide to Moya for API Services and Testing"
date: 2018-05-23T07:10:15+08:00
categories: [API, Testing]
---

Moya is a very high-level library, enforcing you to create your API services with a strict set of protocol methods. Doing so, you will automatically design API with best practices.

If Moya is high-level, then Alamofire is mid-level, and URLSession is low-level.

You can perfectly live without Moya, and use Alamofire directly.

## What Moya does

In essence, Moya is creating this [pipeline](https://github.com/Moya/Moya/tree/master/docs):

![](/images/moya-pipeline.jpg)

Target is a collection of your API endpoints/service, enforced by implementing `TargetType` protocol.

`Endpoint` is a semi-internal structure in Moya that you may or may not deal with.

`Request` is a Alamofire's type, which you have to form when making network calls with Alamofire.

Moya simplify your workflow such that make a request from your target's provider.

```swift
let provider = MoyaProvider<MyService>()
provider.request(.allPopularMovies) { result in
    // Similar to handling Alamofire result
}
```

Read on to see how `MyService` (target) is designed.

## Designing Target

A target is an enum, with the API as cases.

Then implement the protocol `TargetType`, and you will have to [conform to 7 methods](https://github.com/Moya/Moya/blob/master/docs/Targets.md). The example shows for only `path`, which are the endpoints for each case.

```swift
enum MyService: TargetType {

  case allPopularMovies
  case movie(String)

  public var path: String {
    switch self {
    case .allPopularMovies: return "/movies/popular"
    case .movie(let movieId): return "/movies/\(movieId)"
    }
  }

}
```

I will not go into details for the other 6 methods, as they are easy to understand from the method names. You can also refer to Moya's [basic example](https://github.com/Moya/Moya/blob/master/docs/Examples/Basic.md) and [more](https://github.com/Moya/Moya/tree/master/docs/Examples).

## Testing/Stubbing

Instead, I will discuss on unit testing with Moya.

Too much stubbing is bad in testing, but for server-client API, stubbing is good because it gives:

1. consistent response independent from server
2. immediate response

Moya is built with [testing](https://github.com/Moya/Moya/blob/master/docs/Testing.md) in mind. But I didn't like that `sampleData` is a required method in `TargetType` protocol.

**Because if you stub there, then your production app will contain the stubs.**

If stub with JSON files (aka test fixtures), then your app will have to include those files..

My solution is to stub only when creating a mock provider in my **unit tests**. Therefore in my Target, simply return nothing for `sampleData`.

```swift
var sampleData: Data {
    return Data()
}
```

Then in unit test target, set up like this:

```swift
class APITests: XCTestCase {

    var provider: MoyaProvider<MyService>!

    override func setUp() {
        super.setUp()

        // A mock provider with a mocking `endpointClosure` that stub immediately
        provider = MoyaProvider<MyService>(endpointClosure: customEndpointClosure, stubClosure: MoyaProvider.immediatelyStub)
    }

    func customEndpointClosure(_ target: MyService) -> Endpoint {
        return Endpoint(url: URL(target: target).absoluteString,
                        sampleResponseClosure: { .networkResponse(200, target.testSampleData) },
                        method: target.method,
                        task: target.task,
                        httpHeaderFields: target.headers)
    }

}
```

The gist is that I create a mock provider using a custom `endpointClousure`, which in the `sampleResponseClosure`, I use my own `testSampleData`, which is implemented with an extension in the unit test.

```swift
extension MyService {
    var testSampleData: Data {
        switch self {
        case .allPopularMovies:
            // Returning all-popular-movies.json
            let url = Bundle(for: APITests.self).url(forResource: "all-popular-movies", withExtension: "json")!
            return try! Data(contentsOf: url)
        }
    }
}
```

Once again:

- `sampleData` - `TargetType` protocol method which we don't utilize
- `testSampleData` - custom extension in unit test that returns a test fixture JSON
