---
layout: post
title: "Implementing automated email sending with Swift Vapor"
description: " "
date: 2023-10-31
tags: []
comments: true
share: true
---

Sending automated emails is a crucial aspect of many applications. In this tutorial, we will guide you through the process of implementing automated email sending using Swift Vapor, a popular server-side Swift web framework.

Before diving into the implementation, make sure you have Swift and Vapor installed on your system. You can follow the official documentation for installation instructions.

## Setting up the project

To get started, let's create a new Swift Vapor project. Open a terminal and run the following commands:

```shell
vapor new EmailSender
cd EmailSender
```

Next, open the Xcode project by running:

```shell
vapor xcode
```

This will generate an Xcode project for your Vapor application.

## Configuring the email provider

To send emails, we need to configure a mail provider that Vapor can use. In this example, we will be using the **SMTP** provider, but Vapor also supports other providers like SendGrid, Mailgun, and more.

First, open the `configure.swift` file located in the `Sources/App` directory. Uncomment the `vapor/smtp` import at the top of the file and add the following code within the `configure` function:

```swift
import SMTP

// Configure SMTP mail provider
app.smtp.configuration = try .init(
    hostname: "smtp.mailtrap.io",
    port: 465,
    email: "your-email@example.com",
    password: "your-password"
)
app.smtp.defaultMailer = .default
```

Replace the `hostname`, `port`, `email` and `password` with the appropriate values for your SMTP provider.

## Sending an email

Now that we have configured the mail provider, let's implement a route for sending an email.

Open the `routes.swift` file located in the `Sources/App` directory and add the following code within the `routes` function:

```swift
import SMTP

app.post("sendEmail") { req -> EventLoopFuture<String> in
    let email = Email(
        from: EmailAddress(address: "from@example.com", displayName: "Sender"),
        to: [EmailAddress(address: "to@example.com", displayName: "Recipient")],
        subject: "Hello from Vapor",
        text: "This is an automated email sent using Vapor"
    )

    return req.smtp.send(email)
        .transform(to: "Email sent successfully")
}
```

In this example, we create an `Email` object specifying the sender, recipient, subject and body of the email. Then, we use the `req.smtp.send` method to send the email.

## Testing the email sending route

To test the email sending route, start the Vapor application by running:

```shell
vapor run
```

Now, open a web browser or use a tool like cURL to make a `POST` request to `http://localhost:8080/sendEmail`.

If everything is configured correctly, you should see a success message indicating that the email was sent successfully. Check your email inbox to verify that the email was received.

## Conclusion

In this tutorial, we have learned how to implement automated email sending using Swift Vapor. We configured the SMTP provider and created a route for sending emails. Feel free to explore the official Vapor documentation for more advanced email-related features, such as HTML emails, attachments, and email templates.

# References

- [Swift Vapor Documentation](https://docs.vapor.codes)
- [Vapor SMTP package](https://github.com/vapor-community/smtp)