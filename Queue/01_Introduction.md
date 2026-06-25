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

---

## 🚀 Applications of Queue

Queue is used whenever tasks or requests must be processed in the **same order in which they arrive (FIFO)**.

### 1. Breadth First Search (BFS) ⭐⭐⭐⭐⭐

The **most important DSA application**.

Queues process nodes **level by level**.

Examples:

* Level Order Traversal
* Shortest Path in Unweighted Graph

---

### 2. CPU Job Scheduling

Operating Systems schedule waiting processes using queues.

Examples:

* FCFS Scheduling
* Round Robin Scheduling (Circular Queue)

---

### 3. Network Packet Scheduling

Routers and switches temporarily store packets before forwarding them.

Examples:

* Router Packet Queue
* Mail Queue

---

### 4. Shared Resources

Queues act as waiting lists when multiple users share a single resource.

Examples:

* Printer Queue
* Disk Scheduling
* Database Request Queue

---

### 5. Buffer Between Fast and Slow Devices

Queues temporarily store data when two devices work at different speeds.

Examples:

* Keyboard → CPU
* Audio/Video Streaming
* Network Buffer

---

### 6. Waiting Line Simulation

Real-life waiting systems naturally follow FIFO.

Examples:

* ATM Queue
* Ticket Counter
* Hospital Queue
* Call Center Waiting Queue

---

### 7. Producer–Consumer Systems

One process produces tasks while another consumes them in arrival order.

Examples:

* Message Queues
* Task Scheduling Systems

---

## ✅ Advantages

* Maintains **FIFO** ordering automatically.
* Enqueue and Dequeue operations take **O(1)** time.
* Useful when multiple consumers share a single resource.
* Fast for inter-process communication.
* Can be used to implement other data structures.
* Widely used in Operating Systems, Networking, BFS, Scheduling and Streaming.

---

## ❌ Disadvantages

* No direct access to middle elements.
* Insertion and deletion in the middle are expensive.
* Searching takes **O(n)** time.
* Linear array implementation wastes space after repeated dequeues.
* Array implementation requires a predefined maximum size.
* Overflow may occur if the queue becomes full.

---

# 📚 Types of Queue

```text
Queue
│
├── Linear Queue
├── Circular Queue
├── Priority Queue
└── Deque (Double Ended Queue)
```

---

## 1. Linear Queue

A normal FIFO queue.

### Allowed Operations

* ✅ Enqueue → Rear
* ✅ Dequeue → Front
* ✅ Peek/Front
* ✅ Rear/Back

```text
Front                Rear

10 20 30 40
```

---

## 2. Circular Queue ⭐⭐⭐⭐⭐

The last position is connected back to the first.

Freed positions are reused efficiently, eliminating wasted space.

### Allowed Operations

* ✅ Enqueue → Rear
* ✅ Dequeue → Front
* ✅ Peek/Front
* ✅ Rear/Back

```text
      Front
        ↓
+---+---+---+---+
|10 |20 |30 |40 |
+---+---+---+---+
        ↑
       Rear
```

---

## 3. Priority Queue

Elements are removed according to **priority**, not insertion order.

Higher (or lower) priority elements leave first.

### Allowed Operations

* ✅ Insert
* ✅ Delete Highest / Lowest Priority
* ✅ Peek Highest / Lowest Priority

Example:

```text
Priority

5
4
3
2
1
```

---

## 4. Deque (Double Ended Queue)

Insertion and deletion are allowed at **both ends**.

### Allowed Operations

* ✅ Push Front
* ✅ Push Back
* ✅ Pop Front
* ✅ Pop Back
* ✅ Front
* ✅ Back

```text
← Insert / Delete

10 20 30

Insert / Delete →
```

---

## 🎯 Placement Takeaways

* Queue follows **FIFO (First In First Out)**.
* Insert at **Rear**, Remove from **Front**.
* Basic operations are **O(1)**.
* **Circular Queue** solves the wasted-space problem of Linear Queue.
* **Priority Queue** does **NOT** follow FIFO.
* **Deque** allows insertion and deletion at **both ends**.
* **BFS (Trees & Graphs)** is the most important application of Queue.
* The four queue types every interview candidate should know are:

  * Linear Queue
  * Circular Queue
  * Priority Queue
  * Deque

