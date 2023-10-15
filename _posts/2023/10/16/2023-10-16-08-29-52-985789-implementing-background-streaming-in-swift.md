---
layout: post
title: "Implementing background streaming in Swift"
description: " "
date: 2023-10-16
tags: [selector, Streaming]
comments: true
share: true
---

In today's fast-paced world, users expect applications to be able to stream audio or video content, even when the app is running in the background. In this blog post, we will explore how to implement background streaming in Swift, allowing your app to continue playing media seamlessly even when the user switches to other apps or locks their device.

## Table of Contents

- [Introduction](#introduction)
- [Enabling Background Audio](#enabling-background-audio)
- [Handling Interruptions](#handling-interruptions)
- [Background Audio Configuration](#background-audio-configuration)
- [Conclusion](#conclusion)

## Introduction

By default, iOS suspends any media playback when the app is no longer in the foreground, therefore we need to enable background audio capabilities in our app. 

## Enabling Background Audio

The first step is to enable the **Audio, AirPlay, and Picture in Picture** capability in your Xcode project.

1. Open your Xcode project and navigate to the project settings.
2. Select your app target and go to the **Signing & Capabilities** tab.
3. Click the **+ Capability** button and choose **Audio, AirPlay, and Picture in Picture**.

Once background audio capability is enabled, we can proceed with implementing the necessary code to support streaming in the background.

```swift
import AVFoundation

func setupAudioSession() {
    do {
        try AVAudioSession.sharedInstance().setCategory(.playback, mode: .default, options: [.mixWithOthers, .allowAirPlay])
        try AVAudioSession.sharedInstance().setActive(true)
    } catch {
        print("Failed to activate audio session:", error)
    }
}

func startStreaming() {
    let url = URL(string: "https://example.com/stream.mp3")!
    let playerItem = AVPlayerItem(url: url)
    let player = AVPlayer(playerItem: playerItem)

    player.play()
}

setupAudioSession()
startStreaming()
```

In the code snippet above, we first set up the AVAudioSession to allow audio playback and enable mixing with other audio sources such as Siri or system sounds. We then create an AVPlayer and start playing the streaming content.

## Handling Interruptions

Interruptions such as phone calls or Siri activation can impact background streaming. To handle interruptions gracefully and ensure a smooth user experience, we can implement the `AVAudioPlayerDelegate` methods.

```swift
import AVFoundation

class AudioController: NSObject, AVAudioPlayerDelegate {
    var audioPlayer: AVAudioPlayer?

    func setupAudioSession() {
        // ...

        NotificationCenter.default.addObserver(self, selector: #selector(interruptionNotification(_:)), name: AVAudioSession.interruptionNotification, object: nil)
    }

    @objc func interruptionNotification(_ notification: Notification) {
        guard let userInfo = notification.userInfo,
              let interruptionTypeRawValue = userInfo[AVAudioSessionInterruptionTypeKey] as? UInt,
              let interruptionType = AVAudioSession.InterruptionType(rawValue: interruptionTypeRawValue)
        else { return }

        switch interruptionType {
        case .began:
            // Handle interruption began
            break
        case .ended:
            // Handle interruption ended
            break
        }

        guard let optionsRawValue = userInfo[AVAudioSessionInterruptionOptionKey] as? UInt else { return }

        let options = AVAudioSession.InterruptionOptions(rawValue: optionsRawValue)
        if options.contains(.shouldResume) {
            // Resume playback if needed
        }
    }

    // AVAudioPlayerDelegate methods
    func audioPlayerDidFinishPlaying(_ player: AVAudioPlayer, successfully flag: Bool) {
        // Handle playback finished
    }

    // ...
}

let audioController = AudioController()
audioController.setupAudioSession()
audioController.startStreaming()
```

In the above code snippet, we have a custom `AudioController` class that conforms to `AVAudioPlayerDelegate`. We set up the AVAudioSession and register for interruption notifications. When an interruption begins, we can pause or stop playback, and when the interruption ends, we can resume playback if needed. The delegate method `audioPlayerDidFinishPlaying` is called when the playback finishes.

## Background Audio Configuration

To optimize for background audio streaming, you may need to configure your app's **Info.plist** file.

1. Open your **Info.plist** file.
2. Add a new entry with the key `UIBackgroundModes`.
3. Expand the newly added item and add a new string entry.
4. Set the string entry value to `audio`.

This configuration ensures that your app can continue streaming audio in the background without being suspended by the operating system.

## Conclusion

By following the steps outlined in this blog post, you can enable background streaming in your Swift app. Users will appreciate the ability to continue listening to audio or watching videos seamlessly, enhancing their overall experience with your application.

Make sure to explore the official [Apple documentation](https://developer.apple.com/documentation/avfoundation/media_playback_and_selection) for more details and advanced features related to AVFoundation and background audio in Swift.

Remember to properly handle interruptions and configure your background audio settings to ensure a seamless experience for your users.

#hashtags: #Swift #Streaming