---
layout: post
title: "NFC writing capabilities in Swift"
description: " "
date: 2023-10-05
tags: []
comments: true
share: true
---

Near Field Communication (NFC) is a technology that enables devices to communicate wirelessly with each other when they are in close proximity. In iOS, starting from version 13, Apple introduced NFC support, allowing developers to use NFC capabilities in their apps. In this blog post, we will explore how to write NFC tags using Swift.

## Prerequisites
To follow along with this tutorial, you will need the following:

- A device running iOS 13 or later with NFC capabilities.
- Xcode 11 or later installed on your Mac.
- An iPhone with NFC tag reading capabilities or an NFC reader/writer.

## Setting up the Project
Let's start by creating a new Xcode project:

1. Open Xcode and select "Create a new Xcode project".
2. Choose the "Single View App" template and click "Next".
3. Name your project and select the desired folder to save it.
4. Choose the language as "Swift" and make sure "Use SwiftUI" is unchecked.
5. Finish creating the project.

## Adding NFC Capabilities
To enable NFC capabilities in your app, you need to make some changes to the project settings:

1. Select the project in the Xcode project navigator.
2. Select the app target and go to the "Signing & Capabilities" tab.
3. Click the "+" button and search for "Near Field Communication Tag Reading".
4. Select "Near Field Communication Tag Reading" from the list.
5. Build and run your app on a compatible device.

## Writing NFC Tags
To write data to an NFC tag, you'll need to make use of the `Core NFC` framework. Add the following import statement to the top of your Swift file:

```swift
import CoreNFC
```

Next, you'll need to request permission from the user to access the NFC capabilities. Add the following code snippet inside the `viewDidLoad` method of your view controller:

```swift
guard NFCNDEFReaderSession.readingAvailable else {
    print("NFC is not supported on this device")
    return
}

let session = NFCNDEFReaderSession(delegate: self, queue: DispatchQueue.main, invalidateAfterFirstRead: false)
session.begin()
```

This code snippet checks if NFC is supported on the device. If it is supported, it creates an instance of `NFCNDEFReaderSession` with the current view controller as the delegate, and starts the session.

To handle the writing process, implement the `NFCNDEFReaderSessionDelegate` protocol in your view controller. Add the following code snippet to your view controller:

```swift
extension ViewController: NFCNDEFReaderSessionDelegate {
    
    func readerSession(_ session: NFCNDEFReaderSession, didDetect tags: [NFCNDEFTag]) {
        guard let tag = tags.first else {
            session.invalidate(errorMessage: "No tags found")
            return
        }
        
        session.connect(to: tag) { error in
            if error != nil {
                session.invalidate(errorMessage: "Connection error")
            }
            
            // Create an NDEF message with your data
            let payload = NFCNDEFPayload(format: .nfcForumExternal, type: "T".data(using: .utf8)!, identifier: "uniqueIdentifier".data(using: .utf8)!, payload: "Payload data".data(using: .utf8)!)
            let message = NFCNDEFMessage(records: [payload])
            
            // Write the message to the tag
            tag.writeNDEF(message) { error in
                if error != nil {
                    session.invalidate(errorMessage: "Writing error")
                } else {
                    session.alertMessage = "Tag written successfully"
                }
                
                session.invalidate()
            }
        }
    }
    
    func readerSession(_ session: NFCNDEFReaderSession, didInvalidateWithError error: Error) {
        print("Session invalidated: \(error.localizedDescription)")
    }
}
```

This code snippet handles the writing process by connecting to the detected tag, creating an NDEF message with your data, and writing that message to the tag.

## Conclusion
In this blog post, we explored how to leverage NFC writing capabilities in Swift. We discussed the prerequisites, setting up the project, enabling NFC capabilities, and writing data to NFC tags using the Core NFC framework. By following the steps outlined above, you can now start developing applications that can write data to NFC tags in your iOS apps.

Remember to properly handle NFC session errors and provide appropriate error messages to the user for a better user experience.

#iOS #NFC