---
layout: post
title: "Using Codable with User Defaults in Swift"
description: " "
date: 2023-09-22
tags: [Codable]
comments: true
share: true
---

In Swift, `UserDefaults` is a convenient way to store simple data like strings, integers, and booleans persistently. However, when it comes to more complex data structures, manually serializing and deserializing the data can become cumbersome. 

Fortunately, Swift 4 introduced the `Codable` protocol which provides a convenient way to convert custom types to and from various external representations, including property lists. By using `Codable`, we can easily store and retrieve custom objects in `UserDefaults` without the need for manual serialization and deserialization.

Let's take a look at an example of how we can use `Codable` with `UserDefaults`:

```swift
struct Person: Codable {
    let name: String
    let age: Int
}

// Storing the person object in UserDefaults
func savePerson(_ person: Person) {
    let encoder = JSONEncoder()
    if let encodedPerson = try? encoder.encode(person) {
        UserDefaults.standard.set(encodedPerson, forKey: "person")
    }
}

// Retrieving the person object from UserDefaults
func getPerson() -> Person? {
    if let encodedPerson = UserDefaults.standard.data(forKey: "person") {
        let decoder = JSONDecoder()
        if let decodedPerson = try? decoder.decode(Person.self, from: encodedPerson) {
            return decodedPerson
        }
    }
    return nil
}

// Usage example
let person = Person(name: "John Doe", age: 30)
savePerson(person)

if let retrievedPerson = getPerson() {
    print("Name: \(retrievedPerson.name)")
    print("Age: \(retrievedPerson.age)")
}
```

In the above example, we define a `Person` struct that conforms to the `Codable` protocol. We then have two functions: `savePerson(_:)` and `getPerson()`. 

The `savePerson(_:)` function encodes the person object using a `JSONEncoder` and stores the encoded data in `UserDefaults` under the "person" key. 

The `getPerson()` function retrieves the encoded data from `UserDefaults` using the "person" key, and then uses a `JSONDecoder` to decode the data back into a `Person` object.

Finally, we can use these functions to store and retrieve a `Person` object with ease. In the example, we create a `Person` object, save it, and then retrieve it from `UserDefaults`.

Using Codable with User Defaults in Swift simplifies the process of storing and retrieving complex data structures like custom objects. It eliminates the need for manual serialization and deserialization, making our code cleaner and more maintainable.

#Swift #Codable