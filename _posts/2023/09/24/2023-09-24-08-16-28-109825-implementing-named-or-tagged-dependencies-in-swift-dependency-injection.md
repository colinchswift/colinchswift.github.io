---
layout: post
title: "Implementing named or tagged dependencies in Swift dependency injection"
description: " "
date: 2023-09-24
tags: [swift, dependencyinjection]
comments: true
share: true
---

In dependency injection (DI), we often need to resolve dependencies based on a name or tag. This allows us to differentiate between multiple dependencies of the same type. While Swift's built-in dependency injection using initializers is powerful, it doesn't provide a straightforward way to handle named or tagged dependencies out of the box. In this blog post, we will explore how to implement named or tagged dependencies in Swift using a simple yet effective approach.

## Understanding the Problem

Let's say we have a class `UserService` that depends on a protocol `DataStorage`. There are two implementations of `DataStorage` - `FileStorage` and `DatabaseStorage`. When injecting the dependency into `UserService`, we need to specify which implementation to use.

## Approach

To implement named or tagged dependencies in Swift, we can leverage Swift's built-in `KeyPath` feature. Using `KeyPath`, we can specify a unique key for each implementation of a dependency. 

First, let's define a protocol `DependencyKey` that acts as a marker protocol for dependency keys:

```swift
protocol DependencyKey {}
```

Next, we can create a type-safe enumeration of dependency keys for our `DataStorage` implementations:

```swift
enum DataStorageKey: DependencyKey {
    case file
    case database
}
```

Now, we modify `DataStorage` and its implementations to use the dependency keys:

```swift
protocol DataStorage {
    func save(data: Data)
}

class FileStorage: DataStorage {
    func save(data: Data) {
        // Save data to file
    }
}

class DatabaseStorage: DataStorage {
    func save(data: Data) {
        // Save data to database
    }
}
```

Then, we update the `UserService` to accept a dependency key along with the `DataStorage` dependency:

```swift
class UserService {
    let dataStorage: DataStorage

    init(dataStorage: DataStorage, key: DependencyKey) {
        switch key {
        case DataStorageKey.file:
            self.dataStorage = FileStorage()
        case DataStorageKey.database:
            self.dataStorage = DatabaseStorage()
        default:
            fatalError("Unknown dependency key")
        }
    }
}
```

Finally, when injecting dependencies, we can use the appropriate key to specify which implementation to use:

```swift
let userService = UserService(dataStorage: FileStorage(), key: DataStorageKey.file)
```

## Conclusion

By using a combination of `KeyPath` and marker protocols, we can easily implement named or tagged dependencies in Swift. This approach allows us to differentiate between multiple implementations of the same dependency type, providing more flexibility and control in our dependency injection setup.

#swift #dependencyinjection