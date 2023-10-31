---
layout: post
title: "Implementing image processing and manipulation in Swift Vapor"
description: " "
date: 2023-10-31
tags: [Vapor]
comments: true
share: true
---

In this article, we will explore how to implement image processing and manipulation in Swift using the Vapor framework. We will cover various techniques including resizing, cropping, and applying filters to images. 

# Prerequisites

To follow along with the examples in this tutorial, you should have basic knowledge of Swift programming and be familiar with the Vapor framework. Additionally, ensure that you have Vapor installed and set up on your machine. 

# Setting up the Project

Begin by creating a new Vapor project using the command-line tool:

```shell
vapor new ImageProcessingProject
```

Navigate to the project directory:

```shell
cd ImageProcessingProject
```

# Adding Dependencies

To work with images, we need to add the Vapor Image package to our project. Open the `Package.swift` file and add the following dependency:

```swift
dependencies: [
    // Other dependencies
    .package(url: "https://github.com/vapor-community/image.git", from: "1.3.2"),
],
```

Then, add the Vapor Image package to your target:

```swift
targets: [
    .target(
        name: "App",
        dependencies: [
            // Other dependencies
            .product(name: "Image", package: "image"),
        ]
    ),
]
```

Save the changes and run `vapor update` to fetch the package.

# Image Processing Routes

Next, we will create two routes to handle image processing requests. Open the `routes.swift` file and replace the existing code with the following:

```swift
import Vapor
import Image

func routes(_ app: Application) throws {
    app.post("resize", ":width", ":height") { req -> EventLoopFuture<Response> in
        let width = req.parameters.get("width", as: Int.self)!
        let height = req.parameters.get("height", as: Int.self)!
        
        return try req.content.decode(UploadedImage.self).flatMap { image in
            return req.eventLoop.makeSucceededFuture(try image.resize(width: width, height: height))
        }.encodeResponse(status: .ok, headers: HTTPHeaders())
    }
    
    app.post("crop", ":x", ":y", ":width", ":height") { req -> EventLoopFuture<Response> in
        let x = req.parameters.get("x", as: Int.self)!
        let y = req.parameters.get("y", as: Int.self)!
        let width = req.parameters.get("width", as: Int.self)!
        let height = req.parameters.get("height", as: Int.self)!
        
        return try req.content.decode(UploadedImage.self).flatMap { image in
            return req.eventLoop.makeSucceededFuture(try image.crop(x: x, y: y, width: width, height: height))
        }.encodeResponse(status: .ok, headers: HTTPHeaders())
    }
}
```

We have created two routes, `/resize` and `/crop`, which allow us to resize and crop images, respectively. The `UploadedImage` struct is a Vapor content type for handling image uploads.

# Image Processing Functions

Now let's define the `UploadedImage` struct and add extension methods for image manipulation. Create a new file called `UploadedImage.swift` in the `App/Models` directory and add the following code:

```swift
import Foundation
import Vapor
import Image

struct UploadedImage: Content {
    let data: Data
}

extension UploadedImage {
    func resize(width: Int, height: Int) throws -> Response {
        let image = try Image(data: data)
        let resizedImage = try image.resizedTo(width: width, height: height)
        let resizedImageData = try resizedImage.jpegData(compressionQuality: 1)
        
        return Response(status: .ok, body: .init(data: resizedImageData))
    }
    
    func crop(x: Int, y: Int, width: Int, height: Int) throws -> Response {
        let image = try Image(data: data)
        let croppedImage = try image.croppedTo(x: x, y: y, width: width, height: height)
        let croppedImageData = try croppedImage.jpegData(compressionQuality: 1)
        
        return Response(status: .ok, body: .init(data: croppedImageData))
    }
}
```

In the `resize` and `crop` methods, we create an instance of `Image` from the uploaded data, perform the respective operations, convert the result back to `Data`, and create a `Response` instance with the modified image.

# Running the Server

Finally, we need to run the Vapor server to test the image processing routes. Open a terminal window, navigate to the project directory, and run the following command:

```shell
vapor run
```

Now you can make HTTP POST requests to `http://localhost:8080/resize/:width/:height` or `http://localhost:8080/crop/:x/:y/:width/:height` to resize or crop images, respectively.

# Conclusion

In this tutorial, we learned how to implement image processing and manipulation in Swift using the Vapor framework. We covered resizing and cropping images using the Vapor Image package. Feel free to explore additional features of the package and experiment with different image processing techniques for your projects.

**References:**

- Vapor: [https://vapor.codes](https://vapor.codes)
- Vapor Image Package: [https://github.com/vapor-community/image](https://github.com/vapor-community/image)

*Tags: #Swift #Vapor*