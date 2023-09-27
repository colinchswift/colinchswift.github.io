---
layout: post
title: "Conditional conformance for variadic generics in Swift"
description: " "
date: 2023-09-27
tags: [generics]
comments: true
share: true
---

In Swift, **conditional conformance** allows you to define specific behavior for generic types based on certain conditions. It provides a powerful mechanism for extending the functionality of generic types in a more flexible way. 

One scenario where conditional conformance can be beneficial is when dealing with **variadic generics**. Variadic generics allow you to define a generic type that accepts a varying number of arguments of the same type. However, handling conditional conformance for variadic generics can be a bit tricky.

Let's take a look at an example to understand this better. Suppose we have a generic `Stack` type that accepts a variadic number of elements:

``` swift
struct Stack<Element> {
    private var elements: [Element] = []
    
    mutating func push(_ element: Element) {
        elements.append(element)
    }
    
    mutating func pop() -> Element? {
        return elements.popLast()
    }
}
```

Now, let's say we want to provide a conditional conformance for the `Stack` type when its `Element` is of type `Equatable`. We only want to provide the `==` operator for comparing elements if they are `Equatable`.

To achieve this, we can use the `where` clause in Swift's `extension` to conditionally conform to the `Equatable` protocol:

``` swift
extension Stack: Equatable where Element: Equatable {
    static func == (lhs: Stack<Element>, rhs: Stack<Element>) -> Bool {
        return lhs.elements == rhs.elements
    }
}
```

With this conditional conformance, the `==` operator will only be provided for `Stack` instances where the `Element` type is `Equatable`. This allows you to compare two `Stack` objects only if they contain `Equatable` elements.

Now, you can utilize this conditional conformance as follows:

``` swift
let stack1 = Stack<Int>()
stack1.push(1)
stack1.push(2)

let stack2 = Stack<Int>()
stack2.push(1)
stack2.push(2)

let stack3 = Stack<String>()
stack3.push("Swift")
stack3.push("is awesome")

print(stack1 == stack2) // true
print(stack1 == stack3) // error: Operator '==' cannot be applied to two Stack<String> operands
```

In the above example, the `Stack<Int>` instances can be compared using the `==` operator because the `Int` type is `Equatable`. However, attempting to compare `Stack<String>` instances results in a compilation error since `String` is not `Equatable`.

Conditional conformance is a powerful feature in Swift that allows you to extend the functionality of generic types based on certain conditions. By using conditional conformance with variadic generics, like in the example above, you can ensure that certain operations are only available for specific types.

#swift #generics