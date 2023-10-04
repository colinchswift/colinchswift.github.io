---
layout: post
title: "Playback and recording audio in Swift Core Audio"
description: " "
date: 2023-10-02
tags: [CoreAudio]
comments: true
share: true
---

Core Audio is a powerful framework in Swift that allows you to work with audio on iOS and macOS devices. Whether you want to play audio files, stream audio from a microphone, or record audio, Core Audio provides the necessary tools to accomplish these tasks.

In this blog post, we will explore how to use Core Audio to play and record audio in a Swift application.

## Playing Audio using Core Audio
To play audio using Core Audio, we need to use the `AudioQueue` API. First, we need to set up an audio queue and add audio buffers to it. Then, we can start the audio queue to begin playback.

```swift
import AVFoundation

class AudioPlayer {
    var audioQueue: AudioQueueRef?
    
    init(audioFileURL: URL) {
        AVAudioSession.sharedInstance().setCategory(.playAndRecord)
        AudioQueueNewOutput(&streamFormat, AudioPlayerCallback, UnsafeMutableRawPointer(Unmanaged.passUnretained(self).toOpaque()), nil, nil, 0, &audioQueue)
        
        // Set up audio file
        var audioFileID: AudioFileID?
        guard AudioFileOpenURL(audioFileURL as CFURL, .readPermission, 0, &audioFileID) == noErr else {
            // Handle error
        }
        
        // Set up audio buffers
        var audioBuffers: [AudioQueueBufferRef?] = []
        for _ in 0..<3 { // Number of audio buffers to use
            var buffer: AudioQueueBufferRef?
            AudioQueueAllocateBuffer(audioQueue!, bufferSize, &buffer)
            audioBuffers.append(buffer)
            
            // Read audio data from file into the buffer
            guard AudioFileReadPacketData(audioFileID!, false, nil, nil, nil, &bufferSize, buffer!.pointee.mAudioData) == noErr else {
                // Handle error
            }
        }
        
        // Enqueue buffers in the audio queue
        for i in 0..<audioBuffers.count {
            AudioQueueEnqueueBuffer(audioQueue!, audioBuffers[i]!, 0, nil)
        }
        
        // Start the audio queue
        AudioQueueStart(audioQueue!, nil)
    }
    
    // Audio player callback function
    let AudioPlayerCallback: AudioQueueOutputCallback = { userData, audioQueue, buffer in
        let player = Unmanaged<AudioPlayer>.fromOpaque(userData!).takeUnretainedValue()
        player.handlePlayback(buffer: buffer)
    }
    
    func handlePlayback(buffer: AudioQueueBufferRef) {
        // Perform any necessary audio processing
        
        // Enqueue the buffer back in the audio queue for continuous playback
        AudioQueueEnqueueBuffer(audioQueue!, buffer, 0, nil)
    }
}
```

In the above code, we create an `AudioPlayer` class that handles the playback of audio. We set up the audio format, create an audio queue, and allocate audio buffers from the queue. Then, we read audio data from a file into the buffers and enqueue them in the audio queue. Finally, we start the audio queue, and the `handlePlayback()` function is called whenever a buffer is ready for processing.

## Recording Audio using Core Audio
To record audio using Core Audio, we utilize the `AudioQueue` API as well. We need to set up an audio queue for recording and provide audio buffers to capture the recorded audio.

```swift
import AudioToolbox

class AudioRecorder {
    var audioQueue: AudioQueueRef?
    
    init() {
        AVAudioSession.sharedInstance().setCategory(.record)
        AudioQueueNewInput(&streamFormat, AudioRecorderCallback, UnsafeMutableRawPointer(Unmanaged.passUnretained(self).toOpaque()), nil, nil, 0, &audioQueue)
        
        // Set up audio buffers
        var audioBuffers: [AudioQueueBufferRef?] = []
        for _ in 0..<3 { // Number of audio buffers to use
            var buffer: AudioQueueBufferRef?
            AudioQueueAllocateBuffer(audioQueue!, bufferSize, &buffer)
            audioBuffers.append(buffer)
        }
        
        // Enqueue buffers in the audio queue
        for i in 0..<audioBuffers.count {
            AudioQueueEnqueueBuffer(audioQueue!, audioBuffers[i]!, 0, nil)
        }
        
        // Start the audio queue
        AudioQueueStart(audioQueue!, nil)
    }
    
    // Audio recorder callback function
    let AudioRecorderCallback: AudioQueueInputCallback = { userData, audioQueue, buffer, startTime, numPackets, packetDescriptions in
        let recorder = Unmanaged<AudioRecorder>.fromOpaque(userData!).takeUnretainedValue()
        recorder.handleRecording(buffer: buffer, numPackets: numPackets, packetDescriptions: packetDescriptions)
    }
    
    func handleRecording(buffer: AudioQueueBufferRef, numPackets: UInt32, packetDescriptions: UnsafePointer<AudioStreamPacketDescription>?) {
        // Perform any necessary audio processing or save the recorded audio
        
        // Re-enqueue the buffer back in the audio queue for continuous recording
        AudioQueueEnqueueBuffer(audioQueue!, buffer, 0, nil)
    }
}
```

In the above code, we create an `AudioRecorder` class responsible for recording audio. We set up the audio format, create an audio queue for recording, and allocate audio buffers from the queue. Then, we start the audio queue, and the `handleRecording()` function is called whenever a buffer captures audio data.

## Conclusion
Core Audio provides a robust set of APIs for playback and recording audio in Swift. By utilizing the `AudioQueue` API, you can create audio players and recorders to bring audio capabilities to your applications. Whether you want to play background music, create a sound effect player, or implement voice recording, Core Audio will empower you to achieve your audio goals.

#Swift #CoreAudio