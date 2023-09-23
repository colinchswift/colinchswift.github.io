---
layout: post
title: "Custom operators for working with regular expressions in Swift"
description: " "
date: 2023-09-23
tags: [swift, regex]
comments: true
share: true
---

Regular expressions are a powerful tool for pattern matching and text manipulation. In Swift, there are built-in functions and methods for working with regular expressions, such as `NSRegularExpression` and `String`'s `range(of:options:range:locale:)` method. However, with the help of custom operators, we can make working with regular expressions even more convenient and intuitive.

## Defining Custom Operators

In Swift, custom operators can be defined using the `prefix`, `infix`, or `postfix` keywords. For working with regular expressions, we'll focus on prefix and postfix operators.

### Prefix Operator: `~`

We can define a prefix operator, `~`, to match a string against a regular expression. Here's an example implementation:

```swift
prefix operator ~

prefix func ~(pattern: String) -> (String) -> Bool {
    return { input in
        let regex = try? NSRegularExpression(pattern: pattern)
        let range = NSRange(location: 0, length: input.utf16.count)
        return regex?.firstMatch(in: input, range: range) != nil
    }
}
```

With this prefix operator, we can now use `~` to match a string against a regular expression:

```swift
let isEmail = ~"^\\w+@[a-zA-Z_]+?\\.[a-zA-Z]{2,3}$"
let email = "test@example.com"

if isEmail(email) {
    print("Valid email")
}
```

### Postfix Operator: `~?`

We can also define a postfix operator, `~?`, to extract substrings that match a regular expression. Here's an example implementation:

```swift
postfix operator ~?

postfix func ~?(pattern: String) -> (String) -> String? {
    return { input in
        let regex = try? NSRegularExpression(pattern: pattern)
        let range = NSRange(location: 0, length: input.utf16.count)
        if let match = regex?.firstMatch(in: input, range: range) {
            return String(input[Range(match.range, in: input)!])
        }
        return nil
    }
}
```

Using the postfix operator `~?`, we can extract substrings that match the regular expression:

```swift
let phoneNumber: String? = "Phone: 123-456-7890" ~? "\\d{3}-\\d{3}-\\d{4}"

if let number = phoneNumber {
    print("Phone number: \(number)")
}
```

## Conclusion

Custom operators can make working with regular expressions in Swift more expressive and intuitive. By defining custom prefix and postfix operators, we can simplify the process of pattern matching and extracting substrings from a given string. However, it's important to use these operators judiciously and follow best practices to ensure clean and maintainable code.

#swift #regex