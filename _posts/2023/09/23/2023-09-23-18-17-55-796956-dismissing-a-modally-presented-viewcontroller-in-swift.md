---
layout: post
title: "Dismissing a modally presented ViewController in Swift"
description: " "
date: 2023-09-23
tags: []
comments: true
share: true
---

To dismiss a modally presented view controller, there are a few different approaches you can take depending on your specific use case.

## Dismiss using Presenting View Controller

One straightforward way to dismiss a modally presented view controller is to use the reference to the presenting view controller. This approach is especially useful when you want to dismiss the presented view controller from a button action or when a specific event occurs.

Here's an example of how you can dismiss a modally presented view controller using the presenting view controller:

```swift
@IBAction func closeButtonTapped(_ sender: UIButton) {
    self.dismiss(animated: true, completion: nil)
}
```

In the above code snippet, we have an action method for a "Close" button. When this button is tapped, the `dismiss(animated:completion:)` method is called on the current view controller, which results in dismissing the modally presented view controller.

## Dismiss using Delegate Pattern

Another common approach to dismissing a modally presented view controller is by using the delegate pattern. This method is useful when you need to communicate between the presented view controller and the presenting view controller.

Here's an example of how to use the delegate pattern to dismiss a modally presented view controller:

1. First, define a protocol in the presented view controller:

```swift
protocol ModalViewControllerDelegate: class {
    func didDismissModalViewController()
}
```

2. Next, add a delegate property to the presented view controller:

```swift
class ModalViewController: UIViewController {
    weak var delegate: ModalViewControllerDelegate?
    
    // ...
}
```

3. When you want to dismiss the view controller, call the delegate method:

```swift
@IBAction func closeButtonTapped(_ sender: UIButton) {
    delegate?.didDismissModalViewController()
}
```

4. Finally, in the presenting view controller, implement the delegate method:

```swift
class PresentingViewController: UIViewController, ModalViewControllerDelegate {
    
    // ...
    
    func didDismissModalViewController() {
        dismiss(animated: true, completion: nil)
    }
}
```

In the above code snippets, we define the `ModalViewControllerDelegate` protocol in the presented view controller, set a delegate property, and finally call the delegate method to notify the presenting view controller to dismiss the modally presented view controller.

## Conclusion

Dismissing a modally presented view controller is a common task in iOS app development. By using the presenting view controller or the delegate pattern, you can easily handle the dismissal of a modally presented view controller in Swift. These approaches provide flexibility and enable seamless transitions in your app's user interface.

#iOS #Swift