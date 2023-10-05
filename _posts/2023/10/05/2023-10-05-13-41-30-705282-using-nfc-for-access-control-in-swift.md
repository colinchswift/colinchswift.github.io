---
layout: post
title: "Using NFC for access control in Swift"
description: " "
date: 2023-10-05
tags: []
comments: true
share: true
---

With the increasing use of mobile devices, Near Field Communication (NFC) has gained popularity as a convenient technology for contactless communication. NFC allows devices to exchange data over short distances, making it perfect for implementing access control systems.

In this blog post, we will explore how to use NFC for access control in Swift, one of the most popular programming languages for iOS app development.

## Table of Contents

1. [What is NFC](#what-is-nfc)
2. [Setting up the Project](#setting-up-the-project)
3. [Reading NFC Tags](#reading-nfc-tags)
4. [Writing NFC Tags](#writing-nfc-tags)
5. [Conclusion](#conclusion)

## What is NFC

NFC is a wireless technology that enables devices to communicate by simply bringing them close together. It operates on the same frequency range as contactless payment systems and is widely supported on modern smartphones, including iPhones.

NFC tags, also known as smart tags, are small passive devices that can store information, such as text or URLs. When an NFC-enabled phone comes into contact with an NFC tag, it can read or write data to the tag.

## Setting up the Project

To begin, create a new Xcode project or open an existing one. Make sure your project supports NFC by enabling the "Near Field Communication Tag Reading" capability in your app's settings.

Next, import the Core NFC framework into your project by adding the following statement at the top of your Swift file:

```swift
import CoreNFC
```

## Reading NFC Tags

To read NFC tags, you need to implement the `NFCTagReaderSessionDelegate` protocol. This protocol provides callbacks for handling NFC tag sessions.

First, create an instance of `NFCTagReaderSession` and assign the delegate:

```swift
class NFCTagReaderDelegate: NSObject, NFCTagReaderSessionDelegate {
  
  func tagReaderSession(_ session: NFCTagReaderSession, didDetect tags: [NFCTag]) {
    // Handle detected tags
  }
  
  func tagReaderSession(_ session: NFCTagReaderSession, didInvalidateWithError error: Error) {
    // Handle session error
  }
  
}

let tagReaderSession = NFCTagReaderSession(pollingOption: .iso14443, delegate: NFCTagReaderDelegate(), queue: nil)
```

Next, implement the delegate method `tagReaderSession(_:didDetect:)` to handle detected NFC tags. You can extract the data from the tag and perform access control logic, such as comparing it against a database of authorized users:

```swift
func tagReaderSession(_ session: NFCTagReaderSession, didDetect tags: [NFCTag]) {
  guard let tag = tags.first else { return }
  
  session.connect(to: tag) { (error: Error?) in
    if error != nil {
      session.invalidate(errorMessage: "Connection error. Please try again.")
      return
    }
    
    switch tag {
    case let .iso7816(tag):
      let apdu = NFCISO7816APDU(instructionClass: 0x00, instructionCode: 0xB0, p1Parameter: 0x00, p2Parameter: 0x00, data: Data(), expectedResponseLength: 16)
      
      tag.sendCommand(apdu: apdu) { (response: Data, sw1: UInt8, sw2: UInt8, error: Error?) in
        if error != nil {
          session.invalidate(errorMessage: "Error reading NFC tag.")
        } else {
          let responseData = response.prefix(Int(sw1) * 256 + Int(sw2))
          // Process the response data here
        }
      }
      
    default:
      session.invalidate(errorMessage: "Invalid tag type. Please try again with a different tag.")
    }
  }
}
```

## Writing NFC Tags

To write data to an NFC tag, you need to implement the `NFCNDEFPayload` protocol. This protocol represents a payload for an NFC Data Exchange Format (NDEF) record.

First, create an instance of `NFCNDEFMessage` with the desired payload:

```swift
let message = NFCNDEFMessage(records: [])
let payload = NFCNDEFPayload(format: .nfcWellKnown, type: Data(), identifier: Data(), payload: Data())
message.records.append(payload)
```

Next, create an instance of `NFCNDEFWriterSession` and assign the delegate:

```swift
class NFCNDEFWriterDelegate: NSObject, NFCNDEFWriterSessionDelegate {
  
  func writerSession(_ session: NFCNDEFWriterSession, didWriteNDEFs messages: [NFCNDEFMessage]) {
    // Handle successful writing
  }
  
  func writerSession(_ session: NFCNDEFWriterSession, didInvalidateWithError error: Error) {
    // Handle session error
  }
  
}

let writerSession = NFCNDEFWriterSession(delegate: NFCNDEFWriterDelegate(), queue: nil)
```

Implement the delegate method `writerSession(_:didWriteNDEFs:)` to handle successful writing:

```swift
func writerSession(_ session: NFCNDEFWriterSession, didWriteNDEFs messages: [NFCNDEFMessage]) {
  // Handle successful writing
  session.invalidate()
}
```

To initiate the writing process, call the `begin()` method on the writer session:

```swift
let message = makeNDEFMessage()
writerSession.connect(to: tag) { (error: Error?) in
  if error != nil {
    session.invalidate(errorMessage: "Connection error. Please try again.")
    return
  }
  
  writerSession.begin()
}
```

## Conclusion

In this blog post, we explored how to use NFC for access control in Swift. We learned how to read NFC tags and perform access control logic, as well as how to write data to NFC tags.

NFC technology opens up a world of possibilities for secure and convenient access control systems. By leveraging the power of Swift, we can easily develop robust applications that harness the capabilities of NFC.

#technology #Swift