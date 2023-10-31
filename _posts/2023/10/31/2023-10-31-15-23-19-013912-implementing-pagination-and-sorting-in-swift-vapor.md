---
layout: post
title: "Implementing pagination and sorting in Swift Vapor"
description: " "
date: 2023-10-31
tags: [Vapor]
comments: true
share: true
---

In web applications, handling large sets of data often requires pagination and sorting functionality to improve user experience and performance. In this tutorial, we'll explore how to implement pagination and sorting in Swift Vapor, a popular web framework written in Swift.

## Prerequisites

Before proceeding, ensure that you have Vapor installed and set up in your development environment. You can find detailed instructions in the [Vapor documentation](https://docs.vapor.codes/4.0/).

## Setting up the Project

To keep things simple, let's assume that we have a `User` model with properties like `id`, `name`, and `email`. Begin by creating a new Vapor project or use an existing project that already has the basic CRUD operations implemented.

## Pagination

To implement pagination, we need to define the page number and the number of items to be displayed per page. We'll create a `GET` route to retrieve a paginated list of users.

1. Open the `routes.swift` file in the `Sources/App` directory.

2. Define a new route to handle the paginated user list. Add the following code:

   ```swift
   // Get paginated list of users
   router.get("users", ":page", ":size") { req -> EventLoopFuture<Page<User>> in
       let page = try req.parameters.require("page", as: Int.self)
       let size = try req.parameters.require("size", as: Int.self)
       
       return User.query(on: req.db)
           .range((page - 1) * size ..< page * size)  // Apply pagination using range
           .all()
           .flatMapThrowing { users in
               let total = try User.query(on: req.db).count().wait()  // Get total count for pagination
               let page = Page(items: users, pageNumber: page, pageSize: size, totalItems: total)
               return page
           }
   }
   ```

   In the above code, we extract the page number and size from the URL parameters and use the `range` method to apply pagination. We then fetch the paginated user list and calculate the total count for proper pagination using `count().wait()`.

3. Additionally, define a custom `Page` struct to hold the paginated data. Add the following code at the bottom of the `routes.swift` file:

   ```swift
   struct Page<T: Content>: Content {
       let items: [T]
       let pageNumber: Int
       let pageSize: Int
       let totalItems: Int
   }
   ```

   This struct will be used to return the paginated data as JSON.

## Sorting

Adding sorting functionality is relatively straightforward. We'll modify the existing `GET` route for the user list to accept a sorting parameter and apply the requested sorting on the query.

1. Open the `routes.swift` file again.

2. Update the route for the paginated user list by adding an optional `sort` parameter. Modify the code as shown below:

   ```swift
   // Get paginated and sorted list of users
   router.get("users", ":page", ":size", ":sort") { req -> EventLoopFuture<Page<User>> in
       let page = try req.parameters.require("page", as: Int.self)
       let size = try req.parameters.require("size", as: Int.self)
       let sort = try req.parameters.optional("sort", as: String.self)
       
       var query = User.query(on: req.db)
       if let sort = sort {
         query = User.query(on: req.db).sort(\.name, .custom(sort, .ignoreCase))
       }
       
       return query
           .range((page - 1) * size ..< page * size)
           .all()
           .flatMapThrowing { users in
               let total = try User.query(on: req.db).count().wait()
               let page = Page(items: users, pageNumber: page, pageSize: size, totalItems: total)
               return page
           }
   }
   ```

   Here, we accept an optional `sort` parameter from the URL. If provided, we apply the sorting on the user query using the `sort` method.

3. Finally, update the `Page` struct to include the sorting parameter:

   ```swift
   struct Page<T: Content>: Content {
       let items: [T]
       let pageNumber: Int
       let pageSize: Int
       let totalItems: Int
       let sort: String?  // Add sort parameter
   }
   ```

## Conclusion

In this tutorial, we explored how to implement pagination and sorting in Swift Vapor. By adding pagination and sorting functionality, you can efficiently handle large sets of data in your Vapor applications and provide a better user experience.

Remember to consult the [Vapor documentation](https://docs.vapor.codes/4.0/) for more information on advanced features and additional capabilities of Vapor. Happy coding!

#hashtags
#Swift #Vapor