---
layout: post
title: "Core Graphics in Swift: drawing and manipulating graphics"
description: " "
date: 2023-10-01
tags: [coregraphics]
comments: true
share: true
---

When it comes to creating and manipulating graphics in Swift, the Core Graphics framework is a powerful tool to have in your toolkit. Whether you're building a custom UI component, generating charts, or simply wanting to draw on a view, Core Graphics provides a high-performance and flexible solution. In this article, we will explore some key concepts and techniques for working with Core Graphics.

## Getting Started with Core Graphics

To begin using Core Graphics, you first need to import the framework into your Swift project:

```swift
import CoreGraphics
```

## Drawing on a View

One of the most common use cases for Core Graphics is drawing on a view. To do this, you need to subclass `UIView` and override the `draw(_ rect: CGRect)` method. This method is called whenever the view needs to be redrawn, such as when it's first displayed or when its content needs to be updated.

Here's an example of how you can use Core Graphics to draw a simple rectangle on a view:

```swift
class CustomView: UIView {
    override func draw(_ rect: CGRect) {
        guard let context = UIGraphicsGetCurrentContext() else {
            return
        }
        
        // Set the fill color
        context.setFillColor(UIColor.red.cgColor)
        
        // Draw a rectangle
        context.addRect(CGRect(x: 100, y: 100, width: 200, height: 100))
        context.fillPath()
    }
}
```

In this example, we first get a reference to the current graphics context using `UIGraphicsGetCurrentContext()`. Then, we set the fill color to red using `context.setFillColor()`. Finally, we add a rectangle to the context using `context.addRect()` and fill it using `context.fillPath()`.

## Manipulating Graphics

Core Graphics provides a range of functions for manipulating graphics, such as drawing lines, circles, and curves. These functions allow you to create complex shapes and apply transformations to them.

Let's see an example of drawing a circle on a view and applying a rotation transformation:

```swift
class CustomView: UIView {
    override func draw(_ rect: CGRect) {
        guard let context = UIGraphicsGetCurrentContext() else {
            return
        }
        
        // Set the fill color
        context.setFillColor(UIColor.blue.cgColor)
        
        // Draw a circle
        context.addEllipse(in: CGRect(x: 100, y: 100, width: 100, height: 100))
        context.fillPath()
        
        // Apply rotation transformation
        context.rotate(by: .pi / 4)
        
        // Set the stroke color
        context.setStrokeColor(UIColor.red.cgColor)
        
        // Draw a line
        context.move(to: CGPoint(x: 200, y: 200))
        context.addLine(to: CGPoint(x: 300, y: 300))
        context.strokePath()
    }
}
```

In this example, we use `context.addEllipse()` to draw a circle and `context.fillPath()` to fill it with blue color. Then, we apply a rotation transformation using `context.rotate()`. Lastly, we use `context.setStrokeColor()` to set the stroke color to red and draw a line using `context.move(to:)` and `context.addLine(to:)`.

## Conclusion

Core Graphics is a versatile framework that offers powerful capabilities for drawing and manipulating graphics in Swift. Whether you need to create custom UI components, generate charts, or simply draw on a view, Core Graphics has the tools you need. By understanding the key concepts and techniques we covered in this article, you'll be well-equipped to leverage Core Graphics to bring your graphics to life.

#swift #coregraphics