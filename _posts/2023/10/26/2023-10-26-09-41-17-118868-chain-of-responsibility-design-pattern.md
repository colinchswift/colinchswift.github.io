---
layout: post
title: "Chain of responsibility design pattern"
description: " "
date: 2023-10-26
tags: [References, designpattern]
comments: true
share: true
---

The Chain of Responsibility design pattern is a behavioral pattern that allows an object to pass a request along a chain of potential handlers until the request is handled or reaches the end of the chain. This pattern decouples the sender and receiver of a request, giving multiple objects a chance to handle the request without explicitly knowing which object will handle it.

## When to use the Chain of Responsibility design pattern?

The Chain of Responsibility pattern is useful in scenarios where a request needs to be processed by multiple objects, each having the potential to handle the request differently. It allows for flexibility in dynamically assigning responsibilities, adding or removing handlers without modifying the client code. Some common use cases include:

- User input validation
- Request processing in web applications
- Event propagation in GUI frameworks

## How does it work?

The Chain of Responsibility design pattern consists of a chain of handler objects, where each handler either handles the request or passes it to the next handler in the chain. The requesting object is unaware of the specific handler and simply sends the request to the first handler in the chain. The chain is typically established during runtime.

Here's an example implementation of the Chain of Responsibility pattern in Python:

```python
class Handler:
    def __init__(self, successor=None):
        self._successor = successor

    def handle_request(self, request):
        if self._successor is not None:
            self._successor.handle_request(request)
        else:
            print("Request cannot be handled.")

class ConcreteHandlerA(Handler):
    def handle_request(self, request):
        if request == "A":
            print("Handling request A.")
        else:
            super().handle_request(request)

class ConcreteHandlerB(Handler):
    def handle_request(self, request):
        if request == "B":
            print("Handling request B.")
        else:
            super().handle_request(request)

# Client code
handler_a = ConcreteHandlerA()
handler_b = ConcreteHandlerB(handler_a)

handler_b.handle_request("A")  # Output: Handling request A.
handler_b.handle_request("B")  # Output: Handling request B.
handler_b.handle_request("C")  # Output: Request cannot be handled.
```

In this example, we have two concrete handlers, `ConcreteHandlerA` and `ConcreteHandlerB`, each capable of handling a specific type of request. The client code creates an instance of `ConcreteHandlerB` passing `ConcreteHandlerA` as its successor. When a request is made to `handler_b`, it will first try to handle the request itself and if it can't, it will pass the request to `handler_a`.

## Benefits of using the Chain of Responsibility pattern

The Chain of Responsibility design pattern provides several benefits:

- Decouples sender and receiver: The sender does not need to know the specific handler object, promoting loose coupling.
- Flexible and dynamic: Handlers can be added, removed, or rearranged at runtime without modifying the client code.
- Single responsibility principle: Each handler has a single responsibility, making the code easier to understand, modify, and maintain.

## Drawbacks of using the Chain of Responsibility pattern

While the Chain of Responsibility pattern offers benefits, it also has some drawbacks:

- Requests can go unhandled: If a request reaches the end of the chain without a handler, it may remain unhandled, which can lead to unexpected behavior.
- Performance impact: The chain may introduce additional overhead due to the dynamic nature of passing requests along the chain.


#References
- Design Patterns: Elements of Reusable Object-Oriented Software by Erich Gamma, Richard Helm, Ralph Johnson, and John Vlissides
- [Chain of Responsibility Design Pattern - tutorialspoint.com](https://www.tutorialspoint.com/design_pattern/chain_of_responsibility_pattern.htm)

\#designpattern \#chainofresponsibility