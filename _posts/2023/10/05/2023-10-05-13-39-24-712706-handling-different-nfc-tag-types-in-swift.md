---
layout: post
title: "Handling different NFC tag types in Swift"
description: " "
date: 2023-10-05
tags: []
comments: true
share: true
---

Near Field Communication (NFC) technology allows devices to communicate with each other by simply being in close proximity. This technology has become increasingly popular, especially with the introduction of NFC-enabled payment systems and smart cards.

In iOS, you can use the Core NFC framework to interact with various types of NFC tags. This framework provides the ability to read and write NFC tags using the capabilities of the device.

In this blog post, we will explore how to handle different types of NFC tags in Swift. Let's get started!

## Table of Contents
- [Introduction to NFC Tag Types](#introduction-to-nfc-tag-types)
- [Reading NFC Tags](#reading-nfc-tags)
- [Handling Different Tag Types](#handling-different-tag-types)
- [Writing to NFC Tags](#writing-to-nfc-tags)
- [Conclusion](#conclusion)

## Introduction to NFC Tag Types

NFC tags come in different formats, and each format has a specific type. Some common NFC tag types include:

1. NFC Data Exchange Format (NDEF): This is the most common and widely supported tag type. It allows for storing and exchanging data between NFC-enabled devices. NDEF tags can contain various types of records, such as plain text, URLs, and MIME types.

2. NFC Smart Poster: These tags are primarily used for advertising or informational purposes. They typically contain a URL and may include additional data like icons or titles.

3. NFC URI: A simple tag type that represents a URL, allowing direct access to a specific web page.

4. NFC Text: This tag type contains plain text information that can be read by NFC-enabled devices.

## Reading NFC Tags

To read NFC tags, you first need to obtain an instance of the `NFCTagReaderSession` class. This class provides methods for starting and stopping NFC tag reading sessions. Once you have the tag reader instance, you can use the `didDetect tags` delegate method to handle the detected NFC tags.

```swift
import CoreNFC

class NFCReaderViewController: UIViewController, NFCTagReaderSessionDelegate {

    var tagReaderSession: NFCTagReaderSession?
    
    func beginScanning() {
        tagReaderSession = NFCTagReaderSession(pollingOption: .iso14443, delegate: self)
        tagReaderSession?.alertMessage = "Hold your iPhone near an NFC tag."
        tagReaderSession?.begin()
    }
    
    func tagReaderSession(_ session: NFCTagReaderSession, didDetect tags: [NFCTag]) {
        guard let firstTag = tags.first else {
            session.invalidate(errorMessage: "No NFC tags found.")
            return
        }
        
        session.connect(to: firstTag) { error in
            // Handle tag connection
        }
    }
    
    // ...
}
```

## Handling Different Tag Types

Once you have successfully connected to an NFC tag, you can determine its tag type using the `type` property of the `NFCTag` object. Based on the tag type, you can then handle the data stored on the tag accordingly.

Here's an example of how you can handle different tag types:

```swift
func tagReaderSession(_ session: NFCTagReaderSession, didDetect tags: [NFCTag]) {
    guard let firstTag = tags.first else {
        session.invalidate(errorMessage: "No NFC tags found.")
        return
    }
    
    session.connect(to: firstTag) { error in
        if error != nil {
            session.invalidate(errorMessage: "Unable to connect to NFC tag.")
            return
        }
        
        switch firstTag.type {
        case .iso7816:
            // Handle ISO7816 tag
        case .iso15693:
            // Handle ISO15693 tag
        case .iso18092:
            // Handle ISO18092 tag
        default:
            session.invalidate(errorMessage: "Unsupported NFC tag type.")
        }
    }
}
```

By using the `type` property, you can differentiate between different NFC tag types and handle them accordingly.

## Writing to NFC Tags

In addition to reading NFC tags, you can also write data to them using the Core NFC framework. The process involves creating and encoding an `NFCNDEFPayload` object with the desired data and then writing it to the NFC tag.

Here's an example of how to write data to an NFC tag:

```swift
func writeData(to tag: NFCISO7816Tag) {
    let payload = NFCNDEFPayload(format: .wellKnown, type: Data.init([0x54, 0x02]), identifier: Data(), payload: Data.init("Hello, World!".utf8))
            
    let message = NFCNDEFMessage(records: [payload])
    
    tag.writeNDEF(message) { error in
        if error != nil {
            // Handle write error
        }
        
        // Data successfully written to the tag
    }
}
```

In this example, we create an `NFCNDEFPayload` object with a well-known format, a custom type, and the payload data (in this case, "Hello, World!"). We then create an `NFCNDEFMessage` object with the payload and write it to the NFC tag using the `writeNDEF` method.

## Conclusion

Handling different NFC tag types in Swift is relatively straightforward using the Core NFC framework. By determining the tag type and handling it accordingly, you can interact with NFC tags and read or write data as needed.

Keep in mind that not all iOS devices support NFC functionality, so it's essential to check the device's capabilities before implementing NFC features in your app.

With this knowledge, you can now start exploring the possibilities of NFC technology and integrate it into your iOS apps. Happy coding!

\#iOS #Swift