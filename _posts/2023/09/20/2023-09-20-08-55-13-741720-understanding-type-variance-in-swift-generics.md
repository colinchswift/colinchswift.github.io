---
layout: post
title: "Understanding type variance in Swift generics"
description: " "
date: 2023-09-20
tags: [generics]
comments: true
share: true
---

Swift generics provide a powerful tool to write flexible, reusable code that works with different types. One important concept to grasp when working with generics is type variance. Type variance in Swift generics refers to the relationship between the type arguments of a generic type or function. Understanding type variance is essential for properly utilizing generics in your Swift code.

## Covariance

Covariance describes the relationship between types where a generic type parameter can be substituted with its subtype. In other words, if a generic class or function is declared to work with a type `A`, it can also work with a subtype `B` of `A`. This allows for more flexible usage of generic constructs.

For example, let's consider a generic `Box` class that holds a value of a certain type:

```swift
class Box<T> {
    var value: T

    init(value: T) {
        self.value = value
    }
}
```

Now, let's define two classes `Animal` and `Cat`, where `Cat` is a subclass of `Animal`. We can demonstrate covariance by creating a `Box` of `Animal` values and assigning it a `Box` of `Cat` values:

```swift
class Animal { /* ... */ }
class Cat: Animal { /* ... */ }

let catBox: Box<Cat> = Box(value: Cat())
let animalBox: Box<Animal> = catBox
```

In this example, we can assign a `Box<Cat>` to a `Box<Animal>` because `Cat` is a subtype of `Animal`. This is an example of covariance, where the substitution preserves subtyping relationships.

## Contravariance

Contravariance, on the other hand, describes the relationship between types where a generic type parameter can be substituted with its supertype. In other words, if a generic function is declared to work with a type `A`, it can also work with a supertype `B` of `A`. Contravariance allows for greater flexibility when working with generic function types.

Consider a contravariant generic function that takes a function with a parameter of type `Any` and returns a value of type `Void`:

```swift
func process(anyFunction: (Any) -> Void) { /* ... */ }
```

We can demonstrate contravariance by passing a function that takes a parameter of type `String` to `process` function:

```swift
func handleString(str: String) { /* ... */ }

process(anyFunction: handleString)
```

Here, the `handleString` function can be passed to `process` even though it expects a function with a parameter of type `Any`. This is possible due to contravariance, where the substitution preserves the supertyping relationships.

## Conclusion

Understanding type variance in Swift generics is crucial for writing flexible, reusable code. Covariance allows for subtyping relationships, and contravariance allows for supertyping relationships to be preserved when dealing with generics. Knowing how variance affects your generic code will help you design more robust and flexible systems.

#swift #generics