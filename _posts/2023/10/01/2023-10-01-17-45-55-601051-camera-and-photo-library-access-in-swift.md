---
layout: post
title: "Camera and photo library access in Swift"
description: " "
date: 2023-10-01
tags: []
comments: true
share: true
---

With the advancement of mobile technology, allowing users to access the camera and photo library has become an essential feature in many apps. In this tutorial, we will explore how to implement camera and photo library access in Swift using the AVFoundation framework.

## Camera Access

To provide camera access in your app, you need to request permission from the user and then capture photos or videos.

### Requesting Camera Authorization

To request camera authorization, you need to add the `NSCameraUsageDescription` key in your app's `Info.plist` file with a brief description of why your app needs access to the camera. 

```swift
<key>NSCameraUsageDescription</key>
<string>Allow this app to access the camera for capturing photos and videos.</string>
```

Next, you can use the `AVCaptureDevice` class to check and request camera authorization.

```swift
import AVFoundation

func requestCameraPermission() {
    AVCaptureDevice.requestAccess(for: .video) { authorized in
        if authorized {
            // User granted access to the camera
        } else {
            // User denied access to the camera
        }
    }
}
```

### Capturing Photos

To capture photos using the camera, you can utilize the `AVCaptureSession` and `AVCapturePhotoOutput` classes.

```swift
import AVFoundation

func capturePhoto() {
    guard let device = AVCaptureDevice.default(for: .video),
          let input = try? AVCaptureDeviceInput(device: device) else {
        return
    }

    let session = AVCaptureSession()

    guard session.canAddInput(input) else {
        return
    }

    session.addInput(input)

    let output = AVCapturePhotoOutput()

    guard session.canAddOutput(output) else {
        return
    }

    session.addOutput(output)

    session.startRunning()

    let settings = AVCapturePhotoSettings()

    output.capturePhoto(with: settings, delegate: self)
}
```

### Photo Library Access

To allow users to choose photos from their photo library, you need to request permission and present the photo library picker.

### Requesting Photo Library Authorization

To request photo library authorization, you need to add the `NSPhotoLibraryUsageDescription` key in your app's `Info.plist` file with a brief description of why your app needs access to the photo library. 

```swift
<key>NSPhotoLibraryUsageDescription</key>
<string>Allow this app to access your photo library to choose photos.</string>
```

Next, you can use the `PHPhotoLibrary` class to check and request photo library authorization.

```swift
import Photos

func requestPhotoLibraryPermission() {
    PHPhotoLibrary.requestAuthorization { status in
        switch status {
        case .authorized:
            // User granted access to the photo library
        case .denied, .restricted:
            // User denied or restricted access to the photo library
        case .notDetermined:
            // Authorization status not determined
        @unknown default:
            break
        }
    }
}
```

### Presenting Photo Library Picker

To present the photo library picker, you can use the `UIImagePickerController` class.

```swift
import UIKit

class ViewController: UIViewController, UIImagePickerControllerDelegate, UINavigationControllerDelegate {
    
    func presentPhotoPicker() {
        let picker = UIImagePickerController()
        picker.sourceType = .photoLibrary
        picker.delegate = self
        present(picker, animated: true, completion: nil)
    }
    
    func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [UIImagePickerController.InfoKey : Any]) {
        let image = info[.originalImage] as? UIImage
        // Use the selected image
        dismiss(animated: true, completion: nil)
    }
    
    func imagePickerControllerDidCancel(_ picker: UIImagePickerController) {
        dismiss(animated: true, completion: nil)
    }
}
```

## Conclusion

In this tutorial, we learned how to implement camera and photo library access in Swift using the AVFoundation and Photos frameworks. By providing camera and photo library access, your app can enhance the user experience and allow them to capture and choose photos effortlessly.

#iOS #Swift