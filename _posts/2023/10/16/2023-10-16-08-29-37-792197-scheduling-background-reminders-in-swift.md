---
layout: post
title: "Scheduling background reminders in Swift"
description: " "
date: 2023-10-16
tags: []
comments: true
share: true
---

In many mobile applications, it is common to have background reminders or notifications to keep users informed about important events or tasks. In Swift, you can utilize the `UNUserNotificationCenter` framework to schedule and manage these notifications. In this tutorial, we will walk you through the process of scheduling background reminders in Swift.

## Table of Contents
- [Enabling User Notifications](#enabling-user-notifications)
- [Requesting Authorization](#requesting-authorization)
- [Scheduling a Reminder](#scheduling-a-reminder)
- [Handling Reminder Actions](#handling-reminder-actions)

## Enabling User Notifications

To start, you need to enable user notifications in your application. Open your project and go to the `Capabilities` tab. Scroll down to `Background Modes` and enable the `Remote notifications` option.

## Requesting Authorization

Before scheduling reminders, you need to request authorization from the user to display notifications. In your `AppDelegate` or any relevant class, import the `UserNotifications` framework:

```swift
import UserNotifications
```

Next, add the following code in the `didFinishLaunchingWithOptions` method:

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .sound, .badge]) { (granted, error) in
        if granted {
            print("Authorization granted")
        } else {
            print("Authorization denied")
        }
    }
    return true
}
```

This code requests authorization for displaying alert, sound, and badge notifications to the user. If the user grants permission, you will see "Authorization granted" in the console.

## Scheduling a Reminder

Now, let's schedule a background reminder. In your relevant view controller, import the `UserNotifications` framework:

```swift
import UserNotifications
```

To schedule a reminder, add the following code:

```swift
func scheduleReminder() {
    let content = UNMutableNotificationContent()
    content.title = "Meeting Reminder"
    content.body = "Don't forget about the important meeting at 2 PM!"
    content.sound = .default

    let trigger = UNTimeIntervalNotificationTrigger(timeInterval: 60, repeats: false)

    let request = UNNotificationRequest(identifier: "reminder", content: content, trigger: trigger)

    UNUserNotificationCenter.current().add(request) { (error) in
        if let error = error {
            print("Error scheduling reminder: \(error.localizedDescription)")
        } else {
            print("Reminder scheduled successfully")
        }
    }
}
```

In this example, we create an instance of `UNMutableNotificationContent` to define the content of the reminder. We set the title, body, and sound properties accordingly.

We then create a `UNTimeIntervalNotificationTrigger` instance to specify when the reminder should be displayed. In this case, we set it to 1 minute (60 seconds) from now.

Lastly, we create a `UNNotificationRequest` with a unique identifier, the content, and the trigger. We add the request to the notification center, and in the completion block, we handle any errors or print a success message.

## Handling Reminder Actions

To handle actions when the user interacts with the reminder, you need to implement the `UNUserNotificationCenterDelegate` protocol. In your `AppDelegate` or any relevant class, declare conformance to the protocol and set the delegate:

```swift
class AppDelegate: UIResponder, UIApplicationDelegate, UNUserNotificationCenterDelegate {

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        // ...

        UNUserNotificationCenter.current().delegate = self

        // ...
    }

    // ...

    func userNotificationCenter(_ center: UNUserNotificationCenter, didReceive response: UNNotificationResponse, withCompletionHandler completionHandler: @escaping () -> Void) {
        // Handle the user's action here

        completionHandler()
    }
}
```

In the `didFinishLaunchingWithOptions` method, set the delegate to `self`. Then, implement the `userNotificationCenter(_:didReceive:withCompletionHandler:)` method to handle the user's action (e.g. opening the app when tapping on the reminder).

That's it! You have successfully scheduled and handled background reminders in Swift using the `UNUserNotificationCenter` framework.

Remember to test the reminder functionality on a physical device, as notifications may not work properly in the iOS Simulator.

## Conclusion

In this tutorial, we learned how to schedule background reminders using the `UNUserNotificationCenter` framework in Swift. We covered enabling user notifications, requesting authorization, scheduling reminders, and handling reminder actions. Incorporating reminders into your app can greatly enhance the user experience and keep users engaged with your application.

#swift #iOS