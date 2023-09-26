---
layout: post
title: "Interpolating attributed strings in a UILabel"
description: " "
date: 2023-09-26
tags: []
comments: true
share: true
---

When working with `UILabel`, we often need to display formatted text, such as making certain words bold or changing the text color. One powerful feature of `UILabel` is the ability to display attributed strings, which allow us to apply different formatting attributes to different parts of the text.

In this tutorial, we will explore how to interpolate attributed strings in a `UILabel`. Interpolating attributed strings can be useful in scenarios where we want to display dynamic content with certain parts formatted differently.

Let's dive into the code!

## Creating an attributed string

First, we need to create an `NSMutableAttributedString` and set its text and attributes. Here's an example of how to create an attributed string with some formatted parts:

```swift
let attributedString = NSMutableAttributedString(string: "Hello, World!")

let boldAttributes: [NSAttributedString.Key: Any] = [
    .font: UIFont.boldSystemFont(ofSize: 16)
]
attributedString.addAttributes(boldAttributes, range: NSRange(location: 0, length: 5))

let redAttributes: [NSAttributedString.Key: Any] = [
    .foregroundColor: UIColor.red
]
attributedString.addAttributes(redAttributes, range: NSRange(location: 7, length: 6))
```

In the above code snippet, we create an `NSMutableAttributedString` with the text "Hello, World!". Then, we define two sets of attributes: one for making the first 5 characters bold and another for changing the color of the characters from index 7 to index 12 to red. Finally, we apply these attributes to the corresponding ranges in the attributed string using the `addAttributes(_:range:)` method.

## Displaying the attributed string in a UILabel

Now that we have our formatted attributed string, we can set it as the text of a `UILabel`:

```swift
let label = UILabel()
label.attributedText = attributedString
```

By setting the `attributedText` property of the `UILabel` to our `NSMutableAttributedString`, the label will display the text with the applied formatting.

## Interpolating attributed strings

To interpolate attributed strings, we can apply the same logic as above but dynamically generate the ranges and attributes based on the content we want to display.

For example, let's say we have a username that we want to display in bold, followed by some regular text:

```swift
let username = "JohnDoe"
let content = "Hello, welcome to our platform!"

let attributedString = NSMutableAttributedString(string: "\(username) \(content)")

let boldAttributes: [NSAttributedString.Key: Any] = [
    .font: UIFont.boldSystemFont(ofSize: 16)
]
attributedString.addAttributes(boldAttributes, range: NSRange(location: 0, length: username.count))
```

In the above code snippet, we interpolate the `username` into the string using string interpolation (`\(username)`). Then, we dynamically calculate the range for the `username` part of the attributed string based on the length of the `username` string.

We can continue to add more formatting attributes as needed, just like in the previous example.

## Conclusion

Interpolating attributed strings in a `UILabel` allows us to display dynamic content with specific formatting for different parts of the text. By creating an `NSMutableAttributedString` and applying attributes to specific ranges, we can achieve the desired formatting effects.