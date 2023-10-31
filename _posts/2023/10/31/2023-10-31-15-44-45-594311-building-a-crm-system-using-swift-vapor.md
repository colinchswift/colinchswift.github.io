---
layout: post
title: "Building a CRM system using Swift Vapor"
description: " "
date: 2023-10-31
tags: []
comments: true
share: true
---

## Introduction

In this blog post, we will explore how to build a CRM (Customer Relationship Management) system using Swift Vapor, a popular server-side Swift web framework. We'll cover the basic setup, database modeling, API endpoints, and authentication mechanisms.

## Prerequisites

Before we dive into building our CRM system, make sure you have the following installed:

- Swift 5.0 or later
- Vapor 4.0 or later
- Xcode or another code editor of your choice

## Setup

First, let's create a new Vapor project by running the following terminal command:

```shell
vapor new CRMSystem
```

Next, navigate to the project directory:

```shell
cd CRMSystem
```

## Database Modeling

To manage our CRM data, we'll be using a relational database. Vapor supports multiple databases, including SQLite, MySQL, and PostgreSQL. Here, we'll use SQLite for simplicity.

1. Open the `Configure.swift` file in the `App` directory and modify the `configure(_:)` function as follows:

```swift
import FluentSQLite

public func configure(_ app: Application) throws {
    // ...
    app.databases.use(.sqlite(.file("crm_system.sqlite")), as: .sqlite)
    // ...
}
```

2. Create a new file named `Contact.swift` in the `Sources/App/Models` directory with the following code:

```swift
import FluentSQLite
import Vapor

final class Contact: Codable {
    var id: Int?
    var name: String
    var email: String
    var phone: String

    init(name: String, email: String, phone: String) {
        self.name = name
        self.email = email
        self.phone = phone
    }
}
```

3. Open the `Configure.swift` file again and update the `configure(_:)` function to add the contact migration:

```swift
import Fluent

public func configure(_ app: Application) throws {
    // ...
    app.migrations.add(CreateContact())
    // ...
    try app.autoMigrate().wait()
}
```

4. Create a new file named `CreateContact.swift` in the `Sources/App/Migrations` directory with the following code:

```swift
import Fluent

struct CreateContact: Migration {
    func prepare(on database: Database) -> EventLoopFuture<Void> {
        return database.schema("contacts")
            .id()
            .field("name", .string, .required)
            .field("email", .string, .required)
            .field("phone", .string, .required)
            .create()
    }

    func revert(on database: Database) -> EventLoopFuture<Void> {
        return database.schema("contacts").delete()
    }
}
```

## API Endpoints

Now that we have our database model set up, let's create the API endpoints to manage contacts.

1. Open the `routes.swift` file in the `Sources/App` directory and update the `routes(_:)` function as follows:

```swift
import Vapor

func routes(_ app: Application) throws {
    // ...
    let contactsController = ContactController()
    app.get("contacts", use: contactsController.index)
    app.post("contacts", use: contactsController.create)
    app.get("contacts", ":id", use: contactsController.show)
    app.put("contacts", ":id", use: contactsController.update)
    app.delete("contacts", ":id", use: contactsController.delete)
    // ...
}
```

2. Create a new file named `ContactController.swift` in the `Sources/App/Controllers` directory with the following code:

```swift
import Vapor

struct ContactController {
    func index(req: Request) throws -> EventLoopFuture<[Contact]> {
        return Contact.query(on: req.db).all()
    }

    func create(req: Request) throws -> EventLoopFuture<Contact> {
        let contact = try req.content.decode(Contact.self)
        return contact.save(on: req.db).map { contact }
    }

    func show(req: Request) throws -> EventLoopFuture<Contact> {
        guard let id = req.parameters.get("id", as: Int.self) else {
            throw Abort(.badRequest)
        }
        return Contact.find(id, on: req.db)
            .unwrap(or: Abort(.notFound))
    }

    func update(req: Request) throws -> EventLoopFuture<Contact> {
        guard let id = req.parameters.get("id", as: Int.self) else {
            throw Abort(.badRequest)
        }
        let updatedContact = try req.content.decode(Contact.self)
        return Contact.find(id, on: req.db)
            .unwrap(or: Abort(.notFound))
            .flatMap { contact in
                contact.name = updatedContact.name
                contact.email = updatedContact.email
                contact.phone = updatedContact.phone
                return contact.update(on: req.db).map { contact }
            }
    }

    func delete(req: Request) throws -> EventLoopFuture<HTTPStatus> {
        guard let id = req.parameters.get("id", as: Int.self) else {
            throw Abort(.badRequest)
        }
        return Contact.find(id, on: req.db)
            .unwrap(or: Abort(.notFound))
            .flatMap { contact in
                return contact.delete(on: req.db)
            }
            .transform(to: .ok)
    }
}
```

## Authentication

To secure our API, we'll implement token-based authentication using JSON Web Tokens (JWT).

1. Add the `JWT` package to your `Package.swift` file:

```swift
.package(url: "https://github.com/vapor/jwt.git", from: "4.0.0")
```

2. Update the `configure(_:)` function in the `Configure.swift` file to set up JWT authentication:

```swift
import JWT

public func configure(_ app: Application) throws {
    // ...
    let jwtSecret = "your-jwt-secret-key"
    let signer = JWTSigner.hs256(key: jwtSecret)
    app.jwt.signers.use(signer)
    // ...
}
```

3. Add a route for user authentication to the `routes(_:)` function:

```swift
app.post("login", use: contactsController.login)
```

4. Add the `login` function to the `ContactController`:

```swift
func login(req: Request) throws -> EventLoopFuture<TokenResponse> {
    let contact = try req.auth.require(Contact.self)
    let jwtToken = try Token.generate(for: contact, on: req)
    let tokenResponse = TokenResponse(token: jwtToken)
    return req.eventLoop.makeSucceededFuture(tokenResponse)
}
```

5. Create a new file named `Token.swift` in the `Sources/App` directory with the following code:

```swift
import JWT
import Vapor

struct Token: Content {
    let token: String

    static func generate(for contact: Contact, on request: Request) throws -> String {
        let signer = try request.jwt.signers.get(JWTSigner.JW