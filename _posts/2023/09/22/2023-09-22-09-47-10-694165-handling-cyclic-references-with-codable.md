---
layout: post
title: "Handling cyclic references with Codable"
description: " "
date: 2023-09-22
tags: [coding, swift]
comments: true
share: true
---

Cyclic references occur when two or more objects refer to each other in a loop. This can pose a challenge when encoding or decoding objects using Codable in Swift. In this blog post, we will explore how to handle cyclic references efficiently using Codable.

## The Issue with Cyclic References

When encoding objects with cyclic references, the default encoding behavior of Codable can result in stack overflow errors or infinite recursion. This is because by default, Codable will encode each object and all its properties, including any references to other objects.

Consider the following example:

```swift
class Person: Codable {
    let name: String
    var friends: [Person] = []
    
    init(name: String) {
        self.name = name
    }
    
    func addFriend(_ person: Person) {
        friends.append(person)
    }
}

let alice = Person(name: "Alice")
let bob = Person(name: "Bob")

alice.addFriend(bob)
bob.addFriend(alice)

let encoder = JSONEncoder()
let data = try encoder.encode(alice)
```

When we attempt to encode `alice`, we encounter an infinite recursion as Codable tries to encode `alice` and all the friends in her `friends` array, which includes `bob`. Similarly, when encoding `bob`, we encounter the same issue.

## Breaking the Cyclic Reference

To break the cyclic reference, we can leverage the power of coding keys and nested containers.

One way to do this is by providing a custom implementation of `encode(to:)` and `init(from:)` methods in the `Person` class, which allows us to manually handle the encoding and decoding process.

```swift
class Person: Codable {
    let name: String
    var friends: [Person] = []

    init(name: String) {
        self.name = name
    }

    func addFriend(_ person: Person) {
        friends.append(person)
    }

    private enum CodingKeys: CodingKey {
        case name
        case friends
    }

    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(name, forKey: .name)

        var friendsContainer = container.nestedUnkeyedContainer(forKey: .friends)
        for friend in friends {
            try friendsContainer.encode(friend)
        }
    }

    required init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        name = try container.decode(String.self, forKey: .name)

        var friendsContainer = try container.nestedUnkeyedContainer(forKey: .friends)
        while !friendsContainer.isAtEnd {
            let friend = try friendsContainer.decode(Person.self)
            friends.append(friend)
        }
    }
}
```

In the above code, we explicitly encode and decode each property of `Person` manually, including the `friends` array. By using a nested unkeyed container, we handle each friend in the `friends` array individually, breaking the cyclic reference.

## Conclusion

By providing custom encoding and decoding implementation using coding keys and nested containers, we can effectively handle cyclic references when working with Codable in Swift. This approach allows us to avoid infinite recursion or stack overflow errors and ensures successful encoding and decoding of objects with cyclic references.

By understanding and implementing this approach, you can confidently handle cyclic references when working with Codable in your Swift projects.

#coding #swift