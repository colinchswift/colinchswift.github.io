---
layout: post
title: "Using NFC for secure document sharing in Swift"
description: " "
date: 2023-10-05
tags: []
comments: true
share: true
---

In this tutorial, we will explore how to use Near Field Communication (NFC) to securely share documents between devices using Swift. NFC is a wireless communication technology that allows devices to communicate by simply bringing them close together. It is commonly used for contactless payments, transportation cards, and access control systems.

## Table of Contents

1. [What is NFC?](#what-is-nfc)
2. [Setting up the Project](#setting-up-the-project)
3. [Reading NFC Tags](#reading-nfc-tags)
4. [Writing Data to NFC Tags](#writing-data-to-nfc-tags)
5. [Document Sharing via NFC](#document-sharing-via-nfc)
6. [Conclusion](#conclusion)

## What is NFC? {#what-is-nfc}

NFC is a short-range wireless communication technology that operates over a distance of a few centimeters. It enables communication between two devices by bringing them close together. NFC tags can store and transmit data, which can be read or written by NFC-enabled devices.

## Setting up the Project {#setting-up-the-project}

To get started, create a new Xcode project and select the "Single View App" template. Make sure to choose Swift as the programming language.

Next, enable NFC capabilities for your project. Navigate to the project settings and select the "Capabilities" tab. Turn on the "Near Field Communication Tag Reading" and "Near Field Communication Tag Writing" options.

## Reading NFC Tags {#reading-nfc-tags}

Now, let's implement the code for reading NFC tags. Create a new Swift file called "NFCManager.swift" and add the following code:

```swift
import CoreNFC

class NFCManager: NSObject, NFCNDEFReaderSessionDelegate {
    var readerSession: NFCNDEFReaderSession?
    
    func startReading() {
        readerSession = NFCNDEFReaderSession(delegate: self, queue: nil, invalidateAfterFirstRead: false)
        readerSession?.begin()
    }
    
    func readerSession(_ session: NFCNDEFReaderSession, didDetectNDEFs messages: [NFCNDEFMessage]) {
        for message in messages {
            for record in message.records {
                guard let payloadString = String(data: record.payload, encoding: .utf8) else { continue }
                print("Received data: \(payloadString)")
            }
        }
    }
    
    func readerSession(_ session: NFCNDEFReaderSession, didInvalidateWithError error: Error) {
        print("Error: \(error.localizedDescription)")
    }
}
```

The `NFCManager` class is responsible for initiating and handling NFC reader sessions. The `startReading` method starts a new reader session and the `readerSession(_:didDetectNDEFs:)` method is called when a NFC tag is detected. It retrieves the payload data from the NDEF records and prints it to the console.

## Writing Data to NFC Tags {#writing-data-to-nfc-tags}

To write data to NFC tags, we need to modify the `NFCManager` class. Add the following method to the class:

```swift
func writeData(data: String) {
    guard let tag = readerSession?.connectedTag else { return }
    let payload = NFCNDEFPayload(format: .nfcWellKnown, type: "T".data(using: .utf8)!, identifier: Data(), payload: data.data(using: .utf8)!)
    let ndefMessage = NFCNDEFMessage(records: [payload])
    readerSession?.connect(to: tag) { error in
        guard error == nil else {
            print("Error: \(error!.localizedDescription)")
            return
        }
        self.readerSession?.write(ndefMessage) { error in
            if let error = error {
                print("Error writing NFC tag: \(error.localizedDescription)")
            } else {
                print("Data written to NFC tag")
            }
            self.readerSession?.invalidate()
        }
    }
}
```

The `writeData` method creates a new NDEF message with a custom payload containing the data we want to write. It then connects to the NFC tag, writes the message to it, and invalidates the reader session.

## Document Sharing via NFC {#document-sharing-via-nfc}

Now that we have the reading and writing functionality in place, let's use NFC for secure document sharing. Add a button to your app's view controller and connect it to an `IBAction` method. Implement the method as follows:

```swift
@IBAction func shareButtonTapped(_ sender: UIButton) {
    let documentData = "This is a confidential document"
    let nfcManager = NFCManager()
    nfcManager.writeData(data: documentData)
}
```

In this example, when the button is tapped, an `NFCManager` instance is created and used to write the document data to an NFC tag. This tag can then be read by another device to securely retrieve the document.

## Conclusion {#conclusion}

In this tutorial, we learned how to use NFC for secure document sharing in Swift. We covered the basics of NFC, setting up the project, reading and writing NFC tags, and implementing document sharing functionality. NFC can be a powerful tool for secure data transfer between devices, and with the right precautions, it can be a reliable method for document sharing. Explore further possibilities with NFC to enhance your app's functionality and user experience.

#swift #nfc