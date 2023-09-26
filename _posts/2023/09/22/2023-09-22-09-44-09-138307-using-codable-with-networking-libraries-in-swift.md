---
layout: post
title: "Using Codable with networking libraries in Swift"
description: " "
date: 2023-09-22
tags: [Codable]
comments: true
share: true
---

With the introduction of Codable in Swift 4, working with JSON responses from networking calls has become even easier. Codable allows for seamless conversion between Swift types and JSON, eliminating the need to manually parse JSON responses. In this article, we will explore how to use Codable with popular networking libraries in Swift.

## Networking Libraries

There are several popular networking libraries available in Swift, such as Alamofire, URLSession, and Moya. We will focus on Alamofire, a widely-used networking library that provides a simplified interface for making HTTP requests.

## Codable with Alamofire

To use Codable with Alamofire, we need to define our model objects conforming to the Codable protocol. Suppose we have a simple API endpoint that returns a JSON response like this:

```swift
{
  "name": "John Doe",
  "age": 30
}
```

We can define a `Person` struct representing this JSON structure as follows:

```swift
struct Person: Codable {
    let name: String
    let age: Int
}
```

Now, let's see how we can use Alamofire to make a GET request to this API endpoint and decode the JSON response into our `Person` model:

```swift
import Alamofire

func fetchPerson(completion: @escaping (Result<Person, Error>) -> Void) {
    AF.request("https://api.example.com/person")
        .validate()
        .responseDecodable(of: Person.self) { response in
            switch response.result {
            case .success(let person):
                completion(.success(person))
            case .failure(let error):
                completion(.failure(error))
            }
    }
}
```

In the above example, we use the `responseDecodable` method provided by Alamofire to automatically decode the JSON response into our `Person` model. The completion closure returns a `Result` type, which is a Swift standard library type that represents a success or failure with an associated value.

## Codable with URLSession

If you prefer to use URLSession for networking instead of Alamofire, the process is similar. We define our Codable model objects as before and use URLSession to make the request:

```swift
func fetchPerson(completion: @escaping (Result<Person, Error>) -> Void) {
    guard let url = URL(string: "https://api.example.com/person") else {
        completion(.failure(NSError(domain: "", code: 0, userInfo: nil)))
        return
    }
    
    URLSession.shared.dataTask(with: url) { (data, response, error) in
        if let error = error {
            completion(.failure(error))
        } else if let data = data {
            do {
                let person = try JSONDecoder().decode(Person.self, from: data)
                completion(.success(person))
            } catch {
                completion(.failure(error))
            }
        }
    }.resume()
}
```

In this example, we use `URLSession.shared.dataTask` to create a data task that performs the GET request. The JSON response is then decoded using `JSONDecoder` and passed to the completion closure.

## Conclusion

Using Codable with networking libraries in Swift simplifies the process of handling JSON responses. Whether you choose to use Alamofire or URLSession, Codable provides a convenient way to map JSON data to Swift objects. This allows for cleaner and more maintainable code when working with APIs. Give it a try in your next networking project!

#Swift #Codable