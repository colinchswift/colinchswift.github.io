---
layout: post
title: "Implementing NFC for public transportation in Swift"
description: " "
date: 2023-10-05
tags: []
comments: true
share: true
---

With the increasing popularity of contactless payment systems, many public transportation systems around the world have adopted NFC (Near Field Communication) technology to allow passengers to conveniently pay for their fares using their smartphones or smart cards. In this tutorial, we will explore how to implement NFC for public transportation in Swift, Apple's programming language.

## Table of Contents

- [What is NFC?](#what-is-nfc)
- [Requirements](#requirements)
- [Enabling NFC Capability](#enabling-nfc-capability)
- [Reading NFC Tags](#reading-nfc-tags)
- [Writing to NFC Tags](#writing-to-nfc-tags)
- [Conclusion](#conclusion)

## What is NFC?

NFC is a short-range wireless communication technology that allows devices to exchange information over a distance of a few centimeters. It enables contactless communication between devices, making it ideal for applications like mobile payments, access control, and public transportation ticketing.

## Requirements

To work with NFC in Swift, you'll need:
- An iOS device with NFC capabilities (iPhone 7 or newer)
- Xcode 11 or later
- An Apple Developer account

## Enabling NFC Capability

The first step is to enable NFC capability in your Xcode project. 

1. Open your project in Xcode.
2. Select your target and go to the "Signing & Capabilities" tab.
3. Click the "+" button to add a new capability.
4. Search for "Near Field Communication Tag Reading" and enable it.

This will add the necessary entitlements to your app and allow it to interact with NFC tags.

## Reading NFC Tags

To read data from an NFC tag, we'll use the `Core NFC` framework provided by Apple. 

1. Import the `CoreNFC` framework in your view controller:

```swift
import CoreNFC
```

2. Conform to the `NFCTagReaderSessionDelegate` protocol:

```swift
class MyViewController: UIViewController, NFCTagReaderSessionDelegate {
   // ...
}
```

3. Initialize an instance of `NFCTagReaderSession`:

```swift
let tagReaderSession = NFCTagReaderSession(pollingOption: .iso14443, delegate: self)
tagReaderSession.begin()
```

4. Implement the delegate methods to handle the tag detection events:

```swift
func tagReaderSession(_ session: NFCTagReaderSession, didDetect tags: [NFCTag]) {
   // Handle the detected tags here
}

func tagReaderSessionDidBecomeActive(_ session: NFCTagReaderSession) {
   // Session is active and ready to detect tags
}

func tagReaderSession(_ session: NFCTagReaderSession, didInvalidateWithError error: Error) {
   // Handle session errors here
}
```

The `didDetect` method will be called when a compatible NFC tag is detected. You can then read data from the tag using the `readNDEF` function.

## Writing to NFC Tags

To write data to an NFC tag, you'll need a compatible tag that supports writing. 

1. Request permission from the user to write to the NFC tag:

```swift
let tagWriterSession = NFCNDEFWriterSession(delegate: self, queue: nil, invalidateAfterFirstWrite: false)
tagWriterSession.begin()
```

2. Implement the delegate methods to handle the tag writing events:

```swift
func writerSession(_ session: NFCNDEFWriterSession, didDetect tags: [NFCNDEFTag]) {
   // Handle the detected tags here
}

func writerSessionDidBecomeActive(_ session: NFCNDEFWriterSession) {
   // Session is active and ready to write to tags
}

func writerSession(_ session: NFCNDEFWriterSession, didInvalidateWithError error: Error) {
   // Handle session errors here
}
```

The `didDetect` method will be called when a compatible writable NFC tag is detected. You can then write data to the tag using the `writeNDEF` function.

## Conclusion

Implementing NFC for public transportation in Swift allows users to conveniently pay for their fares using their smartphones or smart cards. In this tutorial, we covered the basics of reading and writing NFC tags in Swift using the Core NFC framework. You can now explore further possibilities and integrate NFC capabilities into your own iOS applications. Happy coding!

#hashtags: #NFC #Swift