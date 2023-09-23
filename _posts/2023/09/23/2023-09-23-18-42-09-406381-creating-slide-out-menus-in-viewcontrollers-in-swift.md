---
layout: post
title: "Creating slide-out menus in ViewControllers in Swift"
description: " "
date: 2023-09-23
tags: [selector(toggleMenu), swift]
comments: true
share: true
---

In this tutorial, we will learn how to create slide-out menus in ViewControllers using Swift. Slide-out menus, also known as side menus or navigation drawers, provide a convenient way to display additional options or navigation links in an app.

To get started, let's create a new project in Xcode and select the "Single View App" template. Now, follow these steps:

**Step 1**: Add a `UIViewController` subclass for the menu. Let's name it `MenuViewController`.

```swift
class MenuViewController: UIViewController {
    // Implement the menu logic here
}
```

**Step 2**: Create the main `UIViewController` where the slide-out menu will be displayed. Let's name it `MainViewController`.

```swift
class MainViewController: UIViewController {
    var menuVc: MenuViewController?

    override func viewDidLoad() {
        super.viewDidLoad()

        // Add a button to toggle the slide-out menu
        let toggleMenuButton = UIButton(type: .system)
        toggleMenuButton.setTitle("Menu", for: .normal)
        toggleMenuButton.addTarget(self, action: #selector(toggleMenu), for: .touchUpInside)
        self.view.addSubview(toggleMenuButton)
    }

    // Toggle the slide-out menu
    @objc func toggleMenu() {
        if menuVc == nil {
            menuVc = MenuViewController()
            addChild(menuVc!)
            view.addSubview(menuVc!.view)
            menuVc!.didMove(toParent: self)
        } else {
            menuVc!.willMove(toParent: nil)
            menuVc!.view.removeFromSuperview()
            menuVc!.removeFromParent()
            menuVc = nil
        }
    }
}
```

**Step 3**: Implement the desired functionality and layout in the `MenuViewController` class. You can add buttons, labels, or any other UI elements to customize the menu.

```swift
class MenuViewController: UIViewController {
    override func viewDidLoad() {
        super.viewDidLoad()

        view.backgroundColor = .white

        // Example: Add buttons to the menu
        let button1 = UIButton(type: .system)
        button1.setTitle("Option 1", for: .normal)
        view.addSubview(button1)

        let button2 = UIButton(type: .system)
        button2.setTitle("Option 2", for: .normal)
        view.addSubview(button2)
    }

    override func viewDidLayoutSubviews() {
        super.viewDidLayoutSubviews()

        // Example: Layout the buttons in the menu
        button1.frame = CGRect(x: 20, y: 50, width: view.frame.width - 40, height: 40)
        button2.frame = CGRect(x: 20, y: 100, width: view.frame.width - 40, height: 40)
    }
}
```

**Step 4**: Connect the menu to the main view controller. Update the `AppDelegate.swift` file to set the `MainViewController` as the initial view controller.

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    // ...
    let mainVc = MainViewController()
    let navigationController = UINavigationController(rootViewController: mainVc)
    window?.rootViewController = navigationController
    window?.makeKeyAndVisible()
    // ...
    return true
}
```

That's it! Now, when you run the app and tap the "Menu" button, the slide-out menu will appear from the side. You can customize the menu's appearance and functionality according to your app's needs.

Remember to handle additional features such as dismissing the menu when an option is selected or navigating to a different view controller.

I hope you found this tutorial helpful for creating slide-out menus in ViewControllers using Swift! #swift #viewcontrollers