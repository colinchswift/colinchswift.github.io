---
layout: post
title: "Interceptor design pattern"
description: " "
date: 2023-10-26
tags: [references]
comments: true
share: true
---

The **Interceptor design pattern** is a behavioral design pattern that allows modifying the behavior of an object by intercepting method calls before they are executed. It is also known as the **Chain of Responsibility** pattern.

## Introduction

Sometimes, we need to add additional functionality to an existing object without modifying its source code. This is where the interceptor design pattern comes in handy. It provides a way to intercept method calls and perform additional actions either before or after the method execution.

## How it works

The interceptor design pattern involves three main components:

1. **Target Object**: This is the object whose methods we want to intercept and modify.
2. **Interceptor**: Interceptors are responsible for intercepting method calls and performing additional actions.
3. **Interceptor Chain**: Interceptor chains are used to organize multiple interceptors in a specific order.

When a method is called on the target object, it goes through the interceptor chain. Each interceptor in the chain has the opportunity to intercept the method call and modify its behavior. The interceptors can perform tasks such as logging, caching, security checks, or modifying the input/output of the method.

Here's an example code snippet that demonstrates the interceptor design pattern in Java:

```java
public interface Interceptor {
    void intercept(MethodInvocation invocation);
}

public class LoggingInterceptor implements Interceptor {
    @Override
    public void intercept(MethodInvocation invocation) {
        System.out.println("Before method execution");
        invocation.proceed();
        System.out.println("After method execution");
    }
}

public class CachingInterceptor implements Interceptor {
    @Override
    public void intercept(MethodInvocation invocation) {
        if (Cache.contains(invocation.getMethodName())) {
            // Return cached value
        } else {
            invocation.proceed();
            // Cache the result
        }
    }
}

public class TargetObject {
    public void doSomething() {
        System.out.println("Doing something...");
    }
}

public class MethodInvocation {
    private final Object target;
    private final Method method;

    public MethodInvocation(Object target, Method method) {
        this.target = target;
        this.method = method;
    }

    public void proceed() {
        try {
            method.invoke(target);
        } catch (Exception e) {
            // Handle exceptions
        }
    }

    public String getMethodName() {
        return method.getName();
    }
}

public class InterceptorChain {
    private List<Interceptor> interceptors = new ArrayList<>();

    public void addInterceptor(Interceptor interceptor) {
        interceptors.add(interceptor);
    }

    public void invoke(MethodInvocation invocation) {
        if (interceptors.isEmpty()) {
            invocation.proceed();
        } else {
            Interceptor interceptor = interceptors.remove(0);
            interceptor.intercept(invocation);
        }
    }
}

public class Main {
    public static void main(String[] args) {
        TargetObject target = new TargetObject();
        InterceptorChain chain = new InterceptorChain();
        chain.addInterceptor(new LoggingInterceptor());
        chain.addInterceptor(new CachingInterceptor());
        MethodInvocation invocation = new MethodInvocation(target, target.getClass().getMethod("doSomething"));

        chain.invoke(invocation);
    }
}
```

In this example, we have a `TargetObject` with a `doSomething` method. We also have two interceptors: `LoggingInterceptor` and `CachingInterceptor`. The `InterceptorChain` is responsible for organizing the interceptors and invoking them in the specified order.

When the `doSomething` method is called on the `TargetObject`, it goes through the interceptor chain. First, the `LoggingInterceptor` intercepts the method call, logs a message, and then proceeds to the next interceptor. Next, the `CachingInterceptor` checks if the result is already cached, and if not, it proceeds with executing the actual method and caches the result.

## Benefits of using Interceptor design pattern

- Modularity: The interceptor design pattern allows adding and removing interceptors without modifying the target object's source code, promoting modularity and ease of maintenance.
- Reusability: Interceptors can be reused across different target objects, promoting code reuse and reducing duplication.
- Extensibility: It is easy to add new interceptors to change the behavior of target objects without modifying their existing code.
- Separation of concerns: Interceptors assist in separating cross-cutting concerns, such as logging, caching, or security checks, from the core business logic of the target object.

## Conclusion

The interceptor design pattern provides a flexible and modular approach for modifying the behavior of an object by intercepting method calls. It allows us to add additional functionality without modifying the source code of the target object. Interceptors can be used for a variety of purposes, such as logging, caching, or security checks. By using the interceptor design pattern, we can achieve a more flexible and maintainable codebase.

#references
- Design Patterns: Elements of Reusable Object-Oriented Software by Erich Gamma, Richard Helm, Ralph Johnson, and John Vlissides.