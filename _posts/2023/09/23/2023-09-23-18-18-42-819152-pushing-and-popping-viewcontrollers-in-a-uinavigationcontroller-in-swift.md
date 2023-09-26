---
layout: post
title: "Pushing and popping ViewControllers in a UINavigationController in Swift"
description: " "
date: 2023-09-23
tags: []
comments: true
share: true
---

The `UINavigationController` class in Swift provides an easy way to navigate between different view controllers in an iOS app. In this blog post, we will explore how to push and pop view controllers onto a navigation stack using a `UINavigationController`.

## Pushing a View Controller onto the Stack

To push a new view controller onto the navigation stack, you will need to perform the following steps:

1. Create an instance of the view controller you want to push onto the stack.
2. Use the `pushViewController(_:animated:)` method of the `UINavigationController` instance to push the view controller onto the stack.

Here's an example of how to push a new view controller onto the stack:

```swift
let newViewController = NewViewController()
navigationController.pushViewController(newViewController, animated: true)
```

In the above example, `NewViewController` is the view controller that you want to push onto the navigation stack. The `pushViewController(_:animated:)` method is called on the `navigationController` instance, specifying the new view controller to be pushed.

## Popping View Controllers from the Stack

To remove a view controller from the navigation stack and navigate back to the previous view controller, you can use the `popViewController(animated:)` method of the `UINavigationController` instance.

Here's an example of how to pop the current view controller from the stack:

```swift
navigationController.popViewController(animated: true)
```

In the above example, the `popViewController(animated:)` method is called on the `navigationController` instance, which removes the current view controller from the stack and navigates back to the previous view controller.

## Conclusion

The `UINavigationController` class in Swift provides a convenient way to navigate between different view controllers in an iOS app. By using the `pushViewController(_:animated:)` method, you can push new view controllers onto the stack, and with the `popViewController(animated:)` method, you can remove view controllers from the stack and navigate back to the previous view controller.

By understanding how to push and pop view controllers in a `UINavigationController`, you can create seamless and user-friendly navigation within your iOS app.

#iOS #Swift