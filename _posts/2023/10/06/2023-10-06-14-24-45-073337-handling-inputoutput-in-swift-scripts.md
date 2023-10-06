---
layout: post
title: "Handling input/output in Swift scripts"
description: " "
date: 2023-10-06
tags: [InputOutput]
comments: true
share: true
---

When writing Swift scripts, it's important to handle input and output effectively. This allows us to interact with users or fetch data from external sources. In this article, we will explore different techniques for handling input and output in Swift scripts.

## Reading Input

To read input from the user, we can use the `readLine()` function. This function reads a line of input as a string and returns an optional string. We can then use optional binding to check if the input is present and process it accordingly.

```swift
if let input = readLine() {
    // Process the input
} else {
    // Handle the case when there is no input
}
```

We can also display a prompt to the user before reading input using the `print()` function.

```swift
print("Enter your name:")
if let name = readLine() {
    print("Hello, \(name)!")
} else {
    print("No input received.")
}
```

## Writing Output

To display output to the user, we can use the `print()` function. We can pass in one or more values to be displayed, separated by commas.

```swift
let age = 25
print("You are \(age) years old.")
```

If we want to format the output, we can use string interpolation or string formatting.

```swift
let expense = 100.50
print(String(format: "Total expense: $%.2f", expense))
```

## Command-Line Arguments

Swift scripts can also accept command-line arguments. These arguments are passed to the script when it is executed from the terminal. We can access these arguments using the `CommandLine.arguments` property.

```swift
let arguments = CommandLine.arguments
if arguments.count > 1 {
    let name = arguments[1]
    print("Hello, \(name)!")
} else {
    print("No name provided.")
}
```

## Conclusion

Handling input and output effectively is crucial when writing Swift scripts. By using the techniques mentioned in this article, you can create interactive scripts that can accept input from users, display output, and even accept command-line arguments. This allows you to build powerful and user-friendly scripts using Swift.

#Swift #InputOutput