---
layout: post
title: "NFC-based ticketing solutions in Swift"
description: " "
date: 2023-10-05
tags: []
comments: true
share: true
---

## Introduction
With the advancement in mobile technology, NFC (Near Field Communication) has gained popularity as a convenient way to enable contactless transactions. NFC-based ticketing solutions have become widely adopted in various industries, including transportation, events, and access control systems. In this blog post, we will explore how to implement an NFC-based ticketing solution using Swift programming language.

## Prerequisites
To follow along with this tutorial, you will need:
- Xcode installed on your Mac
- An iPhone or iPad with NFC support running on iOS 13 or later
- Basic knowledge of Swift and iOS development

## Setting up the Project
1. Open Xcode and create a new Single View App project.
2. Enable NFC capability for your project by selecting the app target, navigating to the "Signing & Capabilities" tab, and enabling "Near Field Communication Tag Reading" under Capabilities.

## Reading NFC Tags
To read NFC tags, we need to use the Core NFC framework provided by Apple. Follow the steps below to implement NFC tag reading in your Swift project:

1. Import the `CoreNFC` framework by adding the following line at the top of your Swift file:
```swift
import CoreNFC
```

2. Conform to the `NFCNDEFReaderSessionDelegate` protocol in your view controller class:
```swift
class ViewController: UIViewController, NFCNDEFReaderSessionDelegate {
    // ...
}
```

3. Implement the delegate methods to handle NFC tag scanning and reading:
```swift
func readerSession(_ session: NFCNDEFReaderSession, didDetectNDEFs messages: [NFCNDEFMessage]) {
    // Handle the detected NDEF messages
}

func readerSession(_ session: NFCNDEFReaderSession, didInvalidateWithError error: Error) {
    // Handle session invalidation errors
}
```

4. Add code to start an NFC reader session and handle the detected NFC tags within a button action or view loading:
```swift
@IBAction func scanButtonTapped(_ sender: UIButton) {
    let session = NFCNDEFReaderSession(delegate: self, queue: nil, invalidateAfterFirstRead: false)
    session.begin()
}
```

5. Access the NDEF messages tag payload data in the `didDetectNDEFs` delegate method:
```swift
func readerSession(_ session: NFCNDEFReaderSession, didDetectNDEFs messages: [NFCNDEFMessage]) {
    for message in messages {
        for record in message.records {
            let payload = String(data: record.payload, encoding: .utf8)
            print(payload)
        }
    }
}
```

## Writing NFC Tags
To write data to NFC tags, we can use the `NFCTagWriterSession` class provided by the Core NFC framework. Follow the steps below to implement NFC tag writing in your Swift project:

1. Implement the `NFCNDEFReaderSessionDelegate` protocol in your view controller class, as described in the previous section.
   
2. Import the `CoreNFC` framework by adding the following line at the top of your Swift file:
```swift
import CoreNFC
```

3. Add code to start an NFC writer session within a button action or view loading:
```swift
@IBAction func writeButtonTapped(_ sender: UIButton) {
    let session = NFCNDEFReaderSession(delegate: self, queue: nil, invalidateAfterFirstRead: false)
    session.begin()
}
```

4. Access the NFC tag to write data to within the `didDetectNDEFs` delegate method:
```swift
func readerSession(_ session: NFCNDEFReaderSession, didDetectNDEFs messages: [NFCNDEFMessage]) {
    guard let tag = session.connectedTag else {
        session.invalidate(errorMessage: "No NFC tag found")
        return
    }
    
    let payload = "Hello, NFC!"
    let data = payload.data(using: .utf8)!
    let record = NFCNDEFPayload(format: .nfcWellKnown, type: Data(), identifier: Data(), payload: data)
    let message = NFCNDEFMessage(records: [record])
    
    tag.writeNDEF(message) { error in
        if let error = error {
            print("Error writing NFC tag: \(error.localizedDescription)")
        } else {
            print("NFC tag successfully written")
        }
    }
}
```

## Conclusion
In this tutorial, we have learned how to implement NFC-based ticketing solutions in Swift by reading and writing NFC tags using the Core NFC framework. NFC technology offers a seamless and secure way to enable contactless transactions, making it a valuable addition to various mobile applications. With the provided code examples and guidelines, you can start integrating NFC functionality into your iOS app and explore the numerous possibilities it offers.

Remember to consider the specific requirements and limitations of NFC functionality within your app and ensure that the NFC-enabled devices you are targeting have the necessary hardware and software capabilities.

#iOS #Swift