---
layout: post
title: "Implementing a custom photo editing interface with filter controls and adjustment sliders using PhotoKit in Swift"
description: " "
date: 2023-10-23
tags: [selector, selector]
comments: true
share: true
---

In this tutorial, we will explore how to create a custom photo editing interface with filter controls and adjustment sliders using Apple's PhotoKit framework in Swift. PhotoKit provides a powerful set of APIs for accessing and manipulating photos in the user's photo library.

## Table of Contents
1. [Introduction to PhotoKit](#introduction-to-photokit)
2. [Setting Up the Project](#setting-up-the-project)
3. [Fetching and Displaying the Photo](#fetching-and-displaying-the-photo)
4. [Implementing Filter Controls](#implementing-filter-controls)
5. [Implementing Adjustment Sliders](#implementing-adjustment-sliders)
6. [Applying Filters and Adjustments](#applying-filters-and-adjustments)
7. [Conclusion](#conclusion)

## Introduction to PhotoKit

PhotoKit is a powerful framework provided by Apple that allows developers to access and manipulate photos in the user's photo library. It provides a high-level API for fetching and working with photos, as well as advanced editing capabilities like applying filters and adjustments.

## Setting Up the Project

To get started, create a new Xcode project and import the PhotoKit framework. Open the `ViewController.swift` file and import the necessary frameworks.

```swift
import UIKit
import Photos
import CoreImage
import CoreImage.CIFilterBuiltins
```

## Fetching and Displaying the Photo

First, we need to fetch and display the selected photo from the user's photo library. Add a `UIImageView` to the view controller's interface and create an outlet for it.

```swift
@IBOutlet weak var imageView: UIImageView!
```

Next, we need to request the user's permission to access their photo library. Add the following code to the `viewDidLoad` method.

```swift
PHPhotoLibrary.requestAuthorization { (status) in
    DispatchQueue.main.async {
        if status == .authorized {
            self.fetchAndDisplayPhoto()
        } else {
            // Handle authorization failure
        }
    }
}
```

In the `fetchAndDisplayPhoto` method, we will fetch the most recent photo from the library and display it in the `imageView`.

```swift
func fetchAndDisplayPhoto() {
    let fetchOptions = PHFetchOptions()
    fetchOptions.sortDescriptors = [NSSortDescriptor(key: "creationDate", ascending: false)]
    fetchOptions.fetchLimit = 1
    
    let fetchResult = PHAsset.fetchAssets(with: .image, options: fetchOptions)
    
    if let asset = fetchResult.firstObject {
        let requestOptions = PHImageRequestOptions()
        requestOptions.deliveryMode = .highQualityFormat
        requestOptions.isNetworkAccessAllowed = true
        
        PHImageManager.default().requestImage(for: asset, targetSize: CGSize(width: 1000, height: 1000), contentMode: .aspectFill, options: requestOptions) { (image, info) in
            DispatchQueue.main.async {
                self.imageView.image = image
            }
        }
    }
}
```

## Implementing Filter Controls

Now let's implement the filter controls. We will use a `UISegmentedControl` to allow the user to select different filters. Add the following code to the `viewDidLoad` method.

```swift
let filterControl = UISegmentedControl(items: ["Original", "Sepia", "Noir"])
filterControl.addTarget(self, action: #selector(filterControlValueChanged), for: .valueChanged)
filterControl.selectedSegmentIndex = 0
navigationItem.titleView = filterControl
```

Add the following action method to handle the value change event of the filter control.

```swift
@objc func filterControlValueChanged(sender: UISegmentedControl) {
    applyFilterAtIndex(sender.selectedSegmentIndex)
}
```

Next, we need to implement the `applyFilterAtIndex` method to apply the selected filter to the image.

```swift
func applyFilterAtIndex(_ index: Int) {
    guard let image = imageView.image else { return }
    
    // Create a CIImage from the UIImage
    guard let ciImage = CIImage(image: image) else { return }
    
    // Apply the selected filter
    let filter = CIFilter.sepiaTone()
    filter.inputImage = ciImage
    
    // Set the filter parameters based on the selected index
    switch index {
    case 1:
        filter.intensity = 0.8
    case 2:
        filter.intensity = 1.0
    default:
        break
    }
    
    // Get the filtered image
    guard let outputImage = filter.outputImage else { return }
    let context = CIContext()
    guard let cgImage = context.createCGImage(outputImage, from: outputImage.extent) else { return }
    
    // Convert the filtered image back to UIImage and display it
    imageView.image = UIImage(cgImage: cgImage)
}
```

## Implementing Adjustment Sliders

Next, let's implement the adjustment sliders to allow the user to tweak the filter parameters. Add the following code to the `viewDidLoad` method.

```swift
let intensitySlider = UISlider()
intensitySlider.minimumValue = 0.0
intensitySlider.maximumValue = 1.0
intensitySlider.addTarget(self, action: #selector(intensitySliderValueChanged), for: .valueChanged)
navigationController?.toolbar.addSubview(intensitySlider)
```

Add the following action method to handle the value change event of the intensity slider.

```swift
@objc func intensitySliderValueChanged(sender: UISlider) {
    applyIntensity(sender.value)
}
```

Next, implement the `applyIntensity` method to adjust the intensity of the filter.

```swift
func applyIntensity(_ intensity: Float) {
    guard let image = imageView.image else { return }
    
    // Create a CIImage from the UIImage
    guard let ciImage = CIImage(image: image) else { return }
    
    // Get the current filter
    guard let filter = CIFilter(name: "CISepiaTone") else { return }
    filter.setValue(ciImage, forKey: kCIInputImageKey)
    
    // Set the intensity parameter
    filter.setValue(intensity, forKey: kCIInputIntensityKey)
    
    // Get the filtered image
    guard let outputImage = filter.outputImage else { return }
    let context = CIContext()
    guard let cgImage = context.createCGImage(outputImage, from: outputImage.extent) else { return }
    
    // Convert the filtered image back to UIImage and display it
    imageView.image = UIImage(cgImage: cgImage)
}
```

## Applying Filters and Adjustments

Now, when the user selects a different filter from the segmented control or adjusts the intensity using the slider, the image will be updated accordingly.

## Conclusion

In this tutorial, we learned how to create a custom photo editing interface with filter controls and adjustment sliders using PhotoKit in Swift. We covered fetching and displaying photos, implementing filter controls, and adjusting filter parameters. PhotoKit provides a powerful set of APIs for working with photos, allowing you to create rich photo editing experiences in your apps.

#hashtags: #Swift #PhotoKit