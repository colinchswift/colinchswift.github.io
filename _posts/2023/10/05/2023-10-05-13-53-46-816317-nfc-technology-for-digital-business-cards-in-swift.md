---
layout: post
title: "NFC technology for digital business cards in Swift"
description: " "
date: 2023-10-05
tags: []
comments: true
share: true
---

In today's digital age, traditional business cards may seem outdated and inefficient. With NFC (Near Field Communication) technology, digital business cards have become a modern and convenient way to exchange contact information. In this tutorial, we will explore how to use NFC technology to create and share business cards using Swift.

## Table of Contents
- [Introduction to NFC Technology](#introduction-to-nfc-technology)
- [Setting Up the Project](#setting-up-the-project)
- [Reading NFC Tags](#reading-nfc-tags)
- [Writing Data to NFC Tags](#writing-data-to-nfc-tags)
- [Creating the Digital Business Card](#creating-the-digital-business-card)
- [Conclusion](#conclusion)

## Introduction to NFC Technology

NFC is a short-range wireless communication technology that allows devices to communicate with each other by simply touching or bringing them close together. It enables data transfer, contactless payments, and other interactive features.

In the case of digital business cards, an NFC tag is placed on the physical business card. When another NFC-enabled device comes into contact with the tag, it can read or write data, such as contact information, URLs, or social media profiles.

## Setting Up the Project

To start using NFC in your Swift project, you need to enable the "NFC Tag Reading" and "NFC Tag Writing" capabilities. Navigate to your project's settings, select your target, and enable the capabilities under the "Signing & Capabilities" tab.

## Reading NFC Tags

To read NFC tags, you need to implement the `NFCTagReaderSessionDelegate` protocol. This protocol provides methods for handling different tag types.

```swift
import CoreNFC

class NFCReaderViewController: UIViewController, NFCTagReaderSessionDelegate {

    var nfcSession: NFCTagReaderSession?

    func startNFCSession() {
        nfcSession = NFCTagReaderSession(pollingOption: [.iso14443, .iso15693], delegate: self)
        nfcSession?.begin()
    }

    func tagReaderSessionDidDetectTags(_ session: NFCTagReaderSession, tags: [NFCTag]) {
        guard let tag = tags.first else {
            session.invalidate(errorMessage: "No tags found.")
            return
        }

        session.connect(to: tag) { error in
            if error != nil {
                session.invalidate(errorMessage: "Connection error.")
                return
            }

            // Read tag data here

            session.invalidate()
        }
    }

    // Implementation of NFCTagReaderSessionDelegate methods...

}
```

In the `startNFCSession()` method, we initialize an `NFCTagReaderSession` to begin the session. The `pollingOption` determines the types of tags we want to detect.

When a tag is detected, the `tagReaderSessionDidDetectTags` method is called. We connect to the tag and handle any errors that may occur. Finally, we perform the necessary operations to read the tag's data.

## Writing Data to NFC Tags

To write data to an NFC tag, you need to implement the `NFCNDEFReaderSessionDelegate` protocol. This protocol provides methods for handling NDEF (NFC Data Exchange Format) tags.

```swift
import CoreNFC

class NFCWriterViewController: UIViewController, NFCNDEFReaderSessionDelegate {

    var nfcSession: NFCNDEFReaderSession?

    func startNFCSession() {
        nfcSession = NFCNDEFReaderSession(delegate: self, queue: DispatchQueue.main, invalidateAfterFirstRead: false)
        nfcSession?.alertMessage = "Approach an NFC tag to write."
        nfcSession?.begin()
    }

    func readerSession(_ session: NFCNDEFReaderSession, didDetectNDEFs messages: [NFCNDEFMessage]) {
        // Process NDEF messages

        session.invalidate()
    }

    // Implementation of NFCNDEFReaderSessionDelegate methods...

}
```

In the `startNFCSession()` method, we initialize an `NFCNDEFReaderSession` to begin the session. The `invalidateAfterFirstRead` flag determines whether the session should be invalidated after the first read.

When NDEF messages are detected, the `readerSession(_:didDetectNDEFs:)` method is called. Here, we can process the messages and perform any necessary operations, such as writing data to the tag.

## Creating the Digital Business Card

To create a digital business card, you can use the `NFCNDEFMessage` and `NFCNDEFPayload` classes provided by the CoreNFC framework. The example below demonstrates how to create an NDEF message with a text payload containing contact information.

```swift
import CoreNFC

func createDigitalBusinessCard() -> NFCNDEFMessage? {
    let payload = NFCNDEFPayload(
        format: .wellKnown,
        type: Data("T".utf8),
        identifier: Data(),
        payload: Data("John Doe\njohndoe@example.com".utf8)
    )

    return NFCNDEFMessage(records: [payload])
}
```

In this example, we use the `NFCNDEFPayload` initializer to create a payload of type "T" (text). The payload's content is set to "John Doe\njohndoe@example.com".

We can then use the `NFCNDEFMessage` initializer with an array of payloads to create the complete NDEF message.

## Conclusion

In this tutorial, we've explored how to use NFC technology to create and share digital business cards using Swift. NFC offers a convenient and modern way to exchange contact information, eliminating the need for physical cards. By reading and writing data to NFC tags, you can easily transfer your business details in a hassle-free and eco-friendly manner.