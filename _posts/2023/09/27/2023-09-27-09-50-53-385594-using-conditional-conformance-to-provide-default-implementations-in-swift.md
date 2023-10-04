---
layout: post
title: "Using conditional conformance to provide default implementations in Swift"
description: " "
date: 2023-09-27
tags: [SwiftProgramming]
comments: true
share: true
---

One of the powerful features in Swift is conditional conformance, which allows you to provide default implementations for protocols based on certain conditions. This feature is particularly useful when working with generics and protocols in Swift, as it enables you to define behavior that is specific to certain types without having to repeat the same code multiple times. In this blog post, we will explore how to use conditional conformance to provide default implementations in Swift.

## What is Conditional Conformance?

Conditional conformance is a feature that was introduced in Swift 4.1. It allows you to define conformances to a protocol for a generic type, but only under certain conditions. This means that you can provide different implementations of a protocol depending on the properties or constraints of the generic type.

## Example Scenario: Custom Logging for Different Types

Let's consider a scenario where you have a `Logger` protocol that defines a `log` method, and a few types that you want to log. For simplicity, let's assume that the types are `String`, `Int`, and `URL`.

```swift
protocol Logger {
    func log()
}

struct StringLogger: Logger {
    let value: String

    func log() {
        print("String logger: \(value)")
    }
}

struct IntLogger: Logger {
    let value: Int

    func log() {
        print("Int logger: \(value)")
    }
}

struct URLLogger: Logger {
    let value: URL

    func log() {
        print("URL logger: \(value)")
    }
}
```

In this example, we have created separate logger types for each of the types we want to log. However, this approach can become cumbersome if we have many types or if we want to extend our logging functionality in the future.

## Using Conditional Conformance for Default Implementations

To avoid the need for separate logger types, we can use conditional conformance to provide default implementations for the `Logger` protocol. Let's refactor our code to take advantage of this feature.

```swift
protocol Logger {
    func log()
}

extension Logger where Self: CustomStringConvertible {
    func log() {
        print("\(Self.self) logger: \(description)")
    }
}

extension String: Logger {}
extension Int: Logger {}
extension URL: Logger {}
```

In this refactored code, we are extending the `Logger` protocol and providing a default implementation for types that conform to the `CustomStringConvertible` protocol. By doing this, we are effectively saying that any type that can be converted to a string using the `description` property can also be logged.

With this approach, we can now log instances of `String`, `Int`, and `URL` without the need for separate logger types.

## Conclusion

Conditional conformance in Swift is a powerful feature that allows you to provide default implementations for protocols based on certain conditions. By using conditional conformance, you can reduce code duplication and make your code more reusable and flexible. It is particularly useful when working with generics and protocols.

In this blog post, we have explored an example scenario where we used conditional conformance to provide default implementations for a `Logger` protocol. By defining default behavior based on the `CustomStringConvertible` conformance, we were able to log different types without the need for separate logger types.

#Swift #SwiftProgramming