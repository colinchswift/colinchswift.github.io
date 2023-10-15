---
layout: post
title: "Implementing background music playback in Swift"
description: " "
date: 2023-10-16
tags: []
comments: true
share: true
---

In this tutorial, we will explore how to implement background music playback in a Swift iOS app. Background music can enhance the user experience by adding an element of immersion to the app. Whether you are building a game or a music streaming app, background music can bring your app to life.

### Prerequisites

To follow along with this tutorial, you should have a basic understanding of Swift and iOS development. You will also need Xcode installed on your machine.

### Adding audio file to your project

First, let's start by adding the audio file that we want to play in the background. Drag and drop the audio file into your Xcode project. Make sure that the file is added to the target, so it will be bundled with the app when you run it.

### Configuring audio playback

Next, we need to configure the audio session for our app. Open the AppDelegate.swift file and add the following code inside the `didFinishLaunchingWithOptions` method:

```swift
import AVFoundation

func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    do {
        try AVAudioSession.sharedInstance().setCategory(AVAudioSession.Category.playback, mode: .default)
        try AVAudioSession.sharedInstance().setActive(true)
    } catch {
        print("Failed to configure audio session")
    }
    
    return true
}
```

This code configures the audio session to allow playback and sets it as the active session. If any errors occur during the configuration, an error message will be printed to the console.

### Playing background music

To play the background music, create a new Swift file (e.g., `BackgroundMusicPlayer.swift`) and add the following code:

```swift
import AVFoundation

class BackgroundMusicPlayer {
    static let shared = BackgroundMusicPlayer()

    var audioPlayer: AVAudioPlayer?

    func playMusic(withName name: String, andExtension fileExtension: String) {
        guard let path = Bundle.main.path(forResource: name, ofType: fileExtension) else {
            print("Background music file not found")
            return
        }

        let url = URL(fileURLWithPath: path)
        do {
            audioPlayer = try AVAudioPlayer(contentsOf: url)
            audioPlayer?.numberOfLoops = -1
            audioPlayer?.volume = 0.5
            audioPlayer?.prepareToPlay()
            audioPlayer?.play()
        } catch {
            print("Failed to play background music")
        }
    }

    func stopMusic() {
        audioPlayer?.stop()
    }
}
```

The `BackgroundMusicPlayer` class provides methods to play and stop the background music. The `playMusic` method takes the name and file extension of the audio file as parameters. It loads the file and sets the necessary properties for continuous playback, volume, and prepares the player for playing. The `stopMusic` method stops the music playback.

### Using the background music player

To use the `BackgroundMusicPlayer`, call the `playMusic` method with the name of the audio file and its file extension. To stop the music, call the `stopMusic` method. For example, you can add the following code to the `viewDidLoad` method of your view controller:

```swift
override func viewDidLoad() {
    super.viewDidLoad()
    
    BackgroundMusicPlayer.shared.playMusic(withName: "backgroundMusic", andExtension: "mp3")
}
```

Remember to replace "backgroundMusic" with the name of your audio file.

### Conclusion

In this tutorial, we learned how to implement background music playback in a Swift iOS app. By configuring the audio session and utilizing the AVAudioPlayer, we can easily add background music to enhance the user experience. Experiment with different audio files and settings to create an immersive audio environment in your app.

### References

- [AVAudioSession - Apple Developer Documentation](https://developer.apple.com/documentation/avfoundation/avaudiosession)
- [AVAudioPlayer - Apple Developer Documentation](https://developer.apple.com/documentation/avfoundation/avaudioplayer)