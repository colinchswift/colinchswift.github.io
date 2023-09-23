---
layout: post
title: "Using UIAlertController in ViewControllers in Swift"
description: " "
date: 2023-09-23
tags: [Swift, UIAlertController]
comments: true
share: true
---

A common requirement for iOS apps is to display alert messages to the user. This can be accomplished using the `UIAlertController` class in Swift. In this blog post, we will explore how to use `UIAlertController` in ViewControllers in Swift to present alerts to the user.

## Importing the required framework

Before we can use `UIAlertController`, we need to import the `UIKit` framework. To do this, add the following line at the top of your ViewController file:

```swift
import UIKit
```

## Creating and presenting an alert

To display an alert using `UIAlertController`, we first need to create an instance of it. The basic structure of creating an alert is as follows:

```swift
let alert = UIAlertController(title: "Alert title", message: "Alert message", preferredStyle: .alert)
```

Here, we specify the `title` and `message` parameters of the alert. We can also specify the `preferredStyle`, which can be `.alert` for a standard alert view or `.actionSheet` for a sheet-like alert.

Once we have created the alert, we can add actions to it. Actions are the buttons that the user can tap to respond to the alert. To add an action, use the `addAction` method:

```swift
alert.addAction(UIAlertAction(title: "OK", style: .default, handler: nil))
```

Here, we specify the `title` and `style` of the action. The `handler` parameter can be used to provide a closure that is executed when the action is tapped.

Finally, we can present the alert to the user. To do this, use the `present` method:

```swift
self.present(alert, animated: true, completion: nil)
```

In this example, we are presenting the alert from the current ViewController (`self`). The `animated` parameter specifies whether the presentation should be animated. The `completion` parameter can be used to specify a completion closure that is executed after the presentation is complete.

## Conclusion

In this blog post, we have learned how to use `UIAlertController` in ViewControllers in Swift to present alerts to the user. By following these simple steps, you can add alert functionality to your iOS app and enhance the user experience. Happy coding!

## #Swift #UIAlertController