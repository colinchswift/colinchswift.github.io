---
layout: post
title: "Observer design pattern"
description: " "
date: 2023-10-26
tags: [designpattern, observerpattern]
comments: true
share: true
---

In software development, the Observer design pattern is widely used to establish a one-to-many relationship between objects. It allows one object, known as the subject, to notify multiple other objects, known as observers, when its state changes.

## Understanding the Observer Pattern

The Observer design pattern is based on the principle of loose coupling. It ensures that the subject and observers are loosely coupled, allowing them to vary independently. This promotes code reusability and easy maintainability.

### Components of the Observer Pattern

1. **Subject**: This is the object that holds the state and sends notifications to observers when its state changes. It maintains a list of observers and provides methods for attaching, detaching, and notifying them.
   
2. **Observers**: These are the objects that are interested in the state changes of the subject. They register themselves with the subject and receive notifications when the subject's state changes.

### Implementation of the Observer Pattern

To implement the Observer pattern, follow these steps:

1. Create an interface or base class for the observers, defining the methods to be called by the subject when its state changes.

```java
public interface Observer {
    void update();
}
```

2. Implement the observer interface to create concrete observer classes.

```java
public class ConcreteObserver implements Observer {
    @Override
    public void update() {
        // Perform actions upon receiving notification
    }
}
```

3. Create the subject class, which holds the state and maintains a list of observers.

```java
public class Subject {
    private List<Observer> observers = new ArrayList<>();
    private int state;

    public void attach(Observer observer) {
        observers.add(observer);
    }

    public void detach(Observer observer) {
        observers.remove(observer);
    }

    public void setState(int state) {
        this.state = state;
        notifyObservers();
    }

    private void notifyObservers() {
        for (Observer observer : observers) {
            observer.update();
        }
    }
}
```

### Usage of the Observer Pattern

To utilize the Observer pattern, follow these steps:

1. Create instances of the subject and observer classes.

```java
Subject subject = new Subject();
Observer observer1 = new ConcreteObserver();
Observer observer2 = new ConcreteObserver();
```

2. Attach observers to the subject.

```java
subject.attach(observer1);
subject.attach(observer2);
```

3. Modify the state of the subject, which triggers the notification to the observers.

```java
subject.setState(10);
```

4. The update method of each observer will be called upon receiving the notification.

```java
public class ConcreteObserver implements Observer {
    @Override
    public void update() {
        System.out.println("State has changed!");
    }
}
```

### Benefits and Use Cases

- **Easy extensibility**: The Observer pattern allows for easy addition of new observers without modifying the subject.
- **Loose coupling**: The subject and observers are decoupled, allowing for flexible and modular designs.
- **Event-driven systems**: The Observer pattern is commonly used in event-driven systems, where multiple entities need to react to changes in a shared object's state.

### Conclusion

The Observer design pattern is a powerful tool to establish communication between objects in a loosely coupled manner. By following the principles of the Observer pattern, you can create flexible and maintainable systems that react effectively to changes in state. #designpattern #observerpattern