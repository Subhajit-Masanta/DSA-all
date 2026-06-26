# Queue Using Array

| Topic | Difficulty | Pattern              |
| ----- | ---------- | -------------------- |
| Queue | Basic      | Queue Implementation |

---

# 📌 Problem Description

Implement a Queue using an **Array**.

Support the following operations:

* `enqueue(x)`
* `dequeue()`
* `getFront()`
* `getRear()`
* `isEmpty()`
* `isFull()`

---

# 🧠 Intuition

A Queue follows the **FIFO (First In First Out)** principle.

Using an array:

* Insertions happen at the **Rear**.
* Deletions happen from the **Front**.

We maintain two indices:

* `front`
* `rear`

along with:

* `size`
* `capacity`

---

# 🎯 Pattern Recognition

Keywords:

* Queue
* FIFO
* Array Implementation
* Front & Rear Pointers

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

| Variable   | Purpose                           |
| ---------- | --------------------------------- |
| `arr`      | Stores queue elements             |
| `front`    | Points to first element           |
| `rear`     | Points to next insertion position |
| `size`     | Current number of elements        |
| `capacity` | Maximum queue size                |

---

# ⚙️ Constructor

```cpp
myQueue(int n)
```

The constructor receives the maximum capacity of the queue.

Initialize:

```text
capacity = n
front = 0
rear = 0
size = 0
```

Allocate memory dynamically:

```cpp
arr = new int[capacity];
```

---

# 🧪 Dry Run

Queue Size:

```text
5
```

Initially:

```text
Front = 0
Rear  = 0
Size  = 0

[]
```

---

### enqueue(10)

```text
10

Front = 0
Rear  = 1
Size  = 1
```

---

### enqueue(20)

```text
10 20

Front = 0
Rear  = 2
Size  = 2
```

---

### dequeue()

Remove from front.

```text
20

Front = 1
Rear  = 2
Size  = 1
```

Notice that **front moves forward**, while `rear` remains unchanged.

---

# 📚 Operation Logic

### Enqueue

* Check if queue is full.
* Insert at `rear`.
* Increment `rear`.
* Increment `size`.

---

### Dequeue

* Check if queue is empty.
* Increment `front`.
* Decrement `size`.

---

### getFront()

Return:

```cpp
arr[front]
```

If empty:

```cpp
return -1;
```

---

### getRear()

Since `rear` always points to the **next free position**, the last inserted element is stored at:

```cpp
arr[rear - 1]
```

If empty:

```cpp
return -1;
```

---

### isEmpty()

```cpp
size == 0
```

---

### isFull()

```cpp
size == capacity
```

---

# ⚠️ Limitation of Linear Queue

Consider:

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

Now:

```text
Front = 3
Rear  = 5
Size  = 2
```

The queue still has free space at the beginning of the array, but `rear` has reached the end.

Trying another `enqueue()` causes an overflow even though there are empty positions.

This is the major drawback of a **Linear Queue**.

👉 **Circular Queue** solves this problem by reusing freed positions.

---

# ⚠️ Common Mistakes

### 1. Returning `arr[rear]`

Wrong:

```cpp
return arr[rear];
```

Correct:

```cpp
return arr[rear - 1];
```

because `rear` points to the next insertion position.

---

### 2. Forgetting Empty Check

Always check:

```cpp
isEmpty()
```

before accessing front or rear.

---

### 3. Using Local Array

Wrong:

```cpp
int arr[n];
```

The array should be a **class member**, allocated dynamically.

---

### 4. Forgetting to Update Size

Every enqueue:

```text
size++
```

Every dequeue:

```text
size--
```

---

# 🎯 Key Takeaways

* Queue follows **FIFO**.
* Insert at **Rear**.
* Delete from **Front**.
* `rear` points to the next insertion position.
* `front` points to the current front element.
* Linear Queue wastes space after multiple dequeues.
* Circular Queue fixes this limitation.

---

# 💻 Implementation (C++)

```cpp
class myQueue {
private:
    int* arr;
    int front;
    int rear;
    int size;
    int capacity;

public:
    myQueue(int n) {
        capacity = n;
        arr = new int[capacity];
        front = rear = 0;
        size = 0;
    }

    bool isEmpty() {
        return size == 0;
    }

    bool isFull() {
        return size == capacity;
    }

    void enqueue(int x) {
        if (isFull())
            return;

        arr[rear] = x;
        rear++;
        size++;
    }

    void dequeue() {
        if (isEmpty())
            return;

        front++;
        size--;
    }

    int getFront() {
        if (isEmpty())
            return -1;

        return arr[front];
    }

    int getRear() {
        if (isEmpty())
            return -1;

        return arr[rear - 1];
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
| getRear   | **O(1)** |
| isEmpty   | **O(1)** |
| isFull    | **O(1)** |

### Space Complexity

**O(n)** — Array of size `capacity`.
