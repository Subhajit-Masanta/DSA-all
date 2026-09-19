# 232. Implement Queue using Stacks

| Topic | Difficulty | Pattern                         |
| ----- | ---------- | ------------------------------- |
| Stack | Easy       | Queue Simulation / Stack Design |

---

## 📌 Problem Description

Implement a **Queue (FIFO)** using only **Stacks (LIFO)**.

Implement the following operations:

* `push(x)`
* `pop()`
* `peek()`
* `empty()`

Only standard stack operations are allowed.

---

## 🧠 Intuition

A stack removes the **last inserted** element.

A queue removes the **first inserted** element.

Use **two stacks** to reverse the order.

---

## 🎯 Pattern Recognition

Keywords:

* Queue Simulation
* Two Stacks
* FIFO using LIFO
* Data Structure Design

---

## ⚙️ Approach

Maintain two stacks:

```cpp
s1 → Input Stack
s2 → Output Stack
```

### Push

Push directly into `s1`.

---

### Pop / Peek

If `s2` is empty:

* Move every element from `s1` to `s2`.

This reverses the order.

The oldest element now becomes the top of `s2`.

---

### Empty

Queue is empty only when:

```cpp
s1.empty() && s2.empty()
```

---

## 🧪 Dry Run

Operations:

```text
push(1)
push(2)
push(3)
```

---

### After Pushes

```text
s1

Top
3
2
1

s2
Empty
```

---

### pop()

Move all elements:

```text
s2

Top
1
2
3
```

Pop:

```text
1
```

Queue becomes:

```text
Front

2
3

Back
```

---

### peek()

Top of `s2`:

```text
2
```

---

## 📚 Stack Visualization

Initially:

```text
s1

Top
3
2
1

s2

Empty
```

After first Pop:

```text
s1

Empty

s2

Top
2
3
```

---

## ⚠️ Common Mistakes

### 1. Moving Elements Every Time

Wrong:

Always transferring from `s1` to `s2`.

Correct:

Only transfer when:

```cpp
s2.empty()
```

This gives amortized **O(1)** operations.

---

### 2. Forgetting Empty Condition

Queue is empty only when **both** stacks are empty.

---

### 3. Reversing in Wrong Direction

Always move:

```cpp
s1 → s2
```

Never the opposite during `pop()` or `peek()`.

---

## 🔁 Similar Problems

* Implement Stack using Queues (225)
* Min Stack
* Queue Design

---

## 🎯 Key Takeaways

* `s1` stores newly inserted elements.
* `s2` serves elements in queue order.
* Transfer only when `s2` becomes empty.
* Each element moves from `s1` to `s2` at most once.

---

## 💻 Implementation (C++)

```cpp
class MyQueue {
public:

    stack<int> s1;
    stack<int> s2;

    MyQueue() {

    }

    void push(int x) {
        s1.push(x);
    }

    int pop() {

        if(s2.empty()) {
            while(!s1.empty()) {
                s2.push(s1.top());
                s1.pop();
            }
        }

        int val = s2.top();
        s2.pop();

        return val;
    }

    int peek() {

        if(s2.empty()) {
            while(!s1.empty()) {
                s2.push(s1.top());
                s1.pop();
            }
        }

        return s2.top();
    }

    bool empty() {
        return s1.empty() && s2.empty();
    }
};
```

---

## ⏱️ Complexity Analysis

| Operation | Complexity         |
| --------- | ------------------ |
| Push      | **O(1)**           |
| Pop       | **Amortized O(1)** |
| Peek      | **Amortized O(1)** |
| Empty     | **O(1)**           |

### Why Amortized O(1)?

Although transferring elements takes **O(n)**, each element is:

* pushed into `s1` once,
* moved to `s2` once,
* popped from `s2` once.

So each element is processed a constant number of times, making the average cost per operation **O(1)**.
