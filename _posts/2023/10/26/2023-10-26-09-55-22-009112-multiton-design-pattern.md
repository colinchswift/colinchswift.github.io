---
layout: post
title: "Multiton design pattern"
description: " "
date: 2023-10-26
tags: [References]
comments: true
share: true
---

The multiton design pattern is a variation of the singleton design pattern that allows for the creation of a limited number of instances instead of just one. It ensures that there are multiple instances of a class available for use, each with a unique identifier.

## How it works

In the multiton design pattern, a class keeps track of multiple instances in a dictionary-like data structure, with each instance associated with a specific key or identifier. When a new instance is requested, the multiton checks if an instance with the provided key already exists. If it does, the existing instance is returned; if not, a new instance is created, added to the multiton, and returned.

## Advantages of the Multiton Design Pattern

### 1. Multiple instances with different characteristics

The multiton design pattern allows for the creation of multiple instances, each with its own unique characteristics. This is useful when you need different instances of a class that have slightly different configurations or behaviors.

### 2. Controlled resource allocation

By limiting the number of instances that can be created, the multiton design pattern helps in controlling resource usage. It ensures that only a certain number of instances are created, preventing resource overutilization.

### 3. Simplified object retrieval

With the multiton design pattern, you can easily retrieve instances based on their unique identifiers. This makes it easy to access specific instances without needing to track their creation or manage them manually.

## Example Implementation

Let's consider an example where we have a `Logger` class that we want to use in different parts of our application. We can implement the multiton design pattern to ensure that we have different instances of the `Logger` class for different sections of our application.

```python
class Logger:
    _instances = {}

    def __init__(self, name):
        self.name = name

    @classmethod
    def get_instance(cls, name):
        if name not in cls._instances:
            cls._instances[name] = Logger(name)
        return cls._instances[name]
```

In the example above, the `Logger` class keeps track of instances in a dictionary named `_instances`. The `get_instance` method is used to retrieve or create a new instance based on the provided `name`. If an instance with the provided `name` already exists, it is returned; otherwise, a new instance is created and added to the dictionary.

## Conclusion

The multiton design pattern is a useful variation of the singleton design pattern when you need to have multiple instances of a class with unique identifiers. It provides control over resource allocation and simplifies object retrieval. By implementing this pattern, you can easily manage and reuse instances of a class throughout your application.

#References:
- [Design Patterns: Elements of Reusable Object-Oriented Software](https://www.amazon.com/Design-Patterns-Elements-Reusable-Object-Oriented/dp/0201633612)
- [Multiton Design Pattern - GeeksforGeeks](https://www.geeksforgeeks.org/multiton-design-pattern/)