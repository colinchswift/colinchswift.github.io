---
layout: post
title: "Implementing NFC for interactive museum exhibits in Swift"
description: " "
date: 2023-10-05
tags: []
comments: true
share: true
---

As technology continues to advance, museums are finding innovative ways to engage with their visitors. One such technology is Near Field Communication (NFC), which allows for the seamless exchange of data between devices in close proximity, without the need for an internet connection.

In this article, we will explore how to implement NFC in Swift to create interactive museum exhibits that can provide visitors with additional information and a more immersive experience.

## Table of Contents
- [What is NFC?](#what-is-nfc)
- [How Does NFC Work?](#how-does-nfc-work)
- [Setting Up Your Project](#setting-up-your-project)
- [Reading NFC Tags](#reading-nfc-tags)
- [Writing NFC Tags](#writing-nfc-tags)
- [Conclusion](#conclusion)

## What is NFC?
NFC is a short-range wireless communication technology that enables devices in close proximity to securely exchange data. It is commonly used in contactless payment systems, access control, and now, interactive museum exhibits.

## How Does NFC Work?
NFC operates by using electromagnetic radio fields to establish communication between two devices. It can operate in two modes: Reader/Writer mode and Card Emulation mode.

In Reader/Writer mode, the device acts as an initiator and can read or write data from an NFC tag. This mode is useful for retrieving information from NFC tags attached to exhibits in a museum.

In Card Emulation mode, the device behaves like an NFC tag and can be used for contactless payments or access control purposes. For the scope of this article, we will focus on Reader/Writer mode.

## Setting Up Your Project
To begin, you'll need to create a new Xcode project.

1. Open Xcode and select "Create a new Xcode project."
2. Choose the "Single View App" template and click "Next."
3. Enter a name for your project, select "Swift" as the language, and click "Next."
4. Choose a location to save your project and click "Create."

Make sure your project is configured to support NFC by enabling the "Near Field Communication Tag Reading" capability in the project settings.

## Reading NFC Tags
To read an NFC tag, we need to implement the `NFCTagReaderSessionDelegate` protocol. This protocol provides methods that are called when a tag is detected and when the session is invalidated.

```swift
import CoreNFC

class NFCReaderViewController: UIViewController, NFCTagReaderSessionDelegate {

    var readerSession: NFCTagReaderSession?

    override func viewDidLoad() {
        super.viewDidLoad()
        
        guard NFCNDEFReaderSession.readingAvailable else {
            // NFC not supported on this device
            return
        }

        readerSession = NFCTagReaderSession(pollingOption: .iso14443, delegate: self)
        readerSession?.begin()
    }

    func tagReaderSessionDidDetectTags(_ session: NFCTagReaderSession, tags: [NFCTag]) {
        for tag in tags {
            session.connect(to: tag) { (error: Error?) in
                if error != nil {
                    session.invalidate(errorMessage: "Connection error. Please try again.")
                } else {
                    // Read data from tag
                    self.readData(from: tag)
                }
            }
        }
    }

    func readData(from tag: NFCTag) {
        // Implement reading logic here
    }

    func tagReaderSession(_ session: NFCTagReaderSession, didInvalidateWithError error: Error) {
        // Handle session invalidation here
    }
}
```

In the above code, we create an instance of `NFCTagReaderSession` and start the session. When a tag is detected, the `tagReaderSessionDidDetectTags` method is called. We then connect to the tag and read the data using the `readData(from:)` method. The `tagReaderSession(_:didInvalidateWithError:)` method is called if the session is invalidated.

## Writing NFC Tags
In addition to reading NFC tags, we can also write data to them. To do this, we implement the `NFCNDEFReaderSessionDelegate` protocol.

```swift
import CoreNFC

class NFCWriterViewController: UIViewController, NFCNDEFReaderSessionDelegate {

    var writerSession: NFCNDEFReaderSession?

    override func viewDidLoad() {
        super.viewDidLoad()
        
        guard NFCNDEFReaderSession.readingAvailable else {
            // NFC not supported on this device
            return
        }

        writerSession = NFCNDEFWriterSession(delegate: self, queue: nil, invalidateAfterFirstWrite: false)
        writerSession?.begin()
    }

    func readerSession(_ session: NFCNDEFReaderSession, didDetectNDEFs messages: [NFCNDEFMessage]) {
        let payload = NFCNDEFPayload(
            format: .utf8,
            type: "Text".data(using: .utf8)!,
            identifier: "Example".data(using: .utf8)!,
            payload: "Welcome to the exhibit!".data(using: .utf8)!
        )

        let message = NFCNDEFMessage(records: [payload])
        session.connect(to: session.connectedTag!) { (error: Error?) in
            if error != nil {
                session.invalidate(errorMessage: "Connection error. Please try again.")
            } else {
                session.write(message)
            }
        }
    }

    func readerSession(_ session: NFCNDEFReaderSession, didInvalidateWithError error: Error) {
        // Handle session invalidation here
    }
}
```

In the above code, we create an instance of `NFCNDEFWriterSession` and start the session. When an NDEF message is detected, we create a `NFCNDEFPayload` with the desired data. We then create an `NFCNDEFMessage` with the payload and connect to the tag using `session.connect(to:completionHandler:)`. Finally, we write the message to the tag using `session.write(_:)`.

## Conclusion
Implementing NFC in Swift allows us to create interactive museum exhibits that can provide visitors with additional information and a more immersive experience. By utilizing the capabilities of NFC, museums can engage visitors in new and exciting ways. Whether it's reading information from NFC tags attached to exhibits or writing data to create interactive experiences, the possibilities are limitless.

By following the steps outlined in this article, you can start implementing NFC in your own Swift projects and create interactive museum exhibits that will captivate your visitors.

#technolgy #museum #NFC