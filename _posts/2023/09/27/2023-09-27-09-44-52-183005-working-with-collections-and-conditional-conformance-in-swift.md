---
layout: post
title: "Working with collections and conditional conformance in Swift"
description: " "
date: 2023-09-27
tags: [conditonalconformance]
comments: true
share: true
---

Collections are an essential part of any programming language, allowing us to store, retrieve, and manipulate groups of elements. In Swift, we have various collection types such as arrays, sets, and dictionaries. 

One powerful feature of Swift is its ability to provide **conditional conformance** to protocols for types that meet certain requirements. This feature allows us to extend the functionality of collection types based on the properties and behaviors of their elements. Let's explore this concept further with some examples.

## Conditional Conformance with Equatable

The `Equatable` protocol in Swift defines the equality relationship between two values of the same type. We can leverage conditional conformance to make collections of `Equatable` elements conform to `Equatable` as well. This allows us to compare two collections based on the equality of their elements.

```swift
extension Array: Equatable where Element: Equatable {
    static func == (lhs: Array<Element>, rhs: Array<Element>) -> Bool {
        return lhs.elementsEqual(rhs)
    }
}
```

In the code snippet above, we extend the `Array` type and provide conditional conformance to the `Equatable` protocol. We specify that the extension is valid only for arrays whose elements are also `Equatable`. We then implement the equality operator (`==`) by utilizing the `elementsEqual` function.

Now, we can compare two arrays of `Equatable` elements for equality like this:

```swift
let array1 = [1, 2, 3]
let array2 = [1, 2, 3]

if array1 == array2 {
    print("The arrays are equal.")
} else {
    print("The arrays are not equal.")
}
```

## Conditional Conformance with Codable

The `Codable` protocol in Swift provides a convenient way to encode and decode types to and from various data representations, such as JSON or Property Lists. By using conditional conformance, we can make collections of `Codable` elements conform to `Codable` as well. This allows us to serialize and deserialize collections of objects with ease.

```swift
struct Person: Codable {
    let name: String
    let age: Int
}

extension Array: Codable where Element: Codable { }

let people = [Person(name: "John", age: 25), Person(name: "Jane", age: 30)]

do {
    let encoder = JSONEncoder()
    let encodedData = try encoder.encode(people)
    let jsonString = String(data: encodedData, encoding: .utf8)
    print(jsonString ?? "")

    let decoder = JSONDecoder()
    let decodedPeople = try decoder.decode([Person].self, from: encodedData)
    print(decodedPeople)
} catch {
    print("Error: \(error)")
}
```

In the code snippet above, we have a `Person` struct that conforms to `Codable`. We then extend the `Array` type and provide conditional conformance to `Codable`, ensuring that the extension only applies to arrays whose elements also conform to `Codable`.

We then create an array of `Person` objects and encode it into JSON format using `JSONEncoder`. We also decode the JSON data back into an array of `Person` objects using `JSONDecoder`. This allows us to serialize and deserialize the array effortlessly.

## Conclusion

Conditional conformance is a powerful feature in Swift that enables us to extend the functionality of collection types based on the properties and behaviors of their elements. By leveraging this feature, we can make our code more concise, reusable, and expressive. Whether it's equality comparisons or serialization and deserialization, conditional conformance provides flexibility and enhances the capabilities of collections in Swift.

#swift #conditonalconformance