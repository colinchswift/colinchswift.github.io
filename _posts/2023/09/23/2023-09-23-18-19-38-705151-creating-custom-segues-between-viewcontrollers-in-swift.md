---
layout: post
title: "Creating custom segues between ViewControllers in Swift"
description: " "
date: 2023-09-23
tags: [iOSDevelopment, Swift]
comments: true
share: true
---

When working with iOS app development, segues provide a convenient way to transition between ViewControllers. By default, iOS provides several standard segues like `show`, `show detail`, `present modally`, and more. These segues come with pre-defined animations and behaviors.

However, there may be scenarios when you want to create custom segues with your own animations or transitions. In this blog post, we will explore how to create custom segues between ViewControllers using Swift.

## Step 1: Create a Custom Segue Class

To create a custom segue, we need to subclass the `UIStoryboardSegue` class and override the `perform()` method. This method provides the opportunity to define our own animations or transitions.

```swift
import UIKit

class CustomSegue: UIStoryboardSegue {

    override func perform() {
        // Implement custom animations or transitions
    }
}
```

In the `perform()` method, you can implement the desired animations or transitions that you want to occur when the segue is triggered.

## Step 2: Assign the Custom Segue Class

Now that we have created our custom segue class, we need to assign it to a specific segue in the Interface Builder.

1. Open the storyboard where the segue is defined.
2. Select the segue connecting the source and destination ViewControllers.
3. In the Attributes Inspector, find the "Class" dropdown and select the custom segue class name (e.g., `CustomSegue`).

## Step 3: Implement the Custom Transition

Inside the `perform()` method, you have the flexibility to implement any custom animation or transition. Here's an example of a custom transition where the destination ViewController slides in from the right side.

```swift
override func perform() {
    let sourceViewController = self.source
    let destinationViewController = self.destination

    sourceViewController.view.addSubview(destinationViewController.view)
    
    let destinationView = destinationViewController.view
    destinationView?.frame.origin.x = sourceViewController.view.frame.size.width
    
    UIView.animate(withDuration: 0.5, animations: {
        destinationView?.frame.origin.x = 0
    }) { (finished) in
        sourceViewController.present(destinationViewController, animated: false, completion: nil)
    }
}
```

In this example, we add the destination ViewController's view as a subview to the source ViewController's view. Then, we set the initial position of the destination view outside the screen bounds. Finally, we animate the destination view to slide in from the right side and present it once the animation is completed.

## Conclusion

Creating custom segues allows you to implement unique and visually appealing transitions between ViewControllers in your iOS app. By subclassing the `UIStoryboardSegue` class and implementing your own animations or transitions, you can achieve the desired visual effects and enhance the user experience.

Remember to assign the custom segue class to the desired segue in the Interface Builder to ensure that your custom segue is triggered when the appropriate event occurs.

#iOSDevelopment #Swift