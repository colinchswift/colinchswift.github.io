---
layout: post
title: "Difference between Private and File-private Access Levels in Swift"
description: " "
date: 2023-09-22
tags: [AccessLevels]
comments: true
share: true
---

In Swift, access levels restrict the visibility and availability of classes, structs, properties, methods, and other code entities. Two commonly used access levels are `private` and `file-private`. While they might seem similar at first, there are important differences between them.

## Private Access Level

The `private` access level restricts the usage of an entity to the scope of the enclosing declaration—typically a class or a struct. It ensures that the entity is accessible only within that particular context and cannot be accessed from external scopes. In other words, anything declared as `private` is encapsulated within the entity where it is defined.

Here is an example to illustrate the usage of `private` access level:

```swift
struct Person {
    private var name: String
    
    private func greet() {
        print("Hello, \(name)!")
    }
    
    func introduce() {
        greet()
        print("I am a person.")
    }
}

let person = Person()
person.introduce() // Output: Hello, [name]! I am a person.
person.name // Compilation Error: 'name' is inaccessible due to 'private' protection level.
person.greet() // Compilation Error: 'greet()' is inaccessible due to 'private' protection level.
```

In the above example, the `name` property and the `greet()` function are both marked as `private`. They can only be accessed within the `Person` struct. Outside of the `Person` struct, attempting to access these entities will result in a compilation error.

## File-private Access Level

The `file-private` access level restricts the visibility of an entity to the same file it is declared in. It allows access only within the source code file, preventing usage from other files within the same module or outside of it.

Here is an example to demonstrate the usage of `file-private` access level:

```swift
fileprivate struct Dog {
    fileprivate var name: String
    
    fileprivate func bark() {
        print("\(name) is barking!")
    }
    
    func play() {
        bark()
        print("\(name) is playing.")
    }
}

let dog = Dog()
dog.play() // Output: [name] is barking! [name] is playing.
dog.name // Compilation Error: 'name' is inaccessible due to 'fileprivate' protection level.
dog.bark() // Compilation Error: 'bark()' is inaccessible due to 'fileprivate' protection level.
```

In the above example, the `name` property and the `bark()` method are marked as `file-private`. They can be accessed within the same file where the `Dog` struct is declared. However, attempting to access them from outside the file will result in a compilation error.

## Conclusion

The main difference between `private` and `file-private` access levels in Swift is their scope of visibility. While `private` restricts access to the containing declaration, `file-private` restricts access to the same file. Both access levels provide encapsulation and help to achieve code modularity and maintainability.

#Swift #AccessLevels