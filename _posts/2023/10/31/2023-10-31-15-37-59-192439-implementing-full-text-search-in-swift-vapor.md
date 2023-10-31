---
layout: post
title: "Implementing full-text search in Swift Vapor"
description: " "
date: 2023-10-31
tags: []
comments: true
share: true
---

In modern web applications, implementing search functionality is a crucial requirement. Full-text search allows users to search for specific keywords within a large dataset, providing more accurate and relevant results.

In this tutorial, we will explore how to implement full-text search in a Swift Vapor project. We'll be using PostgreSQL as our database and leveraging its powerful full-text search capabilities.

## Prerequisites

Before we start, make sure you have the following installed:

- Swift (latest version)
- Vapor (latest version)
- PostgreSQL (latest version)

## Setting Up the Project

1. Create a new Vapor project by running the following command:
```swift
vapor new FullTextSearchProject
```

2. Navigate to the project directory:
```swift
cd FullTextSearchProject
```

3. Open the project in your preferred editor.

4. Configure the PostgreSQL database in your `configure.swift` file:
```swift
import FluentPostgreSQL
import Vapor

public func configure(_ config: inout Config, _ env: inout Environment, _ services: inout Services) throws {
    let postgresqlConfig = PostgreSQLDatabaseConfig(hostname: "localhost", username: "postgres", database: "fulltextsearchdb", password: "password")
    let postgresql = PostgreSQLDatabase(config: postgresqlConfig)
    var databases = DatabasesConfig()
    databases.add(database: postgresql, as: .psql)
    services.register(databases)
    
    // ... Other configurations
}
```

Make sure to replace the values for `hostname`, `username`, `database`, and `password` with your own PostgreSQL credentials.

## Creating a Full-Text Search Index

1. Create a new migration file to add a full-text search index to your desired model. For example, let's say we have a `Post` model:
```swift
import FluentPostgreSQL
import Vapor

struct AddFullTextSearchIndexToPost: PostgreSQLMigration {
    static func prepare(on connection: PostgreSQLConnection) -> Future<Void> {
        return Database.update(Post.self, on: connection) { builder in
            builder.field(for: \.title)
            builder.field(for: \.content)
            
            // Add the full-text search index
            builder.custom("CREATE INDEX post_search_idx ON \(Post.entity). USING gin(to_tsvector('english', title || ' ' || content))")
        }
    }
    
    static func revert(on connection: PostgreSQLConnection) -> Future<Void> {
        return Database.update(Post.self, on: connection) { builder in
            builder.deleteIndexIfExists("post_search_idx")
        }
    }
}
```

2. Apply the migration by running the following command:
```swift
vapor run migrate
```

This will create a new full-text search index named `post_search_idx` on the `title` and `content` fields of the `Post` model.

## Performing a Full-Text Search

Now that we have set up the full-text search index, let's see how we can perform a search query in our Vapor application.

1. Create a new route handler in your `routes.swift` file to handle the search request:
```swift
import Vapor

func searchHandler(_ req: Request) throws -> Future<[Post]> {
    guard let searchTerm = req.query[String.self, at: "q"] else {
        throw Abort(.badRequest)
    }
    
    return Post.query(on: req).group(.or) { queryGroup in
        queryGroup.filter(\.title ~~ searchTerm)
        queryGroup.filter(\.content ~~ searchTerm)
    }.all()
}

func routes(_ router: Router) throws {
    router.get("search", use: searchHandler)
}
```

2. Make a GET request to `/search?q={searchTerm}` with your desired search term to retrieve the search results.

## Conclusion

In this tutorial, we've learned how to implement full-text search in Swift Vapor using PostgreSQL. We set up a full-text search index and performed search queries in our Vapor application.

Full-text search is a powerful feature that enhances the search functionality of your application and provides more accurate search results. By utilizing the capabilities of PostgreSQL, you can easily implement full-text search in your Swift Vapor project. Happy coding!

**References:**
- [Vapor official documentation](https://docs.vapor.codes/)
- [PostgreSQL full-text search documentation](https://www.postgresql.org/docs/current/textsearch.html)