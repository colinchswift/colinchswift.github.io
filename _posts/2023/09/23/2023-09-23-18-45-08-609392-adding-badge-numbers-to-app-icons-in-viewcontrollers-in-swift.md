---
layout: post
title: "Adding badge numbers to app icons in ViewControllers in Swift"
description: " "
date: 2023-09-23
tags: [iOSDevelopment]
comments: true
share: true
---

Badges on app icons are a popular way to notify users about new messages, notifications, or updates within an app. In this tutorial, we'll learn how to add badge numbers to app icons in ViewControllers using Swift.

## Prerequisites

Before we get started, make sure you have the following:

- Basic knowledge of Swift programming language and iOS development
- Xcode installed on your machine

## Step 1: Import UserNotifications framework

First, we need to import the UserNotifications framework to handle badge numbers.

```swift
import UserNotifications
```

## Step 2: Request authorization for badges

Next, we need to request authorization from the user to display badge numbers. You can add the following code to the `viewDidLoad` method of your ViewController.

```swift
UNUserNotificationCenter.current().requestAuthorization(options: [.badge]) { (granted, error) in
    if granted {
        DispatchQueue.main.async {
            UIApplication.shared.registerForRemoteNotifications()
        }
    }
}
```

## Step 3: Set the badge number

To set the badge number, you can use the `UIApplication.shared.applicationIconBadgeNumber` property. For example, to set the badge number to 5:

```swift
UIApplication.shared.applicationIconBadgeNumber = 5
```

You can place this code in the appropriate location, such as when a new notification arrives or when a message is received.

## Step 4: Clear the badge number

To clear the badge number, simply set it to 0:

```swift
UIApplication.shared.applicationIconBadgeNumber = 0
```

Place this code where you want to remove the badge number, such as when the user opens a specific screen.

## Conclusion

Adding badge numbers to app icons can be a useful way to keep users informed about new updates in your app. By following these steps, you can easily add and clear badge numbers in ViewControllers using Swift.

Use #Swift #iOSDevelopment hashtags to share your experience with badge numbers in ViewControllers and connect with fellow developers!