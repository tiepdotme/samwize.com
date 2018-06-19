---
layout: post
title: "Guide to RxSwift for iOS Development"
date: 2018-01-15T11:16:05+08:00
categories: [rx, iOS]
published: false
---

https://www.thedroidsonroids.com/blog/ios/rxswift-by-examples-2-observable-and-the-bind
Good examples

[RxSwift](https://github.com/ReactiveX/RxSwift), the gem from ReactiveX community (providing also familiar RxJava, RxEtc).

[RxSwift Community](https://github.com/rxswiftcommunity) for more open source power, such as [RxDataSources](https://github.com/RxSwiftCommunity/RxDataSources) and [RxSwiftExt](https://github.com/RxSwiftCommunity/RxSwiftExt).

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

But [Variable is deprecated](https://github.com/ReactiveX/RxSwift/issues/1501). See the replacement with Relay.

### Relay

Note: Relay, NOT Replay.

Relay is a special ObservableType that will never complete nor error, which means it only emits element.

Replay is [strangely](https://github.com/ReactiveX/RxSwift/issues/1502) a part of RxCocoa, not RxSwift.

- `PublishRelay` does not replay
- `BehaviorRelay` does replay the last event, and requires init with a value
- This is similar to their [subject counterpart](http://reactivex.io/documentation/subject.html)

### `weak` or `unowned` ?

??

### Functional?

RxSwift is in between imperative and functional. It declares the behaviour imperatively, then

### How to write good reactive code

Think hard where to perform side effects.

### Custom operator

Can't find the operator to your needs? Easily [extend](https://github.com/ReactiveX/RxSwift/blob/master/Documentation/GettingStarted.md#custom-operators) `ObservableType` with your own.

## Drive UITableView

### The Simple Case

RxSwift support binding a list of items (eg. `fooList`, an observation of Foo items).

```swift
fooList.bind(to: tableView.rx.items(cellIdentifier: "Cell")) { index, model, cell in
    cell.textLabel?.text = model
}
```

But that is only for simple cases where you don't need sections, nor animating changes to the rows.

### Advanced case using [RxDataSources](https://github.com/RxSwiftCommunity/RxDataSources)

RxDataSources provides more. But first you have to map your items to `SectionOfFoos`, a data structure you have to create for a section and the items it contains.

```swift
struct SectionOfFoos {
    var header: String
    var items: [Item]
}
extension SectionOfFoos: SectionModelType {
    typealias Item = Foo
    init(original: SectionOfFoos, items: [Item]) {
      self = original
      self.items = items
    }
}
```

Then you bind (map if needed) to the table view.

```swift
fooList
    .map { foo -> [SectionOfFoos] in
        // Create and return sections
    }
    .bind(to: tableView.rx.items(dataSource: dataSource))
    .disposed(by: disposeBag)
```

The `dataSource` is where the magic happens:

```swift
var dataSource: RxTableViewSectionedReloadDataSource<SectionOfFoos>!

dataSource = RxTableViewSectionedReloadDataSource<SectionOfFoos>(configureCell: { (ds, tv, ip, item) -> UITableViewCell in
  let cell = tv.dequeueReusableCell(withIdentifier: "Cell", for: ip)
  cell.textLabel?.text = item.xxx
  return cell
})

dataSource.titleForHeaderInSection = { ds, index in
  return ds.sectionModels[index].header
}
```

However, note that the section only supports a simple string, using the simple header view. For custom header view, you have to implement `UITableViewDelegate`'s `viewForHeaderInSection`. Why? Because custom view header/footer is part of UITableViewDelegate. Ask Apple. RxDataSources wants to deal with UITableViewDatasource ONLY. So they [did not support](https://github.com/RxSwiftCommunity/RxDataSources/issues/203) custom header/footer view.

### Row selected

- tableView.rx.modelSelected(Foo.self) -> the Foo instance selected
- tableView.rx.itemSelected -> the index path selected

If you want both the item and index path, you can `zip`.

## Materialize

[RxSwiftExt](https://github.com/RxSwiftCommunity/RxSwiftExt) provides `materialize` operator. It [transforms](https://github.com/RxSwiftCommunity/RxSwiftExt#errors-elements) Observable<T> into another Observable with 2 additional operators:

1. `elements` which produces Observable<T>
2. `errors` which produces Observable<Error>

What makes this special is that an error produced in the sequence will NOT terminate, but instead produced in (2) like an event.

[Adam Borek](http://adamborek.com/how-to-handle-errors-in-rxswift/) explained UI scenario where we never want the sequence to terminate. Using `Result` is another way but not swifty.

## Hot/Warm/Active vs Cold/Cool/Passive

There are 2 types of observable - Hot & Cold - but the terms can be confusing.

[Tailec](http://www.tailec.com/blog/understanding-publish-connect-refcount-share) prefers:

1. Active - produces events all the time, regardless of subscriptions
2. Passive - produces on request

A good explainable of Hot & Cold is looking at [where the producer is created](https://medium.com/@benlesh/hot-vs-cold-observables-f8094ed53339) - outside vs inside.

RxSwift says to think of hot & cold [as property](https://github.com/ReactiveX/RxSwift/blob/master/Documentation/HotAndColdObservables.md) to `Observable`. They are NOT 2 types of observables.

Examples of Hot: UITextField.rx.text

Examples of Cold: Observable.create()

## Share

1 of the surprise that newbie to the Rx world faced is that when you have eg. HTTP request as an observable, and when you subscribe TWICE, the requests will be made TWICE.

`share` will make the observable produce just ONCE, and sharing with multiple subscriptions.

You can [learn](https://blog.kaush.co/2015/01/21/rxjava-tip-for-the-day-share-publish-refcount-and-all-that-jazz/) [deeper](http://www.tailec.com/blog/understanding-publish-connect-refcount-share) on `publish().connect()/refcount()`. Also read the marbles on [connect](http://reactivex.io/documentation/operators/connect.html) and [refcount](http://reactivex.io/documentation/operators/refcount.html).

`share` is actualy a shorthand to `publish().refcount()`.

What `publish()` does is to turn a Observable into a _cold one_ (ConnectedObservable). Events are only emitted after `connect()` is called. This is helpful to make sure all subscribers are ready before calling `connect()` to start emitting events.

`refcount()` is a helper to do _reference counting_. It counts the number of subscribers, and will connect to the observable as long as there is subscribers.

---

## Resources

http://adamborek.com/rxswift-materials-list/ - List of readings, including his own [Thinking in RxSwfit](http://adamborek.com/thinking-rxswift/), with a practical example walking through the common operators such as flatMapLatest

https://medium.com/koolicar-engineering/rxswift-behaviorrelay-over-variable-182865ce10e0 - BehaviorRelay replace Variable

https://medium.com/mercari-engineering/signal-and-relay-in-rxcocoa-4-547fb0d18e11 - Introduction of Signal, BehaviorRelay, PublishRelay in RxCocoa 4
