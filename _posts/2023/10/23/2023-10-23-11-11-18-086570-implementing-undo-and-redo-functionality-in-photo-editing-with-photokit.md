---
layout: post
title: "Implementing undo and redo functionality in photo editing with PhotoKit"
description: " "
date: 2023-10-23
tags: []
comments: true
share: true
---

Undo and redo functionality are important tools to have in photo editing applications. These features allow users to revert changes they made to an image and restore the previous state. In this blog post, we will explore how to implement undo and redo functionality using PhotoKit, Apple's framework for working with the Photos app.

## Table of Contents
- [What is PhotoKit?](#what-is-photokit)
- [Implementing Undo Functionality](#implementing-undo-functionality)
- [Implementing Redo Functionality](#implementing-redo-functionality)
- [Conclusion](#conclusion)
- [References](#references)

## What is PhotoKit?
[PhotoKit](https://developer.apple.com/documentation/photokit) is a powerful framework provided by Apple to access and manage photos and videos in the user's Photos library. It allows developers to perform various photo editing operations, such as adjusting filters, cropping, and applying effects.

## Implementing Undo Functionality
To implement undo functionality, we need to keep track of the changes made to the photo editing session. One approach is to use the `PHContentEditingController` protocol, which provides methods to handle the editing session.

```swift
import Photos

class PhotoEditViewController: PHContentEditingController {
    var initialPhoto: PHContentEditingInput?
    var currentPhoto: PHContentEditingInput?
    var editedPhoto: PHContentEditingOutput?

    // Called when starting a new edit session
    override func startContentEditing(with contentEditingInput: PHContentEditingInput, placeholderImage: UIImage) {
        initialPhoto = contentEditingInput
        currentPhoto = contentEditingInput
        editedPhoto = nil
    }

    // Called when applying changes to the photo
    override func finishContentEditing(completionHandler: @escaping (PHContentEditingOutput?) -> Void) {
        // Apply the changes to the currentPhoto and generate the editedPhoto
        // ...
        
        completionHandler(editedPhoto)
    }

    // Implement undo functionality
    func undo() {
        currentPhoto = initialPhoto
        // Apply the changes to the currentPhoto to revert to the initial state
        // ...
        
        // Call this method to notify the system that the editedPhoto has changed
        self.editedContentIdentifier = UUID().uuidString
    }
}
```

In the above code, we create a custom `PhotoEditViewController` that extends `PHContentEditingController`. We keep track of the initial photo, current photo, and edited photo using `PHContentEditingInput` and `PHContentEditingOutput` objects.

The `startContentEditing(with:placeholderImage:)` method is called when the user starts a new editing session. We initialize the initialPhoto, currentPhoto, and editedPhoto properties here.

The `finishContentEditing(completionHandler:)` method is called when the user applies changes to the photo. We apply the changes to the currentPhoto and generate the editedPhoto. Finally, we call the completion handler with the editedPhoto.

To implement the undo functionality, we create the `undo()` method which sets the currentPhoto back to the initialPhoto and reverts the changes. We also need to call `self.editedContentIdentifier = UUID().uuidString` to notify the system that the editedPhoto has changed.

## Implementing Redo Functionality
To implement redo functionality, we can make use of a stack to keep track of the previous states of the photo editing session. Whenever an undo operation is performed, we push the previous state onto the stack. When redo is triggered, we pop the state from the stack and apply it to the currentPhoto.

```swift
class PhotoEditViewController: PHContentEditingController {
    // ...

    var undoStack: [PHContentEditingInput] = []
    var redoStack: [PHContentEditingInput] = []

    // ...
    
    // Implement undo functionality
    func undo() {
        redoStack.append(currentPhoto!)
        currentPhoto = undoStack.popLast()
        // Apply the changes to the currentPhoto to revert to the previous state
        // ...
        
        self.editedContentIdentifier = UUID().uuidString
    }

    // Implement redo functionality
    func redo() {
        undoStack.append(currentPhoto!)
        currentPhoto = redoStack.popLast()
        // Apply the changes to the currentPhoto to restore the state
        // ...
        
        self.editedContentIdentifier = UUID().uuidString
    }
}
```

In the updated code, we introduce two stacks, `undoStack` and `redoStack`, to keep track of the previous and future states of the photo editing session.

When an undo operation is performed, we push the currentPhoto onto the redoStack and pop the previous state from the undoStack. We then apply the changes to the currentPhoto to revert to the previous state.

When redo is triggered, we push the currentPhoto onto the undoStack and pop the next state from the redoStack. We apply the changes to the currentPhoto to restore the state.

## Conclusion
Implementing undo and redo functionality in photo editing applications can greatly enhance the user experience. By leveraging PhotoKit and its content editing capabilities, we can easily keep track of the editing session's state and allow users to revert and restore changes effortlessly.

In this blog post, we discussed how to implement undo and redo functionality using PhotoKit. We explored the `PHContentEditingController` protocol, which provides methods to handle the editing session, and demonstrated how to use stacks to keep track of previous and future states.

By following these guidelines, you can make your photo editing app more user-friendly and provide an intuitive editing experience.

## References
- [PhotoKit - Apple Developer Documentation](https://developer.apple.com/documentation/photokit)
- [PHContentEditingController - Apple Developer Documentation](https://developer.apple.com/documentation/photos/phcontenteditingcontroller)