---
layout: post
title: "Flyweight design pattern"
description: " "
date: 2023-10-26
tags: [references]
comments: true
share: true
---

The Flyweight design pattern is a structural pattern that aims to minimize memory usage by sharing as much data as possible between multiple objects. It is useful when we need to create a large number of similar objects that have some common state. By sharing this common state, we can reduce the memory footprint and improve performance.

## How it works

The key idea behind the Flyweight pattern is to separate the intrinsic state from the extrinsic state of an object. The intrinsic state is the part that can be shared between multiple objects, while the extrinsic state is the part that varies and cannot be shared.

In practice, we create a Flyweight Factory that contains a pool of shared Flyweight objects. When a client wants to create a new object, it first checks if a Flyweight with the same intrinsic state already exists in the pool. If it does, the client reuses the existing Flyweight instead of creating a new one. If the Flyweight does not exist, the Factory creates a new one and adds it to the pool for future use.

The Flyweight objects themselves are usually immutable, as their state should not change once created. They only store the intrinsic state, while the extrinsic state is passed to them whenever they are used.

## Example

Let's consider an example where we need to render a large number of trees on a computer screen in a game. Each tree has some shared attributes, such as its type (e.g. oak, pine) and texture. These attributes can be considered as the intrinsic state of the tree.

We can create a `TreeFlyweight` class that stores the intrinsic state of a tree, like its type and texture. The `TreeFlyweightFactory` class will manage a pool of shared tree flyweight objects. When rendering a tree on the screen, we create a `Tree` object and pass the extrinsic state (e.g. position) to the `TreeFlyweight` object. The `TreeFlyweightFactory` checks if a Flyweight with the same intrinsic state already exists in the pool, and if it does, it returns the existing Flyweight. Otherwise, it creates a new one and adds it to the pool.

```python
class TreeFlyweight:
    def __init__(self, type, texture):
        self.type = type
        self.texture = texture

class TreeFlyweightFactory:
    def __init__(self):
        self.flyweights = {}

    def get_tree_flyweight(self, type, texture):
        if (type, texture) not in self.flyweights:
            self.flyweights[(type, texture)] = TreeFlyweight(type, texture)
        return self.flyweights[(type, texture)]

class Tree:
    def __init__(self, x, y, type, texture):
        self.x = x
        self.y = y
        self.flyweight = TreeFlyweightFactory().get_tree_flyweight(type, texture)

    def render(self):
        # Rendering code here

# Client code
tree1 = Tree(0, 0, "oak", "leaf.jpg")
tree2 = Tree(10, 20, "pine", "snow.jpg")
tree3 = Tree(5, 5, "oak", "leaf.jpg")

print(tree1.flyweight is tree3.flyweight)  # True, they are the same flyweight
print(tree2.flyweight is tree3.flyweight)  # False, they are different flyweights
```

In the example above, the `TreeFlyweight` class represents the intrinsic state of a tree, while the `Tree` class represents the extrinsic state of a specific tree instance. The `TreeFlyweightFactory` ensures that the same flyweight is shared between trees with the same intrinsic state.

## Conclusion

The Flyweight design pattern is a powerful technique for reducing memory usage and improving performance when working with a large number of similar objects. By carefully separating the intrinsic and extrinsic state and sharing the common state among objects, we can optimize our application's memory footprint.

Using the Flyweight pattern in the right scenarios can greatly enhance the performance and efficiency of your application. It is particularly useful in situations where a large number of objects need to be created and the memory consumption needs to be minimized.

#references: 
- [Flyweight Design Pattern in the Real World](https://refactoring.guru/design-patterns/flyweight)
- [Flyweight Design Pattern in Python](https://sourcemaking.com/design_patterns/flyweight/python/1) 
#tag: design-pattern, flyweight