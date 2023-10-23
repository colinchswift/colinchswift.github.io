---
layout: post
title: "Playing videos with AVPlayer using PhotoKit in Swift"
description: " "
date: 2023-10-23
tags: []
comments: true
share: true
---

PhotoKit is a powerful framework in iOS that allows developers to work with photos and videos in a seamless way. One of the common use cases is to play videos stored in the user's photo library. In this tutorial, we will learn how to use AVPlayer along with PhotoKit to play videos in a Swift application.

## Prerequisites 

To follow along with this tutorial, you should have a basic understanding of Swift programming language and iOS app development. Make sure you have Xcode installed on your system.

## Step 1: Add PhotoKit and AVFoundation frameworks

First, we need to add the PhotoKit and AVFoundation frameworks to our project. To do this, follow these steps:

1. Open your Xcode project.
2. Select your project name from the project navigator.
3. Select the target for your app.
4. Go to the "General" tab.
5. Scroll down to the "Frameworks, Libraries, and Embedded Content" section.
6. Click on the "+" button.
7. Search for "PhotoKit" and click on it to add it to your project.
8. Repeat the same steps to add the "AVFoundation" framework.

## Step 2: Request authorization for accessing the photo library

Before we can access the user's photo library, we need to request authorization. Add the following code to your view controller:

```swift
import Photos

PHPhotoLibrary.requestAuthorization { status in
    if status == .authorized {
        // You have the authorization to access the photo library
    } else {
        // Authorization failed
    }
}
```

Make sure you import the `Photos` framework at the top of your view controller.

## Step 3: Fetch videos using PHFetchOptions

Next, we need to fetch the videos from the user's photo library. Add the following code to your view controller:

```swift
let fetchOptions = PHFetchOptions()
fetchOptions.predicate = NSPredicate(format: "mediaType = %d", PHAssetMediaType.video.rawValue)
let fetchResult = PHAsset.fetchAssets(with: fetchOptions)

if let videoAsset = fetchResult.firstObject {
    // Play the video
}
```

This code fetches all the videos from the user's photo library using `PHFetchOptions` and filters for media type `video`. It then retrieves the first video in the fetch result.

## Step 4: Play the video using AVPlayer

Finally, we can play the video using AVPlayer. Add the following code to your view controller:

```swift
let options = PHVideoRequestOptions()
options.isNetworkAccessAllowed = true

PHImageManager.default().requestPlayerItem(forVideo: videoAsset, options: options) { playerItem, _ in
    DispatchQueue.main.async {
        let player = AVPlayer(playerItem: playerItem)
        let playerViewController = AVPlayerViewController()
        playerViewController.player = player
        self.present(playerViewController, animated: true) {
            playerViewController.player?.play()
        }
    }
}
```

This code uses `PHImageManager` to request a player item for the video asset. It then creates an instance of AVPlayer and AVPlayerViewController to play the video. Finally, it presents the player view controller and starts playing the video.

## Conclusion

In this tutorial, we learned how to use AVPlayer along with PhotoKit to play videos from the user's photo library in a Swift application. We covered the steps to request authorization, fetch videos, and play them using AVPlayer. You can now implement video playback functionality in your own iOS apps. Happy coding!

---

**References:**
- [Apple Developer Documentation - PhotoKit](https://developer.apple.com/documentation/photokit)
- [Apple Developer Documentation - AVFoundation](https://developer.apple.com/documentation/avfoundation)
- [Ray Wenderlich - PhotoKit Tutorial](https://www.raywenderlich.com/10317670-photo-kit-tutorial-for-ios-getting-started)
- [Swift by Sundell - Playing videos in Swift with AVKit](https://www.swiftbysundell.com/articles/playing-videos-in-swift-with-avkit/)