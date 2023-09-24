---
layout: post
title: "Subscribing to Observables in Swift Reactive Programming"
description: " "
date: 2023-09-24
tags: [reactiveprogramming, swiftprogramming]
comments: true
share: true
---

Reactive programming has gained popularity in the Swift community due to its ability to simplify asynchronous and event-driven code. One of the core concepts in reactive programming is the Observable - a stream of values that can be observed and reacted to.

In this blog post, we will explore how to subscribe to Observables in Swift and handle the emitted values.

## Installing Reactive Libraries

Before we start, it's important to have a reactive library installed in your project. Two popular options for reactive programming in Swift are **RxSwift** and **Combine**. Make sure you have either of them integrated into your Xcode project, as it provides the necessary building blocks for working with Observables.

## Creating an Observable

To subscribe to an Observable, we first need to create one. This can be done using the provided operators or by converting existing asynchronous operations into Observables.

```swift
import RxSwift

let observable = Observable.of(1, 2, 3, 4, 5)
```

In this example, we created an Observable that emits the values 1, 2, 3, 4, and 5 sequentially.

## Subscribing to an Observable

Once we have the Observable, we can subscribe to it to receive and react to the emitted values. This is done by calling the `subscribe` method on the Observable instance.

```swift
observable.subscribe(onNext: { value in
    print(value)
}, onCompleted: {
    print("Observable completed")
})
```

In the above code snippet, we provided two closures: `onNext` and `onCompleted`. The `onNext` closure will be called for each emitted value, allowing us to perform actions based on those values. The `onCompleted` closure is called when the Observable completes, indicating that no more values will be emitted.

## Handling Errors

Observables can also emit errors, and it's important to handle them properly. We can provide an `onError` closure when subscribing to an Observable to handle any errors that occur during the execution.

```swift
observable.subscribe(onNext: { value in
    print(value)
}, onError: { error in
    print("Error: \(error)")
}, onCompleted: {
    print("Observable completed")
})
```

Here, the `onError` closure will be called if an error occurs during the execution of the Observable.

## Disposing Observables

It's crucial to dispose of the Observables when we no longer need them. Failing to do so can lead to memory leaks and unwanted side effects. Both RxSwift and Combine provide mechanisms for disposing of Observables.

In RxSwift, we can use the `dispose` method to manually dispose of an Observable.

```swift
let disposable = observable.subscribe(onNext: { value in
    print(value)
}, onCompleted: {
    print("Observable completed")
})

disposable.dispose()
```

In Combine, we can use the `cancel` method to cancel the subscription to an Observable.

```swift
let cancellable = observable.sink { value in
    print(value)
} receiveCompletion: { completion in
    print("Observable completed")
}

cancellable.cancel()
```

By properly disposing of Observables, we ensure that resources are freed up and no unnecessary computations are performed.

## Conclusion

Subscribing to Observables in Swift reactive programming allows us to handle and react to asynchronous and event-driven code with ease. By using reactive libraries like RxSwift or Combine, we can create powerful and efficient code that is easier to understand and maintain.

Remember to install the necessary libraries, create Observables, subscribe to them, handle errors properly, and dispose of them when they are no longer needed. With these practices in place, you can harness the power of reactive programming in Swift effectively.

#reactiveprogramming #swiftprogramming