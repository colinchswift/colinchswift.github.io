---
layout: post
title: "Conditional conformance for convenience initializers in Swift"
description: " "
date: 2023-09-27
tags: [conditional]
comments: true
share: true
---

## Introduction

With the introduction of Swift 5.5, developers gained a powerful new feature called "Conditional Conformance". This allows types to conditionally conform to a protocol based on certain requirements. One area where this feature can be particularly useful is when working with convenience initializers. In this blog post, we will explore how to leverage conditional conformance to enable convenience initializers for conditional conformances in Swift.

## Understanding Conditional Conformance

First, let's quickly recap what conditional conformance is all about. Prior to Swift 5.5, conforming to a protocol in Swift was an all-or-nothing situation. Once a type conformed to a protocol, it had to fulfill all the requirements of that protocol. This could be restrictive in some cases where only certain conditions applied.

Conditional conformance allows for a more flexible approach. It enables types to conform to a protocol only when specific conditions are met. This allows for more granular control over protocol conformance and improves code clarity and maintainability.

## Conditional Conformance for Convenience Initializers

Imagine a scenario where you have a generic type `Box` that can hold any type of value. You want to provide a convenience initializer that creates a new `Box` instance with a default value. However, this convenience initializer should only be available for types that conform to the `DefaultValueProvidable` protocol, which defines a static property `defaultValue`.

Here's how you can achieve this using conditional conformance:

```swift
protocol DefaultValueProvidable {
    static var defaultValue: Self { get }
}

struct Box<T> {
    var value: T

    init(value: T) {
        self.value = value
    }
}

extension Box where T: DefaultValueProvidable {
    init() {
        self.init(value: T.defaultValue)
    }
}
```

In the code snippet above, we define the `DefaultValueProvidable` protocol, which requires a static property `defaultValue`. Then, we define the `Box` struct with a generic type `T` and a regular initializer that accepts a value of type `T`. 

Next, we extend `Box` and provide a convenience initializer using conditional conformance. The initializer is only available for `Box` instances where `T` conforms to `DefaultValueProvidable`. Inside the initializer, we create a new `Box` instance with the default value provided by `T.defaultValue`.

## Conclusion

Conditional conformance is a powerful feature in Swift that allows for more flexibility and control when conforming to protocols. Leveraging conditional conformance for convenience initializers can lead to cleaner and more expressive code. In this blog post, we explored an example of how to use conditional conformance to enable convenience initializers for types that meet certain conditions. Hopefully, this gives you a good starting point to leverage this feature in your own projects.

#swift #conditional-conformance