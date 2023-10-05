---
layout: post
title: "NFC tag detection in Swift"
description: " "
date: 2023-10-05
tags: []
comments: true
share: true
---

Near Field Communication (NFC) is a wireless technology that allows devices to communicate with each other by bringing them close together. NFC is commonly used for contactless payments, access control, and data exchange between devices.

In this tutorial, we will explore how to detect NFC tags using Swift, the programming language used for iOS app development. This will allow us to build applications that can interact with NFC tags and retrieve information from them.

## Prerequisites

Before we begin, make sure you have the following prerequisites:

- A Mac running macOS with Xcode installed.
- An iPhone running iOS 11 or later which supports NFC.

## Setting Up the Project

1. Open Xcode and create a new project or open an existing project.
2. Make sure your project has the necessary entitlements to access NFC features. To do this, go to the project settings, select the target, and navigate to the "Signing & Capabilities" tab. Enable the "Near Field Communication Tag Reading" capability.

## Detecting NFC Tags

To detect NFC tags in your Swift app, you need to use the `Core NFC` framework. This framework provides classes and protocols for accessing and interacting with NFC tags.

1. Import the `CoreNFC` framework by adding the following line at the beginning of your Swift file:

   ```swift
   import CoreNFC
   ```

2. Implement the `NFCTagReaderSessionDelegate` protocol to handle NFC events. For example:

   ```swift
   class ViewController: UIViewController, NFCTagReaderSessionDelegate {
       // ...
   }
   ```

3. Create an instance of `NFCTagReaderSession` and begin the session when you want to start detecting tags. For example, you can trigger the session when a button is tapped:

   ```swift
   func startNFCSession() {
       if NFCTagReaderSession.readingAvailable {
           let session = NFCTagReaderSession(pollingOption: .iso15693, delegate: self)
           session?.begin()
       } else {
           // NFC is not available on this device
       }
   }
   ```

   In this example, we are using the `.iso15693` polling option, but you can choose the appropriate option for the type of NFC tags you want to detect.

4. Implement the required delegate method `tagReaderSession(_:didDetect:)` to handle detected tags:

   ```swift
   func tagReaderSession(_ session: NFCTagReaderSession, didDetect tags: [NFCTag]) {
       if let tag = tags.first {
           session.connect(to: tag) { (error: Error?) in
               if let tag = tag as? NFCNDEFTag {
                   // Handle the NFC tag and retrieve information
               }
               session.invalidate()
           }
       }
   }
   ```

   In this method, we check if the detected tag is an `NFCNDEFTag`, which represents an NFC Forum Data Exchange Format (NDEF) tag. You can access the tag information and perform operations such as reading or writing data.

5. Finally, you can stop the NFC session whenever you want by calling the `invalidate` method:

   ```swift
   func stopNFCSession() {
       // Call this method to stop the NFC session
       session.invalidate()
   }
   ```

That's it! You now have the basic structure for detecting NFC tags in your Swift app. You can customize the tag detection logic based on your application's requirements.

## Conclusion

In this tutorial, we learned how to detect NFC tags in a Swift app using the `Core NFC` framework. We covered the basic setup and implementation steps required to get started with NFC tag detection.

NFC technology opens up a wide range of possibilities for building contactless experiences in iOS apps. Whether you want to enable payments, access control, or data exchange with other devices, NFC provides a convenient and secure way to do so.

#technical #iOS