---
layout: post
title: "Customizing the comparison of custom objects in a set in Swift"
description: " "
date: 2023-10-03
tags: [CustomObjects]
comments: true
share: true
---

When working with custom objects in Swift, you might encounter scenarios where you need to add these objects to a set - a collection that doesn't allow duplicates. By default, Swift compares objects based on their identity or memory address, which may not always be what you desire. In such cases, you can customize the comparison behavior of your custom objects within a set. 

To achieve this, you need to conform to the `Hashable` protocol and implement the `Equatable` protocol for your custom object. This will allow you to define your own criteria for comparing and determining the uniqueness of objects within a set.

## Step 1: Conform to the `Hashable` protocol

To begin, let's assume we have a custom `Person` class that contains two properties: `name` and `age`. It's necessary to conform to the `Hashable` protocol by implementing the `hash(into:)` method. The `hashValue` in Swift has been deprecated since Swift 4, therefore, we'll use the new `hash(into:)` method.

Here's an example implementation:

```swift
class Person: Hashable {
    let name: String
    let age: Int
    
    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
    
    func hash(into hasher: inout Hasher) {
        hasher.combine(name)
        hasher.combine(age)
    }
}
```

In this example, we've overridden the `hash(into:)` method to provide a custom hash value for our class. The `hasher.combine()` method allows us to combine the hash values of `name` and `age` into a single hash value for the `Person` object.

## Step 2: Implement the `Equatable` protocol

After conforming to the `Hashable` protocol, we need to implement the `Equatable` protocol to define how two `Person` objects should be compared for equality. 

Here's an example implementation:

```swift
extension Person: Equatable {
    static func == (lhs: Person, rhs: Person) -> Bool {
        return lhs.name == rhs.name && lhs.age == rhs.age
    }
}
```

In this example, we've implemented the `==` operator to compare two `Person` objects based on their `name` and `age` properties. If both properties match, the objects are considered equal.

## Step 3: Use the custom object in a set

Now that we've customized the comparison behavior of our custom `Person` object, we can use it within a set and ensure that duplicates are prevented. Let's see an example:

```swift
let person1 = Person(name: "John", age: 25)
let person2 = Person(name: "Jane", age: 30)
let person3 = Person(name: "John", age: 25)

var personSet: Set<Person> = [person1, person2, person3]

print(personSet) // Output: [Person(name: "John", age: 25), Person(name: "Jane", age: 30)]
```

In this example, we create three instances of `Person`. Even though `person1` and `person3` have the same values, the set only contains one instance of them due to our custom comparison criteria.

By implementing the `Hashable` and `Equatable` protocols, you can ensure that your custom objects are compared and considered unique within a set according to your desired criteria.

#Swift #CustomObjects #SetComparison