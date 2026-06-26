# Design Circular Queue (LeetCode 622)

| Topic | Difficulty | Pattern                       |
| ----- | ---------- | ----------------------------- |
| Queue | Medium     | Circular Queue Implementation |

---

# 📌 Problem Description

Design a **Circular Queue (Ring Buffer)** without using the built-in queue data structure.

Implement the following operations:

* `enQueue(value)`
* `deQueue()`
* `Front()`
* `Rear()`
* `isEmpty()`
* `isFull()`

Unlike a Linear Queue, a Circular Queue reuses empty spaces created after dequeuing elements.

---

# 🧠 Intuition

A **Linear Queue** wastes space after multiple dequeue operations.

Example:

```text
Capacity = 5

enqueue(1)
enqueue(2)
enqueue(3)
enqueue(4)
enqueue(5)

dequeue()
dequeue()
dequeue()
```

Array:

```text
_ _ _ 4 5
```

Although indices `0,1,2` are empty, a new insertion is impossible because `rear` has reached the end.

A **Circular Queue** solves this by treating the array as a circle.

```text
      ┌──────────────┐
      │              │
      ▼              │

0 → 1 → 2 → 3 → 4

▲                 │
└─────────────────┘
```

---

# 🎯 Pattern Recognition

Keywords:

* Circular Queue
* Ring Buffer
* FIFO
* Modulo (`%`)
* Wrap Around

---

# 📦 Data Members

```cpp
int* arr;
int front;
int rear;
int size;
int capacity;
```

### Purpose

| Variable   | Purpose                             |
| ---------- | ----------------------------------- |
| `arr`      | Stores queue elements               |
| `front`    | Points to the front element         |
| `rear`     | Points to the last inserted element |
| `size`     | Current number of elements          |
| `capacity` | Maximum queue capacity              |

---

# ⚙️ Constructor

Initialize:

```text
capacity = k
front = 0
rear = -1
size = 0
```

Allocate memory:

```cpp
arr = new int[k];
```

---

# 🔑 Core Idea

Instead of increasing indices normally:

```cpp
rear++;
front++;
```

Use modulo arithmetic:

```cpp
rear = (rear + 1) % capacity;
front = (front + 1) % capacity;
```

This wraps indices back to `0` after reaching the last position.

---

# 🧪 Dry Run

Capacity = **3**

Initially:

```text
Array : _ _ _

Front = 0
Rear  = -1
Size  = 0
```

---

### enQueue(10)

```text
10 _ _

Front = 0
Rear  = 0
Size  = 1
```

---

### enQueue(20)

```text
10 20 _

Front = 0
Rear  = 1
Size  = 2
```

---

### enQueue(30)

```text
10 20 30

Front = 0
Rear  = 2
Size  = 3
```

Queue becomes full.

---

### deQueue()

Remove front element.

```text
_ 20 30

Front = 1
Rear  = 2
Size  = 2
```

---

### enQueue(40)

```text
40 20 30

Front = 1
Rear  = 0
Size  = 3
```

Notice how **rear wraps back to index 0**.

---

# 📚 Operation Logic

## enQueue(value)

1. If queue is full → return `false`.
2. Move rear:

```cpp
rear = (rear + 1) % capacity;
```

3. Insert value.
4. Increment size.
5. Return `true`.

---

## deQueue()

1. If queue is empty → return `false`.
2. Move front:

```cpp
front = (front + 1) % capacity;
```

3. Decrement size.
4. Return `true`.

---

## Front()

If queue is empty:

```cpp
return -1;
```

Otherwise:

```cpp
return arr[front];
```

---

## Rear()

If queue is empty:

```cpp
return -1;
```

Otherwise:

```cpp
return arr[rear];
```

---

## isEmpty()

```cpp
return size == 0;
```

---

## isFull()

```cpp
return size == capacity;
```

---

# ⚠️ Why Maintain `size`?

Without `size`, it becomes difficult to distinguish between:

```text
front == rear
```

which could mean:

* Queue is empty
* Queue is full

Maintaining `size` makes both checks simple and efficient.

---

# ⚠️ Common Mistakes

### 1. Forgetting Modulo

Wrong:

```cpp
rear++;
```

Correct:

```cpp
rear = (rear + 1) % capacity;
```

---

### 2. Incrementing Rear Twice

Wrong:

```cpp
rear = (rear + 1) % capacity;
rear++;
```

Only update it once.

---

### 3. Forgetting Full Check

Always check:

```cpp
if(isFull())
    return false;
```

before insertion.

---

### 4. Returning Wrong Rear Element

With this implementation:

```text
rear → Last inserted element
```

So:

```cpp
return arr[rear];
```

No `rear - 1` is needed.

---

# 🆚 Linear Queue vs Circular Queue

| Linear Queue                          | Circular Queue               |
| ------------------------------------- | ---------------------------- |
| Wastes space                          | Reuses freed space           |
| Rear reaches end permanently          | Rear wraps using modulo      |
| Overflow may occur despite free space | Efficient memory utilization |
| Simpler                               | Slightly more complex        |

---

# 🎯 Key Takeaways

* Circular Queue follows **FIFO**.
* Uses a fixed-size array.
* Reuses freed positions.
* `% capacity` enables wrap-around.
* `size` simplifies `isEmpty()` and `isFull()`.
* Both enqueue and dequeue remain **O(1)**.

---

# 💻 Implementation (C++)

```cpp
class MyCircularQueue {
private:
    int* arr;
    int front;
    int rear;
    int size;
    int capacity;

public:
    MyCircularQueue(int k) {
        arr = new int[k];
        capacity = k;
        front = 0;
        rear = -1;
        size = 0;
    }

    bool enQueue(int value) {
        if (isFull())
            return false;

        rear = (rear + 1) % capacity;
        arr[rear] = value;
        size++;

        return true;
    }

    bool deQueue() {
        if (isEmpty())
            return false;

        front = (front + 1) % capacity;
        size--;

        return true;
    }

    int Front() {
        if (isEmpty())
            return -1;

        return arr[front];
    }

    int Rear() {
        if (isEmpty())
            return -1;

        return arr[rear];
    }

    bool isEmpty() {
        return size == 0;
    }

    bool isFull() {
        return size == capacity;
    }
};
```

---

# ⏱️ Complexity Analysis

| Operation | Time     |
| --------- | -------- |
| enQueue   | **O(1)** |
| deQueue   | **O(1)** |
| Front     | **O(1)** |
| Rear      | **O(1)** |
| isEmpty   | **O(1)** |
| isFull    | **O(1)** |

### Space Complexity

**O(n)** — Array of size `capacity`.

---

# 🚀 Placement Takeaways

* Circular Queue is also called a **Ring Buffer**.
* It solves the wasted-space problem of a Linear Queue.
* The most important concept is **modulo arithmetic (`%`)**.
* `front` points to the current front element.
* `rear` points to the last inserted element.
* `size` is maintained to distinguish between **empty** and **full** queues efficiently.
* Circular Queue is one of the most common queue implementation questions in coding interviews.
