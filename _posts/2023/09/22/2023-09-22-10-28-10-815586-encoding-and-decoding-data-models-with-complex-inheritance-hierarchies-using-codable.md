---
layout: post
title: "Encoding and decoding data models with complex inheritance hierarchies using Codable"
description: " "
date: 2023-09-22
tags: [Codable]
comments: true
share: true
---

In Swift, the Codable protocol provides a convenient way to convert data structures to and from various encoded formats, such as JSON or Property List. While encoding and decoding simple data models is straightforward, dealing with complex inheritance hierarchies can be more challenging. In this blog post, we will explore how to encode and decode data models with complex inheritance hierarchies using Codable.

## Understanding Codable

The Codable protocol in Swift is a combination of the Encodable and Decodable protocols. By adopting Codable, a type can be both encoded to and decoded from an external representation, such as JSON. The protocol provides default implementations for basic types like String, Int, Double, etc., making it easy to encode and decode simple data models.

## Dealing with complex inheritance hierarchies

When it comes to complex inheritance hierarchies, things can get a bit more complicated. Let's consider an example where we have a base class `Animal` and two subclasses `Cat` and `Dog`. Each subclass has additional properties specific to the type of animal.

To make these classes Codable, we need to implement the `init(from:)` and `encode(to:)` methods defined by the Decodable and Encodable protocols, respectively. We'll start by implementing the `init(from decoder: Decoder)` method for decoding our data model.

```swift
class Animal: Codable {
    var name: String

    init(name: String) {
        self.name = name
    }

    required init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        name = try container.decode(String.self, forKey: .name)
    }
}

class Cat: Animal {
    var hasWhiskers: Bool

    init(name: String, hasWhiskers: Bool) {
        self.hasWhiskers = hasWhiskers
        super.init(name: name)
    }

    required init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        hasWhiskers = try container.decode(Bool.self, forKey: .hasWhiskers)
        try super.init(from: decoder)
    }

    override func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(hasWhiskers, forKey: .hasWhiskers)
        try super.encode(to: encoder)
    }
}

class Dog: Animal {
    var hasTail: Bool

    init(name: String, hasTail: Bool) {
        self.hasTail = hasTail
        super.init(name: name)
    }

    required init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        hasTail = try container.decode(Bool.self, forKey: .hasTail)
        try super.init(from: decoder)
    }

    override func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(hasTail, forKey: .hasTail)
        try super.encode(to: encoder)
    }
}
```

In the above example, we have implemented the `init(from decoder: Decoder)` method for each class. This method uses a `Container` to decode the additional properties specific to each subclass. We also call the superclass's `init(from:)` method to decode the common properties.

Similarly, we implement the `encode(to encoder: Encoder)` method to encode the additional properties of each subclass, and we call the superclass's `encode(to:)` method to encode the common properties.

Now, let's see an example of encoding and decoding our data models:

```swift
let fluffyCat = Cat(name: "Fluffy", hasWhiskers: true)
let doggo = Dog(name: "Doggo", hasTail: true)

let encoder = JSONEncoder()
let data = try encoder.encode(fluffyCat)

let decoder = JSONDecoder()
let decodedCat = try decoder.decode(Cat.self, from: data)
```

In the above example, we create instances of our data models, `Cat` and `Dog`. We then use a `JSONEncoder` to encode the `fluffyCat` object to JSON data. The encoded data can later be decoded using a `JSONDecoder`.

## Conclusion

In this blog post, we have learned how to encode and decode data models with complex inheritance hierarchies using Codable in Swift. By implementing the `init(from:)` and `encode(to:)` methods for each subclass, we can handle the additional properties specific to each subclass while still supporting the encoding and decoding functionality provided by Codable. With these techniques, you can deal with complex inheritance hierarchies seamlessly when working with encoded data structures. #Swift #Codable