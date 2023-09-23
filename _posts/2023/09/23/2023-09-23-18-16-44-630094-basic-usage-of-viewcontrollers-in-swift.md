---
layout: post
title: "Basic usage of ViewControllers in Swift"
description: " "
date: 2023-09-23
tags: [selector(buttonTapped), programming]
comments: true
share: true
---

ViewControllers are an essential part of building user interfaces in Swift. They act as the intermediary between your app's data and the views displayed to the user. In this blog post, we will explore the basic usage of ViewControllers in Swift and how they can be leveraged to build interactive user interfaces.

## Creating a ViewController

To create a new ViewController in Swift, you need to subclass the `UIViewController` class. First, let's start by creating a simple ViewController called `MyViewController`.

```swift
import UIKit

class MyViewController: UIViewController {
    // Your code here
}
```

## Adding Views to ViewControllers

Once you have created a ViewController, you can add views to it. Views are visual elements that are displayed to the user. The most common way to add views is by using Interface Builder, which provides a visual way to design your user interface.

To add views programmatically, you can do so in the `viewDidLoad` method of your ViewController. Here's an example of adding a UILabel and a UIButton to `MyViewController`.

```swift
class MyViewController: UIViewController {
    override func viewDidLoad() {
        super.viewDidLoad()
        
        let label = UILabel(frame: CGRect(x: 0, y: 0, width: 200, height: 50))
        label.text = "Hello, World!"
        view.addSubview(label)
        
        let button = UIButton(frame: CGRect(x: 0, y: 60, width: 100, height: 50))
        button.setTitle("Tap Me", for: .normal)
        button.addTarget(self, action: #selector(buttonTapped), for: .touchUpInside)
        view.addSubview(button)
    }
    
    @objc func buttonTapped() {
        // Callback for button tap
    }
}
```

## Navigating between ViewControllers

In a typical app, you may need to navigate from one ViewController to another. For example, when a user taps a button, you may want to present a different ViewController.

To navigate from one ViewController to another, you typically use a `UINavigationController`. Here's an example of navigating from `MyViewController` to a new ViewController called `NextViewController`.

```swift
class MyViewController: UIViewController {
    @objc func buttonTapped() {
        let nextViewController = NextViewController()
        navigationController?.pushViewController(nextViewController, animated: true)
    }
}
```

## Conclusion

ViewControllers are fundamental building blocks for creating user interfaces in Swift. They allow you to add views, respond to user interactions, and navigate between different screens in your app. By understanding the basic usage of ViewControllers, you can start building interactive and engaging user interfaces in your Swift apps.

#programming #Swift