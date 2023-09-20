---
layout: post
title: "Default generic types in Swift"
description: " "
date: 2023-09-20
tags: [swift, generics]
comments: true
share: true
---

In Swift, generic types allow us to write reusable and flexible code that can work with different types. However, sometimes we may want to provide default types for our generic parameters. This can be useful when we often work with a specific type but still have the flexibility to change it if needed.

## Default Type Syntax

To specify default types for generic parameters, we can make use of the `where` clause in the generic type declaration. This allows us to define a default type for a specific generic parameter.

```swift
struct MyGenericStruct<T> where T == String {
    var value: T
}

let myStruct = MyGenericStruct(value: "Hello, World!")
```

In the example above, we have a generic struct `MyGenericStruct` which takes a generic parameter `T`. We use the `where` clause to specify that `T` must be of type `String`. This means that if we don't explicitly specify the type when creating an instance of `MyGenericStruct`, the default type will be `String`.

## Using Default Types

Using default types simplifies the usage of generic types, as we don't always need to provide the type explicitly. For instance, with our `MyGenericStruct` example, we can create an instance without explicitly specifying the type as long as it matches the default type:

```swift
let myStruct = MyGenericStruct(value: "Hello, Swift!")  // Default type is assumed as String
```

However, if we want to use a different type, we can still do so by specifying it explicitly:

```swift
let myStruct = MyGenericStruct<Int>(value: 42)  // Specify the type explicitly as Int
```

By providing default types in our generic code, we strike a balance between flexibility and ease of use. We can take advantage of default types to make our code cleaner and more concise, while still allowing for customization when needed.

#swift #generics