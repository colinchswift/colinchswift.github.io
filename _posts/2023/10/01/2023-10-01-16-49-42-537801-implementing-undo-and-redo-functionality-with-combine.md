---
layout: post
title: "Implementing undo and redo functionality with Combine"
description: " "
date: 2023-10-01
tags: [Combine, UndoRedo]
comments: true
share: true
---

Undo and redo functionality is a common requirement in many applications, especially those that deal with user interactions and data modifications. In this blog post, we will explore how to implement undo and redo functionality using Combine, Apple's framework for reactive programming.

## Prerequisites

Before diving into the implementation, make sure you have a basic understanding of Combine and its concepts. If you are new to Combine, I recommend checking out Apple's documentation and tutorials to get started.

## Step 1: Setting up the Undo/Redo Stack

The first step is to set up a stack to keep track of the state changes that need to be undone or redone. Let's define a `HistoryStack` class with two properties: `undoStack` and `redoStack`, both of type `Array`.

```swift
class HistoryStack {
    private var undoStack: [State] = []
    private var redoStack: [State] = []

    // Rest of the implementation...
}
```

In this example, we assume that the state changes are represented by the `State` struct. You can replace this with your own data structure depending on your application's needs.

## Step 2: Adding State Changes

Next, we'll add methods to our `HistoryStack` class to push state changes onto the undo stack and clear the redo stack.

```swift
extension HistoryStack {
    func pushStateChange(_ state: State) {
        undoStack.append(state)
        redoStack.removeAll()
    }

    // Rest of the implementation...
}
```

The `pushStateChange` method adds the new state to the undo stack and clears the redo stack since any redo actions should be discarded when a new state change is made.

## Step 3: Undoing State Changes

To implement the undo functionality, we'll add a method to our `HistoryStack` class that pops the top state from the undo stack and moves it to the redo stack.

```swift
extension HistoryStack {
    func undoStateChange() {
        guard let state = undoStack.popLast() else {
            return
        }
        
        redoStack.append(state)
    }

    // Rest of the implementation...
}
```

In this example, we use `popLast` to retrieve the most recent state change from the undo stack. If the stack is empty, we simply return without performing any action.

## Step 4: Redoing State Changes

To implement the redo functionality, we'll add a method to our `HistoryStack` class that pops the top state from the redo stack and moves it to the undo stack.

```swift
extension HistoryStack {
    func redoStateChange() {
        guard let state = redoStack.popLast() else {
            return
        }
        
        undoStack.append(state)
    }

    // Rest of the implementation...
}
```

Similar to the undo functionality, we check if the redo stack is empty before performing the redo action.

## Step 5: Publishing State Changes

To keep our UI in sync with the state changes, we'll use Combine's `CurrentValueSubject` to publish the current state. Let's add a property `currentState` of type `CurrentValueSubject<State, Never>` to our `HistoryStack` class.

```swift
class HistoryStack {
    private var undoStack: [State] = []
    private var redoStack: [State] = []
    
    private(set) var currentState: CurrentValueSubject<State, Never>

    init(initialState: State) {
        currentState = CurrentValueSubject(initialState)
    }

    // Rest of the implementation...
}
```

In the `init` method, we initialize `currentState` with the initial state of the application.

## Step 6: Updating State Changes

Finally, we need to update the `currentState` whenever a state change occurs.

```swift
extension HistoryStack {
    func pushStateChange(_ state: State) {
        undoStack.append(state)
        redoStack.removeAll()

        currentState.send(state)
    }

    // Rest of the implementation...
}
```

By calling `currentState.send(state)` after adding a new state change, we notify any subscribers (such as our UI) about the new state.

## Conclusion

By following these steps, you can implement undo and redo functionality using Combine in your application. With Combine's powerful reactive programming capabilities, you can easily keep track of state changes and update your UI accordingly.

Remember to incorporate error handling and edge case scenarios in your implementation for a robust undo and redo functionality.

#Combine #UndoRedo