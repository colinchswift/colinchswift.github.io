---
layout: post
title: "Implementing audio panning using AudioPanner in Swift Core Audio"
description: " "
date: 2023-10-02
tags: [coreaudio]
comments: true
share: true
---

Audio panning is a technique used in audio production to create a sense of space and movement. In simple terms, it allows you to position the audio signal in the stereo field, making it sound as if it's coming from a specific direction.

In Swift, you can implement audio panning using the AudioPanner class from the Core Audio framework. Here's an example of how you can use it to pan an audio signal from left to right:

```swift
import AVFoundation
import CoreAudio

// Create an audio engine
let engine = AVAudioEngine()

// Create an audio player node
let playerNode = AVAudioPlayerNode()
engine.attach(playerNode)

// Connect the player node to the output
let outputMixer = engine.mainMixerNode
engine.connect(playerNode, to: outputMixer, format: nil)

// Create an audio panner
let pannerDesc = AudioUnitParameterID(kListenerObjectDistanceGainParameter)
let panner = AVAudioUnitPanner(audioComponentDescription: pannerDesc)
engine.attach(panner)

// Connect the panner to the player node
engine.connect(panner, to: playerNode, format: nil)

// Set the pan position
panner.pan = -1.0 // -1.0 for left, 0.0 for center, 1.0 for right

// Start the audio engine
do {
    try engine.start()
} catch {
    print("Failed to start audio engine: \(error)")
}

// File URL of the audio file to play
let fileURL = URL(fileURLWithPath: "path/to/audio/file.wav")

// Load the audio file
do {
    let audioFile = try AVAudioFile(forReading: fileURL)
    playerNode.scheduleFile(audioFile, at: nil)
    playerNode.play()
} catch {
    print("Failed to load audio file: \(error)")
}
```

In this example, we first create an audio engine and attach an audio player node to it. We then create an audio panner using the AVAudioUnitPanner class and attach it to the engine. Connect the panner to the player node, and set the pan position using the `pan` property. Finally, we start the audio engine and schedule and play the audio file.

Remember to replace `"path/to/audio/file.wav"` with the actual path to your audio file.

Using the AudioPanner class, you can easily implement audio panning in your Swift Core Audio projects. Experiment with different pan positions to create dynamic and immersive audio experiences.

#swift #coreaudio