# Queue Using Linked List

| Topic | Difficulty | Pattern                                |
| ----- | ---------- | -------------------------------------- |
| Queue | Basic      | Queue Implementation using Linked List |

---

# 📌 Problem Description

Implement a Queue using a **Linked List**.

Support the following operations:

* `enqueue(x)`
* `dequeue()`
* `getFront()`
* `isEmpty()`
* `size()`

Unlike an array implementation, a Linked List Queue has **no fixed capacity** and can grow dynamically until memory is exhausted.

---

# 🧠 Intuition

A Queue follows the **FIFO (First In First Out)** principle.

Using a Linked List:

* Insertions happen at the **Rear**.
* Deletions happen from the **Front**.

To perform both operations in **O(1)** time, maintain two pointers:

* `front`
* `rear`

---

# 🎯 Pattern Recognition

Keywords:

* Queue
* FIFO
* Linked List
* Front Pointer
* Rear Pointer

---

# 📦 Data Members

```cpp
Node* front;
Node* rear;
int sizec;
```

### Purpose

| Variable | Purpose                               |
| -------- | ------------------------------------- |
| `front`  | Points to the first node              |
| `rear`   | Points to the last node               |
| `sizec`  | Stores the current number of elements |

---

# ⚙️ Constructor

Initially, the queue is empty.

```text
front = nullptr
rear  = nullptr
sizec = 0
```

---

# 🧪 Dry Run

Initially:

```text
front = nullptr
rear  = nullptr
size = 0
```

---

### enqueue(10)

```text
front
 ↓
10 → NULL
 ↑
rear

size = 1
```

---

### enqueue(20)

```text
front
 ↓
10 → 20 → NULL
      ↑
     rear

size = 2
```

---

### enqueue(30)

```text
front
 ↓
10 → 20 → 30 → NULL
           ↑
          rear

size = 3
```

---

### dequeue()

Remove the front node.

```text
front
 ↓
20 → 30 → NULL
      ↑
     rear

size = 2
```

---

# 📚 Operation Logic

## Enqueue

1. Create a new node.
2. If queue is empty:

   * `front = rear = newNode`
3. Otherwise:

   * `rear->next = newNode`
   * `rear = newNode`
4. Increment size.

---

## Dequeue

1. If queue is empty, return.
2. Store the current front in a temporary pointer.
3. Move `front` to `front->next`.
4. Delete the old front node.
5. Decrement size.
6. If queue becomes empty:

   * `rear = nullptr`

---

## getFront()

Return:

```cpp
front->data
```

If empty:

```cpp
return -1;
```

---

## isEmpty()

```cpp
return front == nullptr;
```

---

## size()

Return:

```cpp
sizec
```

---

# ⚠️ Important Edge Case

Queue contains only one node.

Before:

```text
front
 ↓
10 → NULL
 ↑
rear
```

After `dequeue()`:

```text
front = nullptr
rear  = nullptr
```

Both pointers must become `nullptr`.

---

# ⚠️ Common Mistakes

### 1. Forgetting to Update `rear`

After removing the last node:

```cpp
rear = nullptr;
```

Otherwise, `rear` becomes a dangling pointer.

---

### 2. Forgetting to Delete the Removed Node

Wrong:

```cpp
front = front->next;
```

Correct:

```cpp
Node* temp = front;
front = front->next;
delete temp;
```

Otherwise, memory leaks occur.

---

### 3. Forgetting to Update Size

Every enqueue:

```text
size++
```

Every dequeue:

```text
size--
```

---

### 4. Traversing to Insert

Wrong:

```text
Traverse to the last node every enqueue.
```

This makes insertion **O(n)**.

Always maintain a `rear` pointer so enqueue remains **O(1)**.

---

# 🆚 Queue Using Array vs Linked List

| Queue using Array              | Queue using Linked List |
| ------------------------------ | ----------------------- |
| Fixed Capacity                 | Dynamic Size            |
| May Waste Space (Linear Queue) | No Wasted Space         |
| Uses Indices                   | Uses Pointers           |
| Overflow Possible              | Limited only by Memory  |
| Enqueue: O(1)                  | O(1)                    |
| Dequeue: O(1)                  | O(1)                    |

---

# 🎯 Key Takeaways

* Queue follows **FIFO**.
* `front` points to the first node.
* `rear` points to the last node.
* Both enqueue and dequeue are **O(1)**.
* Linked List implementation removes the fixed-size limitation of arrays.
* When the queue becomes empty, both `front` and `rear` must be `nullptr`.

---

# 💻 Implementation (C++)

```cpp
class Node {
public:
    int data;
    Node* next;

    Node(int new_data) {
        data = new_data;
        next = nullptr;
    }
};

class myQueue {
private:
    Node* front;
    Node* rear;
    int sizec;

public:
    myQueue() {
        front = nullptr;
        rear = nullptr;
        sizec = 0;
    }

    bool isEmpty() {
        return front == nullptr;
    }

    void enqueue(int x) {
        Node* newNode = new Node(x);

        if (front == nullptr) {
            front = rear = newNode;
        } else {
            rear->next = newNode;
            rear = newNode;
        }

        sizec++;
    }

    void dequeue() {
        if (front == nullptr)
            return;

        Node* temp = front;
        front = front->next;
        delete temp;

        sizec--;

        if (front == nullptr)
            rear = nullptr;
    }

    int getFront() {
        if (front == nullptr)
            return -1;

        return front->data;
    }

    int size() {
        return sizec;
    }
};
```

---

# ⏱️ Complexity Analysis

| Operation | Time     |
| --------- | -------- |
| Enqueue   | **O(1)** |
| Dequeue   | **O(1)** |
| getFront  | **O(1)** |
| isEmpty   | **O(1)** |
| size      | **O(1)** |

### Space Complexity

**O(n)** — One node is allocated for each element in the queue.
