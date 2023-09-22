---
layout: post
title: "Encoding and decoding recursive data structures with Codable"
description: " "
date: 2023-09-22
tags: [swift, codable]
comments: true
share: true
---

If you are working with complex data structures in your Swift application, you may come across the need to encode and decode recursive data structures using the Codable protocol. Codable is a powerful feature introduced in Swift 4 that allows you to easily convert custom types to and from various data representations, such as JSON or Property Lists. However, encoding and decoding recursive data structures can present some challenges. In this blog post, we will explore how to handle recursive data structures with Codable.

## Understanding Recursive Data Structures

A recursive data structure is a data structure that can contain references to instances of the same type. For example, a binary tree is a recursive data structure where each node can have two child nodes, which are themselves instances of the same type.

```swift
class Node<T> {
    var value: T
    var left: Node<T>?
    var right: Node<T>?
    
    init(value: T) {
        self.value = value
    }
}
```

In this example, the `Node` class represents a binary tree, where each node has a value of type `T` and optional `left` and `right` child nodes.

## Encoding Recursive Data Structures

To encode a recursive data structure with Codable, we need to conform to the `Codable` protocol and implement the required `init(from:)` initializer and `encode(to:)` method.

```swift
class Node<T>: Codable {
    var value: T
    var left: Node<T>?
    var right: Node<T>?
    
    init(value: T) {
        self.value = value
    }
    
    // MARK: - Codable
    
    required init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        value = try container.decode(T.self, forKey: .value)
        left = try container.decodeIfPresent(Node<T>.self, forKey: .left)
        right = try container.decodeIfPresent(Node<T>.self, forKey: .right)
    }
    
    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        try container.encode(value, forKey: .value)
        try container.encodeIfPresent(left, forKey: .left)
        try container.encodeIfPresent(right, forKey: .right)
    }
    
    private enum CodingKeys: String, CodingKey {
        case value, left, right
    }
}
```

In the `init(from:)` initializer, we use a keyed container to decode the value, left child node, and right child node from the decoder. We use `decodeIfPresent` for the child nodes since they can be optional.

In the `encode(to:)` method, we encode the value, left child node, and right child node to the encoder using a keyed container. We use `encodeIfPresent` to skip encoding the child nodes if they are nil.

## Working with Recursive Data Structures

Once you have defined your recursive data structure and implemented the encoding and decoding methods, you can use the `JSONEncoder` and `JSONDecoder` classes to encode and decode your data structures to and from JSON.

```swift
let rootNode = Node(value: 1)
let leftChild = Node(value: 2)
let rightChild = Node(value: 3)
rootNode.left = leftChild
rootNode.right = rightChild

let encoder = JSONEncoder()
encoder.outputFormatting = .prettyPrinted

if let encodedData = try? encoder.encode(rootNode) {
    if let jsonString = String(data: encodedData, encoding: .utf8) {
        print(jsonString)
    }
}

let decoder = JSONDecoder()

if let decodedData = jsonString.data(using: .utf8) {
    if let decodedNode = try? decoder.decode(Node<Int>.self, from: decodedData) {
        print(decodedNode)
    }
}
```

In this example, we create a simple binary tree and encode it to JSON using the `JSONEncoder`. We then decode the JSON back into a binary tree using the `JSONDecoder`.

## Conclusion

Handling recursive data structures with Codable can be a bit challenging, but with the proper implementation of the `Codable` protocol, you can easily encode and decode complex data structures in your Swift application. By following the steps outlined in this blog post, you'll be able to effectively work with recursive data structures using Codable.

#swift #codable