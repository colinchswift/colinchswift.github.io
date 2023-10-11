---
layout: post
title: "Building a step tracker with Core Motion in Swift"
description: " "
date: 2023-10-11
tags: []
comments: true
share: true
---

With the rise in popularity of fitness trackers and health apps, step tracking has become a common feature that users expect. In this tutorial, we will learn how to build a step tracker in Swift using Core Motion, a framework provided by Apple for working with the motion sensors in iOS devices.

## Table of Contents
- [Introduction to Core Motion](#introduction-to-core-motion)
- [Setting Up the Project](#setting-up-the-project)
- [Accessing Step Count Data](#accessing-step-count-data)
- [Displaying the Step Count](#displaying-the-step-count)
- [Updating the Step Count in Real-time](#updating-the-step-count-in-real-time)
- [Conclusion](#conclusion)

## Introduction to Core Motion

Core Motion is a framework that provides access to the motion sensors present in iOS devices, such as the accelerometer and gyroscope. It allows us to gather and analyze motion data, including step count, distance traveled, and more. In this tutorial, we will focus on using Core Motion to track the number of steps a user takes.

## Setting Up the Project

To get started, create a new Swift project in Xcode. Open the project, and in the project navigator, right-click on the project folder and select "Add Files to [Project Name]" to add a new Swift file. Name the file "StepTracker.swift" and click "Create".

In the StepTracker.swift file, import the CoreMotion framework:

```swift
import CoreMotion
```

## Accessing Step Count Data

To access the step count data, we need to create an instance of the `CMPedometer` class provided by Core Motion. Add the following code to the StepTracker.swift file:

```swift
class StepTracker {
    private let pedometer = CMPedometer()

    func getStepCount(completion: @escaping (Int) -> Void) {
        pedometer.queryPedometerData(from: Date(), to: Date()) { (data, error) in
            if let stepCount = data?.numberOfSteps.intValue {
                completion(stepCount)
            } else {
                completion(0)
            }
        }
    }
}
```

In the above code, we create a private instance of `CMPedometer` called `pedometer`. We then define a method `getStepCount(completion:)` which takes a completion closure as a parameter. Inside the method, we use the `queryPedometerData(from:to:completion:)` method to query the step count data from the current date to the current date. The result is returned through the completion closure.

## Displaying the Step Count

To display the step count in your app's UI, you can use a `UILabel` or any other suitable UI element. In your view controller, create an instance of the `StepTracker` class and call the `getStepCount(completion:)` method to fetch the step count. Update the UI element with the step count value.

```swift
let stepTracker = StepTracker()

stepTracker.getStepCount { [weak self] stepCount in
    DispatchQueue.main.async {
        self?.stepCountLabel.text = "\(stepCount) steps"
    }
}
```

In the above code, we create an instance of `StepTracker` called `stepTracker`. We then call the `getStepCount(completion:)` method to fetch the step count. Inside the completion closure, we update the `stepCountLabel` with the step count value. Note that we use `DispatchQueue.main.async` to update any UI elements on the main thread.

## Updating the Step Count in Real-time

To update the step count in real-time, we can use the `startUpdates(from:withHandler:)` method provided by `CMPedometer`. This method continuously updates the step count at regular intervals.

Add the following code to the `StepTracker` class:

```swift
func startUpdatingStepCount(completion: @escaping (Int) -> Void) {
    if CMPedometer.isStepCountingAvailable() {
        pedometer.startUpdates(from: Date()) { (data, error) in
            if let stepCount = data?.numberOfSteps.intValue {
                completion(stepCount)
            } else {
                completion(0)
            }
        }
    } else {
        completion(0)
    }
}

func stopUpdatingStepCount() {
    pedometer.stopUpdates()
}
```

In the above code, we define two methods: `startUpdatingStepCount(completion:)` and `stopUpdatingStepCount()`. The `startUpdatingStepCount(completion:)` method starts updating the step count and returns the updated values through the completion closure. The `stopUpdatingStepCount()` method stops the updates.

## Conclusion

In this tutorial, we learned how to build a step tracker in Swift using Core Motion. We explored how to access the step count data and display it in the user interface. We also looked at how we can update the step count in real-time using the `startUpdates(from:withHandler:)` method.

By leveraging Core Motion, you can add step tracking functionality to your iOS apps and enhance the overall user experience.