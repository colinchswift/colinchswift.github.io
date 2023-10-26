---
layout: post
title: "Mediator design pattern"
description: " "
date: 2023-10-26
tags: []
comments: true
share: true
---

In software development, it is common for objects to need to communicate with each other. However, directly coupling objects together can lead to complex and tangled code. The Mediator design pattern provides a solution by introducing a central mediator object that facilitates communication between other objects.

## Table of Contents
- [Introduction](#introduction)
- [How it Works](#how-it-works)
- [Benefits](#benefits)
- [Example Implementation](#example-implementation)
- [Conclusion](#conclusion)
- [References](#references)

## Introduction
The Mediator design pattern promotes loose coupling by encapsulating the interactions among objects within a mediator object. Rather than having objects communicate directly with each other, they only communicate through the mediator. This reduces dependencies and makes the codebase more maintainable and extensible.

## How it Works
The Mediator design pattern consists of the following key components:
1. Mediator: Acts as a central point of communication between objects. It defines an interface that objects can use to send and receive messages.
2. Colleague: Represents objects that communicate with each other. They are aware of the mediator and use it to communicate with other colleagues.
3. Concrete Mediator: Implements the mediator interface and manages the communication between the colleagues. It knows how to route messages between objects.

When a colleague needs to communicate with another colleague, it sends a message to the mediator. The mediator then handles the message and decides how to forward it to the appropriate colleague. This encapsulation of communication logic within the mediator simplifies the interaction between objects and reduces their coupling.

## Benefits
The Mediator design pattern offers several benefits, including:

- **Decoupling:** Objects don't depend on each other directly, but on the mediator. This reduces the complexity and dependencies between objects.
- **Flexibility:** It becomes easier to add new objects to the system, as they don't need to be aware of all other objects. They only need to communicate through the mediator.
- **Centralized Control:** With the mediator acting as a central hub, it becomes simpler to manage and control the communication between objects.
- **Improved Reusability:** Mediators can be reused across multiple systems, promoting code reusability.

## Example Implementation
Let's consider an example where a chat application uses the Mediator design pattern. The chat room acts as the mediator and the users are the colleagues. The users can send messages to each other through the chat room, without directly interacting with other users.

```java
// Mediator interface
interface ChatRoom {
    void sendMessage(User sender, String message);
}

// Concrete mediator
class ChatRoomImpl implements ChatRoom {
    @Override
    public void sendMessage(User sender, String message) {
        // Logic to forward the message to the appropriate user(s)
    }
}

// Colleague
class User {
    private String name;
    private ChatRoom chatRoom;

    public User(String name, ChatRoom chatRoom) {
        this.name = name;
        this.chatRoom = chatRoom;
    }

    public void sendMesssage(String message) {
        chatRoom.sendMessage(this, message);
    }
}
```

In this example, the `ChatRoom` interface defines the mediator's API, while the `ChatRoomImpl` class implements the actual logic for routing messages. The `User` class represents the colleagues who communicate through the mediator.

## Conclusion
The Mediator design pattern provides a way to simplify communication between objects and promote loose coupling. By encapsulating communication logic within a central mediator, objects become less dependent on each other and the code becomes more maintainable and extensible.

Using the Mediator pattern requires careful design and consideration of the communication needs between objects. However, when applied appropriately, it can greatly simplify complex systems and improve code quality.

## References
- Design Patterns: Elements of Reusable Object-Oriented Software by Erich Gamma, Richard Helm, Ralph Johnson, John Vlissides