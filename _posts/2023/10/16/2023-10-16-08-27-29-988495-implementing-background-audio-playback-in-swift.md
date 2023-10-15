---
layout: post
title: "Implementing background audio playback in Swift"
description: " "
date: 2023-10-16
tags: [selector]
comments: true
share: true
---

As an iOS developer, you may come across scenarios where you need to implement background audio playback in your app. This can be useful for apps that play music or podcasts, allowing the audio to continue playing even when the app is in the background or the device is locked.

In this tutorial, we will walk through the steps to implement background audio playback in Swift using AVFoundation framework.

## Table of Contents:
1. [Prerequisites](#prerequisites)
2. [Setting Up the Project](#setting-up-the-project)
3. [Configuring Audio Session](#configuring-audio-session)
4. [Handling Interruptions](#handling-interruptions)
5. [Background Audio Playback](#background-audio-playback)
6. [Conclusion](#conclusion)

## Prerequisites
To follow along with this tutorial, you should have a basic understanding of iOS development and Swift programming language. Additionally, make sure you have Xcode installed on your machine.

## Setting Up the Project
1. Create a new iOS project in Xcode.
2. Import the AVFoundation framework by adding the following line at the top of your ViewController.swift file:
```
import AVFoundation
```
3. In your project's Info.plist file, add the key `UIBackgroundModes` with a value of `audio`.

## Configuring Audio Session
To enable background audio playback, we need to configure the audio session appropriately. This is done in the `viewDidLoad` method of your ViewController.

Add the following code:
```swift
override func viewDidLoad() {
    super.viewDidLoad()
    
    let audioSession = AVAudioSession.sharedInstance()
    
    do {
        try audioSession.setCategory(.playback, mode: .default, options: [])
        try audioSession.setActive(true)
    } catch {
        print("Failed to configure audio session: \(error.localizedDescription)")
    }
}
```

In the above code, we are setting the audio session category to `.playback` which allows audio playback even when the app is in the background. We also activate the audio session.

## Handling Interruptions
When an audio interruption occurs, such as an incoming call, we need to handle it gracefully. This can be done by implementing the interruption handler in your ViewController.

Add the following code:
```swift
// Add this outside the viewDidLoad method
override func viewWillAppear(_ animated: Bool) {
    super.viewWillAppear(animated)
    
    NotificationCenter.default.addObserver(self, selector: #selector(handleInterruption(_:)), name: AVAudioSession.interruptionNotification, object: nil)
}

@objc func handleInterruption(_ notification: Notification) {
    guard let userInfo = notification.userInfo,
          let interruptionType = userInfo[AVAudioSessionInterruptionTypeKey] as? UInt,
          let type = AVAudioSession.InterruptionType(rawValue: interruptionType) else {
        return
    }
    
    switch type {
    case .began:
        // Handle interruption began
        break
    case .ended:
        guard let optionsValue = userInfo[AVAudioSessionInterruptionOptionKey] as? UInt else {
            return
        }
        let options = AVAudioSession.InterruptionOptions(rawValue: optionsValue)
        if options.contains(.shouldResume) {
            // Handle interruption ended and audio should resume
        } else {
            // Handle interruption ended and audio should not resume
        }
    }
}
```

In the code above, we register for the `AVAudioSession.interruptionNotification` and handle interruptions using the `handleInterruption` method. This allows us to pause and resume audio playback as needed.

## Background Audio Playback
To enable background audio playback, we can make use of the `AVPlayer` class from the AVFoundation framework.

Add the following code to play audio in the background:
```swift
// Add this outside the viewDidLoad method
var audioPlayer: AVPlayer?

func playBackgroundAudio() {
    let audioURL = URL(fileURLWithPath: Bundle.main.path(forResource: "background_music", ofType: "mp3")!)
    audioPlayer = AVPlayer(url: audioURL)
    audioPlayer?.play()
}
```
In the code above, `playBackgroundAudio` method is used to play an audio file named `background_music.mp3`. Modify this code to load and play your desired audio file.

## Conclusion
By following the steps mentioned in this tutorial, you have successfully implemented background audio playback in your Swift app. Users will now be able to enjoy uninterrupted audio even when the app is in the background or the device is locked.

Remember to test your app thoroughly to ensure that it works as expected. Happy coding!