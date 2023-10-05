---
layout: post
title: "Implementing NFC for secure file sharing in Swift"
description: " "
date: 2023-10-05
tags: []
comments: true
share: true
---

With the increasing use of mobile devices for file sharing, it is crucial to ensure secure transmission of sensitive information. Near Field Communication (NFC) is a wireless technology that enables devices to establish a connection by simply bringing them close together. In this tutorial, we will explore how to implement NFC for secure file sharing in Swift.

## Prerequisites

To follow along with this tutorial, you will need:

- Xcode installed on your Mac
- An iOS device with NFC capabilities running iOS 11 or later
- Basic knowledge of Swift programming

## Getting Started with NFC

1. Create a new Xcode project or open an existing one.
2. Ensure that your project has NFC capability enabled. To do this, go to the "Signing & Capabilities" tab in the project settings and enable the "Near Field Communication Tag Reading" capability.
3. Import the CoreNFC framework into your project by adding `import CoreNFC` at the top of your Swift file.

## Reading NFC Tags

To securely share files using NFC, we need to first read the contents of an NFC tag. NFC tags can store various data types, such as text, URLs, or custom payload.

### 1. Enable NFC scanning

Add the `NFCTagReaderSession` capability to your view controller by conforming to the `NFCTagReaderSessionDelegate` protocol.

```swift
class ViewController: UIViewController, NFCTagReaderSessionDelegate {
    // ...
}
```

### 2. Start an NFC tag reader session

Inside your view controller, add a button that will initiate the NFC tag scanning process. When the button is pressed, create an instance of `NFCTagReaderSession` and start the session.

```swift
@IBAction func startNFCScan(_ sender: Any) {
    let nfcSession = NFCTagReaderSession(pollingOption: .iso14443, delegate: self)
    nfcSession?.alertMessage = "Hold your device near the NFC tag to scan."
    nfcSession?.begin()
}
```

### 3. Implement the delegate methods

Add the required delegate methods for `NFCTagReaderSessionDelegate` to handle NFC tag scanning. In the `tagReaderSession(_:didDetect tags:)` method, you can access the NFC tag and read its contents.

```swift
func tagReaderSession(_ session: NFCTagReaderSession, didDetect tags: [NFCTag]) {
    guard let tag = tags.first else {
        session.invalidate(errorMessage: "Unsupported NFC tag found.")
        return
    }
    
    session.connect(to: tag) { (error: Error?) in
        if let error = error {
            session.invalidate(errorMessage: "Error connecting to NFC tag. \(error.localizedDescription)")
            return
        }
        
        switch tag {
        case .miFare(let miFareTag):
            self.readMiFareTag(miFareTag)
        case .iso15693(let iso15693Tag):
            self.readISO15693Tag(iso15693Tag)
        default:
            session.invalidate(errorMessage: "Unsupported NFC tag found.")
        }
    }
}

func readMiFareTag(_ tag: NFCMiFareTag) {
    guard case .success(let tagInfo) = tag.getMiFareTagInfo() else {
        session.invalidate(errorMessage: "Error reading NFC tag.")
        return
    }
    
    // Parse and handle the retrieved tag information
}

func readISO15693Tag(_ tag: NFCISO15693Tag) {
    // Read and handle data from ISO15693 tag
}
```

## Secure File Sharing using NFC

With the ability to read data from an NFC tag, we can now proceed to securely share files between devices.

### Writing Data to an NFC Tag

To write data to an NFC tag, you can use the `NFCNDEFPayload` class to create an NDEF payload with the file information you want to share. Then, write this payload to the NFC tag.

```swift
let nfcPayload = NFCNDEFPayload(format: .nfcWellKnown, type: Data("T".utf8), identifier: Data(), payload: fileData)
let nfcMessage = NFCNDEFMessage(records: [nfcPayload])
let nfcWriteStatus = nfcTag.writeNDEF(nfcMessage)
```

### Receiving Data from another Device

To receive data from another device, you can use `CoreNFC` with a similar approach. When the user brings their device close to yours, you can read the NDEF message from their device using the `tagReaderSession(_:didDetect tags:)` delegate method.

```swift
func tagReaderSession(_ session: NFCTagReaderSession, didDetect tags: [NFCTag]) {
    guard let tag = tags.first else {
        session.invalidate(errorMessage: "Unsupported NFC tag found.")
        return
    }
    
    session.connect(to: tag) { (error: Error?) in
        if let error = error {
            session.invalidate(errorMessage: "Error connecting to NFC tag. \(error.localizedDescription)")
            return
        }
        
        switch tag {
        case .iso7816(let iso7816Tag):
            self.readISO7816Tag(iso7816Tag)
        // Add support for other tag types
        }
    }
}

func readISO7816Tag(_ tag: NFCISO7816Tag) {
    tag.sendCommand(commandPacket: Data.init(hexString: "00A404000ED2760000850101")) { (response: Data, sw1: UInt8, sw2: UInt8, error: Error?) in
        // Handle response
    }
}
```

## Conclusion

Implementing NFC for secure file sharing in Swift is a powerful feature that enhances the user experience and ensures secure transmission of data. By following the steps outlined in this tutorial, you can easily implement it in your iOS applications. NFC is a versatile technology that can also be used for various other purposes, such as contactless payments or access control systems.

#swift #NFC