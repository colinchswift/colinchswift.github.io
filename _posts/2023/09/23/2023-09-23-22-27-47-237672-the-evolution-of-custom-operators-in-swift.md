---
layout: post
title: "The evolution of custom operators in Swift"
description: " "
date: 2023-09-23
tags: [swift, customoperators]
comments: true
share: true
---

Swift is known for its expressive and powerful programming features, and one of those features is the ability to define custom operators. Custom operators can be useful for creating domain-specific languages (DSLs), enhancing code readability, and improving developer productivity. In this article, we'll take a look at the evolution of custom operators in Swift.

## Swift 1.0 - 2.2: Limited Custom Operators

In the early versions of Swift, the support for custom operators was limited. Only a predefined set of operators such as arithmetic, logical, and bit-wise operators could be overloaded. Creating custom operators with unique symbols was not possible.

For example, if you wanted to define a custom operator to concatenate two strings, you had to resort to using functions or methods:

```swift
func +(lhs: String, rhs: String) -> String {
    return lhs + rhs
}

let result = "Hello, " + "World!"
```

## Swift 3.0: Custom Infix Operators with Existing Symbols

With the release of Swift 3.0, custom operators got a significant update. Now, developers could define and use custom infix operators using existing symbols. This allowed for more expressive and concise code.

For example, you could define a custom operator `^` to calculate the power of a number:

```swift
infix operator ^

func ^(base: Int, exponent: Int) -> Int {
    return Int(pow(Double(base), Double(exponent)))
}

let result = 2 ^ 3
```

## Swift 4.0: Custom Prefix and Postfix Operators

Swift 4.0 introduced support for custom prefix and postfix operators. This further expanded the possibilities of creating more readable and DSL-like code.

Now, you can define a custom prefix operator `++` to increment a value:

```swift
prefix operator ++

prefix func ++(value: inout Int) -> Int {
    value += 1
    return value
}

var num = 5
let result = ++num
```

Or a custom postfix operator `!` to factorialize a number:

```swift
postfix operator !

postfix func !(value: Int) -> Int {
    if value <= 1 {
        return 1
    } else {
        return value * ((value - 1)!)
    }
}

let result = 5!
```

## Swift 5.1 and Later: Custom Operator Precedence Groups

With the introduction of Swift 5.1, custom operators gained even more flexibility and control through the concept of operator precedence groups. Operator precedence groups allow you to define the associativity and precedence level of your custom operators.

For example, you can define a custom infix operator `±` (plus or minus) with the same precedence as addition and subtraction:

```swift
infix operator ±: AdditionPrecedence

func ±(lhs: Int, rhs: Int) -> Int {
    return lhs + rhs
}

let result = 5 ± 3 - 2
```

## Conclusion

The evolution of custom operators in Swift has provided developers with more expressive and concise ways to write code. From limited support in the early versions to the introduction of custom prefix and postfix operators, and the added flexibility of operator precedence groups in later versions, Swift continues to evolve and empower developers with its powerful features.

#swift #customoperators