---
layout: post
title: "Implementing video playback in ViewControllers in Swift"
description: " "
date: 2023-09-23
tags: [iPhoneAppDevelopment, SwiftProgramming]
comments: true
share: true
---

Are you looking to integrate video playback functionality in your iOS app built with Swift? Look no further! In this tutorial, we will explore how to implement video playback in ViewControllers using AVPlayer and AVPlayerViewController.

## Getting Started

Before we begin, let's make sure we have the necessary prerequisites set up.

### Prerequisites
- Xcode installed on your macOS system
- Basic knowledge of Swift and iOS development

## Step 1: Set Up the Project
1. Open Xcode and create a new iOS project. Choose the Single View App template.
2. Name your project and select Swift as the language.
3. Choose a location to save your project and click "Create".

## Step 2: Add a Video File to Your Project
1. Right-click on the project folder in the Project Navigator.
2. Select "Add Files to [Your Project Name]".
3. Choose the video file you want to use for playback in your app.
4. Make sure to check the option "Copy items if needed" and click "Add".

## Step 3: Import AVFoundation Framework
1. In the Project Navigator, select your project's target.
2. Click on "Build Phases".
3. Expand the "Link Binary With Libraries" section.
4. Click on the "+" button and search for "AVFoundation".
5. Select `AVFoundation.framework` and click "Add".

## Step 4: Add Video Playback Code
```swift
import AVKit

class VideoPlayerViewController: AVPlayerViewController {
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // Create a URL object for the video file
        guard let videoURL = Bundle.main.url(forResource: "exampleVideo", withExtension: "mp4") else {
            fatalError("Video file not found")
        }
        
        // Create an AVPlayer with the URL
        let player = AVPlayer(url: videoURL)
        
        // Set the AVPlayer as the player for the AVPlayerViewController
        self.player = player
        
        // Automatically play the video
        player.play()
    }
}
```
In this code snippet, we import the `AVKit` framework, subclass `AVPlayerViewController`, and override the `viewDidLoad` method. Inside `viewDidLoad`, we create a URL object for the video file and create an `AVPlayer` with the URL. We set the `AVPlayer` as the player for the `AVPlayerViewController` and automatically play the video.

## Step 5: Present the Video Player
To present the video player, open the `ViewController.swift` file and modify the code as follows:
```swift
import UIKit

class ViewController: UIViewController {
    @IBAction func playVideoButtonTapped(_ sender: UIButton) {
        // Initialize and present the video player VC
        let videoPlayerVC = VideoPlayerViewController()
        present(videoPlayerVC, animated: true, completion: nil)
    }
}
```
In this code snippet, we import `UIKit`, subclass `UIViewController`, and add an `IBAction` method for the play video button. Inside the `playVideoButtonTapped` method, we initialize the `VideoPlayerViewController` and present it modally.

## Step 6: Connect the Play Video Button
1. Open the Main.storyboard file.
2. Drag and drop a `UIButton` from the Object Library onto the view controller.
3. Customize the play video button's appearance and position.
4. Connect the play video button to the `ViewController` class by control-dragging from the button to the `ViewController` in the Document Outline.
5. Choose the `playVideoButtonTapped` method.

## Step 7: Run the App
Now is the moment of truth! Click the "Run" button in Xcode to build and run the app on the iOS Simulator or a physical device. Once the app launches, tap the play video button, and you should see the video playback in action.

That's it! You have successfully implemented video playback in ViewControllers using Swift. This basic implementation can serve as a starting point for more advanced video playback features in your app.

#iPhoneAppDevelopment #SwiftProgramming