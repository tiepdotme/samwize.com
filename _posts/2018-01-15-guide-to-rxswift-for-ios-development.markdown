---
layout: post
title: "Guide to RxSwift for iOS Development"
date: 2018-01-15T11:16:05+08:00
categories: [rx, iOS]
---

[RxSwift](https://github.com/ReactiveX/RxSwift), the gem from ReactiveX community (providing also familiar RxJava, RxEtc).

[RxSwift Community](https://github.com/rxswiftcommunity) for more open source power.

## Why RxSwift?

RxSwift provides a new way of programming known as **Reactive Programming**. In short, that means providing better way to deal with asynchronous/event-based code.

iOS has many different ways of dealing with asynchronous code -- delegate pattern, closures, Notification, KVO, GCD.. RxSwift could unify all of them in a consistent way.

Other pros include binding, composable streams, throttle, retry, etc..

## 1. Observable

`Observable` is the foundation of Rx (aka sequence, stream).

`Observable<T>` is a type that produces a sequence of **events** with immutable data of type `T`.

There are 3 types of events: `next` with the next data, `completed` and `error`.

## 2. Operators

`Observable`'s methods are called [operators](http://reactivex.io/documentation/operators.html). 

There are hundreds of operators, but probably just a handful that you will be using. It is like a toolbox with hundreds of tool, some you use more than others.

[RxJS marbles](http://rxmarbles.com) has interative diagrams that easily show you what each operation does. I highlight the few most important/popular ones, in the [categories](http://reactivex.io/documentation/observable.html):

- Creating
  - `create`
  - `just` - emit 1 value and complete
- Filtering
  - `filter`
  - `elementAt`
  - `skip`
  - `take`
  - `distinctUntilChanged`
- Transformation
- Mathematical Transformation
- Combining
- Conditional 
- Utility
  - `share` - does not emit old events
  - `shareReplay` - keeps a buffer of last items and replay them

### Subscribe and Dispose

You subscribe to event by calling `subscribe` with a observable. The signature:

`public func subscribe(_ on: @escaping (RxSwift.Event<Self.E>) -> Swift.Void) -> Disposable`

Notice that it returns a `Disposable`. While the closure is where you handle the events.

You can call `dispose` with a disposable when you are done with the subscription. Because this is a common operation, and you will likely have multiple subscriptions, you will often call `disposed(by:)` with a DisposeBag. A DisposeBag will manage and call `dispose` for each subscription when deallocated. Using of DisposeBag is merely a common pattern.

So, a simple common pattern is to subscribe and dispose.

```swift
observable
  .subscribe {
    ...
  }
  .dispose(by: disposeBag)
```

### `debug`

During development and learning, you will want to use the `debug` operator, which will print every event.

### `flatMap`

Does 2 operations:

1. For each emitted item, apply a function and returns an `Observable` (instead of transform to an item like in `map`)
2. Merge the `Observable`s 

Note that the merge could overlap. Use `concatMap` to have strict ordering. `flatMap` is aka `mergeMap`.

### `subscribe` vs `do`

```swift
public func subscribe(onNext: ((Self.E) -> Swift.Void)? = default, onError: ((Error) -> Swift.Void)? = default, onCompleted: (() -> Swift.Void)? = default, onDisposed: (() -> Swift.Void)? = default) -> Disposable

public func `do`(onNext: ((Self.E) throws -> Swift.Void)? = default, onError: ((Error) throws -> Swift.Void)? = default, onCompleted: (() throws -> Swift.Void)? = default, onSubscribe: (() -> ())? = default, onSubscribed: (() -> ())? = default, onDispose: (() -> ())? = default) -> RxSwift.Observable<Self.E>
```

`do` invokes a side-effect, and then returns the original observable.

## 3. RxCocoa

`RxSwift` is like Foundation framework, while `RxCocoa` is like UIKit framework.

## 4. Same stuff, new ways

**Key-Value Observing (KVO)** can be done in a [reactive way](https://github.com/ReactiveX/RxSwift/blob/master/Documentation/GettingStarted.md#kvo).

```swift
view
  .rx.observe(CGRect.self, "frame")
  .subscribe(
    ...
  )
```

There is also a `rx.observeWeakly`.

## 5. Error Handling

You can't bind failure to UIKit controls because that is undefined behavior.

To ensure your `Observable` won't fail, you can use `catchErrorJustReturn(valueThatIsReturnedWhenErrorHappens)`, which is an operator to transform your error to a value!


## 6. MVVM & in Practice

Firstly, Rx can be used with _any architecture_, be it MVC or MVP. 

But we will highlight for MVVM here, because MVVM plays nice with Rx. In esscense, View Model expose `Observable`, which can can bind to the views in View Controller.


## More

### Scheduler

We didn't mention scheduler, but it is also a foundation block of Rx, along with `Observable`. It is like GDC.

### Subject

Both an Observable and Observer.

1. `PublishSubject` - observers get only new events
2. `BehaviorSubject` - observers get the last event (buffer of 1) and new events
3. `ReplaySubject` - has a buffer to replay 
4. `Variable` - has a `current`, never emit errors, but will emit completed

But [Variable is deprecated](https://github.com/ReactiveX/RxSwift/issues/1501).

### `weak` or `unowned` ?

??

### Functional? 

RxSwift is in between imperative and functional. It declares the behaviour imperatively, then 

### How to write good reactive code

Think hard where to perform side effects.

### Custom operator

Can't find the operator to your needs? Easily [extend](https://github.com/ReactiveX/RxSwift/blob/master/Documentation/GettingStarted.md#custom-operators) `ObservableType` with your own.