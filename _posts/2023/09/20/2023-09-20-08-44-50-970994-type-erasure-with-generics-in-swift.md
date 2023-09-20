---
layout: post
title: "Type erasure with generics in Swift"
description: " "
date: 2023-09-20
tags: [Swift, Generics]
comments: true
share: true
---

In Swift, generics allow us to write flexible and reusable code by abstracting the type of a value, collection, or function. However, there are situations where we want to hide the specific type information and treat objects uniformly, irrespective of their concrete types. This is where type erasure comes into play.

Type erasure is a design pattern in Swift that allows us to hide the type information of a generic object. It provides a way to encapsulate a generic type into a non-generic wrapper, making it easier to work with heterogeneous collections or abstracting away implementation details.

## The Need for Type Erasure

Consider a scenario where we have a protocol called `Drawable` that defines a `draw()` method. We want to create a collection of different drawable objects without exposing their concrete types. We can achieve this using generics as follows:

```swift
protocol Drawable {
    func draw()
}

struct Circle: Drawable {
    func draw() {
        print("Drawing a circle")
    }
}

struct Square: Drawable {
    func draw() {
        print("Drawing a square")
    }
}

let shapes: [Drawable] = [Circle(), Square()]

for shape in shapes {
    shape.draw()
}
```

In the above code, we are able to create a collection of `Drawable` objects and invoke the `draw()` method on each object without knowing their specific types.

However, in some scenarios, we might not want to expose the generic nature of `Drawable` objects. For example, if `Drawable` were a large and complex protocol hierarchy or if we wanted to create a type-erased wrapper to ensure type safety, we would need to use type erasure.

## Implementing Type Erasure

To implement type erasure, we create a non-generic wrapper that hides the concrete type information. Here's an example:

```swift
class AnyDrawable {
    private let _draw: () -> Void
    
    init<T: Drawable>(_ drawable: T) {
        _draw = {
            drawable.draw()
        }
    }
    
    func draw() {
        _draw()
    }
}
```

In the above code, `AnyDrawable` is a non-generic wrapper that holds a closure `_draw` capturing a value of the generic type `T`. The closure calls the `draw()` method on the object of type `T`.

Now, let's modify our previous example to use type erasure:

```swift
let shapes: [AnyDrawable] = [AnyDrawable(Circle()), AnyDrawable(Square())]

for shape in shapes {
    shape.draw()
}
```

By using the `AnyDrawable` wrapper, we are now able to create a collection of type-erased `Drawable` objects, allowing us to invoke the `draw()` method without exposing their underlying types.

## Conclusion

Type erasure is an essential technique in Swift when we need to hide the type information of our generic objects. It provides a way to encapsulate the concrete types into a non-generic wrapper, enabling more flexibility in working with heterogeneous collections and abstracting away implementation details.

By taking advantage of type erasure, we can write cleaner, more reusable code that is easier to maintain and understand.

#Swift #Generics #TypeErasure