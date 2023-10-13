---
layout: post
title: "Unowned References: Using unowned references in Swift deinitialization"
description: " "
date: 2023-10-13
tags: [reference, reference]
comments: true
share: true
---

In Swift, managing object references and preventing strong reference cycles is crucial to avoid memory leaks. One way to break reference cycles is by using **unowned references**. Unowned references are references that do not keep a strong hold on the object they refer to and are guaranteed to always have a non-nil value.

## Understanding Strong Reference Cycles

Before delving into unowned references, it's essential to understand strong reference cycles. A strong reference cycle occurs when two objects hold strong references to each other, creating a cycle that prevents both objects from being deallocated, even if they are no longer in use.

Consider the following example:

```swift
class Person {
    var name: String
    var dog: Dog?
    
    init(name: String) {
        self.name = name
    }
    
    deinit {
        print("Person \(name) is being deallocated.")
    }
}

class Dog {
    var name: String
    var owner: Person?
    
    init(name: String) {
        self.name = name
    }
    
    deinit {
        print("Dog \(name) is being deallocated.")
    }
}

var john: Person? = Person(name: "John")
var spot: Dog? = Dog(name: "Spot")

john?.dog = spot
spot?.owner = john

john = nil
spot = nil
```

In this code snippet, both `Person` and `Dog` classes have a reference to each other. When we create instances of `Person` and `Dog`, assign them to each other, and set their references to `nil`, the deinitializers are not called because the reference cycle prevents them from being deallocated.

## Using Unowned References

To break the strong reference cycle, we can use **unowned references**. Unlike weak references, unowned references are always expected to have a value and are never nil. Therefore, we don't need to unwrap them to access their value.

Going back to our previous example, we can modify it to use unowned references:

```swift
class Person {
    var name: String
    unowned var dog: Dog?
    
    init(name: String) {
        self.name = name
    }
    
    deinit {
        print("Person \(name) is being deallocated.")
    }
}

class Dog {
    var name: String
    unowned var owner: Person?
    
    init(name: String) {
        self.name = name
    }
    
    deinit {
        print("Dog \(name) is being deallocated.")
    }
}

var john: Person? = Person(name: "John")
var spot: Dog? = Dog(name: "Spot")

john?.dog = spot
spot?.owner = john

john = nil // Prints "Person John is being deallocated."
spot = nil // Prints "Dog Spot is being deallocated."
```

In this updated code, we change the reference between `Person` and `Dog` to unowned references. Now, when we set `john` and `spot` to `nil`, both objects are deallocated because the reference cycle is successfully broken.

## When to Use Unowned References

It's important to note that we should only use unowned references when we are certain that the referenced object will outlive the object holding the reference. If the referenced object is deallocated before the object holding the unowned reference, accessing the unowned reference will result in a runtime error.

Unowned references are a powerful tool for breaking strong reference cycles in Swift, but they require careful consideration of object lifetimes. It's crucial to analyze the relationships between objects and determine the appropriate use of unowned references.

# References
- [Swift documentation on Automatic Reference Counting](https://docs.swift.org/swift-book/LanguageGuide/AutomaticReferenceCounting.html) #reference #Swift
- [Apple's Memory Management Guide](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html) #reference #memorymanagement