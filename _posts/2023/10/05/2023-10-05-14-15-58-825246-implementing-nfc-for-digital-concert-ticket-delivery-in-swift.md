---
layout: post
title: "Implementing NFC for digital concert ticket delivery in Swift"
description: " "
date: 2023-10-05
tags: []
comments: true
share: true
---

![NFC](https://example.com/nfc.png)

As technology continues to advance, traditional paper tickets for events are being replaced by digital alternatives. Near Field Communication (NFC) is a popular technology that allows devices to communicate with each other when in close proximity. In this blog post, we will explore how to implement NFC for digital concert ticket delivery using Swift.

## Table of Contents
- [What is NFC?](#what-is-nfc)
- [Setting Up the Project](#setting-up-the-project)
- [Working with NFC](#working-with-nfc)
- [Handling NFC Reader Session](#handling-nfc-reader-session)
- [Writing Data to NFC Tag](#writing-data-to-nfc-tag)
- [Reading Data from NFC Tag](#reading-data-from-nfc-tag)
- [Conclusion](#conclusion)

## What is NFC?

Near Field Communication (NFC) is a wireless technology that allows devices to establish communication by bringing them close together, usually within a few centimeters. It is widely used for contactless payment systems, access control, and information sharing between devices. In the context of digital ticket delivery, NFC can be utilized to transfer ticket information securely and quickly.

## Setting Up the Project

To get started, create a new project in Xcode and select "Single View App" as the template. Make sure to enable "Use Core Data" and "Include Unit Tests" options. Once the project is created, we need to add support for NFC by enabling the capability. 

To enable NFC capability, follow these steps:
1. Select your project in the project navigator.
2. Select the target for your app.
3. Go to the "Signing & Capabilities" tab.
4. Click on the "+ Capability" button.
5. Search for "Near Field Communication Tag Reading" and enable it.

## Working with NFC

To work with NFC in Swift, we need to import the CoreNFC framework into our project. CoreNFC provides classes and protocols necessary for implementing NFC tag reading and writing functionalities. 

Add the following import statement at the top of your Swift file:

```swift
import CoreNFC
```

## Handling NFC Reader Session

To handle NFC reader session, we need to conform to the `NFCTagReaderSessionDelegate` protocol. This protocol provides methods to handle success and failure scenarios when reading or writing to an NFC tag.

Here is an example implementation of the delegate methods:

```swift
class NFCReaderViewController: UIViewController, NFCTagReaderSessionDelegate {

    var nfcSession: NFCTagReaderSession?

    func beginNFCSession() {
        nfcSession = NFCTagReaderSession(pollingOption: .iso14443, delegate: self)
        nfcSession?.begin()
    }

    func tagReaderSessionDidBecomeActive(_ session: NFCTagReaderSession) {
        // NFC session did become active
    }

    func tagReaderSession(_ session: NFCTagReaderSession, didInvalidateWithError error: Error) {
        // NFC session did invalidate with error
    }

    func tagReaderSession(_ session: NFCTagReaderSession, didDetect tags: [NFCTag]) {
        // NFC tag detected
    }
}
```

## Writing Data to NFC Tag

To write data to an NFC tag, we first need to detect the tag using `tagReaderSession(_:didDetect:)` delegate method. Once the tag is detected, we can write data to it using the `write(_:to:)` method of the `NFCNDEFTag` object.

Here is an example implementation of writing data to an NFC tag:

```swift
func tagReaderSession(_ session: NFCTagReaderSession, didDetect tags: [NFCTag]) {
    guard let firstTag = tags.first else {
        return
    }

    session.connect(to: firstTag) { (error: Error?) in
        if error != nil {
            session.invalidate(errorMessage: "Error connecting to tag")
            return
        }

        if case let .iso7816(tag) = firstTag {
            // Write data to NFC tag
            let apdu = NFCISO7816APDU(instructionClass: 0x00, instructionCode: 0xA4, p1Parameter: 0x04, p2Parameter: 0x00, data: [0xA0, 0x00, 0x00, 0x05, 0x04, 0x06, 0x07, 0x08, 0x09], expectedResponseLength: 0)

            tag.sendCommand(apdu: apdu) { (data: Data?, sw1: UInt8, sw2: UInt8, error: Error?) in
                if error != nil {
                    session.invalidate(errorMessage: "Error writing data to tag")
                } else {
                    // Data written successfully
                }
            }
        }
    }
}
```

## Reading Data from NFC Tag

To read data from an NFC tag, we follow a similar approach as writing data. Once the tag is detected, we can read data from it using the `read(_:completionHandler:)` method of the `NFCNDEFTag` object.

Here is an example implementation of reading data from an NFC tag:

```swift
func tagReaderSession(_ session: NFCTagReaderSession, didDetect tags: [NFCTag]) {
    guard let firstTag = tags.first else {
        return
    }

    session.connect(to: firstTag) { (error: Error?) in
        if error != nil {
            session.invalidate(errorMessage: "Error connecting to tag")
            return
        }

        if case let .iso7816(tag) = firstTag {
            // Read data from NFC tag
            let apdu = NFCISO7816APDU(instructionClass: 0x00, instructionCode: 0xB0, p1Parameter: 0x00, p2Parameter: 0x00, data: [], expectedResponseLength: 16)

            tag.sendCommand(apdu: apdu) { (data: Data?, sw1: UInt8, sw2: UInt8, error: Error?) in
                if error != nil {
                    session.invalidate(errorMessage: "Error reading data from tag")
                } else {
                    // Data read successfully
                    if let data = data {
                        // Process the read data
                    }
                }
            }
        }
    }
}
```

## Conclusion

Implementing NFC for digital concert ticket delivery using Swift can greatly enhance the user experience by providing secure and convenient access to events. By following the steps outlined in this blog post, you can get started with implementing NFC in your own application. Further customization and handling of NFC features can be explored by referring to the official Apple documentation.

#nfc #swift