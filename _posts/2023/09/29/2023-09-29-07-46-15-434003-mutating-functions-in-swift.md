---
layout: post
title: "Mutating Functions in Swift"
description: " "
date: 2023-09-29
tags: [MutatingFunctions]
comments: true
share: true
---

When working with Swift, you will often come across situations where you need to modify the properties of a structure or an enumeration. However, by default, functions in Swift are immutable, meaning they cannot modify the properties of the instance they are called on. To overcome this limitation, you can use the `mutating` keyword in Swift to create mutating functions.

## What are mutating functions?

Mutating functions in Swift are a special kind of functions that can modify the properties of the instance they are called on. They are specifically used with structures and enumerations since these value types are passed by value rather than by reference.

By default, when you call a function on a value type such as a structure or an enumeration, a new copy of the value is created, and any modifications made within the function are only applied to that copy. The original instance remains unaffected. However, by declaring a function as `mutating`, you are allowed to modify the properties of the original instance directly.

## How to declare a mutating function?

To declare a mutating function in Swift, you need to use the `mutating` keyword before the `func` keyword in the function declaration. Let's take an example of a structure representing a point in a 2D coordinate system:

```swift
struct Point {
    var x: Double
    var y: Double

    mutating func moveBy(deltaX: Double, deltaY: Double) {
        x += deltaX
        y += deltaY
    }
}
```

In the above example, the `moveBy(deltaX:deltaY:)` function is declared as `mutating` since it modifies the properties `x` and `y` of the `Point` instance it is called on.

## Calling mutating functions

You can call a mutating function on a mutable instance of a structure or an enumeration. Since the function modifies the original instance, it cannot be called on constant instances or immutable variables.

```swift
var point = Point(x: 3.0, y: 5.0)
point.moveBy(deltaX: 2.0, deltaY: -1.0)
print("New coordinates: (point.x), (point.y)")  # #Swift #MutatingFunctions
```

In the above example, the `moveBy(deltaX:deltaY:)` function is called on a mutable instance of the `Point` structure, `point`. It modifies the `x` and `y` properties of `point`, resulting in new coordinates for the point.

## Conclusion

Mutating functions in Swift allow you to modify the properties of a structure or an enumeration directly, even though value types are passed by value. By using the `mutating` keyword, you can make your functions mutable and enable them to modify the original instance. This is an important feature that helps in working with value types effectively. #Swift #MutatingFunctions