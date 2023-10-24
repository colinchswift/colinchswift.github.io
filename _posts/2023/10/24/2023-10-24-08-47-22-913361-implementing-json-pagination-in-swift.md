---
layout: post
title: "Implementing JSON pagination in Swift"
description: " "
date: 2023-10-24
tags: []
comments: true
share: true
---

When working with APIs that return large amounts of data, it is essential to implement pagination to retrieve data in batches and avoid overwhelming network requests. JSON pagination is a common approach to handle this task.

In this blog post, we will explore how to implement JSON pagination in Swift, leveraging URLSession and Codable to handle network requests and parse JSON responses.

## Table of Contents
- [Understanding JSON Pagination](#understanding-json-pagination)
- [Setting Up the Project](#setting-up-the-project)
- [Implementing JSON Pagination](#implementing-json-pagination)
  - [Making the Initial Request](#making-the-initial-request)
  - [Parsing the JSON Response](#parsing-the-json-response)
  - [Fetching Additional Pages](#fetching-additional-pages)
- [Conclusion](#conclusion)

## Understanding JSON Pagination

JSON pagination typically involves two main components: 
- A request to retrieve the initial page of data.
- The use of pagination parameters (e.g., page number, items per page) to fetch subsequent pages.

The API response usually includes metadata containing information about the total number of pages, the current page number, and any links to navigate between pages.

## Setting Up the Project

Before diving into the implementation, we need to set up a Swift project. Follow these steps:

1. Open Xcode and create a new project using the "Single View App" template.
2. Give your project a name and choose Swift as the language.
3. Open the `ViewController.swift` file and remove the default code.

## Implementing JSON Pagination

Let's start by implementing the logic to handle JSON pagination:

### Making the Initial Request

```swift
struct Person: Codable {
    let id: Int
    let name: String
    // Additional properties
}

func fetchInitialData(completion: @escaping ([Person]) -> Void) {
    let url = URL(string: "https://example.com/api/persons")!
    
    URLSession.shared.dataTask(with: url) { data, response, error in
        guard let jsonData = data else {
            completion([])
            return
        }
        
        do {
            let persons = try JSONDecoder().decode([Person].self, from: jsonData)
            completion(persons)
        } catch {
            print("Error decoding JSON: \(error.localizedDescription)")
            completion([])
        }
    }.resume()
}
```

In the above code, we define a `Person` struct that represents the data we expect to receive from the API. The `fetchInitialData` function sends a request to the API endpoint and decodes the response into an array of `Person` objects. The completion closure is called with the fetched persons or an empty array if there was an error.

### Parsing the JSON Response

Once we receive the initial page of data, we can extract the pagination metadata from the JSON response. Usually, this information is provided in the response headers or within the body itself. Let's assume the API includes the metadata in the response body as a dictionary:

```swift
struct APIResponse<T: Codable>: Codable {
    let data: T
    let pageInfo: PageInfo
}

struct PageInfo: Codable {
    let totalRecords: Int
    let totalPages: Int
    let currentPage: Int
}

// Update fetchInitialData function
func fetchInitialData(completion: @escaping ([Person]) -> Void) {
    // ...

    do {
        let apiResponse = try JSONDecoder().decode(APIResponse<[Person]>.self, from: jsonData)
        completion(apiResponse.data)
        
        let totalPages = apiResponse.pageInfo.totalPages
        let currentPage = apiResponse.pageInfo.currentPage
        // Store this information to fetch additional pages later
    } catch {
        // ...
    }
}
```

The updated code introduces two new structs: `APIResponse` and `PageInfo`. These structs represent the structure of the API JSON response. We extract the pagination metadata from `apiResponse.pageInfo` and store it for fetching additional pages later.

### Fetching Additional Pages

To fetch subsequent pages, we need to include the appropriate pagination parameters in the request. Assuming the API accepts the `page` parameter to specify the page number, we can modify the `fetchInitialData` function to fetch additional pages:

```swift
func fetchNextPage(page: Int, completion: @escaping ([Person]) -> Void) {
    var components = URLComponents(string: "https://example.com/api/persons")!
    components.queryItems = [
        URLQueryItem(name: "page", value: "\(page)")
    ]
    
    guard let url = components.url else {
        completion([])
        return
    }
    
    URLSession.shared.dataTask(with: url) { data, response, error in
        // ...
    }.resume()
}
```

In the `fetchNextPage` function, we construct the URL by including the `page` parameter in the query string. We then perform a data task to fetch the data for the next page and handle it similarly to the initial request.

To fetch subsequent pages, you can loop through the total number of pages and call `fetchNextPage` with appropriate page numbers.

## Conclusion

Implementing JSON pagination in Swift allows us to efficiently retrieve and handle large amounts of data from APIs. By leveraging URLSession and Codable, we can fetch and parse JSON responses, maintaining control over the pagination process.

By implementing pagination, we can ensure that our applications can handle large datasets without overwhelming network requests.