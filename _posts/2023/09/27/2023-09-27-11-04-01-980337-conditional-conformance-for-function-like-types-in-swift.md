---
layout: post
title: "Conditional conformance for function-like types in Swift"
description: " "
date: 2023-09-27
tags: [conditional]
comments: true
share: true
---

Swift is a powerful and flexible programming language that allows for conditional conformance, which means that you can define how a type conforms to a protocol under certain conditions. One area where this feature really shines is with function-like types, such as closures or functions themselves. In this article, we will explore how to use conditional conformance to make function-like types conform to protocols in Swift.

## The Problem

Consider a scenario where you have a protocol called `Equatable` that defines the equality operation, and you want to make your custom closure type conform to this protocol. However, closures in Swift do not automatically conform to `Equatable` because there is no default implementation available. So how can we solve this problem?

## Conditional Conformance

With conditional conformance, we can specify that a type only conforms to a protocol under certain conditions. In this case, we want our closure type to conform to `Equatable` if the closure's input type and return type also conform to `Equatable`.

```swift
struct MyClosure<Input, Output> {
    let closure: (Input) -> Output
}

extension MyClosure: Equatable where Input: Equatable, Output: Equatable {
    static func ==(lhs: MyClosure, rhs: MyClosure) -> Bool {
        // Compare the input and output of the closures
        return lhs.closure == rhs.closure
    }
}
```

In this example, we define a struct called `MyClosure` that holds a closure. We then extend the `MyClosure` type and conditionally conform it to `Equatable` using the `where` clause. We specify that `Input` and `Output` must also conform to `Equatable` in order for the closure to conform to `Equatable`.

Inside the `==` function, we compare the input and output of the closures to determine if they are equal.

## Example Usage

Now that we have defined our conditional conformance, let's see how we can use it in practice.

```swift
let closure1 = MyClosure<Int, Bool> { $0 > 5 }
let closure2 = MyClosure<Int, Bool> { $0 > 10 }

if closure1 == closure2 {
    print("Closures are equal")
} else {
    print("Closures are not equal")
}
```

In this example, we create two closures of type `MyClosure`, where the input type is `Int` and the output type is `Bool`. We compare the two closures using the `==` operator, which triggers the implementation of `Equatable` for `MyClosure`.

## Conclusion

Conditional conformance allows us to define custom conformance rules for function-like types in Swift. By using the `where` clause, we can specify conditions that must be met in order for a type to conform to a protocol. This gives us powerful flexibility when it comes to defining custom behavior for our types.

With this knowledge, you'll be able to leverage the full potential of conditional conformance to make your function-like types conform to any protocol that suits their requirements.

#swift #conditional-conformance