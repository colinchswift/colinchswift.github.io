---
layout: post
title: "Conditional conformance for sets in Swift"
description: " "
date: 2023-09-27
tags: []
comments: true
share: true
---

Sets are a powerful data structure in Swift that allow you to store unique values in an unordered collection. With the introduction of conditional conformance in Swift 4, you can now define custom behaviors for `Set` types based on the types of elements they contain. This feature opens up new possibilities for writing clean and flexible code.

## Conditional Conformance

Conditional conformance allows you to define conformance to a protocol only when certain conditions are met. In the case of sets, you can define custom behaviors based on the conformance of the element type to a given protocol.

Let's say we have a protocol called `EquatableAndPrintable` that requires a type to be both `Equatable` and `CustomStringConvertible`:

```swift
protocol EquatableAndPrintable: Equatable, CustomStringConvertible { }
```

Now, we want to extend the `Set` type to conform to `CustomStringConvertible` only when the element type of the set conforms to `EquatableAndPrintable`. We can achieve this using conditional conformance:

```swift
extension Set: CustomStringConvertible where Element: EquatableAndPrintable {
    var description: String {
        let elements = self.map { $0.description }
        let joinedElements = elements.joined(separator: ", ")
        return "[\(joinedElements)]"
    }
}
```

In the above code, we extend the `Set` type with the conformance to `CustomStringConvertible` only when the set's element type conforms to `EquatableAndPrintable`. In the `description` property, we map each element of the set to its description and join them into a comma-separated string to represent the set.

Now, we can use `description` on a set that contains elements conforming to `EquatableAndPrintable`:

```swift
struct Person: EquatableAndPrintable {
    let name: String
    var description: String {
        return name
    }
}

let peopleSet: Set<Person> = [Person(name: "John"), Person(name: "Jane")]
print(peopleSet.description) // Output: [John, Jane]
```

By utilizing conditional conformance, we were able to provide a custom behavior for sets only when the element type conforms to the `EquatableAndPrintable` protocol.

## Conclusion

Conditional conformance in Swift allows you to extend types conditionally based on the conformance of their element types. With this powerful feature, you can customize the behavior of `Set` types and other collections to match the requirements of your specific use cases.