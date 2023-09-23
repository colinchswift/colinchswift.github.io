---
layout: post
title: "Passing data between ViewControllers in Swift"
description: " "
date: 2023-09-23
tags: [Swift]
comments: true
share: true
---

When developing iOS applications with Swift, it is quite common to pass data between different `ViewControllers`. This allows you to share information and ensure a seamless user experience. In this blog post, we will explore some common techniques for passing data between `ViewControllers` in Swift.

## 1. Using Segues

`Segues` in the `UIKit` framework provide an intuitive way to perform a transition between `ViewControllers`. They also allow you to pass data between them. Here's an example of how you can pass data using `Segues`:

```swift
// Source ViewController
class SourceViewController: UIViewController {
    // Define a variable to hold the data to be passed
    var dataToPass: String = "Hello World"

    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        if segue.identifier == "destinationSegue" {
            // Get the destination ViewController
            if let destinationVC = segue.destination as? DestinationViewController {
                // Pass the data to the destination ViewController
                destinationVC.receivedData = dataToPass
            }
        }
    }
}

// Destination ViewController
class DestinationViewController: UIViewController {
    // Define a variable to receive the passed data
    var receivedData: String = ""

    override func viewDidLoad() {
        super.viewDidLoad()
        // Use the receivedData in this ViewController
        print(receivedData) // Output: Hello World
    }
}
```

In this example, the `SourceViewController` has a variable `dataToPass` that holds the data we want to pass. In the `prepare(for:sender:)` method, we check if the `segue.identifier` matches the identifier of the segue we want to execute. If it does, we cast the destination ViewController as `DestinationViewController` and pass the data to its `receivedData` variable.

## 2. Using Delegation

Delegation is another powerful technique for passing data between `ViewControllers`. It involves creating a protocol in the source ViewController and making the destination ViewController conform to that protocol. Here's an example:

```swift
// Source ViewController
class SourceViewController: UIViewController {
    // Define a delegate property
    weak var delegate: DestinationViewControllerDelegate?

    func sendData() {
        let dataToPass = "Hello World"
        delegate?.receiveData(dataToPass)
    }
}

// Destination ViewController
protocol DestinationViewControllerDelegate: AnyObject {
    func receiveData(_ data: String)
}

class DestinationViewController: UIViewController, DestinationViewControllerDelegate {
    func receiveData(_ data: String) {
    	print(data) // Output: Hello World
    }
}
```

In this example, the `SourceViewController` defines a delegate property of type `DestinationViewControllerDelegate`. The `sendData()` method is invoked when you want to pass the data. The delegate's `receiveData(_:)` method is then called, allowing the destination ViewController to receive the data.

## Summary

Passing data between ViewControllers is a crucial aspect of iOS app development. In this blog post, we explored two common techniques for achieving this: using segues and delegation. Both approaches offer flexibility and can be used depending on the specific requirements of your application.

#iOS #Swift