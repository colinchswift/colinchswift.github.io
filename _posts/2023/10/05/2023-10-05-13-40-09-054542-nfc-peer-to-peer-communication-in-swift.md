---
layout: post
title: "NFC peer-to-peer communication in Swift"
description: " "
date: 2023-10-05
tags: []
comments: true
share: true
---

Near Field Communication (NFC) enables devices to communicate wirelessly over short distances. With NFC peer-to-peer communication, devices can exchange information by simply bringing them close together. In this blog post, we will explore how to implement NFC peer-to-peer communication in Swift.

## Prerequisites
To work with NFC in Swift, you will need:

- Xcode 11 or later
- An iPhone model that supports NFC (iPhone 7 or later)
- A physical iOS device running iOS 11 or later

## Setting Up the Project
1. Open Xcode and create a new iOS project.
2. Select the "Single View App" template and click "Next".
3. Enter the project details and choose a location to save it.
4. Make sure the "Use Core Data" and "Include Unit Tests" checkboxes are unchecked, then click "Next".
5. Choose a location to save the project and click "Create".

## Add NFC Capabilities
To enable NFC capabilities in your app, follow these steps:

1. Select the project in the Project Navigator.
2. Select the target under "Targets" and go to the "Signing & Capabilities" tab.
3. Click the "+ Capability" button and search for "NFC".
4. Check the "Near Field Communication Tag Reading" and "Near Field Communication Tag Writing" checkboxes.
5. Close the "Signing & Capabilities" tab.

## Implementing NFC Peer-to-Peer Communication

To implement NFC peer-to-peer communication in Swift, we will need to utilize the Core NFC framework. This framework provides classes for interacting with NFC tags and devices.

1. Open the ViewController.swift file.
2. Import the CoreNFC framework at the top of the file.

```swift
import CoreNFC
```

3. Conform to the `NFCNDEFReaderSessionDelegate` protocol by adding it to the class declaration.

```swift
class ViewController: UIViewController, NFCNDEFReaderSessionDelegate {
```

4. Add a button to trigger the NFC session.

```swift
@IBAction func startNFCSession(_ sender: UIButton) {
  let session = NFCNDEFReaderSession(delegate: self, queue: DispatchQueue.main, invalidateAfterFirstRead: false)
  session.begin()
}
```

5. Implement the required delegate methods.

```swift
func readerSession(_ session: NFCNDEFReaderSession, didDetectNDEFs messages: [NFCNDEFMessage]) {
  // Process the received NDEF messages
}

func readerSession(_ session: NFCNDEFReaderSession, didInvalidateWithError error: Error) {
  // Handle session invalidation
}
```

6. In the `didDetectNDEFs` method, you can access the received NDEF messages and process them according to your app's logic.

## Testing NFC Peer-to-Peer Communication
1. Connect your iPhone to your Mac and select your iOS device as the target destination in Xcode.
2. Build and run the app on your iOS device.
3. Tap the button that triggers the NFC session.
4. Bring your device close to another device that supports NFC.
5. If the devices are compatible and NFC is enabled on both devices, they will establish a peer-to-peer connection and exchange information.

## Conclusion
Implementing NFC peer-to-peer communication in Swift allows your app to leverage the power of NFC technology for seamless data exchange between compatible devices. By following the steps outlined in this blog post, you can easily integrate NFC into your Swift app and enable new and exciting features. So go ahead and start exploring the possibilities of NFC in your own apps!

## Hashtags
#NFC #Swift