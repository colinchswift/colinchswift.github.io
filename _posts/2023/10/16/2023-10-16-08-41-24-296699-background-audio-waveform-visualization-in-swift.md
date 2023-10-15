---
layout: post
title: "Background audio waveform visualization in Swift"
description: " "
date: 2023-10-16
tags: []
comments: true
share: true
---

When building a music player or any audio-related app, it is often helpful to visualize the audio waveform to give users a better understanding of the audio playback. In this blog post, we will explore how to achieve background audio waveform visualization in Swift.

## Table of Contents
- [Introduction](#introduction)
- [Approach](#approach)
- [Implementing Background Audio Playback](#implementing-background-audio-playback)
- [Generating the Audio Waveform](#generating-the-audio-waveform)
- [Displaying the Audio Waveform](#displaying-the-audio-waveform)
- [Conclusion](#conclusion)

## Introduction

Visualizing audio waveforms is a common way to represent the amplitude of an audio signal over time. It allows users to see the changes in music or audio files and helps them navigate through the audio content. We will create a background audio player with a waveform visualization in our iOS app.

## Approach

1. Implement audio playback using AVFoundation framework.
2. Generate the audio waveform data from the audio file.
3. Use the generated waveform data to display a visual representation of the audio waveform.

## Implementing Background Audio Playback

To implement background audio playback, we will use the AVFoundation framework in Swift. The AVFoundation framework provides classes for playing audio and video media. It supports background audio playback by using the AVAudioSession category `AVAudioSessionCategoryPlayback`.

```swift
import AVFoundation

// Configure the audio session for background playback
let audioSession = AVAudioSession.sharedInstance()
try? audioSession.setCategory(AVAudioSessionCategoryPlayback)
try? audioSession.setActive(true)

// Initialize and configure the audio player
let audioURL = URL(fileURLWithPath: "path_to_audio_file")
let audioPlayer = try? AVAudioPlayer(contentsOf: audioURL)
audioPlayer?.prepareToPlay()
audioPlayer?.play()
```

## Generating the Audio Waveform

To generate the audio waveform, we need to extract the audio data from the audio file and calculate its amplitude over time. We can accomplish this by using the `AVAssetReader` and `AVAssetReaderTrackOutput` classes from the AVFoundation framework.

```swift
import AVFoundation

func generateWaveform(from audioURL: URL, completion: @escaping ([Float]) -> Void) {
    let asset = AVURLAsset(url: audioURL)
    let track = asset.tracks(withMediaType: .audio).first
    
    let reader = try? AVAssetReader(asset: asset)
    let output = AVAssetReaderTrackOutput(track: track!, outputSettings: nil)
    reader?.add(output)
    
    var samples: [Float] = []
    
    while reader?.status == .reading {
        if let sampleBuffer = output.copyNextSampleBuffer() {
            guard let blockBuffer = CMSampleBufferGetDataBuffer(sampleBuffer) else { continue }
            
            var lengthAtOffset = 0
            var totalLength = 0
            var dataPointer: UnsafeMutablePointer<Int8>? = nil
            
            if CMBlockBufferGetDataPointer(blockBuffer, atOffset: 0, lengthAtOffsetOut: &lengthAtOffset, totalLengthOut: &totalLength, dataPointerOut: &dataPointer) != noErr {
                continue
            }
            
            let samplesCount = totalLength / MemoryLayout<Int16>.size
            let audioData = UnsafeMutablePointer<Int16>(OpaquePointer(dataPointer))
            
            for i in 0..<samplesCount {
                let sample = Float(audioData[i]) / Float(Int16.max)
                samples.append(sample)
            }
            
            CMSampleBufferInvalidate(sampleBuffer)
            CMSampleBufferSetDataBufferFromAudioBufferList(sampleBuffer, blockBufferAllocator: nil, bufferList: nil, bufferListSize: 0, blockBufferMemoryAllocator: nil, blockBuffer: blockBuffer, blockBufferOffset: 0, blockBufferLength: totalLength, flags: 0)
        }
    }
    
    completion(samples)
}
```

## Displaying the Audio Waveform

After generating the audio waveform data, we can use it to display a visual representation of the audio waveform. This can be achieved using various UI components such as `UIBezierPath`, `CAShapeLayer`, or custom views. Here is a simplified example using a `CAShapeLayer`.

```swift
import UIKit

func displayWaveform(on waveformView: UIView, waveform: [Float]) {
    let shapeLayer = CAShapeLayer()
    shapeLayer.strokeColor = UIColor.blue.cgColor
    shapeLayer.lineWidth = 2.0
    
    let waveformPath = UIBezierPath()
    waveformPath.move(to: CGPoint(x: 0, y: waveformView.bounds.height / 2))
    
    for (index, sample) in waveform.enumerated() {
        let x = CGFloat(index) * waveformView.bounds.width / CGFloat(waveform.count)
        let y = waveformView.bounds.height / 2 - CGFloat(sample) * waveformView.bounds.height / 2
        waveformPath.addLine(to: CGPoint(x: x, y: y))
    }
    
    shapeLayer.path = waveformPath.cgPath
    waveformView.layer.addSublayer(shapeLayer)
}
```

To use the `displayWaveform` function, simply call it after generating the waveform data:

```swift
generateWaveform(from: audioURL) { waveform in
    displayWaveform(on: waveformView, waveform: waveform)
}
```

## Conclusion

In this blog post, we explored how to implement background audio waveform visualization in Swift. By utilizing the AVFoundation framework, we were able to play audio in the background and generate the audio waveform data. We also learned how to use the generated waveform data to create a visual representation of the audio waveform. This can be a useful feature to enhance the audio playback experience in music players or any audio-related app.