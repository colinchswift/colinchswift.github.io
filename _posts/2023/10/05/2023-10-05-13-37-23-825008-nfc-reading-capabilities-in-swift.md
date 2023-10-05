---
layout: post
title: "NFC reading capabilities in Swift"
description: " "
date: 2023-10-05
tags: []
comments: true
share: true
---

Near Field Communication (NFC) is a technology that allows devices to exchange data wirelessly over short distances. With the introduction of iOS 11, Apple introduced NFC reading capabilities for certain iPhone models. In this article, we'll explore how to implement NFC reading in Swift.

## Setting Up

Before we dive into the code, let's make sure we have everything set up properly.

1. Device: Ensure you have an iPhone 7 (or later model) that supports NFC reading capabilities.

2. Xcode: Make sure you have Xcode 11 (or a later version) installed on your development machine.

3. Enable NFC Capability: Open your Xcode project, go to the project settings, select your target, and navigate to the "Signing & Capabilities" tab. Enable the "Near Field Communication Tag Reading" capability.

## Reading NFC Tags

Once your project is set up, it's time to start reading NFC tags. Follow the steps below to implement NFC reading in your Swift project.

1. Import the Core NFC framework by adding the following line at the top of your Swift file:

```swift
import CoreNFC
```

2. Adopt the `NFCNDEFReaderSessionDelegate` protocol in your view controller:

```swift
class ViewController: UIViewController, NFCNDEFReaderSessionDelegate {
    // ...
}
```

3. Implement the necessary delegate methods. For example, add the following code to your view controller:

```swift
func readerSession(_ session: NFCNDEFReaderSession, didDetectNDEFs messages: [NFCNDEFMessage]) {
    for message in messages {
        for record in message.records {
            let payload = String(data: record.payload, encoding: .utf8)
            print("NFC Tag Contents: \(payload ?? "")")
        }
    }
}

func readerSession(_ session: NFCNDEFReaderSession, didInvalidateWithError error: Error) {
    print("Error reading NFC tag: \(error.localizedDescription)")
}
```

4. Create an instance of `NFCNDEFReaderSession` and start the session when you want to begin reading NFC tags:

```swift
let session = NFCNDEFReaderSession(delegate: self, queue: nil, invalidateAfterFirstRead: false)
session.begin()
```

5. Handle the session closure in the `readerSession(_:didInvalidateWithError:)` method:

```swift
func readerSession(_ session: NFCNDEFReaderSession, didInvalidateWithError error: Error) {
    // Handle session closure
}
```

## Handling NFC Tag Data

Once you have successfully read an NFC tag, you can handle the tag data based on your application's requirements. You can extract the payload from the `NFCNDEFRecord` object and perform specific actions or display the data to the user.

Remember to handle potential errors that may occur during the reading process and provide appropriate error messages or feedback to the user.

## Conclusion

With the introduction of NFC reading capabilities in iOS, you can leverage NFC technology to perform various tasks in your Swift applications. In this article, we explored the basics of implementing NFC reading in Swift, and you can now start incorporating NFC functionality into your own iOS projects.

#swift #NFC