---
layout: post
title: "Implementing delegation between ViewControllers in Swift"
description: " "
date: 2023-09-23
tags: [Swift]
comments: true
share: true
---

Delegation is a powerful design pattern in iOS development that allows one object to communicate with another object without having a strong coupling between them. In the context of ViewControllers, delegation can be used to send information or trigger actions between different ViewControllers in your app.

Here's a step-by-step guide on how to implement delegation between ViewControllers in Swift:

## Step 1: Define a Protocol

First, we need to define a protocol that specifies the methods that the delegate should implement. This protocol acts as a contract between the two ViewControllers:

```swift
protocol DataDelegate: AnyObject {
    func didFinishTask(data: String)
}
```

In the above example, we define a protocol called `DataDelegate` with a single method `didFinishTask(data:)` that takes a `String` parameter.

## Step 2: Create a Delegate Property

Next, we need to create a delegate property in the source ViewController. This property will hold a reference to the delegate object and allow us to call its methods:

```swift
class SourceViewController: UIViewController {
    weak var delegate: DataDelegate?
    
    // ...
}
```

Make sure to mark the delegate property as `weak` to avoid strong reference cycles and potential memory leaks.

## Step 3: Call the Delegate Method

In the source ViewController, when an important task is completed, we can call the delegate method to notify the delegate:

```swift
class SourceViewController: UIViewController {
    // ...
    
    func taskCompleted() {
        delegate?.didFinishTask(data: "Task completed successfully")
    }
}
```

In the above example, when the `taskCompleted()` method is called, it checks if the delegate object exists and if the `didFinishTask(data:)` method is implemented.

## Step 4: Set the Delegate

Finally, in the destination ViewController, we need to set itself as the delegate of the source ViewController. This is usually done when navigating from the source ViewController to the destination ViewController:

```swift
class DestinationViewController: UIViewController, DataDelegate {
    // ...
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        let sourceViewController = getSourceViewController()
        sourceViewController.delegate = self
    }
    
    // Implement the delegate method
    func didFinishTask(data: String) {
        print("Received data: \(data)")
    }
}
```

In the above example, the destination ViewController conforms to the `DataDelegate` protocol and sets itself as the delegate of the source ViewController. It also implements the `didFinishTask(data:)` method to receive the data sent from the source ViewController.

That's it! You have successfully implemented delegation between ViewControllers in Swift. Delegation provides a clean and flexible way to communicate between ViewControllers, making your code more modular and maintainable.

#iOS #Swift