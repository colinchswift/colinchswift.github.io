---
layout: post
title: "Adding PDF and document support in ViewControllers in Swift"
description: " "
date: 2023-09-23
tags: [tech]
comments: true
share: true
---

In this blog post, we will learn how to add PDF and document support in ViewControllers using Swift. We will be using the `UIDocumentPickerViewController` and the `PDFKit` framework to achieve this functionality.

## Prerequisites
Before we begin, ensure that you have the following in place:
- Xcode installed on your machine
- Basic knowledge of Swift and UIKit

## Step 1: Import PDFKit Framework
To get started, we need to import the `PDFKit` framework into our project. To do this, open your Xcode project and navigate to your target settings. Under the "General" tab, scroll down to the "Frameworks, Libraries, and Embedded Content" section. Click the "+" button and select "PDFKit.framework" from the list. This will add the framework to your project.

## Step 2: Create a PDF Viewer ViewController
Next, let's create a new View Controller that will serve as our PDF viewer. Create a new Swift file, name it `PDFViewController.swift`, and add the following code:

```swift
import UIKit
import PDFKit

class PDFViewController: UIViewController {

    var pdfView: PDFView!
    var documentURL: URL!

    override func viewDidLoad() {
        super.viewDidLoad()

        pdfView = PDFView(frame: view.bounds)
        view.addSubview(pdfView)
        
        let document = PDFDocument(url: documentURL)
        pdfView.document = document
    }
}
```

In the code above, we import the necessary modules, create a `PDFView` instance, and set its frame to match the size of the view. We then create a `PDFDocument` using the provided `documentURL` and assign it to the `pdfView`.

## Step 3: Add Document Picker ViewController
To enable document picking, we will use the `UIDocumentPickerViewController`. This allows users to select PDF and other document files from various sources. Add the following code to your existing View Controller to display the document picker:

```swift
func showDocumentPicker() {
    let documentPicker = UIDocumentPickerViewController(documentTypes: ["public.content"], in: .import)
    documentPicker.delegate = self
    present(documentPicker, animated: true, completion: nil)
}

extension ViewController: UIDocumentPickerDelegate {

    func documentPicker(_ controller: UIDocumentPickerViewController, didPickDocumentsAt urls: [URL]) {
        guard let selectedDocumentURL = urls.first else { return }
        let pdfViewController = PDFViewController()
        pdfViewController.documentURL = selectedDocumentURL
        navigationController?.pushViewController(pdfViewController, animated: true)
    }
}
```

In the code above, we define a function `showDocumentPicker()` which presents the `UIDocumentPickerViewController`. We set the delegate to `self` and implement the delegate method `documentPicker(_:didPickDocumentsAt:)` to handle the document selection. We create an instance of `PDFViewController`, assign the selected document URL, and push it onto the navigation stack for display.

## Step 4: Trigger Document Picker
To trigger the document picker, add a button to your existing View Controller and connect it to the `showDocumentPicker()` method. This can be done using Interface Builder or programmatically.

```swift
@IBAction func selectDocument(_ sender: UIButton) {
    showDocumentPicker()
}
```

And that's it! You have now successfully added PDF and document support to your View Controllers in Swift.

Remember to import the necessary modules, create a `PDFView` instance, and assign the `PDFDocument` to the `pdfView` to display the PDF. Utilize the `UIDocumentPickerViewController` to enable users to select PDF and document files from various sources.

#tech #swift