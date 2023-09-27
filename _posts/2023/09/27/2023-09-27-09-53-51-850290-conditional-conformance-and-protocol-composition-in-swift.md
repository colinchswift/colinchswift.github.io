---
layout: post
title: "Conditional conformance and protocol composition in Swift"
description: " "
date: 2023-09-27
tags: [protocols]
comments: true
share: true
---

In Swift, protocols are a powerful way to define a set of requirements that types can conform to. With protocol composition and conditional conformance, we can further enhance the flexibility and functionality of protocols in our code. In this article, we will explore how conditional conformance and protocol composition work together to make our code more concise and expressive.

## Protocol Composition

Protocol composition allows us to combine multiple protocols into a single requirement. This is especially useful when we want to define a single type that conforms to multiple protocols. To create a protocol composition, we simply list the protocols separated by an ampersand (&) symbol.

```swift
protocol Printable {
    func print()
}

protocol Editable {
    func edit()
}

// Protocol composition of Printable and Editable
typealias PrintableAndEditable = Printable & Editable
```
In the above example, we define two protocols: `Printable` and `Editable`. We then create a protocol composition called `PrintableAndEditable` that requires a type to conform to both the `Printable` and `Editable` protocols.

## Conditional Conformance

Conditional conformance allows a generic type to conform to a protocol only under certain conditions. This is particularly useful when we want to provide default implementations for protocol requirements based on the type's constraints.

```swift
struct Array<Element>: Collection {
    // Implementation details
}

// Conditional conformance of Array to Equatable
extension Array: Equatable where Element: Equatable {
    static func == (lhs: Array<Element>, rhs: Array<Element>) -> Bool {
        // Implementation details
    }
}
```

In the above example, the `Array` struct is made conform to the `Collection` protocol. Additionally, it conditionally conforms to the `Equatable` protocol only if the `Element` type of the array is itself `Equatable`. This enables us to use the `==` operator to compare arrays of `Equatable` elements.

## Combining Protocol Composition and Conditional Conformance

Now, let's see how we can combine protocol composition and conditional conformance to create more flexible code.

```swift
protocol Loggable {
    func log()
}

struct FileManager<T> where T: PrintableAndEditable {
    let items: [T]
    
    // Additional implementation details
}

extension FileManager: Loggable where T: Loggable {
    func log() {
        for item in items {
            item.log()
        }
    }
}
```

In the above example, we define a protocol called `Loggable` that requires a type to provide a `log()` method. We then create a generic struct called `FileManager`, which contains an array of items of type `T` that conforms to the `PrintableAndEditable` protocol (a composition of `Printable` and `Editable`). 

Next, we provide a conditional conformance of `FileManager` to the `Loggable` protocol. This means `FileManager` will only conform to `Loggable` if the `T` type also conforms to `Loggable`. This allows us to call the `log()` method on `FileManager` instances, but only if the contained `T` items also conform to `Loggable`.

By combining protocol composition and conditional conformance, we can create more flexible and concise code. It enables us to define requirements for types based on multiple protocols and conditionally conform types to protocols based on their constraints. This approach makes our code more expressive and versatile, promoting code reuse and reducing redundancy.

#swift #protocols