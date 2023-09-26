---
layout: post
title: "Hot and cold Observables in Swift Reactive Programming"
description: " "
date: 2023-09-24
tags: [reactiveprogramming]
comments: true
share: true
---

When it comes to reactive programming in Swift, understanding the concepts of hot and cold observables is crucial. These terms refer to different behaviors of observables and can have a significant impact on how your reactive code works.

## Cold Observables

A **cold observable** is an observable sequence that starts emitting events only when an observer subscribes to it. Each observer gets its own separate stream of events. This means that if multiple observers subscribe to the same cold observable, they will receive the events from the beginning of the sequence independently.

One common example of a cold observable is the `Observable` type from the RxSwift framework. When you create an instance of `Observable`, it starts with no events and begins emitting events only when a subscriber subscribes to it.

Here's an example of creating a cold observable using RxSwift:

```swift
let observable = Observable<Int>.create { observer in
    observer.onNext(1)
    observer.onNext(2)
    observer.onNext(3)
    observer.onCompleted()
    return Disposables.create()
}
```

In the above example, the observable starts emitting events only when a subscriber subscribes to it.

## Hot Observables

A **hot observable**, on the other hand, is an observable sequence that emits events regardless of whether there's an active observer or not. When a new observer subscribes to a hot observable, it starts receiving events from the current point in the sequence.

One popular example of a hot observable is the `PublishSubject` from the RxSwift framework. It acts as a stream of events that multiple subscribers can listen to simultaneously. Once the events are emitted, all subscribers will receive them from that point onwards.

Here's an example of creating a hot observable using RxSwift:

```swift
let subject = PublishSubject<String>()

subject.onNext("Hello")
subject.onNext("World")

let firstObserver = subject.subscribe(onNext: { value in
    print("First observer: \(value)")
})

subject.onNext("Goodbye")

let secondObserver = subject.subscribe(onNext: { value in
    print("Second observer: \(value)")
})

subject.onNext("Final message")
```

In the above example, both `firstObserver` and `secondObserver` will receive the events emitted by the `subject` starting from the point they subscribed. They will also receive any future events emitted by the `subject`.

#reactiveprogramming #swift