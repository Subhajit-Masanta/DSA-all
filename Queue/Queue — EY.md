# Queue — EY Coding Practice

> **EY Focus:** Basic implementation + correct logic + edge cases.
> Do not overcomplicate these problems.

---

# 1. Implement Queue Using Array

## Problem

Implement a Queue using an array.

Support:

```text
enqueue()
dequeue()
front()
isEmpty()
```

### FIFO

```text
First In → First Out
```

Example:

```text
enqueue(10)
enqueue(20)
enqueue(30)

Queue:
[10] [20] [30]
 ↑           ↑
front       rear

dequeue() → 10
```

---

## Approach

Maintain:

```cpp
int arr[capacity];
int front;
int rear;
```

For a simple linear queue:

```text
front → first element
rear  → last element
```

---

## C++ Implementation

```cpp
#include <iostream>
using namespace std;

class Queue {
private:
    int* arr;
    int capacity;
    int front;
    int rear;

public:
    Queue(int size) {
        capacity = size;
        arr = new int[capacity];
        front = 0;
        rear = -1;
    }

    bool isEmpty() {
        return rear < front;
    }

    bool isFull() {
        return rear == capacity - 1;
    }

    void enqueue(int x) {
        if (isFull()) {
            cout << "Queue Overflow\n";
            return;
        }

        arr[++rear] = x;
    }

    void dequeue() {
        if (isEmpty()) {
            cout << "Queue Underflow\n";
            return;
        }

        front++;
    }

    int getFront() {
        if (isEmpty()) {
            return -1;
        }

        return arr[front];
    }
};

int main() {
    Queue q(5);

    q.enqueue(10);
    q.enqueue(20);
    q.enqueue(30);

    cout << q.getFront() << endl;  // 10

    q.dequeue();

    cout << q.getFront() << endl;  // 20

    return 0;
}
```

---

## Complexity

| Operation | Time |
| --------- | ---: |
| Enqueue   | O(1) |
| Dequeue   | O(1) |
| Front     | O(1) |
| isEmpty   | O(1) |

Space:

```text
O(n)
```

---

## Important Limitation

The above is a **linear queue**.

Suppose:

```text
Capacity = 5

[10] [20] [30] [40] [50]
 ↑
front
```

Remove three:

```text
[ ] [ ] [ ] [40] [50]
            ↑     ↑
          front  rear
```

There is free space at the beginning, but `rear` has reached the end.

This is why we need:

> **Circular Queue**

---

# 2. Circular Queue

## Problem

Implement a Queue where the array positions can be reused.

Support:

```text
enqueue()
dequeue()
front()
isFull()
isEmpty()
```

---

## Key Idea

Treat the array as circular.

```text
[0] [1] [2] [3] [4]
 ↑               ↓
 └───────────────┘
```

When an index reaches the end:

```cpp
(index + 1) % capacity
```

wraps it back to `0`.

### Most Important Formula

```cpp
rear = (rear + 1) % capacity;
```

and:

```cpp
front = (front + 1) % capacity;
```

---

## C++ Implementation

```cpp
#include <iostream>
using namespace std;

class CircularQueue {
private:
    int* arr;
    int capacity;
    int front;
    int rear;
    int count;

public:
    CircularQueue(int size) {
        capacity = size;
        arr = new int[capacity];

        front = 0;
        rear = -1;
        count = 0;
    }

    bool isEmpty() {
        return count == 0;
    }

    bool isFull() {
        return count == capacity;
    }

    void enqueue(int x) {
        if (isFull()) {
            cout << "Queue Overflow\n";
            return;
        }

        rear = (rear + 1) % capacity;
        arr[rear] = x;
        count++;
    }

    void dequeue() {
        if (isEmpty()) {
            cout << "Queue Underflow\n";
            return;
        }

        front = (front + 1) % capacity;
        count--;
    }

    int getFront() {
        if (isEmpty()) {
            return -1;
        }

        return arr[front];
    }
};

int main() {
    CircularQueue q(5);

    q.enqueue(10);
    q.enqueue(20);
    q.enqueue(30);
    q.enqueue(40);
    q.enqueue(50);

    q.dequeue();
    q.dequeue();

    q.enqueue(60);
    q.enqueue(70);

    cout << q.getFront() << endl;

    return 0;
}
```

---

## Circular Queue Example

Capacity:

```text
5
```

Insert:

```text
10 20 30 40 50
```

```text
[10][20][30][40][50]
 ↑                 ↑
front             rear
```

Remove `10`, `20`:

```text
[  ][  ][30][40][50]
          ↑       ↑
        front    rear
```

Now insert `60`:

```text
[60][  ][30][40][50]
 ↑        ↑
rear    front
```

The rear **wrapped around**.

This is the whole point of a circular queue.

---

## Circular Queue Complexity

| Operation | Time |
| --------- | ---: |
| Enqueue   | O(1) |
| Dequeue   | O(1) |
| Front     | O(1) |
| isEmpty   | O(1) |
| isFull    | O(1) |

Space:

```text
O(n)
```

---

## EY Must Understand

You should be able to explain:

### Why circular queue?

> To reuse the empty positions created after dequeue operations.

### Why `% capacity`?

> It makes the index wrap from the last position back to the first.

### Why `count`?

It makes checking these conditions simple:

```cpp
count == 0       // empty
count == capacity // full
```

---

# 3. Reverse a Queue

## Problem

Reverse the order of elements in a Queue.

### Input

```text
1 2 3 4 5
```

### Output

```text
5 4 3 2 1
```

---

# Approach 1 — Using Stack

This is the easiest approach to remember.

### Idea

Queue:

```text
1 2 3 4 5
```

Move everything into Stack:

```text
Stack:

5
4
3
2
1
```

Then move everything back into Queue:

```text
5 4 3 2 1
```

---

## C++ Code

```cpp
#include <iostream>
#include <queue>
#include <stack>

using namespace std;

void reverseQueue(queue<int>& q) {
    stack<int> st;

    while (!q.empty()) {
        st.push(q.front());
        q.pop();
    }

    while (!st.empty()) {
        q.push(st.top());
        st.pop();
    }
}

int main() {
    queue<int> q;

    q.push(1);
    q.push(2);
    q.push(3);
    q.push(4);
    q.push(5);

    reverseQueue(q);

    while (!q.empty()) {
        cout << q.front() << " ";
        q.pop();
    }

    return 0;
}
```

Output:

```text
5 4 3 2 1
```

---

## Complexity

Every element is:

1. Removed from Queue
2. Added to Stack
3. Removed from Stack
4. Added to Queue

Therefore:

```text
Time  → O(n)
Space → O(n)
```

---

# Approach 2 — Using Recursion

This is also worth knowing because it tests recursion + Queue.

### Idea

1. Remove front element.
2. Recursively reverse remaining queue.
3. Push removed element at the back.

Example:

```text
Queue:
1 2 3 4 5

remove 1
→ reverse 2 3 4 5

remove 2
→ reverse 3 4 5

...

then add back:
1 → 2 → 3 → 4 → 5
```

Final:

```text
5 4 3 2 1
```

---

## C++ Code

```cpp
#include <iostream>
#include <queue>

using namespace std;

void reverseQueue(queue<int>& q) {
    if (q.empty()) {
        return;
    }

    int x = q.front();
    q.pop();

    reverseQueue(q);

    q.push(x);
}

int main() {
    queue<int> q;

    q.push(1);
    q.push(2);
    q.push(3);
    q.push(4);
    q.push(5);

    reverseQueue(q);

    while (!q.empty()) {
        cout << q.front() << " ";
        q.pop();
    }

    return 0;
}
```

Output:

```text
5 4 3 2 1
```

---

## Complexity

```text
Time  → O(n)
Space → O(n)
```

The space comes from the recursion call stack.

---

# EY Interview Comparison

| Problem                       | Main Concept      |     Time | Space |
| ----------------------------- | ----------------- | -------: | ----: |
| Queue using Array             | FIFO + pointers   | O(1) ops |  O(n) |
| Circular Queue                | Modulo / wrapping | O(1) ops |  O(n) |
| Reverse Queue using Stack     | Queue + Stack     |     O(n) |  O(n) |
| Reverse Queue using Recursion | Queue + Recursion |     O(n) |  O(n) |

---

# What You Must Be Able to Write From Memory

## 1. Normal Queue

```cpp
enqueue()
dequeue()
getFront()
isEmpty()
```

Know:

```text
front
rear
```

---

## 2. Circular Queue

Know:

```cpp
rear = (rear + 1) % capacity;
front = (front + 1) % capacity;
```

and:

```cpp
isEmpty()
isFull()
```

---

## 3. Reverse Queue

Know both:

```text
Queue + Stack
```

and:

```text
Queue + Recursion
```

---

# EY Practice Order

Do them in this exact order:

```text
1. Queue using Array
       ↓
2. Circular Queue
       ↓
3. Reverse Queue using Stack
       ↓
4. Reverse Queue using Recursion
```

### Target

You should be able to write all four **without looking at the solution**.

Don't memorize the code line-by-line. Understand:

```text
Normal Queue
front / rear
     ↓
Circular Queue
% capacity
     ↓
Reverse Queue
Stack / Recursion
```

EY interview reports specifically include basic queue insertion/deletion and generally emphasize fundamentals rather than advanced competitive-programming techniques.
