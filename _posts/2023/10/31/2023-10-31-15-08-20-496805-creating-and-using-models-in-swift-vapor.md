---
layout: post
title: "Creating and using models in Swift Vapor"
description: " "
date: 2023-10-31
tags: []
comments: true
share: true
---

In Swift Vapor, models are used to represent and manipulate data from your application's database. They help to organize and structure the data, making it easier to work with.

## Creating a Model

To create a model in Swift Vapor, you'll need to define a struct or class that conforms to the `Model` protocol. This protocol provides a set of properties and methods that models should implement.

Here's an example of how you can create a simple model for a `User`:

```swift
import Vapor
import Fluent

final class User: Model {
    static let schema = "users"
    
    @ID(key: .id)
    var id: UUID?
    
    @Field(key: "name")
    var name: String
    
    // ...
    
    init() { }
}
```

In this example, the `User` class conforms to the `Model` protocol and includes an `id` property which is used as the primary key for the model. The `name` property is annotated with `@Field` and maps to a column in the database table.

To use this model, you'll also need to configure a database and migration. You can do this in your `configure.swift` file:

```swift
import Fluent

public func configure(_ app: Application) throws {
    app.databases.use(.postgres(
        hostname: "localhost",
        username: "postgres",
        password: "password",
        database: "mydatabase"
    ), as: .psql)
    
    app.migrations.add(User.migration)
}
```

## Querying and Manipulating Data

Once you have defined your model, you can use Fluent, which is Vapor's database toolkit, to query and manipulate data from your model.

Here's an example of how to create a new user and save it to the database:

```swift
let user = User()
user.name = "John Doe"

try user.save(on: req.db).wait()
```

And here's an example of how to fetch all users from the database:

```swift
let users = User.query(on: req.db).all()
```

These are just a few examples of what you can do with models in Swift Vapor. Check out the [Vapor documentation](https://docs.vapor.codes/4.0/fluent/model/) for more information on how to work with models and databases in Vapor.

## Conclusion

Models are an essential part of building robust and scalable applications in Swift Vapor. They help to structure and organize your data, making it easier to work with your application's database. By using Fluent, you can easily query and manipulate data from your models.