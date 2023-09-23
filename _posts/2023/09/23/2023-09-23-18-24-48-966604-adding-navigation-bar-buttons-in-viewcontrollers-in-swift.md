---
layout: post
title: "Adding navigation bar buttons in ViewControllers in Swift"
description: " "
date: 2023-09-23
tags: [selector(menuButtonTapped)), selector(searchButtonTapped))]
comments: true
share: true
---

When building iOS apps, it's often useful to add navigation bar buttons to provide users with additional functionality. These buttons can be used to perform actions, navigate to different screens, or trigger specific behaviors in your app.

In this tutorial, we will walk you through the steps to add navigation bar buttons in ViewControllers using Swift.

## Step 1: Create a new UIViewController

First, create a new UIViewController subclass in your project. You can do this by selecting **File** > **New** > **File.** Choose **Swift File** as the template and name your file and class accordingly.

## Step 2: Customize the Navigation Bar

To add navigation bar buttons, we need to customize the navigation bar in our view controller. In the `viewDidLoad` method of your view controller, add the following code:

```swift
override func viewDidLoad() {
    super.viewDidLoad()

    // Customize the navigation bar appearance
    navigationController?.navigationBar.barTintColor = .blue
    navigationController?.navigationBar.tintColor = .white
    navigationController?.navigationBar.titleTextAttributes = [NSAttributedString.Key.foregroundColor: UIColor.white]
}
```

In this code snippet, we are setting the background color of the navigation bar to blue, the text color to white, and the title text attributes to a white color.

## Step 3: Add Navigation Bar Buttons

Now, let's add some buttons to our navigation bar. In the `viewDidLoad` method, add the following code to create and add the buttons:

```swift
override func viewDidLoad() {
    super.viewDidLoad()

    // Customize the navigation bar appearance
    navigationController?.navigationBar.barTintColor = .blue
    navigationController?.navigationBar.tintColor = .white
    navigationController?.navigationBar.titleTextAttributes = [NSAttributedString.Key.foregroundColor: UIColor.white]

    // Add left and right navigation bar buttons
    let leftButton = UIBarButtonItem(title: "Menu", style: .plain, target: self, action: #selector(menuButtonTapped))
    navigationItem.leftBarButtonItem = leftButton

    let rightButton = UIBarButtonItem(image: UIImage(named: "search"), style: .plain, target: self, action: #selector(searchButtonTapped))
    navigationItem.rightBarButtonItem = rightButton
}

@objc func menuButtonTapped() {
    // Handle menu button tap action
}

@objc func searchButtonTapped() {
    // Handle search button tap action
}
```

In this code snippet, we create two buttons: a left button with the title "Menu" and a right button with an image of a search icon. We then set the target and action for each button, which will be called when the buttons are tapped.

## Step 4: Handle Button Actions

To handle the button actions, we need to define the corresponding methods in our view controller. In the code snippet above, we added two methods: `menuButtonTapped` and `searchButtonTapped`. Customize these methods to perform the desired functionality for your app.

That's it! You have successfully added navigation bar buttons in ViewControllers using Swift. Customize the appearance and actions to fit your app's requirements.

Happy coding! 🚀

#iOS #Swift