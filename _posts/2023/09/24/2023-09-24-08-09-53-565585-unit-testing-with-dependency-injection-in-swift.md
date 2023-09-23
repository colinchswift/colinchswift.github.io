---
layout: post
title: "Unit testing with dependency injection in Swift"
description: " "
date: 2023-09-24
tags: [testing, dependencyinjection]
comments: true
share: true
---

Unit testing is a crucial aspect of software development as it helps ensure the correctness and stability of our code. Dependency injection is a design pattern that enables us to write testable code by allowing us to inject dependencies into our classes or functions. In this blog post, we will explore how to perform unit testing with dependency injection in Swift.

## Why use Dependency Injection for Unit Testing?

When writing unit tests, we often encounter situations where our code depends on external resources such as network calls, databases, or other classes. These dependencies can make our code difficult to test because they introduce unpredictability and make it challenging to isolate specific units of code for testing.

By using dependency injection, we can decouple our code from its dependencies and replace them with test doubles or mocks during unit testing. This allows us to have greater control over the test environment, making our tests more reliable and reproducible.

## How to Use Dependency Injection for Unit Testing in Swift

Let's start by understanding how to use dependency injection for unit testing in Swift. Here's an example:

```swift
class DataManager {
    let networkManager: NetworkManager

    init(networkManager: NetworkManager) {
        self.networkManager = networkManager
    }

    func fetchData(completion: @escaping (Result<Data, Error>) -> Void) {
        // Fetch data from the network using networkManager
        networkManager.fetchData { result in
            completion(result)
        }
    }
}

// Example usage
let networkManager = NetworkManager()
let dataManager = DataManager(networkManager: networkManager)
dataManager.fetchData { result in
    // Handle result
}
```

In the code above, `DataManager` has a dependency on `NetworkManager` to fetch data from the network. By injecting `networkManager` into the `DataManager` initializer, we can easily replace it with a test double during unit testing.

## Unit Testing with Dependency Injection in Swift

To perform unit testing with dependency injection in Swift, we need to create a test double for `NetworkManager`. Here's an example using a mock:

```swift
class MockNetworkManager: NetworkManager {
    var fetchDataCalled = false

    override func fetchData(completion: @escaping (Result<Data, Error>) -> Void) {
        fetchDataCalled = true
        // Call the completion handler with test data
        completion(.success(Data()))
    }
}

class DataManagerTests: XCTestCase {
    func testFetchData() {
        let mockNetworkManager = MockNetworkManager()
        let dataManager = DataManager(networkManager: mockNetworkManager)

        dataManager.fetchData { result in
            XCTAssert(mockNetworkManager.fetchDataCalled)
            // Additional assertions for the data returned
        }
    }
}
```

In the unit test above, we instantiate a `MockNetworkManager` and pass it to the `DataManager`'s initializer. We then call the `fetchData` method on `dataManager` and assert that the `fetchData` method was called on the mock network manager.

By using dependency injection and test doubles, we have created a controlled environment for testing the `DataManager` class, without relying on the actual network request.

## Conclusion

In this blog post, we learned how to perform unit testing with dependency injection in Swift. By decoupling our code from its dependencies and injecting them during testing, we can create more reliable and isolated unit tests. This approach allows us to have fine-grained control over the test environment, making our tests easier to write, read, and maintain.

#testing #dependencyinjection #swift