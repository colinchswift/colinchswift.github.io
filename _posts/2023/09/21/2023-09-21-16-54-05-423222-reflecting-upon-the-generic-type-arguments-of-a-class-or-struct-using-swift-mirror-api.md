---
layout: post
title: "Reflecting upon the generic type arguments of a class or struct using Swift Mirror API"
description: " "
date: 2023-09-21
tags: [reflection]
comments: true
share: true
---

In Swift, the `Mirror` API provides a way to interrogate the runtime representation of any instance. It allows us to introspect various properties of an object, including its members, superclass, and generic type arguments.

When dealing with generic types, such as classes or structs with type parameters, it can be useful to reflect upon their generic type arguments at runtime. This allows us to dynamically inspect the actual types used to instantiate the generic type.

To demonstrate this, let's consider a simple example where we have a generic `Box` struct:

```swift
struct Box<T> {
    let value: T
}
```

Now, let's instantiate a `Box` with a specific type, for example, a `Box<Int>`:

```swift
let box = Box<Int>(value: 42)
```

To reflect upon the generic type arguments of the `box` instance, we need to create a `Mirror` object for it:

```swift
let mirror = Mirror(reflecting: box)
```

Now, we can iterate over the `mirror`'s children to access information about the generic type:

```swift
for child in mirror.children {
    if let label = child.label, label == "value" {
        print("Generic type argument: \(child.value)")
    }
}
```

In this example, we check each child's label and match it against the property we are interested in, which in this case is `"value"`. If the label matches, we can retrieve the generic type argument using `child.value`.

In the case of our `box` instance of type `Box<Int>`, the output will be:

```
Generic type argument: 42
```

By using the `Mirror` API, we can dynamically explore and access the generic type arguments of a generic class or struct at runtime. This can be particularly useful when we need to work with instances that are instantiated using different type parameters.

#swift #reflection