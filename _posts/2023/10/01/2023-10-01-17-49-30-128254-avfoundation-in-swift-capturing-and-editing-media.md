---
layout: post
title: "AVFoundation in Swift: capturing and editing media"
description: " "
date: 2023-10-01
tags: [AVFoundation, Swift]
comments: true
share: true
---

In this blog post, we will explore the AVFoundation framework in Swift, which provides powerful tools for capturing and editing media. Whether you are building a photography app or a video editing app, AVFoundation offers the flexibility and functionality you need.

## Capturing Media

AVFoundation allows you to capture various types of media, including photos and videos, using the device's built-in camera. Let's look at a simple example of capturing a photo:

```swift
import AVFoundation
import UIKit

class CameraViewController: UIViewController, AVCapturePhotoCaptureDelegate {
    var captureSession: AVCaptureSession!
    var photoOutput: AVCapturePhotoOutput!
    var previewLayer: AVCaptureVideoPreviewLayer!
    var capturedImage: UIImage?

    override func viewDidLoad() {
        super.viewDidLoad()
        
        captureSession = AVCaptureSession()
        captureSession.sessionPreset = AVCaptureSession.Preset.photo
        
        guard let backCamera = AVCaptureDevice.default(for: .video) else { return }
        
        do {
            let input = try AVCaptureDeviceInput(device: backCamera)
            
            if captureSession.canAddInput(input) {
                captureSession.addInput(input)
                
                photoOutput = AVCapturePhotoOutput()
                
                if captureSession.canAddOutput(photoOutput) {
                    captureSession.addOutput(photoOutput)
                    
                    previewLayer = AVCaptureVideoPreviewLayer(session: captureSession)
                    previewLayer.videoGravity = AVLayerVideoGravity.resizeAspectFill
                    previewLayer.connection?.videoOrientation = AVCaptureVideoOrientation.portrait
                    view.layer.addSublayer(previewLayer)
                    
                    captureSession.startRunning()
                }
            }
        } catch {
            print(error.localizedDescription)
        }
    }
    
    // Capture button action
    @IBAction func captureButtonPressed(_ sender: UIButton) {
        let settings = AVCapturePhotoSettings()
        photoOutput.capturePhoto(with: settings, delegate: self)
    }
    
    // Delegate method for captured photo
    func photoOutput(_ output: AVCapturePhotoOutput, didFinishProcessingPhoto photo: AVCapturePhoto, error: Error?) {
        if let imageData = photo.fileDataRepresentation(), let image = UIImage(data: imageData) {
            capturedImage = image
            // Save or display the captured image here
        }
    }
    
    override func viewDidLayoutSubviews() {
        super.viewDidLayoutSubviews()
        previewLayer.frame = view.bounds
    }
}
```

This code sets up a `CameraViewController` and captures photos using the device's back camera. The captured image is stored in the `capturedImage` variable and can be saved or displayed as needed.

## Editing Media

AVFoundation also provides powerful tools for editing media, allowing you to manipulate photos and videos in various ways. Let's take a look at an example of applying a filter to a captured image:

```swift
import AVFoundation
import UIKit

class EditViewController: UIViewController {
    var imageView: UIImageView!

    override func viewDidLoad() {
        super.viewDidLoad()
        
        imageView = UIImageView(frame: view.bounds)
        imageView.contentMode = .scaleAspectFit
        view.addSubview(imageView)
        
        // Assuming capturedImage contains the captured photo
        if let capturedImage = capturedImage {
            
            let ciImage = CIImage(image: capturedImage)
            
            guard let colorFilter = CIFilter(name: "CIColorControls") else { return }
            colorFilter.setValue(ciImage, forKey: kCIInputImageKey)
            colorFilter.setValue(1.0, forKey: kCIInputSaturationKey)
            
            if let filteredImage = colorFilter.outputImage {
                let context = CIContext(options: nil)
                if let cgImage = context.createCGImage(filteredImage, from: filteredImage.extent) {
                    let processedImage = UIImage(cgImage: cgImage)
                    imageView.image = processedImage
                    // Display the filtered image here
                }
            }
        }
    }
}
```

This code sets up an `EditViewController` and applies a `CIColorControls` filter to the captured image. The resulting filtered image is then displayed in the `imageView`.

With AVFoundation, you have access to a wide range of features for capturing and editing media. Take advantage of this powerful framework to create engaging and interactive apps that make the most of the device's camera capabilities.

#AVFoundation #Swift