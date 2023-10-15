---
layout: post
title: "Using Background Tasks API for executing tasks in Swift"
description: " "
date: 2023-10-16
tags: []
comments: true
share: true
---

In iOS development, there are often situations where we need to perform tasks in the background without interrupting the main app's user interface. These tasks can include downloading data, processing images, or syncing with remote servers. Previously, developers had to rely on workarounds and hacks to achieve background execution. However, with the release of iOS 13, Apple introduced the Background Tasks API, making it easier to execute tasks in the background.

In this blog post, we will explore how to use the Background Tasks API in Swift to execute tasks efficiently and reliably.

## Table of Contents
1. [Introduction to the Background Tasks API](#introduction-to-the-background-tasks-api)
2. [Setting Up Background Execution](#setting-up-background-execution)
3. [Implementing Long-Running Tasks](#implementing-long-running-tasks)
4. [Handling Background Task Expiration](#handling-background-task-expiration)
5. [Managing Multiple Background Tasks](#managing-multiple-background-tasks)
6. [Conclusion](#conclusion)

## Introduction to the Background Tasks API

The Background Tasks API allows developers to register and perform tasks that continue executing even if the app is suspended or in the background. It provides a way for apps to request additional execution time to complete critical tasks.

## Setting Up Background Execution

To enable background execution for your app, you need to make a few setup changes in your project's `Info.plist` file. Add the following keys:

```swift
<key>UIBackgroundModes</key>
<array>
    <string>fetch</string>
    <string>processing</string>
    <string>remote-notification</string>
</array>
```

These keys specify the types of background tasks your app can perform, such as data fetching, background processing, and handling remote notifications. Make sure to select the appropriate background modes based on your app's requirements.

## Implementing Long-Running Tasks

Once you have configured background execution, you can implement long-running tasks using the `BGTaskScheduler` API. Here's an example of how to schedule a background task to fetch data:

```swift
import BackgroundTasks

func scheduleBackgroundFetch() {
    let identifier = "com.yourapp.backgroundfetch"
    BGTaskScheduler.shared.register(forTaskWithIdentifier: identifier, using: nil) { task in
        // Perform data fetching here
        
        task.setTaskCompleted(success: true)
    }
    
    do {
        let request = BGAppRefreshTaskRequest(identifier: identifier)
        try BGTaskScheduler.shared.submit(request)
    } catch {
        print("Unable to schedule background fetch: \(error.localizedDescription)")
    }
}
```

In this example, we define a function `scheduleBackgroundFetch()` that registers a background task with the identifier "com.yourapp.backgroundfetch". Inside the task block, we perform the necessary data fetching operations and call `task.setTaskCompleted(success:)` to mark the task as completed.

Finally, we create an instance of `BGAppRefreshTaskRequest` and submit it to the `BGTaskScheduler` to schedule the background fetch task.

## Handling Background Task Expiration

Background tasks have a limited time window for execution. If the task exceeds its allocated time, it will be terminated. To prevent this, you can use the `BGTaskRequest`'s `earliestBeginDate` property to specify the earliest time the task should start.

```swift
let request = BGProcessingTaskRequest(identifier: identifier)
request.requiresNetworkConnectivity = true
request.earliestBeginDate = Date(timeIntervalSinceNow: 1 * 60) // Start after 1 minute

do {
    try BGTaskScheduler.shared.submit(request)
} catch {
    print("Unable to schedule background task: \(error.localizedDescription)")
}
```

In this example, we set `earliestBeginDate` to one minute in the future, which gives the system time to handle other critical tasks before starting our long-running task.

## Managing Multiple Background Tasks

If your app requires multiple background tasks, you can differentiate them using unique identifiers. Make sure to register each task separately and provide their respective logic inside the task block.

```swift
func scheduleBackgroundTask1() {
    let identifier = "com.yourapp.task1"
    BGTaskScheduler.shared.register(forTaskWithIdentifier: identifier, using: nil) { task in
        // Perform task 1 operations
        
        task.setTaskCompleted(success: true)
    }
    
    // Submit request
}

func scheduleBackgroundTask2() {
    let identifier = "com.yourapp.task2"
    BGTaskScheduler.shared.register(forTaskWithIdentifier: identifier, using: nil) { task in
        // Perform task 2 operations
        
        task.setTaskCompleted(success: true)
    }
    
    // Submit request
}
```

In this example, we have two separate functions, `scheduleBackgroundTask1()` and `scheduleBackgroundTask2()`, each with its own unique identifier and task logic. Register and submit each task independently.

## Conclusion

The Background Tasks API in Swift provides a systematic and clean way to execute tasks in the background without affecting the main app's performance. By using this API, you can handle long-running tasks efficiently and ensure that crucial operations complete even when the app is not actively in the foreground.

By incorporating the Background Tasks API into your app's architecture, you can enhance the user experience and make your app more powerful and capable.

**References:**
- [Apple Developer Documentation](https://developer.apple.com/documentation/backgroundtasks)