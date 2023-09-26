---
layout: post
title: "Example of a custom string concatenation operator in Swift"
description: " "
date: 2023-09-23
tags: [stringconcatenation]
comments: true
share: true
---

In Swift, you can use the `+` operator to concatenate two strings together. However, if you want to create a custom string concatenation operator that offers additional functionality, you can do so by overloading the existing operator.

To create a custom string concatenation operator in Swift, you'll need to define a new operator function using the `infix` keyword. Let's see an example:

```swift
infix operator +++

func +++(lhs: String, rhs: String) -> String {
    return "\(lhs) 🚀 \(rhs)"
}
```

In the above code, we define the `+++` operator as our custom string concatenation operator. It takes two `String` operands `lhs` and `rhs`, and returns a new `String` that combines the two operands with an additional emoji and a separator.

Now, let's see how we can use our custom operator:

```swift
let str1 = "Hello"
let str2 = "World"
let combined = str1 +++ str2
print(combined) // Output: Hello 🚀 World
```

As you can see, we use the `+++` operator to concatenate `str1` and `str2`, resulting in a new string with the emoji and separator added.

By creating custom operators, you can extend the functionality of Swift and make your code more expressive. However, it's important to use custom operators judiciously and ensure they enhance the readability and maintainability of your code.

So, next time you need to concatenate strings in a unique way, consider creating a custom string concatenation operator like the one we've discussed here!

#swift #stringconcatenation