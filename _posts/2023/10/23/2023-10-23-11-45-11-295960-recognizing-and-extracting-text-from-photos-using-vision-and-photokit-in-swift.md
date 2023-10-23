---
layout: post
title: "Recognizing and extracting text from photos using Vision and PhotoKit in Swift"
description: " "
date: 2023-10-23
tags: []
comments: true
share: true
---

In this tutorial, we will learn how to use the Vision and PhotoKit frameworks in Swift to recognize and extract text from photos. This can be useful for tasks such as scanning documents, extracting information from images, or creating OCR (optical character recognition) applications.

## Table of Contents
- [Introduction to Vision and PhotoKit frameworks](#introduction-to-vision-and-photokit-frameworks)
- [Setting up the project](#setting-up-the-project)
- [Accessing the photo library](#accessing-the-photo-library)
- [Analyzing the image using Vision](#analyzing-the-image-using-vision)
- [Extracting recognized text](#extracting-recognized-text)
- [Conclusion](#conclusion)

## Introduction to Vision and PhotoKit frameworks

**Vision** is a powerful framework provided by Apple that enables developers to perform various computer vision tasks, including image and face recognition, object tracking, and text recognition. It provides high-performance algorithms for analyzing images and videos, making it suitable for real-time and offline applications.

**PhotoKit** is a framework that allows developers to work with the photo library on iOS, including accessing, enumerating, and modifying photos and videos. It provides a high-level API for working with assets, collections, and moments.

## Setting up the project

To start, create a new iOS project in Xcode. Make sure to select the appropriate options for device and language.

Next, we need to add the necessary frameworks to our project. Select the project target, navigate to the "General" tab, and scroll down to the "Frameworks, Libraries, and Embedded Content" section. Click the "+" button and add both Vision and PhotoKit frameworks.

## Accessing the photo library

To access the photo library, we need to request permission from the user. Open the `Info.plist` file and add the following key-value pair:

```swift
<key>NSPhotoLibraryUsageDescription</key>
<string>Accessing the photo library for extracting text</string>
```

Next, open your view controller file and import the necessary frameworks:

```swift
import UIKit
import Vision
import Photos
import PhotoKit
```

Add a button to your view controller's storyboard and create an action for it:

```swift
@IBAction func selectPhotoButtonPressed(_ sender: UIButton) {
    let photoAuthorizationStatus = PHPhotoLibrary.authorizationStatus()
    
    switch photoAuthorizationStatus {
    case .authorized:
        // Access the photo library
        presentPhotoPicker()
    case .notDetermined:
        // Request photo library access
        PHPhotoLibrary.requestAuthorization { [weak self] (authorizationStatus) in
            DispatchQueue.main.async {
                if authorizationStatus == .authorized {
                    // Access the photo library
                    self?.presentPhotoPicker()
                }
            }
        }
    case .denied, .restricted:
        // Show an alert for denied or restricted access
        showPhotoLibraryAccessDeniedAlert()
    @unknown default:
        break
    }
}
```

## Analyzing the image using Vision

Once we have access to the photo library, we can present the photo picker and allow the user to select an image.

```swift
func presentPhotoPicker() {
    let picker = UIImagePickerController()
    picker.delegate = self
    picker.sourceType = .photoLibrary
    picker.allowsEditing = false
    present(picker, animated: true, completion: nil)
}
```

Implement the `UIImagePickerControllerDelegate` methods to handle the selected image:

```swift
extension ViewController: UIImagePickerControllerDelegate, UINavigationControllerDelegate {
    func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [UIImagePickerController.InfoKey : Any]) {
        dismiss(animated: true, completion: nil)
        
        guard let image = info[.originalImage] as? UIImage else {
            return
        }
        
        // Analyze the image using Vision
        recognizeText(in: image)
    }
    
    func imagePickerControllerDidCancel(_ picker: UIImagePickerController) {
        dismiss(animated: true, completion: nil)
    }
}
```

## Extracting recognized text

Now that we have the selected image, we can use the Vision framework to recognize and extract the text from it.

```swift
func recognizeText(in image: UIImage) {
    guard let cgImage = image.cgImage else {
        return
    }
    
    let requestHandler = VNImageRequestHandler(cgImage: cgImage, options: [:])
    
    let request = VNRecognizeTextRequest { [weak self] (request, error) in
        DispatchQueue.main.async {
            if let error = error {
                print("Error: \(error)")
                return
            }
            
            guard let observations = request.results as? [VNRecognizedTextObservation], !observations.isEmpty else {
                print("No text found")
                return
            }
            
            for observation in observations {
                guard let topCandidate = observation.topCandidates(1).first else {
                    continue
                }
                
                print("Recognized text: \(topCandidate.string)")
            }
        }
    }
    
    do {
        try requestHandler.perform([request])
    } catch {
        print("Error: \(error)")
    }
}
```

## Conclusion

In this tutorial, we have shown how to use the Vision and PhotoKit frameworks in Swift to recognize and extract text from photos. By combining the power of computer vision and photo library access, you can create powerful applications for tasks such as document scanning, text extraction, and more. Make sure to experiment further with the Vision framework to explore its other capabilities beyond text recognition.

References:
- [Apple Developer Documentation - Vision](https://developer.apple.com/documentation/vision)
- [Apple Developer Documentation - PhotoKit](https://developer.apple.com/documentation/photokit)