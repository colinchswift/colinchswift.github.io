---
layout: post
title: "Creating custom Observables in Swift Reactive Programming"
description: " "
date: 2023-09-24
tags: [SwiftReactiveProgramming, CustomObservables]
comments: true
share: true
---

Reactive programming has gained popularity in recent years due to its ability to handle asynchronous events in a more structured and declarative way. Swift, being a powerful and modern programming language, provides its own native implementation of reactive programming with the **Combine** framework. In this blog post, we will explore the concept of custom observables and how to create them in Swift using Combine.

## Understanding Observables

Observables are a fundamental concept in reactive programming. They represent a stream of events that can be observed and react to in some way. This stream of events can be anything from user interactions, network requests, or even simple timer events.

In Swift Combine, observables are represented by the `Publisher` protocol. A publisher emits values over time and can be subscribed to by subscribers. Subscribers can then receive and react to the emitted values.

## Creating Custom Observables

To create a custom observable in Swift Combine, we need to implement a type that conforms to the `Publisher` protocol. Let's take a look at an example of creating a custom observable that emits a sequence of numbers:

```swift
import Combine

struct NumberSequencePublisher: Publisher {
    typealias Output = Int
    typealias Failure = Never
    
    private let start: Int
    private let end: Int
    
    init(start: Int, end: Int) {
        self.start = start
        self.end = end
    }
    
    func receive<S>(subscriber: S) where S : Subscriber, Self.Failure == S.Failure, Self.Output == S.Input {
        let subscription = NumberSequenceSubscription(start: start, end: end, subscriber: subscriber)
        subscriber.receive(subscription: subscription)
    }
}

private final class NumberSequenceSubscription<S: Subscriber>: Subscription where S.Input == Int {
    private let start: Int
    private let end: Int
    private var subscriber: S?
    
    init(start: Int, end: Int, subscriber: S) {
        self.start = start
        self.end = end
        self.subscriber = subscriber
    }
    
    func request(_ demand: Subscribers.Demand) {
        guard let subscriber = subscriber else {
            return
        }
        
        for i in start...end {
            let demand = subscriber.receive(i)
            if demand == .none {
                break
            }
        }
        
        subscriber.receive(completion: .finished)
    }
    
    func cancel() {
        subscriber = nil
    }
}
```

In the above example, we have created a custom publisher `NumberSequencePublisher` that emits a sequence of numbers from a start to end value. The `Output` associated type is set to `Int`, indicating that our publisher will emit integer values. The `Failure` associated type is set to `Never`, indicating that the publisher will never fail.

We then implement the `receive(subscriber:)` method, which creates a subscription and passes it to the subscriber. In our case, the subscriber receives the emitted values and completes the stream by calling `receive(completion: .finished)`.

The `NumberSequenceSubscription` class implements the `Subscription` protocol, which represents the connection between the publisher and subscriber. Inside the `request(_ demand:)` method, we use a for loop to emit each number to the subscriber, respecting the demand requested. If the requested demand is `.none`, we break the loop, indicating that the subscriber no longer wants to receive values.

## Subscribing to Custom Observables

Once we have created our custom observable, we can subscribe to it and handle the emitted values. Let's see an example of how to subscribe to our `NumberSequencePublisher`:

```swift
let publisher = NumberSequencePublisher(start: 1, end: 5)
let subscription = publisher.sink { value in
    print("Received value: \(value)")
}

// Output:
// Received value: 1
// Received value: 2
// Received value: 3
// Received value: 4
// Received value: 5
```

In the above example, we create an instance of `NumberSequencePublisher` that emits values from 1 to 5. We then use the `sink` operator to subscribe to the publisher and handle the emitted values. In this case, we simply print the received value, but you can perform any desired action with the value.

## Conclusion

Creating custom observables in Swift Combine allows us to have full control over the streams of events in our reactive programming applications. By conforming to the `Publisher` protocol and implementing our own subscription logic, we can create observables that emit values according to our specific requirements. This gives us the flexibility to handle various types of events in a reactive and declarative way.

#SwiftReactiveProgramming #CustomObservables