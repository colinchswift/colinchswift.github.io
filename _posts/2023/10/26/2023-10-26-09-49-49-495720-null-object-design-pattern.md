---
layout: post
title: "Null Object design pattern"
description: " "
date: 2023-10-26
tags: [designpatterns, nullobject]
comments: true
share: true
---

The Null Object design pattern is a behavioral pattern that is used to provide a default or null implementation of an interface without returning null. It helps to avoid null pointer exceptions by providing a safe and valid alternative.

## How it works

The Null Object pattern defines a common interface that all classes, including the null object, implement. The null object class provides the default behavior for the methods defined in the interface, but these methods do not necessarily perform any meaningful operations.

When an object is expected, but the null object is provided, it behaves like a regular object but without any side effects. This ensures that the code does not need to perform null checks before invoking methods on the object.

## Advantages

- Null pointer exceptions are avoided because a null object is returned instead of null.
- Simplifies coding by eliminating the need for null checks.
- Provides a default behavior when an actual object is not available or relevant.
- Easy to extend and add new behaviors based on the common interface.

## Example Application

Let's consider a banking application with a `Customer` class that represents a bank account holder. The `Customer` class has a method `getAddress()` that returns the address of the customer as a string.

To implement the Null Object pattern, we create a `NullCustomer` class that implements the `Customer` interface and provides a default implementation for the `getAddress()` method. This implementation returns an empty string.

```java
public interface Customer {
    String getAddress();
}

public class NullCustomer implements Customer {
    @Override
    public String getAddress() {
        return "";
    }
}

public class RealCustomer implements Customer {
    private String address;

    public RealCustomer(String address) {
        this.address = address;
    }

    @Override
    public String getAddress() {
        return address;
    }
}
```

In our application code, instead of returning null when a customer does not have an address, we return an instance of the `NullCustomer` class. This ensures that no null pointer exceptions occur when calling `getAddress()`.

```java
public class Application {
    public static void main(String[] args) {
        Customer customer1 = new RealCustomer("123 Street, City");
        Customer customer2 = new NullCustomer();

        System.out.println(customer1.getAddress()); // Output: 123 Street, City
        System.out.println(customer2.getAddress()); // Output: ""
    }
}
```

## Conclusion

The Null Object design pattern provides a safe and valid alternative to returning null objects. By implementing a null object that adheres to the same interface as other objects, we can avoid null pointer exceptions and simplify our code. This pattern is especially useful in scenarios where null values are common or where default behavior is required. 

Try using the Null Object pattern in your code whenever you need to handle null values in a more efficient and elegant way. 

**#designpatterns #nullobject**