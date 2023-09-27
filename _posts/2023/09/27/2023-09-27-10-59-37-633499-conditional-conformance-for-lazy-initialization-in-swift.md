---
layout: post
title: "Conditional conformance for lazy initialization in Swift"
description: " "
date: 2023-09-27
tags: [Swift, ConditionalConformance]
comments: true
share: true
---

In Swift, lazy initialization allows us to delay the creation of an object until it is needed. It is a powerful feature that helps improve performance by avoiding unnecessary overhead. However, sometimes we need to provide different implementations of lazy initialization based on certain conditions. This is where conditional conformance comes into play.

Conditional conformance is a language feature in Swift that allows you to specify a condition under which a generic type conforms to a protocol. With conditional conformance, we can have different implementations of lazy initialization for different types.

## Example Scenario

Let's say we have a `ViewController` that needs to display a list of items. The list can be fetched either from a local database or from a remote API, depending on the availability of internet connectivity. We want to lazily initialize the data source based on this condition.

```swift
protocol DataSource {
    associatedtype ItemType
    func fetchData() -> [ItemType]
}

struct LocalDataSource<T>: DataSource {
    func fetchData() -> [T] {
        // Implementation for local data source
    }
}

struct APIDataSource<T>: DataSource {
    func fetchData() -> [T] {
        // Implementation for API data source
    }
}

class ViewController {
    private lazy var dataSource: DataSource = {
        if isInternetConnected {
            return APIDataSource<Item>()
        } else {
            return LocalDataSource<Item>()
        }
    }()
    
    private var isInternetConnected: Bool {
        // Check internet connectivity
    }
    
    // Other methods and properties
}
```

In the above example, we have defined a protocol `DataSource` that declares a method `fetchData()` which returns an array of items. We then have two implementations of `DataSource`: `LocalDataSource` and `APIDataSource`, representing fetching data from a local database and a remote API respectively.

In the `ViewController` class, we have a lazy property `dataSource` of type `DataSource`. Depending on the availability of internet connectivity, we create either an instance of `APIDataSource` or `LocalDataSource` and assign it to the `dataSource` property.

## Conditional Conformance

To enable conditional conformance, we need to add a conditional where clause to the protocol declaration, specifying the condition under which the generic type conforms to the protocol. In our example, we'll add a conditional where clause to the `DataSource` protocol to ensure that only types conforming to `DataSource` with a specific associated type can be initialized lazily.

```swift
protocol DataSource {
    associatedtype ItemType
    func fetchData() -> [ItemType]
}

extension DataSource where Self: LocalDataSource<ItemType> {
    func fetchData() -> [ItemType] {
        // Implementation for local data source
    }
}

extension DataSource where Self: APIDataSource<ItemType> {
    func fetchData() -> [ItemType] {
        // Implementation for API data source
    }
}
```

In the above code, we extend the `DataSource` protocol with two extensions. One for `LocalDataSource` and another for `APIDataSource`. Each extension provides its own implementation of `fetchData()` based on the specific data source.

Now, when we define the `dataSource` property in the `ViewController` class, we don't need to explicitly specify the type anymore. Swift's type inference will automatically infer the correct type based on the conditional conformance.

```swift
private lazy var dataSource = DataSource()
```

## Conclusion

Conditional conformance in Swift enables us to have different implementations based on specific conditions. It allows us to take advantage of lazy initialization with different types based on the conditions, providing flexibility and maintainability to our codebase. By using conditional conformance, we can easily handle different scenarios and improve the efficiency of our applications.

#Swift #ConditionalConformance