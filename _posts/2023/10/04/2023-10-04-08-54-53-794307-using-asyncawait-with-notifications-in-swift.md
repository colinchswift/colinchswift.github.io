---
layout: post
title: "Using async/await with notifications in Swift"
description: " "
date: 2023-10-04
tags: [prerequisites), using]
comments: true
share: true
---

In Swift, asynchronous programming can be made easier and more readable with the `async` and `await` keywords. One common use case is working with notifications, where you want to wait for a notification to be received before continuing with your code execution. In this blog post, we will explore how to use `async/await` with notifications in Swift.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Using async/await with notifications](#using-async-await-with-notifications)
- [Example code](#example-code)
- [Conclusion](#conclusion)

## Prerequisites

To follow along with this tutorial, you need to have a basic understanding of asynchronous programming and notifications in Swift. If you're new to asynchronous programming, I recommend checking out Apple's official documentation on [Concurrency in Swift](https://docs.swift.org/swift-book/LanguageGuide/Concurrency.html).

## Using async/await with notifications

The first step in using `async/await` with notifications is to create an `NotificationCenter` extension that allows us to await for a specific notification. Let's create an extension called `NotificationCenter+AsyncAwait`:

```swift
extension NotificationCenter {
    func awaitNotification(_ name: Name, object: Any? = nil) async -> Notification {
        // Create a notification center observer using async/await
        let notification = await withUnsafeContinuation { continuation in
            let observer = addObserver(forName: name, object: object, queue: nil) { notification in
                continuation.resume(returning: notification)
                removeObserver(observer)
            }
        }
        
        return notification
    }
}
```

In the `awaitNotification` method, we use `withUnsafeContinuation` to create an asynchronous observer for the specified notification. The `continuation.resume(returning:)` call will resume the suspended execution of the awaiting task when the notification is received. Finally, we remove the observer to avoid any memory leaks.

Now, you can use `awaitNotification` to wait for a specific notification to be posted. Let's see an example:

```swift
func performLongRunningTask() async {
    // Simulate a long-running task
    await Task.sleep(2 * 1_000_000_000) // 2 seconds
    
    // Post a notification after the task is completed
    NotificationCenter.default.post(name: .longRunningTaskCompleted, object: nil)
}

Task.detached {
    print("Starting the long-running task...")
    await performLongRunningTask()
    print("Long-running task completed!")
    
    // Wait for the notification after the task is completed
    let notification = await NotificationCenter.default.awaitNotification(.longRunningTaskCompleted)
    
    // Do something with the received notification
    print("Received notification: \(notification.name)")
}
```

In this example, we define a `performLongRunningTask` function that simulates a long-running task using `Task.sleep`. After the task is completed, we post a notification named `longRunningTaskCompleted`. Then, we create a detached task that awaits the completion of the long-running task using `await performLongRunningTask()`. Finally, we use `await NotificationCenter.default.awaitNotification` to wait for the notification to be received and print its name.

## Conclusion

Using `async/await` with notifications in Swift can make your asynchronous code more readable and easier to work with. By creating an extension on `NotificationCenter` that allows you to await for specific notifications, you can wait for notifications to be received before continuing with your code execution. Give it a try in your own projects and enjoy the benefits of cleaner asynchronous code! #swift #asyncawait