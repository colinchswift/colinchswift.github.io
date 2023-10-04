---
layout: post
title: "Conditional conformance for function builders in Swift"
description: " "
date: 2023-09-27
tags: [SwiftTips]
comments: true
share: true
---

Function builders are a powerful feature introduced in Swift 5.4 that allow us to construct complex data structures in a declarative manner. They make it easier to build DSLs (Domain Specific Languages) and reduce the boilerplate code when creating nested structures.

However, sometimes we may want to conditionally apply a function builder to our types. Swift 5.4 introduces conditional conformance for function builders, allowing us to apply a function builder only if a certain condition is met.

## Keyword: #SwiftTips

### Conditional Conformance for Function Builders

To illustrate conditional conformance for function builders, let's create an example where we have a custom `resultBuilder` that adds values to an array, but want to conditionally apply it based on certain conditions.

```swift
@resultBuilder
struct EvenNumbersBuilder {
    static func buildBlock(_ components: Int...) -> [Int] {
        var result: [Int] = []
        for component in components {
            if component % 2 == 0 {
                result.append(component)
            }
        }
        return result
    }
}

struct MyStruct {
    @EvenNumbersBuilder var numbers: [Int]
}

let structWithEvenNumbers = MyStruct {
    2
    3
    4
}
```

In this code snippet, we have defined a custom `resultBuilder` named `EvenNumbersBuilder`. It only adds even numbers to the resulting array. We then apply this `resultBuilder` to a property named `numbers` in the `MyStruct` struct.

Now, let's say we want to conditionally add the `EvenNumbersBuilder` to `MyStruct` based on a certain condition. We can achieve this by using conditional conformance.

```swift
struct MyStruct {
    @EvenNumbersBuilder
    var numbers: [Int]
    
    init(@EvenNumbersBuilder numbers: () -> [Int]) {
        self.numbers = numbers()
    }
    
    init(@EvenNumbersBuilder numbers: () -> [Int], applyBuilder: Bool) {
        if applyBuilder {
            self.numbers = numbers()
        } else {
            self.numbers = numbers()
        }
    }
}

let struct1 = MyStruct {
    2
    3
    4
}

let struct2 = MyStruct(numbers: {
    1
    2
    3
    4
}, applyBuilder: true)
```

In the updated code, we have added two initializers to `MyStruct`. The first initializer takes a closure that returns an array of even numbers, and always applies the function builder. The second initializer takes the same closure and a boolean flag `applyBuilder` to conditionally apply the function builder.

By using conditional conformance, we can choose whether to apply the function builder based on the condition specified. This provides more flexibility and control over the usage of function builders in our code.

## Conclusion

Conditional conformance for function builders in Swift 5.4 allows us to apply function builders conditionally, providing more flexibility and control over their usage. This feature is particularly useful when we need to apply function builders based on specific conditions in our code.

By leveraging conditional conformance, we can further enhance the power and versatility of function builders in Swift. It enables us to create more dynamic and expressive code, especially when building complex data structures using DSLs.

#Swift #SwiftFunctionBuilders