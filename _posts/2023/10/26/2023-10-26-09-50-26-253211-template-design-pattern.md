---
layout: post
title: "Template design pattern"
description: " "
date: 2023-10-26
tags: [tech, designpattern]
comments: true
share: true
---

The Template Design Pattern is a behavioral design pattern that allows the creation of a template or skeleton of an algorithm in a base class, with specific steps implemented in derived classes. It provides a way to define the basic structure of an algorithm while allowing subclasses to override certain implementation steps as needed.

## Overview

The Template Design Pattern follows the "Don't call us, we'll call you" approach. It abstracts the common parts of an algorithm into a base class, called the **template class**, while leaving the specific implementation of certain steps to the derived classes, called **concrete classes**. This enables the reuse of common code while allowing flexibility in tailoring specific implementations.

## Structure

The Template Design Pattern consists of the following components:

1. **Abstract Class**: This is the base class that defines the skeleton of the algorithm. It contains abstract methods that represent the steps to be executed by the algorithm. These methods act as "hooks" that the concrete classes can override to provide their own custom implementation.

2. **Concrete Classes**: These are the derived classes that inherit from the abstract class. They implement the specific steps of the algorithm by overriding the abstract methods defined in the abstract class.

3. **Client**: This is the code that utilizes the template class by creating an instance of a concrete class. It interacts with the template class through the defined public methods.

## Example

Let's consider a simple example of a beverage preparation system using the Template Design Pattern. We have a base class called `Beverage` that represents the template for preparing a beverage. It contains abstract methods `boilWater()`, `addIngredients()`, and `pourIntoCup()`. The concrete classes, such as `Coffee` and `Tea`, inherit from the `Beverage` class and implement their own versions of the abstract methods.

```java
// Abstract class
abstract class Beverage {
    // Template method
    public void prepareBeverage() {
        boilWater();
        addIngredients();
        pourIntoCup();
    }
    
    abstract void boilWater();
    abstract void addIngredients();
    abstract void pourIntoCup();
}

// Concrete class 1
class Coffee extends Beverage {
    void boilWater() {
        System.out.println("Boiling water for coffee");
    }
    
    void addIngredients() {
        System.out.println("Adding coffee grounds");
    }
    
    void pourIntoCup() {
        System.out.println("Pouring coffee into cup");
    }
}

// Concrete class 2
class Tea extends Beverage {
    void boilWater() {
        System.out.println("Boiling water for tea");
    }
    
    void addIngredients() {
        System.out.println("Adding tea leaves");
    }
    
    void pourIntoCup() {
        System.out.println("Pouring tea into cup");
    }
}

// Client code
public class BeverageClient {
    public static void main(String[] args) {
        Beverage coffee = new Coffee();
        coffee.prepareBeverage();
        
        Beverage tea = new Tea();
        tea.prepareBeverage();
    }
}
```

## Benefits and Use Cases

The Template Design Pattern offers several benefits:

- Reusability: The template class provides a reusable structure for algorithms, allowing the common code to be shared among multiple concrete classes.
- Flexibility: The pattern allows customization of specific steps by the concrete classes, providing flexibility to tailor the implementation based on the requirements.
- Extensibility: New functionalities can be easily added by creating new concrete classes without modifying the existing code.

The Template Design Pattern is particularly useful in scenarios where you have a series of similar tasks that follow a defined pattern but require slight variations in implementation.

## Conclusion

The Template Design Pattern is a powerful tool for creating reusable and flexible algorithms. By separating the common structure from the specific implementation, the pattern promotes code reuse, flexibility, and extensibility. It is a valuable addition to any developer’s design pattern toolkit.

#tech #designpattern