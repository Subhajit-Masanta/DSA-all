# Queue - Introduction

---

## 📌 What is a Queue?

A **Queue** is a linear data structure that follows the **FIFO (First In, First Out)** principle.

The element inserted **first** is removed **first**. Insertions happen at the **Rear**, while deletions happen from the **Front**.

---

## 🧠 Real-Life Analogy

Think of people standing in a ticket counter line.

```text
Front                    Rear

A  B  C  D

Serve →
```

* A entered first → A leaves first.
* New people always join at the rear.

---

## 🎯 FIFO Principle

```text
Enqueue(10)
Enqueue(20)
Enqueue(30)

Front

10 20 30

Rear
```

After one dequeue:

```text
Front

20 30

Rear
```

The first inserted element (`10`) is removed first.

---

## 📚 Queue Terminology

### Front

The first element of the queue.

Deletion always happens here.

### Rear

The last element of the queue.

Insertion always happens here.

### Enqueue

Insert an element at the **rear**.

### Dequeue

Remove an element from the **front**.

### Peek / Front

Returns the front element without removing it.

### Size

Current number of elements.

### Empty

Checks whether the queue has no elements.

---

## ⚙️ Basic Operations

| Operation    | Description        | Time     |
| ------------ | ------------------ | -------- |
| Enqueue      | Insert at Rear     | **O(1)** |
| Dequeue      | Remove from Front  | **O(1)** |
| Front / Peek | View first element | **O(1)** |
| Empty        | Check empty        | **O(1)** |
| Size         | Number of elements | **O(1)** |

---

## 🆚 Stack vs Queue

| Stack                       | Queue                               |
| --------------------------- | ----------------------------------- |
| LIFO                        | FIFO                                |
| Push / Pop at Top           | Enqueue at Rear, Dequeue from Front |
| Last inserted removed first | First inserted removed first        |
| One accessible end          | Two accessible ends                 |

---

## 🏗️ Queue Implementations

A Queue can be implemented using:

* Array
* Linked List
* Circular Array
* Two Stacks
* STL Queue (`queue`)
* Deque (`deque`)

---

## 📦 STL Queue (C++)

```cpp
#include <queue>
using namespace std;

queue<int> q;
```

### Operations

```cpp
q.push(x);      // Enqueue
q.pop();        // Dequeue
q.front();      // Front element
q.back();       // Rear element
q.empty();      // Check empty
q.size();       // Number of elements
```

---

## 📊 Time Complexity (STL Queue)

| Operation | Complexity |
| --------- | ---------- |
| push()    | **O(1)**   |
| pop()     | **O(1)**   |
| front()   | **O(1)**   |
| back()    | **O(1)**   |
| empty()   | **O(1)**   |
| size()    | **O(1)**   |

---

## 🚀 Applications of Queue

### 1. CPU Scheduling

Operating systems schedule processes in FIFO order for many scheduling scenarios.

---

### 2. Breadth First Search (BFS)

Nodes are processed level by level using a queue.

---

### 3. Printer Queue

Print requests are processed in the order they arrive.

---

### 4. Network Packet Buffer

Routers and switches temporarily store packets in queues before forwarding them.

---

### 5. Producer-Consumer Problems

Tasks are produced at one end and consumed from the other while maintaining order.

---

### 6. Message Queues

Messaging systems process messages in arrival order.

---

### 7. IO Buffers

Used when two devices operate at different speeds (e.g., keyboard → CPU, network buffers).

---

## ✅ Advantages

* Simple FIFO ordering.
* Fast insertion and deletion (**O(1)**).
* Ideal for scheduling and buffering.
* Widely used in BFS, Operating Systems, Networking and Streaming.

---

## ❌ Disadvantages

* Middle elements cannot be accessed directly.
* Array implementation may waste space unless a Circular Queue is used.
* Fixed-size arrays can overflow.

---

## 🎯 Placement Takeaways

* Queue follows **FIFO**.
* Insert at **Rear**, Remove from **Front**.
* Basic operations are **O(1)**.
* Most interview implementations use:

  * Array
  * Linked List
  * Circular Queue
  * Queue using Stacks
* BFS in Trees and Graphs is one of the most important queue applications.

---

## 📌 Next Topics

* Queue using Array
* Queue using Linked List
* Circular Queue
* Design Circular Queue (LeetCode 622)
