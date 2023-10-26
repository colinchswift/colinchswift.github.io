---
layout: post
title: "Circuit Breaker design pattern"
description: " "
date: 2023-10-26
tags: []
comments: true
share: true
---

## Introduction
In distributed systems, failures are inevitable. When one service makes requests to another service, there is always a possibility of the latter service experiencing issues or becoming unavailable. In order to prevent cascading failures and overload, it is crucial to handle these failures gracefully. This is where the Circuit Breaker design pattern comes into play.

The Circuit Breaker pattern allows services to detect and handle failures in a controlled manner, providing better resilience and fault tolerance. It acts as a safety mechanism between the caller and the remote service, ensuring that excessive and repeated calls to a failing service do not cause further harm.

## How it works
The Circuit Breaker pattern works by monitoring the calls made to a remote service. It keeps track of the number of failures and opens the circuit when a threshold is reached. When the circuit is open, any subsequent calls to the service are handled without making actual service invocations, such as returning cached data or a predetermined fallback response.

The Circuit Breaker transitions between three states: 
- **Closed**: The circuit is closed, allowing normal requests to be made to the service.
- **Open**: The circuit is open, indicating that calls to the service should be avoided.
- **Half-Open**: After a certain period of time, the circuit is automatically moved to the half-open state, allowing a limited number of requests to check if the service has recovered.

## Benefits of Using Circuit Breaker Pattern
- **Fault tolerance**: The Circuit Breaker pattern helps in isolating failing services and prevents them from affecting the entire system.
- **Failfast**: By opening the circuit, calls to a failing service can be avoided, reducing the response times of client applications.
- **Fallback mechanism**: The Circuit Breaker pattern allows you to define fallback responses or use cached data while the service is unavailable.
- **Error resilience**: The pattern protects the system from cascading failures and helps in recovering from failures.

## Implementing the Circuit Breaker Pattern

Below is a simple implementation of the Circuit Breaker pattern in Python:

```python
import time

class CircuitBreaker:
    def __init__(self, failure_threshold=5, recovery_timeout=10):
        self.failure_threshold = failure_threshold
        self.recovery_timeout = recovery_timeout

        self.failure_count = 0
        self.last_failure_time = None
        self.circuit_open = False

    def execute(self, operation):
        if self.circuit_open and self.last_failure_time and \
                time.time() - self.last_failure_time > self.recovery_timeout:
            # Circuit is in Half-Open state
            self.circuit_open = False
            self.failure_count = 0

        if not self.circuit_open:
            try:
                result = operation()
                # Reset failure count on successful invocation
                self.failure_count = 0
                return result
            except Exception as e:
                self.failure_count += 1
                if self.failure_count >= self.failure_threshold:
                    # Open the circuit
                    self.circuit_open = True
                    self.last_failure_time = time.time()
                    # Throw a custom exception or return a fallback response
                    raise CircuitOpenException("Circuit is open")
                else:
                    raise e
        else:
            raise CircuitOpenException("Circuit is open")

class CircuitOpenException(Exception):
    pass
```

## Conclusion
The Circuit Breaker pattern is an essential tool in building resilient and fault-tolerant distributed systems. By monitoring and controlling the interactions with a failing service, it protects the application from potential damage and provides a fallback mechanism to handle failures gracefully.

Using the Circuit Breaker pattern, developers can build robust systems that can withstand failures and prevent them from propagating to other parts of the system.