---
layout: post
title: "Handling conditional conformance clashes in Swift"
description: " "
date: 2023-09-27
tags: [Swift, ConditionalConformance]
comments: true
share: true
---

When working with Swift's conditional conformance feature, you may encounter clashes where multiple types satisfy the same condition, leading to ambiguity and compiler errors. In this blog post, we'll explore how to handle these clashes and resolve them effectively.

## Understanding Conditional Conformance

Conditional conformance is a powerful feature in Swift that allows you to extend a generic type to conform to a protocol only when certain conditions are met. This helps write more flexible and reusable code.

For example, let's say we have a generic `Queue` struct that can hold any type:

```swift
struct Queue<Element> {
    // implementation details
}
```

Now, we want to make this generic `Queue` conform to the `Collection` protocol when the stored element type conforms to `Equatable`. 

```swift
extension Queue : Collection where Element : Equatable {
    // implementation of Collection methods
}
```

## Handling Conditional Conformance Clashes

A conditional conformance clash occurs when multiple types conforming to different conditions satisfy the same protocol. In our previous example, suppose we also have a `BinaryTree` struct that can hold equatable elements:

```swift
struct BinaryTree<Element> {
    // implementation details
}
```

If we try to make it conform to the `Collection` protocol as well, we'll encounter a clash because both `Queue` and `BinaryTree` conform to `Collection` when their element types are equatable.

To handle this clash, we have a few options:

### 1. `where` Clauses

One approach is to use `where` clauses in the extension to make the conditional conformance more specific:

```swift
extension Queue : Collection where Element : Equatable, Self : AnyObject {
    // implementation of Collection methods
}
```

Here, we added an additional constraint that `Queue` must also be a class type (`Self : AnyObject`), making this extension more refined. This resolves the clash as only the `Queue` type will satisfy this condition.

### 2. `@available` Attribute

Another option is to use the `@available` attribute to apply availability conditions to the extensions:

```swift
@available(*, unavailable, message: "Queue and BinaryTree cannot conform to Collection at the same time")
extension Collection where Self : Queue, Element : Equatable { }

extension Collection where Self : BinaryTree, Element : Equatable {
    // implementation of Collection methods
}
```

In this approach, we mark the conformance of `Queue` as unavailable and provide a message explaining the clash. Then, we define a separate extension for `BinaryTree` conforming to `Collection` without any clash.

### 3. Protocol Composition

If the clash occurs between different protocols, protocol composition can be used to resolve the conflict:

```swift
extension Queue : Collection where Element : Equatable { }
extension Queue : BidirectionalCollection where Element : Equatable { }

extension BinaryTree : Collection where Element : Equatable { }
extension BinaryTree : CustomStringConvertible { }
```

Here, we provide separate extensions for `Queue` conforming to `Collection` and `BidirectionalCollection`. Similarly, `BinaryTree` conforms to `Collection` and `CustomStringConvertible`.

## Conclusion

Conditional conformance in Swift is a valuable feature that promotes code reuse and flexibility. However, clashes can occur when multiple types conforming to different conditions satisfy the same protocol. By utilizing `where` clauses, `@available` attribute, or protocol composition, these clashes can be effectively resolved, and you can continue writing clean and maintainable code.

#Swift #ConditionalConformance