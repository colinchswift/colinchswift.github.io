---
layout: post
title: "Conditional conformance for non-optional types in Swift"
description: " "
date: 2023-09-27
tags: [SwiftProgramming, ConditionalConformance]
comments: true
share: true
---

Swift is a powerful and expressive programming language that provides support for conditional conformance. Conditional conformance allows you to apply a protocol conformance only when certain conditions are met. While conditional conformance is commonly used with optional types, it is also possible to make it work with non-optional types.

## Background

In Swift, optional types are handled slightly differently than non-optional types. Optional types can conform to a protocol even if the underlying type does not conform to the same protocol. This behavior makes it possible to write generic code that can handle both optional and non-optional values without having to duplicate the logic.

However, when working with non-optional types, the conformance is only applied if the underlying type itself conforms to the protocol. This can sometimes lead to code duplication or the need for explicit type constraints.

## Conditional Conformance with Non-Optional Types

To enable conditional conformance for non-optional types, we can make use of type constraints and extensions. We start by defining a protocol that we want our non-optional type to conform to conditionally:

```swift
protocol Printable {
    func printValue()
}
```

Next, we create an extension for the non-optional type where we define the conformance to the `Printable` protocol:

```swift
extension SomeNonOptionalType where Wrapped: Printable {
    func printValue() {
        print(wrappedValue)
    }
}
```

In this example, `SomeNonOptionalType` is the non-optional type we are extending. The `Wrapped` associated type is used to reference the underlying value of the non-optional type. And `Printable` is the protocol we want our non-optional type to conform to conditionally.

## Usage

Once we have defined the conditional conformance, we can use the `printValue()` method on instances of `SomeNonOptionalType`.

```swift
let value: SomeNonOptionalType<Int> = SomeNonOptionalType(wrappedValue: 42)
value.printValue() // Prints "42"
```

In this example, we create an instance of `SomeNonOptionalType` with an underlying value of `42` and then call the `printValue()` method on it.

## Conclusion

Conditional conformance in Swift is a powerful feature that allows us to write generic code that can adapt to different types. While it is commonly used with optional types, we have also seen how it can be used with non-optional types. By leveraging type constraints and extensions, we can conditionally apply protocol conformance to non-optional types in a concise and expressive way.

#SwiftProgramming #ConditionalConformance