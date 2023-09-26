---
layout: post
title: "Audio synthesis and MIDI in Swift Playgrounds"
description: " "
date: 2023-09-26
tags: [swift, audio]
comments: true
share: true
---

## Introduction

Swift Playgrounds is a powerful tool for learning and experimenting with Swift programming. While it is mainly used for iOS and macOS development, it can also be leveraged for audio synthesis and MIDI manipulation. In this blog post, we will explore how to create audio synthesis and generate MIDI signals using Swift Playgrounds.

## Setting up the Playground

1. Open Xcode and create a new Playground file.
2. Choose the macOS template.
3. Import the AudioToolbox framework by adding the following code at the top of the playground file:
```
import AudioToolbox
```

## Generating Audio Signals

To generate audio signals, we can make use of the `AudioComponentInstance` provided by the AudioToolbox framework. Here's a simple example of generating a sine wave:

```swift
// Create an AudioComponentInstance
var toneUnit: AudioComponentInstance? = nil
var status = noErr

let audioComponentDescription = AudioComponentDescription(
    componentType: kAudioUnitType_Generator,
    componentSubType: kAudioUnitSubType_MIDISynth,
    componentManufacturer: kAudioUnitManufacturer_Apple,
    componentFlags: 0,
    componentFlagsMask: 0
)

guard let audioComponent = AudioComponentFindNext(nil, &audioComponentDescription) else {
    fatalError("AudioComponent not found")
}

status = AudioComponentInstanceNew(audioComponent, &toneUnit)

// Set audio unit properties
let sampleRate: Float64 = 44100.0
status = AudioUnitSetProperty(toneUnit!,
                              kAudioUnitProperty_SampleRate,
                              kAudioUnitScope_Output,
                              0,
                              &sampleRate,
                              UInt32(MemoryLayout<Float64>.size))

// Start the audio unit
status = AudioUnitInitialize(toneUnit!)
status = AudioOutputUnitStart(toneUnit!)
```

## Generating MIDI Signals

To generate MIDI signals, we can use the `MIDIPacketList` and `MIDISend` functions from the CoreMIDI framework. Here's an example of sending a MIDI note-on message:

```swift
// Create a MIDI client
var client: MIDIClientRef = 0
var status = noErr
status = MIDIClientCreate("MIDI Client", nil, nil, &client)

// Create a MIDI output port
var outputPort: MIDIPortRef = 0
status = MIDIOutputPortCreate(client, "Output Port", &outputPort)

// Create a MIDI packet list
var packetList = MIDIPacketList()
var packet = MIDIPacketListInit(&packetList)

// Create a MIDI message
let noteOnMessage: [UInt8] = [0x90, 60, 127] // Note On message for note C4

// Add the MIDI message to the packet list
packet = MIDIPacketListAdd(&packetList, UInt32(MemoryLayout<MIDIPacketList>.size), packet, 0, UInt32(noteOnMessage.count), noteOnMessage)

// Send the MIDI packet list
status = MIDISend(outputPort, 0, &packetList)
```

## Conclusion

With Swift Playgrounds and the AudioToolbox and CoreMIDI frameworks, we can easily experiment with audio synthesis and MIDI generation. This allows us to create music and sound effects, and opens up opportunities for creative coding and interactive experiences. Swift's syntax makes it easy to work with audio and MIDI, making it an excellent choice for developers interested in music and audio applications.

#swift #audio #MIDI #synthesis