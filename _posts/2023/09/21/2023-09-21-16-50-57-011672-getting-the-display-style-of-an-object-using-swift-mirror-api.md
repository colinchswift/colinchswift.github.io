---
layout: post
title: "Getting the display style of an object using Swift Mirror API"
description: " "
date: 2023-09-21
tags: []
comments: true
share: true
---

To get the display style, we can first create a Mirror instance using the `Mirror(reflecting:)` initializer and then access its `displayStyle` property. Let's look at an example:

```swift
// Create a sample object
struct Person {
    let name: String
    let age: Int
}

let person = Person(name: "John", age: 25)

// Get the Mirror instance for the object
let mirror = Mirror(reflecting: person)

// Access the display style
let displayStyle = mirror.displayStyle

// Check and print the display style
if let style = displayStyle {
    print("Display Style: \(style)")
} else {
    print("Display Style not available")
}
```

In this example, we have a `Person` struct with two properties: `name` and `age`. We create an instance of this struct, and then create a `Mirror` instance using `Mirror(reflecting:)` with the person object as the parameter. 

Next, we access the `displayStyle` property of the mirror instance, which gives us an optional value representing the display style of the object. We use optional binding to check if the display style exists, and then print it out.

To run this code and see the display style of the object, you can copy it into a Swift playground or a Swift file in Xcode.