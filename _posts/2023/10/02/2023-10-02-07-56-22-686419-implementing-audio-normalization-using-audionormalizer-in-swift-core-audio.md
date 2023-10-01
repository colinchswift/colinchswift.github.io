---
layout: post
title: "Implementing audio normalization using AudioNormalizer in Swift Core Audio"
description: " "
date: 2023-10-02
tags: [audio, normalization]
comments: true
share: true
---

Audio normalization is a technique used to adjust the volume levels of audio files to a consistent and optimal level. In this blog post, we will explore how to implement audio normalization in Swift using the AudioNormalizer library in the Core Audio framework.

## What is AudioNormalizer?

AudioNormalizer is a Swift library that provides an easy-to-use interface for normalizing audio files. It uses the Core Audio framework, a powerful audio processing framework provided by Apple. With AudioNormalizer, you can easily adjust the volume levels of your audio files to ensure consistency and enhance the listening experience.

## Getting Started

To get started, you'll need to add the AudioNormalizer library to your project. You can do this by adding the following line to your `Podfile`:

```swift
pod 'AudioNormalizer'
```

Once you have added the library, run `pod install` to install it and then open the `.xcworkspace` file.

## Audio Normalization Process

The process of audio normalization using AudioNormalizer involves the following steps:

1. Read the audio file and extract the audio data using Core Audio.
2. Analyze the audio data to determine the overall loudness and identify amplitude peaks.
3. Calculate the gain adjustment required to normalize the audio.
4. Apply the gain adjustment to the audio data.
5. Write the normalized audio data to a new audio file.

Let's see how we can implement these steps using AudioNormalizer in Swift.

```swift
import AudioToolbox
import AudioToolboxExt
import AudioNormalizer

// 1. Read the audio file and extract the audio data
guard let audioURL = Bundle.main.url(forResource: "sample", withExtension: "wav") else {
    fatalError("Audio file not found.")
}

let audioFile = try AudioFile(url: audioURL)
let audioData = try audioFile.readData()

// 2. Analyze the audio data to determine the overall loudness and identify amplitude peaks
let normalizer = try AudioNormalizer(data: audioData)
let loudness = try normalizer.calculateLoudness()
let peaks = try normalizer.calculatePeaks()

// 3. Calculate the gain adjustment required to normalize the audio
let targetLoudness: Float = -23 // Adjust to your desired target loudness
let gain = normalizer.calculateGain(targetLoudness: targetLoudness)

// 4. Apply the gain adjustment to the audio data
let normalizedData = normalizer.applyGain(gain)

// 5. Write the normalized audio data to a new audio file
let outputURL = try audioURL.deletingLastPathComponent().appendingPathComponent("normalized.wav")
try AudioFile.write(data: normalizedData, to: outputURL)
```

In the example code above, we first read the audio file and extract the audio data using the `AudioFile` class provided by Core Audio. Next, we create an instance of `AudioNormalizer` with the audio data. We then calculate the loudness and amplitude peaks of the audio using the `calculateLoudness` and `calculatePeaks` methods.

We specify a target loudness and calculate the gain adjustment required to normalize the audio using the `calculateGain` method. Once we have the gain adjustment value, we apply it to the audio data using the `applyGain` method.

Finally, we write the normalized audio data to a new audio file using the `write` method provided by `AudioFile`.

## Conclusion

By using the AudioNormalizer library in Swift, you can easily implement audio normalization in your applications. This allows you to adjust the volume levels of your audio files to ensure consistency and enhance the listening experience. With its simple API and integration with Core Audio, AudioNormalizer is a powerful tool for audio processing in Swift.

#audio #normalization