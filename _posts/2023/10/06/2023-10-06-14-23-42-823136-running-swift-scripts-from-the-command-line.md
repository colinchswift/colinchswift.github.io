---
layout: post
title: "Running Swift scripts from the command line"
description: " "
date: 2023-10-06
tags: []
comments: true
share: true
---

Swift is a powerful and versatile programming language that can be used not only for building iOS and macOS applications but also for writing scripts. Running Swift scripts from the command line allows you to quickly execute code without the need for a full-fledged Xcode project.

In this tutorial, we'll explore how to run Swift scripts from the command line on macOS.

## Prerequisites

To follow along with this tutorial, you'll need the following:

- A macOS machine
- Xcode installed (which includes the Swift compiler)

## Writing a Swift script

Let's start by creating a simple Swift script. Open your favorite text editor and create a new file, `hello.swift`. In this example, we'll write a script that prints "Hello, World!" to the console.

```swift
#!/usr/bin/env swift

print("Hello, World!")
```

Save the file and make it executable by running the following command in the terminal:

```bash
chmod +x hello.swift
```

## Running the script

Now that we have our Swift script ready, we can execute it from the command line. Simply navigate to the directory where the script is located and run the following command to execute the script:

```bash
./hello.swift
```

You should see the output `Hello, World!` printed to the console.

## Passing command-line arguments

Swift scripts can also accept command-line arguments. Let's modify our script to take a name as an argument and print a personalized greeting.

```swift
#!/usr/bin/env swift

import Foundation

if CommandLine.arguments.count < 2 {
    print("Usage: ./hello.swift <name>")
} else {
    let name = CommandLine.arguments[1]
    print("Hello, \(name)!")
}
```

Save the updated script and make it executable using the same `chmod` command as before. Now, you can pass a name as an argument when running the script:

```bash
./hello.swift John
```

You should see the output `Hello, John!` printed to the console.

## Conclusion

Running Swift scripts from the command line provides a convenient and efficient way to execute Swift code outside of a full Xcode project. You can leverage the versatility of the Swift language to write scripts for automating tasks, processing data, or performing other operations.