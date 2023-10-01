---
layout: post
title: "Control flow statements in Swift"
description: " "
date: 2023-10-01
tags: [Swift, ControlFlow]
comments: true
share: true
---

Control flow statements are essential in programming languages to control the flow of execution of the program. In Swift, we have several control flow statements that allow us to make decisions, perform loops, and handle different scenarios. Let's explore some of the commonly used control flow statements in Swift.

## 1. If-else Statements

The `if` statement is used to perform a conditional execution of code block based on a condition. It checks whether the condition is true or false, and executes the code inside the block accordingly.

```swift
let number = 10

if number > 0 {
    print("The number is positive")
} else if number == 0 {
    print("The number is zero")
} else {
    print("The number is negative")
}
```

In the code snippet above, we use `if`, `else if`, and `else` keywords to define different conditions. The code inside the block that corresponds to the true condition is executed. Only one block will be executed, according to the first condition that evaluates to true.

## 2. Switch Statements

Switch statements are useful when we have multiple conditions to evaluate. It evaluates a value against several patterns and executes the code block that matches the pattern.

```swift
let grade = "A"

switch grade {
case "A":
    print("Excellent!")
case "B":
    print("Good!")
case "C":
    print("Average!")
default:
    print("Need to improve!")
}
```

In the example above, the `grade` variable is evaluated against different cases using the `switch` statement. Depending on the value, the corresponding code block is executed.

## 3. Loops

Swift provides different types of loops to perform repetitive tasks.

### 3.1 For-in Loop

The `for-in` loop iterates over a sequence, such as an array, a range, or a string. It executes a set of statements for each item in the sequence.

```swift
let numbers = [1, 2, 3, 4, 5]

for number in numbers {
    print(number)
}
```

In the code snippet above, the `for` loop iterates over the `numbers` array and prints each number in the console.

### 3.2 While Loop

The `while` loop executes a set of statements as long as a condition is true.

```swift
var countdown = 10

while countdown > 0 {
    print(countdown)
    countdown -= 1
}
```

In the above example, the `while` loop continuously decrements the `countdown` variable by 1 until it becomes zero.

### 3.3 Repeat-While Loop

The `repeat-while` loop is similar to the `while` loop, but the condition is checked after the loop executes. It guarantees that the code inside the loop is executed at least once.

```swift
var countdown = 10

repeat {
    print(countdown)
    countdown -= 1
} while countdown > 0
```

In the code snippet above, the loop executes the code inside the block and then checks the condition. It continues to execute the block until the condition becomes false.

These control flow statements in Swift provide flexibility and control over the program's execution. Whether it is making decisions with `if-else` statements, handling multiple cases with `switch` statements, or performing repetitive tasks with loops, understanding and using control flow statements is essential for every Swift developer.

#Swift #ControlFlow