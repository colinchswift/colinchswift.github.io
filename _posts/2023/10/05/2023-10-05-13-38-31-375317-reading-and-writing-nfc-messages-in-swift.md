---
layout: post
title: "Reading and writing NFC messages in Swift"
description: " "
date: 2023-10-05
tags: []
comments: true
share: true
---

Although Near Field Communication (NFC) technology has been around for a while, it has gained popularity with the rise of contactless payment methods and smart devices. With an NFC-enabled device, you can read and write data to NFC tags or exchange information between devices.

In this tutorial, we will explore how to read and write NFC messages in Swift using Core NFC framework.

## Table of Contents

* [Prerequisites](#prerequisites)
* [Reading NFC Messages](#reading-nfc-messages)
* [Writing NFC Messages](#writing-nfc-messages)
* [Conclusion](#conclusion)

## Prerequisites
Before getting started, ensure that you have the following:

1. A device with NFC capabilities (iPhone 7 or newer).
2. Xcode 11 or later.
3. iOS 11 or later.

## Reading NFC Messages

To read an NFC message, you will need to enable the "Near Field Communication Tag Reading" capability in your app's target setting.

To start reading NFC messages, add the `CoreNFC` framework to your project and import it in the file you will be working on.

```swift
import CoreNFC
```

Next, implement the `NFCTagReaderSessionDelegate` protocol in your view controller to handle the NFC tag reading session.

```swift
class ViewController: UIViewController, NFCTagReaderSessionDelegate {

    // ...

}
```

Create an instance of `NFCTagReaderSession` and start a reader session.

```swift
let session = NFCTagReaderSession(pollingOption: .iso14443, delegate: self)
session.begin()
```

Implement the `tagReaderSession(_:didDetect tags:)` delegate method to receive a tag object when it is detected.

```swift
func tagReaderSession(_ session: NFCTagReaderSession, didDetect tags: [NFCTag]) {
    if let tag = tags.first {
        session.connect(to: tag) { (error: Error?) in
            guard error == nil else {
                session.invalidate(errorMessage: "Connection Error. Please try again.")
                return
            }
            
            // Read tag data here
            
            session.invalidate()
        }
    } else {
        session.invalidate(errorMessage: "No compatible tags found.")
    }
}
```

Inside the `connect(to:completionHandler:)` closure, you can use the appropriate protocols (`NFCMiFareTag`, `NFCNDEFTag`, etc.) to read data from the tag.

## Writing NFC Messages

Writing NFC messages works similarly to reading NFC messages. Enable the "Near Field Communication Tag Writing" capability in your app's target settings and import the `CoreNFC` framework.

Implement the `NFCNDEFReaderSessionDelegate` protocol in your view controller.

```swift
class ViewController: UIViewController, NFCNDEFReaderSessionDelegate {

    // ...

}
```

Create an instance of `NFCNDEFWriterSession` and start a writer session.

```swift
let session = NFCNDEFWriterSession(delegate: self, queue: nil, invalidateAfterFirstWrite: true)
session.begin()
```

Implement the `readerSession(_:didDetectNDEFs:)` delegate method to handle the detected NFC tag and write the desired message.

```swift
func readerSession(_ session: NFCNDEFReaderSession, didDetectNDEFs messages: [NFCNDEFMessage]) {
    guard let message = messages.first else {
        session.invalidate(errorMessage: "No NDEF messages found.")
        return
    }
    
    guard let urlRecord = message.records.first as? NFCNDEFPayload,
          urlRecord.typeNameFormat == .absoluteURI,
          let url = URL(string: String(data: urlRecord.payload, encoding: .utf8))
    else {
        session.invalidate(errorMessage: "Invalid NDEF message.")
        return
    }
    
    // Write to the tag here
    
    session.invalidate()
}
```

Within the closure, you can use the appropriate protocols (`NFCMiFareTag`, `NFCNDEFTag`, etc.) to write data to the tag.

## Conclusion

In this tutorial, we have explored how to read and write NFC messages using Swift and the Core NFC framework. NFC technology opens up new possibilities for contactless communication and interaction with a wide range of devices. By incorporating NFC functionality into your app, you can enhance user experiences and enable seamless data transfer.

Now that you have a basic understanding of NFC message reading and writing, you can start experimenting further and incorporating NFC capabilities into your own apps. Happy coding!

#iOS #Swift