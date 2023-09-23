---
layout: post
title: "Presenting a ViewController modally in Swift"
description: " "
date: 2023-09-23
tags: [selector(presentModal), iOSDevelopment]
comments: true
share: true
---

In iOS development, it is common to present a new view controller modally when you want to temporarily interrupt the current flow and display a different screen. Modally presented view controllers are displayed on top of the current screen and provide a focused interaction. In this blog post, we will explore how to present a view controller modally in Swift.

To present a view controller modally, you need to follow these steps:

1. Create the view controller you want to present. Let's call it `ModalViewController`.

```swift
import UIKit

class ModalViewController: UIViewController {
    override func viewDidLoad() {
        super.viewDidLoad()
        // Additional setup code
    }
    
    // ... Rest of the class implementation
}
```

2. Inside your existing view controller, create a button or any trigger event that will initiate the presentation of the `ModalViewController`.

```swift
import UIKit

class MainViewController: UIViewController {
    override func viewDidLoad() {
        super.viewDidLoad()
        // Additional setup code
        
        let button = UIButton(type: .system)
        button.setTitle("Present Modal", for: .normal)
        button.addTarget(self, action: #selector(presentModal), for: .touchUpInside)
        view.addSubview(button)
        
        // ... Rest of the class implementation
    }
    
    @objc func presentModal() {
        let modalViewController = ModalViewController()
        let navigationController = UINavigationController(rootViewController: modalViewController)
        navigationController.modalPresentationStyle = .fullScreen
        present(navigationController, animated: true, completion: nil)
    }
    
    // ... Rest of the class implementation
}
```

The `presentModal` method is triggered when the button is pressed. It creates an instance of the `ModalViewController` and embeds it in a `UINavigationController` to provide a navigation bar. Finally, the `modalPresentationStyle` is set to `.fullScreen` to present the view controller in fullscreen mode. The `present(_:animated:completion:)` method is called to present the view controller modally.

3. Build and run the application. When you tap the "Present Modal" button, the `ModalViewController` will be displayed on top of the current screen.

That's it! Now you know how to present a view controller modally in Swift. You can customize the presentation style and add additional logic as needed.

#iOSDevelopment #Swift