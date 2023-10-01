---
layout: post
title: "Audio and video playback in Swift"
description: " "
date: 2023-10-01
tags: [Swift, AVFoundation]
comments: true
share: true
---

In this blog post, we will explore how to incorporate audio and video playback functionality into your iOS applications using Swift. With the help of the AVFoundation framework, you can easily integrate various media formats, such as mp3, WAV, mp4, and MOV, into your app. Let's get started!

## Setting Up the Project

First, create a new project in Xcode or open an existing one. Then, import the AVFoundation framework by adding the following line to your ViewController.swift file:

```swift
import AVFoundation
```

## Audio Playback

To play audio files in your app, you'll need to create an instance of `AVAudioPlayer`. Here's an example of how to play an audio file named "music.mp3" located in your project's bundle:

```swift
guard let url = Bundle.main.url(forResource: "music", withExtension: "mp3") else {
    print("Audio file not found.")
    return
}

do {
    let audioPlayer = try AVAudioPlayer(contentsOf: url)
    audioPlayer.play()
} catch {
    print("Error playing audio: \(error.localizedDescription)")
}
```

In the above code snippet, we retrieve the URL of the audio file using `Bundle.main.url(forResource:withExtension:)`. If the file is found, we create an instance of `AVAudioPlayer` with the file URL and call the `play()` method to start audio playback. Any errors encountered during the process are handled in the catch block.

## Video Playback

To play video files in your app, you'll use `AVPlayerViewController` to handle the playback UI. Add the following code to your ViewController.swift file to play a video named "video.mp4":

```swift
func playVideo() {
    guard let videoURL = Bundle.main.url(forResource: "video", withExtension: "mp4") else {
        print("Video file not found.")
        return
}
    
let player = AVPlayer(url: videoURL)
let playerViewController = AVPlayerViewController()
playerViewController.player = player

present(playerViewController, animated: true) {
    playerViewController.player?.play()
}
```

In the above code, we create an instance of `AVPlayer` with the video file's URL and assign it to `playerViewController`. The `AVPlayerViewController` is then presented modally, and the video playback automatically starts.

## Conclusion

In this blog post, we have covered the basics of audio and video playback in Swift using the AVFoundation framework. With these techniques, you can easily incorporate audio and video files into your iOS applications. There are many other advanced features you can explore to enhance your media playback experience, such as handling interruptions, seeking, and controlling the playback rate. We encourage you to dive deeper into the AVFoundation documentation to learn more. #Swift #AVFoundation