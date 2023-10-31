---
layout: post
title: "Working with databases in Swift Vapor"
description: " "
date: 2023-10-31
tags: []
comments: true
share: true
---

### Introduction

In Swift Vapor, working with databases plays a crucial role in building scalable and robust applications. In this blog post, we will explore various aspects of working with databases in Swift Vapor.

### Table of Contents
- [Setting up a Database](#setting-up-a-database)
- [Connecting to the Database](#connecting-to-the-database)
- [Creating Models](#creating-models)
- [Querying the Database](#querying-the-database)
- [Updating and Deleting Data](#updating-and-deleting-data)
- [Conclusion](#conclusion)

### Setting up a Database

Swift Vapor provides support for various databases like PostgreSQL, MySQL, SQLite, and more. To use a specific database, you need to add the appropriate package dependency to your project.

For example, to work with PostgreSQL, add the following dependency to your `Package.swift` file:

```swift
.package(url: "https://github.com/vapor/postgresql.git", from: "1.0.0")
```

### Connecting to the Database

Once you have added the database package dependency, you can configure your database connection in the `configure.swift` file. 

Here's an example of connecting to a PostgreSQL database:

```swift
import FluentPostgresDriver

app.databases.use(.postgres(
    hostname: "localhost",
    port: 5432,
    username: "myusername",
    password: "mypassword",
    database: "mydatabase"
), as: .psql)
```

### Creating Models

In Swift Vapor, models represent tables in the database. You can define models using structs that conform to the `Model` protocol provided by the Fluent ORM.

Here's an example of defining a `User` model:

```swift
import Fluent

final class User: Model {
    static let schema = "users"

    @ID(key: .id)
    var id: UUID?

    @Field(key: "name")
    var name: String

    init() {}

    init(id: UUID? = nil, name: String) {
        self.id = id
        self.name = name
    }
}
```

### Querying the Database

To query the database and fetch data, Swift Vapor provides a powerful query builder. You can use this query builder to build complex queries and retrieve data from the database.

Here's an example of fetching all users from the `users` table:

```swift
User.query(on: app.db).all().map { users in
    for user in users {
        print(user.name)
    }
}
```

### Updating and Deleting Data

Updating and deleting data in the database is also straightforward using Fluent. You can modify the properties of a model object and call the `save()` method to update the corresponding row in the database.

Here's an example of updating a user's name:

```swift
User.find(id, on: app.db).map { user in
    user?.name = "Updated Name"
    return user?.save(on: app.db)
}
```

To delete a row from the database, you can call the `delete()` method on the model object.

```swift
User.find(id, on: app.db).map { user in
    return user?.delete(on: app.db)
}
```

### Conclusion

Working with databases in Swift Vapor is made easy with the powerful Fluent ORM. In this blog post, we learned how to set up a database connection, create models, query the database, and perform CRUD operations. Swift Vapor's integration with various databases makes it a great choice for building database-driven applications.

References:
- [Swift Vapor Documentation](https://docs.vapor.codes/4.0/fluent/)
- [Fluent ORM GitHub Repository](https://github.com/vapor/fluent)