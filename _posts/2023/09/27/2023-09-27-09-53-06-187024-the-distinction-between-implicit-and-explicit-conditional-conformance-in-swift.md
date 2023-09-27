---
layout: post
title: "The distinction between implicit and explicit conditional conformance in Swift"
description: " "
date: 2023-09-27
tags: [conditionalconformance]
comments: true
share: true
---

In Swift, conditional conformance allows a generic type to conform to a protocol only under certain conditions. This feature is particularly useful when working with generic types that may or may not be able to fulfill the requirements of a protocol.

There are two ways to declare conditional conformance: implicit and explicit. Understanding the difference between these two is important for effectively utilizing conditional conformance in your Swift code.

## Implicit Conditional Conformance

Implicit conditional conformance allows a generic type to automatically conform to a protocol based on the conformances of its type parameters. This means that if all of a generic type's type parameters conform to a certain protocol, the generic type itself will implicitly conform to that protocol.

Here's an example to illustrate this concept:

```swift
struct Stack<Element: Equatable> {
    var elements: [Element] = []
}

extension Stack: Equatable where Element: Equatable {}
```

In this example, the `Stack` struct is defined with a single type parameter `Element`. The `Element` type must conform to the `Equatable` protocol. However, since `Equatable` is only a requirement for the `Element` type, the `Stack` struct itself does not automatically conform to `Equatable`.

To make `Stack` conform to `Equatable`, we can use implicit conditional conformance. By adding an extension to `Stack` where `Element` conforms to `Equatable`, we can now compare two `Stack` instances using the `==` operator.

## Explicit Conditional Conformance

Unlike implicit conditional conformance, explicit conditional conformance requires explicitly declaring conformance for a generic type to a protocol, even if the type parameters already conform to that protocol. 

Let's consider a slightly modified example:

```swift
struct Queue<Element> {
    var elements: [Element] = []
}

extension Queue: Equatable where Element: Equatable {}
```

In this case, the `Queue` struct is defined with a single type parameter `Element`. The `Element` type does not have any requirements specified, including conformance to the `Equatable` protocol. To make `Queue` conform to `Equatable`, we need to explicitly declare the conformance using the `where` clause in an extension.

By explicitly stating the conformance, we make it clear that `Queue` will only conform to `Equatable` when its type parameter `Element` also conforms to `Equatable`. This provides more control and clarity in our code.

## Conclusion

Understanding the distinction between implicit and explicit conditional conformance in Swift allows you to effectively manage conformance to protocols in generic types. Implicit conditional conformance provides automatic conformance based on the conformances of type parameters, while explicit conditional conformance requires explicit declaration even when type parameters already conform to a protocol.

By leveraging these features, you can write more flexible and reusable code that adapts to different types while also ensuring protocol requirements are met.

#swift #conditionalconformance