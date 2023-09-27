---
layout: post
title: "Using conditional conformance to implement type erasure in Swift"
description: " "
date: 2023-09-27
tags: []
comments: true
share: true
---

Type erasure is a technique used in Swift to hide the concrete type of a protocol conforming object. This can be useful in scenarios where you want to work with a protocol, but you don't need to know the underlying type. One way to implement type erasure is by leveraging conditional conformance. In this article, we'll explore how conditional conformance can be used to achieve type erasure in Swift.

## What is Conditional Conformance?

Conditional conformance was introduced in Swift 4.1 and allows a generic type to conditionally conform to a protocol based on certain constraints. Normally, when a type is defined to conform to a protocol, it conforms to the protocol for all instances of that type. However, with conditional conformance, you can specify additional constraints on the generic type to determine when it will conform.

## Implementing Type Erasure with Conditional Conformance

Let's say we have a protocol called `Drawable` that defines a `draw()` method:

```swift
protocol Drawable {
    func draw()
}
```

Now, let's create a concrete type called `Circle` that conforms to `Drawable`:

```swift
struct Circle: Drawable {
    func draw() {
        print("Drawing a circle")
    }
}
```

Next, we'll create a type-erased wrapper called `AnyDrawable`:

```swift
struct AnyDrawable<T>: Drawable where T: Drawable {
    private let drawable: T
    
    init(_ drawable: T) {
        self.drawable = drawable
    }
    
    func draw() {
        drawable.draw()
    }
}
```

In the `AnyDrawable` struct, we have a generic parameter `T` that must conform to the `Drawable` protocol. We then create a private property called `drawable` of type `T`. In the `draw()` method, we call the `draw()` method of the underlying `drawable` object.

Now, let's leverage conditional conformance to make `AnyDrawable` conform to `Drawable`:

```swift
extension AnyDrawable: Drawable where T: Drawable {
    // Implementing the draw() method is not required here,
    // because it will be automatically satisfied by the underlying `drawable` object
}
```

With this extension, any instance of `AnyDrawable` will now be treated as if it conforms to `Drawable`. We can use `AnyDrawable` to type erase any `Drawable` object:

```swift
let circle = Circle()
let anyShape: AnyDrawable = AnyDrawable(circle)

anyShape.draw() // Output: "Drawing a circle"
```

## Conclusion

Conditional conformance in Swift allows for powerful techniques like type erasure. By using conditional conformance to implement the Type Erasure pattern, we can effectively hide the concrete type of an object while still working with it through a protocol. This gives us more flexibility and enables cleaner and more modular code.