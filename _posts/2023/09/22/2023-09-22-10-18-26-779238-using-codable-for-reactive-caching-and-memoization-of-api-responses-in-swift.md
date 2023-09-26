---
layout: post
title: "Using Codable for reactive caching and memoization of API responses in Swift"
description: " "
date: 2023-09-22
tags: [coding]
comments: true
share: true
---

In Swift, Codable is a powerful protocol that allows for easy serialization and deserialization of data to and from various formats, such as JSON. One great use case for Codable is to cache and memoize API responses in a reactive manner. In this blog post, I will demonstrate how to achieve this using Codable and reactive programming.

## Why use Codable for caching API responses?

Caching API responses can greatly improve the performance and responsiveness of your app. By storing the response data locally, you can avoid making unnecessary network requests and provide a quicker user experience. Codable makes it easy to serialize and deserialize JSON data, making it a perfect fit for caching API responses.

## Setting up the caching mechanism

To get started, let's create a simple caching class that will handle the caching logic. The class will have methods to save and retrieve data from the cache.

```swift
class APICache {
    static let shared = APICache()
    private var cache = NSCache<NSString, NSData>()

    private init() {}

    func save<T: Codable>(key: String, data: T) {
        let encoder = JSONEncoder()
        if let encodedData = try? encoder.encode(data) {
            cache.setObject(encodedData as NSData, forKey: key as NSString)
        }
    }

    func retrieve<T: Codable>(key: String) -> T? {
        if let cachedData = cache.object(forKey: key as NSString) as Data? {
            let decoder = JSONDecoder()
            if let decodedData = try? decoder.decode(T.self, from: cachedData) {
                return decodedData
            }
        }
        return nil
    }
}
```

## Caching API responses

Let's say we have an API endpoint that fetches a list of users. We can use the caching mechanism to store the response locally and retrieve it when needed.

```swift
struct User: Codable {
    let id: Int
    let name: String
}

func fetchUsers() {
    if let cachedUsers: [User] = APICache.shared.retrieve(key: "users") {
        // Use the cached data
        print("Using cached users: \(cachedUsers)")
    } else {
        // Fetch users from API
        APIManager.fetchUsers { result in
            switch result {
            case .success(let users):
                // Save the fetched users to cache
                APICache.shared.save(key: "users", data: users)
                print("Fetched and cached users: \(users)")
            case .failure(let error):
                print("Failed to fetch users: \(error)")
            }
        }
    }
}
```

In the code snippet above, we first check if the cached users exist using `APICache.shared.retrieve()`. If they do, we use the cached data. If not, we fetch the users from the API using our APIManager class and save the fetched users to the cache using `APICache.shared.save()`.

## Reactive caching with Combine

If you're using Combine for reactive programming, you can take advantage of its operators to create a reactive caching mechanism. Here's an example of how you can accomplish this:

```swift
import Combine

class APIManager {
    static func fetchUsers() -> AnyPublisher<[User], Error> {
        // Implement API request using Combine
        // ...
    }
}

class CacheManager {
    @Published var users: [User]?

    private var cancellable: AnyCancellable?

    func fetchAndCacheUsers() {
        if users != nil {
            // Use the cached users
            print("Using cached users: \(users!)")
        } else {
            // Fetch users from API
            cancellable = APIManager.fetchUsers()
                .sink(receiveCompletion: { completion in
                    // Handle error
                    if case .failure(let error) = completion {
                        print("Failed to fetch users: \(error)")
                    }
                }, receiveValue: { [weak self] users in
                    // Save the fetched users to cache
                    self?.users = users
                    print("Fetched and cached users: \(users)")
                })
        }
    }
}
```

In this example, we use the `@Published` property wrapper from Combine to create a reactive `users` property in our `CacheManager` class. When the `users` property is accessed, it will either use the cached data or fetch it from the API and update the cache accordingly.

By combining the power of Codable and reactive programming frameworks like Combine, you can create efficient and responsive caching mechanisms for your API responses in Swift.

#swift #coding