---
layout: post
title: "Core NFC in Swift: reading and writing NFC tags"
description: " "
date: 2023-10-01
tags: []
comments: true
share: true
---

NFC (Near Field Communication) is a wireless communication technology that allows devices to exchange data by touching or being in close proximity. With the introduction of Core NFC in iOS 11, developers can now leverage NFC capabilities in their Swift applications.

In this blog post, we will explore how to use Core NFC in Swift for reading and writing NFC tags.

## Reading NFC Tags

To read an NFC tag in your Swift application, follow these steps:

1. Import the Core NFC framework into your project.

```swift
import CoreNFC
```

2. Conform to the `NFCTagReaderSessionDelegate` protocol to handle NFC tag reading events.

```swift
class ViewController: UIViewController, NFCTagReaderSessionDelegate {
    // ...
}
```

3. Create an instance of `NFCTagReaderSession` and start the session.

```swift
let session = NFCTagReaderSession(pollingOption: .iso14443, delegate: self, queue: nil)
session?.begin()
```

4. Implement the delegate methods to handle successful tag detection and data retrieval.

```swift
func tagReaderSession(_ session: NFCTagReaderSession, didDetect tags: [NFCTag]) {
    guard let tag = tags.first else {
        session.invalidate(errorMessage: "No NFC tag found")
        return
    }
    
    session.connect(to: tag) { (error: Error?) in
        if error != nil {
            session.invalidate(errorMessage: "Error connecting to NFC tag")
            return
        }
        
        if case let .iso7816(tag) = tag {
            tag.sendCommand(commandPacket: Data()) { (response: Data, error: Error?) in
                if error != nil {
                    session.invalidate(errorMessage: "Error reading NFC tag data")
                    return
                }
                
                // Process the received data from the NFC tag
                // ...
                
                session.alertMessage = "NFC tag read successfully"
                session.invalidate()
            }
        }
    }
}

func tagReaderSession(_ session: NFCTagReaderSession, didInvalidateWithError error: Error) {
    // Handle session invalidation here
}
```

## Writing NFC Tags

To write data to an NFC tag in your Swift application, follow these steps:

1. Import the Core NFC framework into your project.

```swift
import CoreNFC
```

2. Conform to the `NFCNDEFReaderSessionDelegate` protocol to handle NFC tag writing events.

```swift
class ViewController: UIViewController, NFCNDEFReaderSessionDelegate {
    // ...
}
```

3. Create an instance of `NFCNDEFReaderSession` and start the session.

```swift
let session = NFCNDEFReaderSession(delegate: self, queue: nil, invalidateAfterFirstRead: false)
session?.begin()
```

4. Implement the delegate methods to handle successful tag detection and data writing.

```swift
func readerSession(_ session: NFCNDEFReaderSession, didDetectNDEFs messages: [NFCNDEFMessage]) {
    guard let firstMessage = messages.first else {
        session.invalidate(errorMessage: "No NDEF data found")
        return
    }

    // Create a new NDEF record
    let payload = "Hello, NFC!".data(using: .utf8)
    let message = NFCNDEFMessage(records: [NFCNDEFPayload(format: .wellKnown, type: "T".data(using: .utf8)!, identifier: Data(), payload: payload!)])
    
    // Write the new message to the NFC tag
    session.connect(to: firstMessage.tag) { (error: Error?) in
        if error != nil {
            session.invalidate(errorMessage: "Error connecting to NFC tag")
            return
        }
        
        session.write(message) { (error: Error?) in
            if error != nil {
                session.invalidate(errorMessage: "Error writing NDEF data")
                return
            }
            
            session.alertMessage = "NFC tag write successful"
            session.invalidate()
        }
    }
}

func readerSession(_ session: NFCNDEFReaderSession, didInvalidateWithError error: Error) {
    // Handle session invalidation here
}
```

With these steps, you can now read and write NFC tags in your Swift application using Core NFC. NFC enables seamless interactions between devices and opens up a wide range of possibilities for applications such as contactless payments, ticketing systems, and more.

#iOS #Swift-NFC