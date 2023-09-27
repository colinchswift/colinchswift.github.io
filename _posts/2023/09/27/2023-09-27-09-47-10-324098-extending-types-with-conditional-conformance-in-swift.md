---
layout: post
title: "Extending types with conditional conformance in Swift"
description: " "
date: 2023-09-27
tags: [Swift, ConditionalConformance]
comments: true
share: true
---

In Swift, conditional conformance allows us to extend existing types to conform to a protocol depending on certain conditions. This powerful feature was introduced in Swift 4.1 and is extremely useful when we want to provide specific functionality only for certain types.

## What is Conditional Conformance?

Conditional conformance enables us to extend a generic type to conform to a protocol, but only if certain conditions are met. These conditions can be expressed using where clauses in the extension declaration. With conditional conformance, we can easily define behavior for certain types while leaving others untouched.

## Example Scenario

Let's imagine we have a `Container` protocol that represents a container that can hold elements. We want to add a method `isEmpty()` which returns true if the container is empty, but only for containers whose elements conform to the `Equatable` protocol.

```swift
protocol Container {
    associatedtype Element
    var count: Int { get }
    func add(element: Element)
    func remove(element: Element)
}

extension Container {
    func isEmpty() -> Bool {
        return count == 0
    }
}

```

## Conditional Conformance

Now, we want to extend the `Container` protocol to provide an `isEmpty()` implementation, but only for container types whose element type conforms to `Equatable`.

```swift
extension Container where Element: Equatable {
    func isEmpty() -> Bool {
        return count == 0
    }
}
```

Here, we use the where clause to specify that the `isEmpty()` implementation should only be available when the element type conforms to `Equatable`. If the conformance condition is not satisfied, the default implementation from the original `Container` extension will be used.

## Conclusion

Conditional conformance in Swift is a powerful feature that allows us to extend types to conform to protocols based on certain conditions. It enables us to provide specific functionality only for the types that meet those conditions while keeping other types untouched. This feature promotes code reusability and enhances the flexibility of our codebase.

#Swift #ConditionalConformance