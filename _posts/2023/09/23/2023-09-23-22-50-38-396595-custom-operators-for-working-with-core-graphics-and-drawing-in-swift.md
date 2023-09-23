---
layout: post
title: "Custom operators for working with Core Graphics and drawing in Swift"
description: " "
date: 2023-09-23
tags: [swift, CoreGraphics]
comments: true
share: true
---

When working with Core Graphics in Swift, it is common to use functions like `CGContextAddLineToPoint`, `CGContextAddArc`, and `CGContextAddRect` to draw shapes and lines. However, these functions can be quite verbose and make the code harder to read and maintain.

To make our code more expressive and concise, we can define custom operators that encapsulate these Core Graphics functions. This allows us to write code that looks more like a natural language description of what we want to draw. In this blog post, we'll explore how to create custom operators for working with Core Graphics in Swift.

## Defining Custom Operators

In Swift, custom operators are simply functions with special names and syntax. They can be infix operators (e.g., `+`, `-`), prefix operators (e.g., `!`, `-`), or postfix operators (e.g., `!`, `++`).

To create custom operators for working with Core Graphics, we'll focus on infix operators since they are the most commonly used for drawing operations. Let's define a custom operator `-->` that encapsulates the `CGContextAddLineToPoint` function:

```swift
infix operator --> : AdditionPrecedence

func -->(context: CGContext, point: CGPoint) {
    context.addLine(to: point)
}
```

The `infix` keyword indicates that this is an infix operator, and `: AdditionPrecedence` specifies the operator's precedence level (how it interacts with other operators). In this case, we'll use the `+` precedence level.

The function takes two parameters: `context` of type `CGContext` and `point` of type `CGPoint`. Inside the function, we simply call the `addLine(to:)` method on the `context` object, passing in the `point`.

## Using Custom Operators

Once we have defined our custom operator, we can use it to draw lines in our code. Here's an example:

```swift
// Get the current graphics context
guard let context = UIGraphicsGetCurrentContext() else { return }

// Move to the starting point
context.move(to: CGPoint(x: 100, y: 100))

// Draw a line to the ending point using our custom operator
context --> CGPoint(x: 200, y: 200)

// Stroke the path
context.strokePath()
```

In this example, we first obtain the current graphics context using `UIGraphicsGetCurrentContext()`. We then move to the starting point using the `move(to:)` method, and finally draw a line to the ending point using our custom operator `-->`. After that, we stroke the path to actually draw the line on the screen.

## Conclusion

By defining custom operators that encapsulate Core Graphics functions, we can make our code more expressive and concise. This can greatly improve readability and maintainability, especially when working with complex drawing operations.

Remember to use custom operators judiciously and ensure that they enhance code readability. Custom operators should make the code more natural and intuitive, rather than adding unnecessary complexity.

#swift #CoreGraphics