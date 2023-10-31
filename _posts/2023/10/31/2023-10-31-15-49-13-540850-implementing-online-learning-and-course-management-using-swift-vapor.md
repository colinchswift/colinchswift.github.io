---
layout: post
title: "Implementing online learning and course management using Swift Vapor"
description: " "
date: 2023-10-31
tags: [vapor]
comments: true
share: true
---

## Introduction

In recent years, online learning has grown in popularity, providing a convenient way for people to enhance their skills and gain knowledge from the comfort of their homes. In this article, we will explore how to build an online learning platform and course management system using Swift Vapor, a web framework for creating server-side Swift applications.

## Setting up the Project

Before we begin, make sure you have Swift and Vapor installed on your system. If not, you can refer to the official Vapor documentation for installation instructions.

To create a new Vapor project, open your terminal and run the following commands:

```swift
vapor new OnlineLearningSystem
cd OnlineLearningSystem
```
 
## Designing Models

We need to define our data models for courses, lessons, and users. Open the `Models` folder in your project and create three new Swift files: `Course.swift`, `Lesson.swift`, and `User.swift`.

### Course.swift

```swift
import Vapor
import Fluent

final class Course: Model, Content {
    static let schema = "courses"

    @ID(key: .id)
    var id: UUID?

    @Field(key: "name")
    var name: String

    @Children(for: \.$course)
    var lessons: [Lesson]

    init() { }

    init(id: UUID? = nil, name: String) {
        self.id = id
        self.name = name
    }
}
```

### Lesson.swift

```swift
import Vapor
import Fluent

final class Lesson: Model, Content {
    static let schema = "lessons"

    @ID(key: .id)
    var id: UUID?

    @Field(key: "title")
    var title: String

    @Parent(key: "course_id")
    var course: Course

    init() { }

    init(id: UUID? = nil, title: String, courseID: UUID) {
        self.id = id
        self.title = title
        self.$course.id = courseID
    }
}
```

### User.swift

```swift
import Vapor
import Fluent

final class User: Model, Content {
    static let schema = "users"

    @ID(key: .id)
    var id: UUID?

    @Field(key: "name")
    var name: String

    @Children(for: \.$user)
    var courses: [Course]

    init() { }

    init(id: UUID? = nil, name: String) {
        self.id = id
        self.name = name
    }
}
```

## Creating Routes and Controllers

Now, let's create the routes and controllers to handle the CRUD (Create, Read, Update, Delete) operations for our models.

### `routes.swift`

```swift
import Vapor

func routes(_ app: Application) throws {
    let courseController = CourseController()
    let lessonController = LessonController()
    let userController = UserController()

    app.get("courses", use: courseController.index)
    app.post("courses", use: courseController.create)
    app.group("courses", ":courseID") { courses in
        courses.get(use: courseController.show)
        courses.delete(use: courseController.delete)
        courses.group("lessons") { lessons in
            lessons.get(use: lessonController.index)
            lessons.post(use: lessonController.create)
            lessons.group(":lessonID") { lesson in
                lesson.get(use: lessonController.show)
                lesson.delete(use: lessonController.delete)
            }
        }
    }
    app.get("users", use: userController.index)
    app.post("users", use: userController.create)
    app.group("users", ":userID") { users in
        users.get(use: userController.show)
        users.delete(use: userController.delete)
        users.get("courses", use: userController.courses)
    }
}
```

### `CourseController.swift`

```swift
import Vapor

struct CourseController {
    func index(req: Request) throws -> EventLoopFuture<[Course]> {
        return Course.query(on: req.db).all()
    }

    func create(req: Request) throws -> EventLoopFuture<Course> {
        let course = try req.content.decode(Course.self)
        return course.create(on: req.db).map { course }
    }

    func show(req: Request) throws -> EventLoopFuture<Course> {
        return Course.find(req.parameters.get("courseID"), on: req.db)
            .unwrap(or: Abort(.notFound))
    }

    func delete(req: Request) throws -> EventLoopFuture<HTTPStatus> {
        return Course.find(req.parameters.get("courseID"), on: req.db)
            .unwrap(or: Abort(.notFound))
            .flatMap { $0.delete(on: req.db) }
            .transform(to: .ok)
    }
}
```

### `LessonController.swift`

```swift
import Vapor

struct LessonController {
    func index(req: Request) throws -> EventLoopFuture<[Lesson]> {
        let courseID = req.parameters.get("courseID", as: UUID.self)
        return Lesson.query(on: req.db)
            .filter(\.$course.$id == courseID)
            .all()
    }

    func create(req: Request) throws -> EventLoopFuture<Lesson> {
        let courseID = req.parameters.get("courseID", as: UUID.self)
        let lesson = try req.content.decode(Lesson.self)
        lesson.course.$id.id = courseID
        return lesson.create(on: req.db).map { lesson }
    }

    func show(req: Request) throws -> EventLoopFuture<Lesson> {
        return Lesson.find(req.parameters.get("lessonID"), on: req.db)
            .unwrap(or: Abort(.notFound))
    }

    func delete(req: Request) throws -> EventLoopFuture<HTTPStatus> {
        return Lesson.find(req.parameters.get("lessonID"), on: req.db)
            .unwrap(or: Abort(.notFound))
            .flatMap { $0.delete(on: req.db) }
            .transform(to: .ok)
    }
}
```

### `UserController.swift`

```swift
import Vapor

struct UserController {
    func index(req: Request) throws -> EventLoopFuture<[User]> {
        return User.query(on: req.db).all()
    }

    func create(req: Request) throws -> EventLoopFuture<User> {
        let user = try req.content.decode(User.self)
        return user.create(on: req.db).map { user }
    }

    func show(req: Request) throws -> EventLoopFuture<User> {
        return User.find(req.parameters.get("userID"), on: req.db)
            .unwrap(or: Abort(.notFound))
    }

    func delete(req: Request) throws -> EventLoopFuture<HTTPStatus> {
        return User.find(req.parameters.get("userID"), on: req.db)
            .unwrap(or: Abort(.notFound))
            .flatMap { $0.delete(on: req.db) }
            .transform(to: .ok)
    }

    func courses(req: Request) throws -> EventLoopFuture<[Course]> {
        let userID = req.parameters.get("userID", as: UUID.self)
        return User.find(userID, on: req.db)
            .unwrap(or: Abort(.notFound))
            .flatMap { $0.$courses.get(on: req.db) }
    }
}
```

## Conclusion

In this article, we have explored how to implement an online learning platform and course management system using Swift Vapor. We have designed the necessary models and created routes and controllers to handle the CRUD operations. With this foundation, you can further enhance the system by adding authentication, user roles, and additional features to create a fully functional online learning platform. Vapor's flexibility and scalability make it a great choice for building robust web applications in Swift.

**Reference**:

- [Vapor Documentation](https://docs.vapor.codes/)
- [Swift Vapor on GitHub](https://github.com/vapor/vapor)

#swift #vapor