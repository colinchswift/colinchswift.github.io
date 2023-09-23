---
layout: post
title: "Using dependency injection for handling device motion in Swift"
description: " "
date: 2023-09-24
tags: [Swift]
comments: true
share: true
---

Handling device motion in iOS apps often requires the use of Core Motion framework. However, managing the dependency and integration of Core Motion can become complex as the app grows. In this blog post, we will explore how to use dependency injection to handle device motion in Swift, making our code more modular, testable, and maintainable.

## What is Dependency Injection?

Dependency Injection is a software design pattern that allows components or classes to be loosely coupled and promotes better code organization and testability. In the context of iOS development, dependency injection involves providing dependencies from outside the class instead of creating them within the class itself.

## Dependencies required for handling device motion

Before diving into the implementation, let's first identify the dependencies required for handling device motion using Core Motion. In this case, we will require an instance of the `CMMotionManager` class:

```swift
import CoreMotion

protocol DeviceMotionHandler {
    func startMotionUpdates()
    func stopMotionUpdates()
}

class DeviceMotionHandlerImpl: DeviceMotionHandler {
    private let motionManager: CMMotionManager
    
    init(motionManager: CMMotionManager) {
        self.motionManager = motionManager
    }
    
    func startMotionUpdates() {
        // Start handling device motion updates
        // using self.motionManager
    }
    
    func stopMotionUpdates() {
        // Stop handling device motion updates
    }
}
```

## Implementing Dependency Injection for Device Motion Handling

Now, let's implement dependency injection to handle device motion using the `DeviceMotionHandler` protocol:

```swift
protocol MotionManagerProvider {
    var motionManager: CMMotionManager { get }
}

class DefaultMotionManagerProvider: MotionManagerProvider {
    var motionManager: CMMotionManager {
        return CMMotionManager()
    }
}

class DeviceMotionHandlerImpl: DeviceMotionHandler {
    private let motionManagerProvider: MotionManagerProvider
    
    init(motionManagerProvider: MotionManagerProvider) {
        self.motionManagerProvider = motionManagerProvider
    }
    
    func startMotionUpdates() {
        let motionManager = motionManagerProvider.motionManager
        // Start handling device motion updates
        // using motionManager
    }
    
    func stopMotionUpdates() {
        // Stop handling device motion updates
    }
}
```

With this implementation, we have decoupled our `DeviceMotionHandler` class from the specific implementation of `CMMotionManager`, making it easier to switch or mock the motion manager for testing purposes.

## Using Dependency Injection

To use dependency injection for handling device motion, we need to initialize our `DeviceMotionHandlerImpl` class with an instance of `MotionManagerProvider`. We can use a dependency injection framework or manually set up the dependencies using a dependency container class.

```swift
let motionManagerProvider = DefaultMotionManagerProvider()
let deviceMotionHandler = DeviceMotionHandlerImpl(motionManagerProvider: motionManagerProvider)

deviceMotionHandler.startMotionUpdates()
```

## Conclusion

Using dependency injection to handle device motion in Swift can greatly improve code modularity, testability, and maintainability. By decoupling our device motion handling code from the specific implementation of `CMMotionManager`, we can easily swap out dependencies and write more robust unit tests.

#iOS #Swift