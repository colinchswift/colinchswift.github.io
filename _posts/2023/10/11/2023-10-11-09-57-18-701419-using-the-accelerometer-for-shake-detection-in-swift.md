---
layout: post
title: "Using the accelerometer for shake detection in Swift"
description: " "
date: 2023-10-11
tags: []
comments: true
share: true
---

Shake detection is a common use case in mobile apps, enabling users to trigger certain actions by simply shaking their devices. In iOS, you can utilize the built-in accelerometer to detect device movements, including shakes. In this article, we will explore how to implement shake detection using the accelerometer in Swift.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Setup](#setup)
- [Implementing Shake Detection](#implementing-shake-detection)
- [Handling Shake Events](#handling-shake-events)
- [Conclusion](#conclusion)

## Prerequisites

To follow along with this tutorial, you'll need:
- Xcode installed on your Mac.
- Basic knowledge of Swift programming language.

## Setup

Start by creating a new Swift project in Xcode. Open the `ViewController.swift` file and navigate to the `viewDidLoad()` method.

First, enable the accelerometer by adding the following code inside the `viewDidLoad()` method:

```swift
import CoreMotion

class ViewController: UIViewController {
    let motionManager = CMMotionManager()

    override func viewDidLoad() {
        super.viewDidLoad()
      
        motionManager.accelerometerUpdateInterval = 0.1
        motionManager.startAccelerometerUpdates()
    }
}
```
The above code imports the `CoreMotion` framework and creates an instance of `CMMotionManager` to access the accelerometer data.

## Implementing Shake Detection

Add the following code snippet to implement shake detection:

```swift
override func motionBegan(_ motion: UIEvent.EventSubtype, with event: UIEvent?) {
    if motion == .motionShake {
        // Shake detected
        print("Shake detected!")
    }
}
```

The `motionBegan` function is a built-in method that gets called when a motion event starts. We check if the event subtype is `.motionShake` to determine if the shake gesture was detected.

## Handling Shake Events

To handle shake events, we can extend the `ViewController` class with the `UIResponder` protocol and override the `motionEnded` method:

```swift
extension ViewController {
    override func motionEnded(_ motion: UIEvent.EventSubtype, with event: UIEvent?) {
        if motion == .motionShake {
            // Handle shake event
            showAlert()
        }
    }
  
    func showAlert() {
        let alertController = UIAlertController(title: "Shake Detected", message: "You shook the device!", preferredStyle: .alert)
        let okAction = UIAlertAction(title: "OK", style: .default)
        alertController.addAction(okAction)
        present(alertController, animated: true, completion: nil)
    }
}
```

In this example, we display an alert controller when a shake event is detected. Feel free to customize the `showAlert()` function to perform any desired actions upon detecting a shake.

## Conclusion

In this tutorial, you learned how to use the accelerometer to detect shakes in your Swift app. You implemented shake detection by using the `CMMotionManager`, and then handled shake events by overriding the appropriate methods. Shake detection can add an interactive element to your app and provide users with an intuitive way to trigger certain actions.