---
layout: post
title: "Creating a set of custom objects in Swift"
description: " "
date: 2023-10-03
tags: [Swift, Sets]
comments: true
share: true
---

Sets in Swift allows you to store unique values of the same type in a collection. By default, sets in Swift work with standard data types like integers, strings, or floats. However, you can also create a set of custom objects in Swift by conforming to the `Hashable` protocol. In this blog post, we will explore how to create a set of custom objects in Swift.

### Step 1: Define the Custom Object

First, let's define our custom object. For example, let's say we want to create a set of `Person` objects with a name and age property. We can define our `Person` class as follows:

```swift
class Person: Hashable {
    let name: String
    let age: Int
    
    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
    
    static func == (lhs: Person, rhs: Person) -> Bool {
        return lhs.name == rhs.name && lhs.age == rhs.age
    }
    
    func hash(into hasher: inout Hasher) {
        hasher.combine(name)
        hasher.combine(age)
    }
}
```

Here, we conform to the `Hashable` protocol by implementing the `hash(into:)` method. This method combines the hash values of the name and age properties to generate a unique hash value for each `Person` object. We also implement the `==` operator to compare two `Person` objects for equality based on their name and age.

### Step 2: Create a Set of Custom Objects

Now that we have our custom object defined, let's create a set of `Person` objects and perform operations on it:

```swift
var peopleSet = Set<Person>()

let person1 = Person(name: "John", age: 25)
let person2 = Person(name: "Jane", age: 30)
let person3 = Person(name: "John", age: 25)

peopleSet.insert(person1)
peopleSet.insert(person2)
peopleSet.insert(person3)

print("Number of people in the set:", peopleSet.count)
// Output: Number of people in the set: 2

for person in peopleSet {
    print("Name:", person.name, "Age:", person.age)
}
// Output: Name: John Age: 25
//         Name: Jane Age: 30
```

Here, we create an empty set of `Person` objects by initializing an instance of `Set<Person>`. We then create three `Person` objects, `person1`, `person2`, and `person3`. We insert these objects into the `peopleSet` using the `insert(_:)` method.

Since `person1` and `person3` have the same name and age, only one of them will be added to the set. When we print the count of the set, it will return 2, indicating that there are two unique people in the set.

Finally, we iterate over the set using a for-in loop and print the name and age of each `Person` object in the set.

### Conclusion

Creating a set of custom objects in Swift is straightforward. By conforming to the `Hashable` protocol and providing an implementation for the `hash(into:)` method, you can ensure uniqueness in your set based on the properties of your custom object. This allows you to efficiently store and manipulate sets of custom objects in your Swift applications.

#Swift #Sets