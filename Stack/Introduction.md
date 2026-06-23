# Stack - Introduction

## What is a Stack?

A **Stack** is a linear data structure that follows the **LIFO (Last In First Out)** principle.

The last element inserted into the stack is the first element removed.

### Example

```text
Push(10)
Push(20)
Push(30)

Top
 ↓
30
20
10
```

Removing elements:

```text
Pop() → 30
Pop() → 20
Pop() → 10
```

The most recently added element is always removed first.

---

# Why Do We Need a Stack?

Many real-world and programming problems require us to remember the **most recent unfinished task**.

Examples:

* Browser Back Button
* Undo/Redo Operations
* Function Calls
* Recursion
* Expression Evaluation
* Syntax Checking
* DFS Traversal

A Stack naturally solves these problems because of its **LIFO behavior**.

---

# Relationship with Previous Topics

## Recursion

Recursion internally uses a stack called the **Call Stack**.

Example:

```cpp
func(3)
 └── func(2)
      └── func(1)
```

Internally:

```text
Push func(3)
Push func(2)
Push func(1)
```

Returning:

```text
Pop func(1)
Pop func(2)
Pop func(3)
```

Every recursive solution is using a stack behind the scenes.

---

## Backtracking

Backtracking follows:

```text
Choose
Explore
Undo
```

Example:

```cpp
path.push_back(x);
solve(...);
path.pop_back();
```

This push-pop behavior is exactly Stack thinking.

---

# Basic Operations

## Push

Insert an element at the top.

```cpp
st.push(x);
```

Time Complexity:

```text
O(1)
```

---

## Pop

Remove the top element.

```cpp
st.pop();
```

Time Complexity:

```text
O(1)
```

---

## Top

Access the top element.

```cpp
st.top();
```

Time Complexity:

```text
O(1)
```

---

## Empty

Check whether stack is empty.

```cpp
st.empty();
```

Time Complexity:

```text
O(1)
```

---

# Applications of Stack

## 1. Function Calls

Stacks manage active function calls.

When a function is called:

```text
Push function state
```

When it returns:

```text
Pop function state
```

### Example

```cpp
main()
{
    calculateBill();
}
```

```cpp
calculateBill()
{
    applyDiscount();
}
```

Execution Order:

```text
Push main()
Push calculateBill()
Push applyDiscount()

Pop applyDiscount()
Pop calculateBill()
Pop main()
```

---

## 2. Recursion

Recursion relies entirely on the call stack.

### Example

Factorial:

```cpp
fact(3)
```

Stack:

```text
Push fact(3)
Push fact(2)
Push fact(1)
```

Returning:

```text
Pop fact(1)
Pop fact(2)
Pop fact(3)
```

---

## 3. Expression Evaluation

Used in calculators and compilers.

### Example

Postfix Expression

```text
3 4 + 5 *
```

Process:

```text
Push 3
Push 4
Pop 3,4 → 7
Push 7
Push 5
Pop 7,5 → 35
```

Answer:

```text
35
```

---

## 4. Syntax Parsing

Used to check balanced brackets.

### Example

```text
(a+b) * {c+d}
```

Process:

```text
Push (
Pop )

Push {
Pop }
```

If stack becomes empty at the end:

```text
Valid Expression
```

Otherwise:

```text
Invalid Expression
```

---

## 5. Memory Management

The Stack is a special region of RAM used for:

* Function Calls
* Local Variables
* Return Addresses

### Example

```cpp
void fun()
{
    int x = 10;
}
```

When `fun()` finishes:

```text
x is automatically removed from memory
```

No manual cleanup is required.

---

# Important Interview Applications

These are the most common placement patterns.

## 1. Next Greater Element

Find first greater element on the right.

```text
[2,1,5,3]
```

Uses:

```text
Monotonic Stack
```

---

## 2. Previous Greater Element

Find first greater element on the left.

Uses:

```text
Monotonic Stack
```

---

## 3. Next Smaller Element

Find first smaller element on the right.

Uses:

```text
Monotonic Stack
```

---

## 4. Previous Smaller Element

Find first smaller element on the left.

Uses:

```text
Monotonic Stack
```

---

## 5. Stock Span Problem

Very common interview question.

Uses:

```text
Monotonic Stack
```

---

## 6. Largest Rectangle in Histogram

Classic Stack problem.

Asked in:

* Amazon
* Microsoft
* Adobe
* Flipkart

Uses:

```text
Previous Smaller
Next Smaller
```

---

# Advantages

## Time Efficient

Operations:

```text
Push  → O(1)
Pop   → O(1)
Top   → O(1)
```

---

## Memory Efficient

Only stores necessary elements.

---

## Simple LIFO Model

Perfect for:

* Undo Operations
* Recursion
* Parsing
* DFS

---

# Disadvantages

## Limited Access

Can only access:

```text
Top Element
```

Cannot directly access middle elements.

---

## Overflow Risk

Fixed-size stack implementations may overflow when capacity is exceeded.

---

# Pattern Recognition

Think "Stack" when you see:

* Balanced Brackets
* Undo / Back Button
* Nested Structures
* Expression Evaluation
* Nearest Greater Element
* Nearest Smaller Element
* Stock Span
* Histogram Problems
* DFS Traversal
* Recursion Simulation

---

# Complexity Summary

| Operation | Time |
| --------- | ---- |
| Push      | O(1) |
| Pop       | O(1) |
| Top       | O(1) |
| Empty     | O(1) |

---

# Placement Importance

### Must Know

* Valid Parentheses
* Next Greater Element
* Daily Temperatures
* Stock Span
* Largest Rectangle in Histogram
* Monotonic Stack Pattern

### Good to Know

* Expression Evaluation
* Infix/Postfix/Prefix Conversion
* Min Stack
* Decode String

### Skip Initially

* Advanced Expression Parsing
* Compiler Design Variants
* Rare Stack Implementations
