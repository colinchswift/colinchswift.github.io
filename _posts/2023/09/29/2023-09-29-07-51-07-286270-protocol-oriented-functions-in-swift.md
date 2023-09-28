---
layout: post
title: "Protocol-Oriented Functions in Swift"
description: " "
date: 2023-09-29
tags: [Swift, Protocols]
comments: true
share: true
---

In Swift, protocols are a powerful tool for defining a blueprint of methods and properties that a class, struct, or enum must adhere to. One interesting use case for protocols is to define functions that operate on types that conform to a specific protocol. This approach is known as "protocol-oriented functions" and can lead to more modular and reusable code.

## Using Protocols as Function Parameters

One way to use protocols as function parameters is to define a function that takes an argument of a protocol type. This allows the function to accept any type that conforms to that protocol. Let's see an example:

```swift
protocol Drawable {
    func draw()
}

struct Circle: Drawable {
    func draw() {
        print("Drawing a circle")
    }
}

struct Rectangle: Drawable {
    func draw() {
        print("Drawing a rectangle")
    }
}

func drawShape(_ shape: Drawable) {
    shape.draw()
}

let circle = Circle()
let rectangle = Rectangle()

drawShape(circle) // Output: Drawing a circle
drawShape(rectangle) // Output: Drawing a rectangle
```

In the above example, we define a `Drawable` protocol that declares a `draw()` method. The `Circle` and `Rectangle` structs conform to this protocol by implementing the `draw()` method. The `drawShape(_:)` function accepts an argument of type `Drawable`, which means it can take any object that conforms to the `Drawable` protocol.

## Protocol Extensions and Default Implementations

Another powerful feature of Swift protocols is the ability to provide default implementations for methods. This is useful when you have a common implementation that can be shared across multiple types that conform to the protocol. Let's look at an example:

```swift
protocol Playable {
    func play()
}

extension Playable {
    func play() {
        print("Playing...")
    }
}

struct Song: Playable {
    // No need to implement the play() method
}

struct Movie: Playable {
    func play() {
        print("Playing movie...")
    }
}

let song = Song()
song.play() // Output: Playing...

let movie = Movie()
movie.play() // Output: Playing movie...
```

In the above example, we define a `Playable` protocol with a default implementation of the `play()` method. The `Song` struct doesn't provide a custom implementation, so it automatically inherits the default implementation from the protocol extension. The `Movie` struct, however, provides its own implementation and overrides the default one.

## Conclusion

Using protocols as function parameters and providing default implementations through protocol extensions are powerful techniques in Swift. They allow for more modular and reusable code, making it easier to work with different types that conform to the same protocol. Next time you're writing Swift code, consider using protocol-oriented functions to create more flexible and maintainable software.

#Swift #Protocols