---
layout: post
title: "Instantiating ViewControllers programmatically in Swift"
description: " "
date: 2023-09-23
tags: [SwiftProgramming, iOSDevelopment]
comments: true
share: true
---

When working with SwiftUI, we often create views and view hierarchies using the declarative syntax that SwiftUI provides. However, there may be times when we need to instantiate and present a view controller programmatically. In this blog post, we'll explore how to instantiate view controllers programmatically in Swift.

## 1. Instantiating a View Controller

To instantiate a view controller programmatically, we need to create an instance of the desired view controller class using the `UIViewController()` initializer. We can then configure any properties or settings before presenting the view controller.

```swift
let viewController = UIViewController()
```

## 2. Setting the Root View Controller

After instantiating the view controller, we need to set it as the root view controller of the window or a navigation controller. 

If we have a `UIWindow` object, we can set the `rootViewController` property to the instantiated view controller:

```swift
let mainWindow = UIApplication.shared.windows.first
mainWindow?.rootViewController = viewController
```

If we are working with a navigation controller, we can set the `viewControllers` property to an array containing our instantiated view controller:

```swift
let navigationController = UINavigationController(rootViewController: viewController)
```

## 3. Presenting the View Controller

Depending on the desired presentation style, we can present the view controller modally or push it onto a navigation stack.

For modal presentation, we can use the `present(_:animated:completion:)` method:
```swift
present(viewController, animated: true, completion: nil)
```

To push the view controller onto a navigation stack, we can use the `pushViewController(_:animated:)` method of the navigation controller:

```swift
navigationController?.pushViewController(viewController, animated: true)
```

## Conclusion

In this blog post, we explored how to instantiate view controllers programmatically in Swift. By following these steps, you can dynamically create and present view controllers in your SwiftUI application. Remember to adjust the presentation style and customize the view controller based on your specific needs.

#SwiftProgramming #iOSDevelopment