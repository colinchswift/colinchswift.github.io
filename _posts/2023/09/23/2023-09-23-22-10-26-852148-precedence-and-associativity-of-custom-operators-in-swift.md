---
layout: post
title: "Precedence and associativity of custom operators in Swift"
description: " "
date: 2023-09-23
tags: [CustomOperators]
comments: true
share: true
---

In Swift, operators are essential in expressing and manipulating data. While Swift provides a set of operators for commonly used operations, it also allows developers to define custom operators. However, when using custom operators, it is important to understand their precedence and associativity to avoid unexpected behavior.

## Operator Precedence

In Swift, operators have different levels of precedence, which determine the order in which they are evaluated in an expression. For example, the multiplication operator `*` has higher precedence than the addition operator `+`. This means that in the expression `3 + 4 * 2`, the multiplication is evaluated first, resulting in `3 + 8` and finally `11`.

When defining custom operators, you can assign them a precedence level using the `precedencegroup` keyword. This allows you to control the order in which your custom operators are evaluated alongside the built-in operators. For example:

```swift
precedencegroup ExponentiationPrecedence {
    higherThan: MultiplicationPrecedence
    associativity: right
}
```

In the example above, `ExponentiationPrecedence` is defined as having higher precedence than `MultiplicationPrecedence`. This means that if you have a custom exponentiation operator, it will be evaluated before the multiplication operator.

## Operator Associativity

In addition to precedence, operators also have associativity, which determines how operators of the same precedence are grouped together. Swift supports two types of associativity: left and right.

- Left-associative operators group from left to right. For example, in the expression `4 - 3 - 2`, the subtraction operators are evaluated from left to right, resulting in `(4 - 3) - 2` and finally `1 - 2`.
- Right-associative operators group from right to left. For example, the exponentiation operator `**` can be right-associative. In the expression `2 ** 3 ** 2`, the exponentiation operator is evaluated from right to left, resulting in `2 ** (3 ** 2)`.

When defining custom operators, you can specify their associativity using the `associativity` keyword.

## Example Code

Here's an example that demonstrates the precedence and associativity of custom operators in Swift:

```swift
precedencegroup ExponentiationPrecedence {
    higherThan: MultiplicationPrecedence
    associativity: right
}

infix operator **: ExponentiationPrecedence

func **(base: Double, exponent: Double) -> Double {
    return pow(base, exponent)
}

let result = 2 ** 3 ** 2
print(result) // Output: 512.0
```

In the example above, we define a custom exponentiation operator `**` with right associativity and higher precedence than the multiplication operator. We then use this operator to calculate `2 ** 3 ** 2`, which results in `512.0`.

By understanding the precedence and associativity of custom operators, you can ensure that they work as expected and integrate seamlessly with the built-in operators in Swift.

#Swift #CustomOperators