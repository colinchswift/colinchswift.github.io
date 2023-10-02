---
layout: post
title: "Checking if a set contains nil values in Swift"
description: " "
date: 2023-10-03
tags: [Swift, Sets]
comments: true
share: true
---

# Option 1: Using .contains(nil)

One way to check for `nil` values in a set is by using the `.contains` method provided by Swift's `Set` type. However, this approach only works if the set's element type is optional, meaning it allows for `nil` values.

Here's an example:

```swift
let setWithNil: Set<Int?> = [1, nil, 3, 4]

if setWithNil.contains(nil) {
    print("The set contains nil values")
} else {
    print("The set does not contain nil values")
}
```

In the above code, we create a `setWithNil` set that contains integers and one `nil` value. By calling `.contains(nil)` on the set, we can check if the set contains any `nil` values.

# Option 2: Using Custom Function

If the element type of the set is not optional, we can define a custom function to check for `nil` values. This function will iterate over the set and return `true` if any `nil` value is found.

```swift
func setContainsNil<T>(_ set: Set<T>) -> Bool {
    for element in set {
        if element as? Any == nil {
            return true
        }
    }
    return false
}

let setWithoutNil: Set<Int> = [1, 2, 3, 4]
let setWithNil: Set<Int?> = [1, nil, 3, 4]

if setContainsNil(setWithNil) {
    print("The set contains nil values")
} else {
    print("The set does not contain nil values")
}

if setContainsNil(setWithoutNil) {
    print("The set contains nil values")
} else {
    print("The set does not contain nil values")
}
```

In the code snippet above, we define the `setContainsNil` function that takes a parameter of type `Set<T>` and checks for `nil` values by attempting to typecast each element as `Any`. If the typecast fails (returns `nil`), it means the element is `nil`.

By using this custom function, we can check for `nil` values in sets of both optional and non-optional types.

## Conclusion

Whether the element type of a set is optional or non-optional, Swift provides approaches to check if a set contains `nil` values. By using the `.contains(nil)` method for optional types or a custom function for non-optional types, you can easily perform this check in your code.

#Swift #Sets #OptionalValues