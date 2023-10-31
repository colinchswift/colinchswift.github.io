---
layout: post
title: "Implementing scheduling and appointment booking in Swift Vapor"
description: " "
date: 2023-10-31
tags: [Vapor]
comments: true
share: true
---

In this tutorial, we will learn how to implement scheduling and appointment booking functionality in a Swift Vapor web application. This feature is commonly used in applications like medical appointment systems, salon booking systems, or any other system where users need to schedule appointments.

## Table of Contents

- [Setting up the project](#setting-up-the-project)
- [Modeling the data](#modeling-the-data)
- [Creating the routes and controllers](#creating-the-routes-and-controllers)
- [Implementing the scheduling logic](#implementing-the-scheduling-logic)
- [Testing the functionality](#testing-the-functionality)
- [Conclusion](#conclusion)

## Setting up the project

To begin, make sure you have Swift and Vapor installed on your machine. If not, you can refer to the Vapor documentation for installation instructions.

Once you have Vapor installed, create a new project using the following command:

```
vapor new MyApp
```

Replace `MyApp` with the name of your project.

## Modeling the data

Next, we need to model the data for our scheduling and appointment booking system. In this example, let's consider we have two models: `User` and `Appointment`.

Create a new Swift file `User.swift` in the `App/Models` directory and define the `User` model as follows:

```swift
final class User: Model, Content {
    static let schema = "users"

    @ID(key: .id)
    var id: UUID?

    @Field(key: "name")
    var name: String

    // other user properties

    init() { }

    init(id: UUID? = nil, name: String) {
        self.id = id
        self.name = name
    }
}
```

Similarly, create a new Swift file `Appointment.swift` in the `App/Models` directory and define the `Appointment` model as follows:

```swift
final class Appointment: Model, Content {
    static let schema = "appointments"

    @ID(key: .id)
    var id: UUID?

    @Field(key: "datetime")
    var datetime: Date

    @Parent(key: "user_id")
    var user: User

    // other appointment properties

    init() { }

    init(id: UUID? = nil, datetime: Date, userID: User.IDValue) {
        self.id = id
        self.datetime = datetime
        self.$user.id = userID
    }
}
```

Make sure to run the migrations after defining the models by running the following command:

```
vapor run migrate
```

## Creating the routes and controllers

Now, let's create the necessary routes and controllers to handle scheduling and appointment booking.

Create a new Swift file `AppointmentController.swift` in the `App/Controllers` directory and define the `AppointmentController` as follows:

```swift
struct AppointmentController: RouteCollection {
    func boot(routes: RoutesBuilder) throws {
        let appointments = routes.grouped("appointments")
        appointments.get(use: index)
        appointments.post(use: create)
    }

    func index(req: Request) throws -> EventLoopFuture<[Appointment]> {
        return Appointment.query(on: req.db).all()
    }

    func create(req: Request) throws -> EventLoopFuture<Appointment> {
        let appointment = try req.content.decode(Appointment.self)
        return appointment.save(on: req.db).map { appointment }
    }
}
```

Next, open the `routes.swift` file in the `App` directory and add the following code to register the `AppointmentController` routes:

```swift
app.routes.group("api") { api in
    api.group("v1") { v1 in
        v1.group("users") { users in
            // your user routes
        }
        v1.group("appointments", use: AppointmentController())
    }
}
```

## Implementing the scheduling logic

Now, let's implement the logic to prevent conflicting appointments when scheduling.

Add the following helper method to the `AppointmentController`:

```swift
func canScheduleAppointment(_ appointment: Appointment, req: Request) -> EventLoopFuture<Bool> {
    return Appointment.query(on: req.db)
        .filter(\.$datetime == appointment.datetime)
        .all()
        .map { existingAppointments in
            let conflictingAppointments = existingAppointments.filter { $0.id != appointment.id }
            return conflictingAppointments.isEmpty
        }
}
```

Modify the `create` method in `AppointmentController` as follows:

```swift
func create(req: Request) throws -> EventLoopFuture<Appointment> {
    let appointment = try req.content.decode(Appointment.self)
    
    return canScheduleAppointment(appointment, req: req).flatMap { canSchedule in
        if canSchedule {
            return appointment.save(on: req.db).map { appointment }
        } else {
            throw Abort(.badRequest, reason: "Appointment conflict")
        }
    }
}
```

## Testing the functionality

To test the functionality, you can use tools like Postman or cURL to send HTTP requests to the created routes.

- To get all appointments, send a `GET` request to `/api/v1/appointments`.
- To create a new appointment, send a `POST` request to `/api/v1/appointments` with the appointment details in the request body.

## Conclusion

In this tutorial, we have learned how to implement scheduling and appointment booking functionality in a Swift Vapor web application. We modeled the data using the `User` and `Appointment` models, created the necessary routes and controllers, and implemented the logic to prevent conflicting appointments. Feel free to extend the functionality to fit your specific requirements. Happy coding!

#hashtags: #Swift #Vapor