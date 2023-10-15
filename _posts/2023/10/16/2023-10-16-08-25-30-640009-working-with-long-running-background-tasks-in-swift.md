---
layout: post
title: "Working with long-running background tasks in Swift"
description: " "
date: 2023-10-16
tags: [backgroundtasks]
comments: true
share: true
---

In many iOS applications, there is a need to perform tasks that take a significant amount of time, such as downloading large files or syncing data with a server. These tasks should ideally be performed in the background, so that the user can continue using the app without any interruption. In Swift, Apple provides a couple of mechanisms to handle long-running background tasks effectively. In this article, we will explore two common approaches - Background App Refresh and Background URLSession.

## Background App Refresh

Background App Refresh is a feature provided by iOS that allows apps to schedule tasks to run in the background periodically. This is useful for scenarios where you need to update data or perform any background processing at regular intervals. 

To enable Background App Refresh in your app, you need to add the following code snippet to your AppDelegate's `application(_:didFinishLaunchingWithOptions:)` method:

```swift
UIApplication.shared.setMinimumBackgroundFetchInterval(UIApplication.backgroundFetchIntervalMinimum)
```

Once Background App Refresh is enabled, you can implement the `application(_:performFetchWithCompletionHandler:)` method in your AppDelegate to handle the background fetch. This method will be called by iOS when it's time to perform the background task. You can perform your long-running task inside this method and then call the completion handler when you're done.

```swift
func application(_ application: UIApplication, performFetchWithCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
    // Perform your long-running task here
    
    // Call the completion handler when done
    completionHandler(.newData) // or .noData or .failed
}
```

It's important to note that the time allotted for background processing is limited, so make sure your task completes within the allotted time. Also, keep in mind that Background App Refresh is primarily designed for periodic updates and not for tasks that need to be executed at a specific time or on-demand.

## Background URLSession

If your long-running task involves network operations, using a Background URLSession is a better option. A Background URLSession allows you to perform data tasks, upload tasks, and download tasks in the background. The system takes care of managing the session, even when the app is suspended or terminated.

To use a Background URLSession, you need to create a URLSession configuration with the `background(withIdentifier:)` initializer, specifying a unique identifier for your session. Then, you can create a URLSession with this configuration and use it to create data tasks, upload tasks, or download tasks as needed.

Here's an example of creating a Background URLSession and performing a download task:

```swift
let sessionIdentifier = "com.yourcompany.backgroundsession"
let configuration = URLSessionConfiguration.background(withIdentifier: sessionIdentifier)
let session = URLSession(configuration: configuration, delegate: self, delegateQueue: nil)

let downloadTask = session.downloadTask(with: url)
downloadTask.resume()
```

To handle the progress and completion of the background download task, you need to conform to the `URLSessionDownloadDelegate` protocol. You can implement the delegate methods to update the UI with the download progress and handle the downloaded file when the task completes. 

```swift
extension YourViewController: URLSessionDownloadDelegate {
    func urlSession(_ session: URLSession, downloadTask: URLSessionDownloadTask, didFinishDownloadingTo location: URL) {
        // Handle the downloaded file here
    }
    
    func urlSession(_ session: URLSession, task: URLSessionTask, didCompleteWithError error: Error?) {
        // Handle any error that occurred during the task
    }
}
```

Using a Background URLSession ensures that your network tasks continue to run, even if the app is in the background or gets terminated by the system. However, keep in mind that the system has limitations on the amount of time and resources allocated for background network tasks.

## Conclusion

When working with long-running background tasks in Swift, it's important to choose the appropriate mechanism based on your specific requirements. Background App Refresh is suitable for periodic updates and background processing, while Background URLSession is ideal for performing network tasks in the background. By leveraging these features effectively, you can ensure a smooth user experience and efficient execution of long-running tasks in your iOS app.

# References
- [Apple Developer Documentation - Background Execution](https://developer.apple.com/documentation/backgroundtasks)
- [Apple Developer Documentation - URLSession](https://developer.apple.com/documentation/foundation/urlsession)
- [Ray Wenderlich - Background Tasks in iOS](https://www.raywenderlich.com/5817-background-modes-tutorial-getting-started)

#hashtags: #swift #backgroundtasks