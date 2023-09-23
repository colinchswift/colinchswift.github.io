---
layout: post
title: "Creating custom keyboards in ViewControllers in Swift"
description: " "
date: 2023-09-23
tags: [selector(buttonTapped(_, customkeyboard]
comments: true
share: true
---

In iOS development, creating custom keyboards for specific use cases can greatly enhance the user experience. Whether it's providing a specialized keyboard for entering numbers, dates, or custom characters, implementing a custom keyboard can make data input efficient and intuitive.

In this blog post, we will explore how to create custom keyboards in a `UIViewController` using Swift.

## Step 1: Creating the Custom Keyboard View

First, we need to create the custom keyboard view that will be displayed to the user. This view can be designed using Interface Builder or programmatically within the `UIViewController`. Let's create a simple example using the latter approach.

```swift
class CustomKeyboardViewController: UIViewController {
    private let numberButton: UIButton = {
        let button = UIButton(type: .system)
        button.setTitle("1", for: .normal)
        button.titleLabel?.font = .systemFont(ofSize: 20)
        button.addTarget(self, action: #selector(buttonTapped(_:)), for: .touchUpInside)
        return button
    }()

    // Add additional buttons as needed

    override func viewDidLoad() {
        super.viewDidLoad()
        setupKeyboardView()
    }

    private func setupKeyboardView() {
        let keyboardView = UIView()
        keyboardView.translatesAutoresizingMaskIntoConstraints = false

        view.addSubview(keyboardView)

        NSLayoutConstraint.activate([
            keyboardView.leadingAnchor.constraint(equalTo: view.leadingAnchor),
            keyboardView.trailingAnchor.constraint(equalTo: view.trailingAnchor),
            keyboardView.bottomAnchor.constraint(equalTo: view.bottomAnchor),
            keyboardView.heightAnchor.constraint(equalToConstant: 200)
        ])

        keyboardView.addSubview(numberButton)

        NSLayoutConstraint.activate([
            numberButton.centerXAnchor.constraint(equalTo: keyboardView.centerXAnchor),
            numberButton.centerYAnchor.constraint(equalTo: keyboardView.centerYAnchor)
        ])
    }

    @objc private func buttonTapped(_ sender: UIButton) {
        guard let buttonTitle = sender.titleLabel?.text else { return }
        // Handle button tap event
    }
}
```

In the above code, we have defined a `numberButton` that represents a button on our custom keyboard. We have set its title, font, target action, and added it to the keyboard view. You can add more buttons or customize the design as per your requirements.

## Step 2: Presenting the Custom Keyboard

Next, we need to present the custom keyboard when needed. This can be done by embedding the `CustomKeyboardViewController` within a `UIInputViewController`.

```swift
class CustomKeyboardInputViewController: UIInputViewController {
    private var customKeyboardViewController: CustomKeyboardViewController?

    override func viewDidLoad() {
        super.viewDidLoad()
        setupCustomKeyboard()
    }

    private func setupCustomKeyboard() {
        customKeyboardViewController = CustomKeyboardViewController()
        guard let keyboardView = customKeyboardViewController?.view else { return }
        keyboardView.translatesAutoresizingMaskIntoConstraints = false

        addChild(customKeyboardViewController!)
        view.addSubview(keyboardView)

        NSLayoutConstraint.activate([
            keyboardView.leadingAnchor.constraint(equalTo: view.leadingAnchor),
            keyboardView.trailingAnchor.constraint(equalTo: view.trailingAnchor),
            keyboardView.bottomAnchor.constraint(equalTo: view.bottomAnchor),
            keyboardView.heightAnchor.constraint(equalToConstant: 200)
        ])

        customKeyboardViewController?.didMove(toParent: self)
    }
}
```

In the above code, we create a `CustomKeyboardViewController` instance and add its view as a subview to the `UIInputViewController`. We also set up the necessary constraints to position the keyboard appropriately.

## Step 3: Using the Custom Keyboard

To utilize the custom keyboard, you need to set the input view of the text field or text view to our `CustomKeyboardInputViewController` and become the first responder.

```swift
let textField = UITextField()
let customKeyboardInputViewController = CustomKeyboardInputViewController()

textField.inputView = customKeyboardInputViewController.view
textField.becomeFirstResponder()
```

In the above code, we set the input view of a `UITextField` to the `CustomKeyboardInputViewController` and then make it the first responder to display the custom keyboard.

By following these steps, you can create and present custom keyboards in your `UIViewController`s in Swift. This technique allows you to provide a tailored input experience to users based on your specific requirements.

#customkeyboard #swift