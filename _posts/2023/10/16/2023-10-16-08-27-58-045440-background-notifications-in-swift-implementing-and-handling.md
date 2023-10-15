---
layout: post
title: "Background notifications in Swift: Implementing and handling"
description: " "
date: 2023-10-16
tags: []
comments: true
share: true
---

In many cases, mobile apps need to receive and handle notifications even when they are not actively running in the foreground. This is where background notifications come into play. Background notifications allow apps to receive important updates, such as new messages, without the need for the user to have the app open. In this article, we will explore how to implement and handle background notifications in Swift.

## Table of Contents

- [Introduction](#introduction)
- [Implementing Background Notifications](#implementing-background-notifications)
- [Handling Background Notifications](#handling-background-notifications)
- [Conclusion](#conclusion)

## Introduction

Background notifications require a few steps for implementation. Firstly, you need to enable the necessary capabilities in your Xcode project. Open your project in Xcode, select the target, and navigate to the "Signing & Capabilities" tab. From here, enable the "Background Modes" capability and check the "Remote notifications" box.

## Implementing Background Notifications

Once you have enabled the necessary capabilities, you need to implement the methods to handle background notifications. In your AppDelegate.swift file, add the following code:

```swift
import UIKit
import UserNotifications

@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate, UNUserNotificationCenterDelegate {

    // ...

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        UNUserNotificationCenter.current().delegate = self
        UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .badge, .sound]) { (granted, error) in
            if granted {
                DispatchQueue.main.async {
                    application.registerForRemoteNotifications()
                }
            }
        }
        return true
    }

    // ...

    func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
        let token = deviceToken.map { String(format: "%02.2hhx", $0) }.joined()
        print("Device Token: \(token)")
    }

    // ...

    func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable: Any]) {
        // Handle the received notification
    }

    // ...

}
```

This code sets up the delegate for handling notifications, requests authorization from the user to receive notifications, and registers the app to receive remote notifications.

## Handling Background Notifications

Once a background notification is received, the `application(_:didReceiveRemoteNotification:)` method will be called. You can handle the received notification in this method according to your app's logic.

```swift
func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable: Any]) {
    guard let aps = userInfo["aps"] as? [String: AnyObject] else {
        return
    }

    // Extract the necessary data from the notification
    let title = aps["alert"]["title"] as? String
    let body = aps["alert"]["body"] as? String

    // Perform actions based on the notification data
    if let title = title, let body = body {
        showAlert(title: title, message: body)
    }
}
```

In this example, we extract the title and body of the notification and display them in an alert view. This can be modified to suit your specific needs.

## Conclusion

Implementing and handling background notifications in Swift is a crucial part of providing a seamless user experience. By following the steps outlined in this article, you can enable your app to receive important updates and keep users engaged even when the app is not actively running in the foreground. Happy coding!

_References:_
- [Apple Developer Documentation - UserNotifications](https://developer.apple.com/documentation/usernotifications)
- [Apple Developer Documentation - UIApplicationDelegate](https://developer.apple.com/documentation/uikit/uiapplicationdelegate)