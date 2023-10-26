---
layout: post
title: "Chain of command design pattern"
description: " "
date: 2023-10-26
tags: []
comments: true
share: true
---

The Chain of Responsibility design pattern is a behavioral pattern that allows an object to pass a request along a chain of potential handlers until the request is handled or reaches the end of the chain. It decouples the sender of the request from the receiver, providing multiple alternative objects that can handle the request.

## How it works

1. Create an abstract base class (or an interface) that defines the common methods or properties for all the handlers.
2. Each handler should have a reference to the next handler in the chain.
3. The requesting object sends a request to the first handler in the chain.
4. If the current handler can handle the request, it does so and returns. Otherwise, it passes the request to the next handler in the chain.
5. Repeat until the request is handled or there are no more handlers in the chain.

## Example

Let's say we have a system that processes purchase requests. Each purchase request has a different amount, and we want to handle them based on the request amount. We can implement the Chain of Responsibility pattern to create a chain of handlers that deal with different request amounts.

```java
public abstract class PurchaseHandler {
    private PurchaseHandler nextHandler;

    public void setNextHandler(PurchaseHandler handler) {
        this.nextHandler = handler;
    }

    public abstract void handleRequest(PurchaseRequest request);

    protected void passToNextHandler(PurchaseRequest request) {
        if (nextHandler != null) {
            nextHandler.handleRequest(request);
        }
    }
}

public class ManagerHandler extends PurchaseHandler {
    public void handleRequest(PurchaseRequest request) {
        if (request.getAmount() <= 1000) {
            System.out.println("Manager can handle the request.");
        } else {
            passToNextHandler(request);
        }
    }
}

public class DirectorHandler extends PurchaseHandler {
    public void handleRequest(PurchaseRequest request) {
        if (request.getAmount() <= 5000) {
            System.out.println("Director can handle the request.");
        } else {
            passToNextHandler(request);
        }
    }
}

public class CEOHandler extends PurchaseHandler {
    public void handleRequest(PurchaseRequest request) {
        System.out.println("CEO can handle the request.");
    }
}

public class PurchaseRequest {
    private double amount;

    public PurchaseRequest(double amount) {
        this.amount = amount;
    }

    public double getAmount() {
        return amount;
    }
}

public class Client {
    public static void main(String[] args) {
        PurchaseHandler manager = new ManagerHandler();
        PurchaseHandler director = new DirectorHandler();
        PurchaseHandler ceo = new CEOHandler();

        manager.setNextHandler(director);
        director.setNextHandler(ceo);

        PurchaseRequest request1 = new PurchaseRequest(500);
        PurchaseRequest request2 = new PurchaseRequest(2500);
        PurchaseRequest request3 = new PurchaseRequest(10000);

        manager.handleRequest(request1);
        manager.handleRequest(request2);
        manager.handleRequest(request3);
    }
}
```

In the above example, we have three types of handlers: Manager, Director, and CEO. Each handler checks if it can handle the purchase request based on the amount. If it can handle the request, it does so; otherwise, it passes the request to the next handler in the chain.

Running the client code will output:
```
Manager can handle the request.
Director can handle the request.
CEO can handle the request.
```

## Benefits

- Allows for the dynamic addition or removal of handlers in the chain, without affecting the client code.
- Reduces the coupling between the sender and receiver of the request.
- Provides flexibility in handling requests, as different combinations of handlers can be created.

## Conclusion

The Chain of Responsibility design pattern is a powerful way of implementing a request handling mechanism. It allows for loose coupling between the sender and receiver of a request, providing flexibility and extensibility in the system. This pattern is particularly useful when there are multiple handlers or when the handling logic needs to be changed dynamically at runtime.