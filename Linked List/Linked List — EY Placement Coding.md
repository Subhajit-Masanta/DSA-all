# Linked List — EY Placement Coding & Revision

> **Status: ✅ COMPLETED**
>
> This file covers the Linked List coding patterns required for the current EY preparation.
>
> **Goal:** Be able to implement the basics from scratch and recognize the standard interview patterns.

---

# 1. Phase 1 — Basic Operations

## 1.1 Node Structure

```cpp
struct Node {
    int data;
    Node* next;

    Node(int x) {
        data = x;
        next = nullptr;
    }
};
```

Basic representation:

```text
[10 | next] → [20 | next] → [30 | NULL]
```

---

# 2. Create Linked List From Input

Input:

```text
5
10 20 30 40 50
```

Output structure:

```text
HEAD
 ↓
10 → 20 → 30 → 40 → 50 → NULL
```

```cpp
Node* head = nullptr;
Node* tail = nullptr;

for (int i = 0; i < n; i++) {
    int x;
    cin >> x;

    Node* newNode = new Node(x);

    if (head == nullptr) {
        head = newNode;
        tail = newNode;
    } else {
        tail->next = newNode;
        tail = newNode;
    }
}
```

### Complexity

```text
Time:  O(n)
Space: O(n)
```

---

# 3. Traverse / Print

```cpp
void printList(Node* head) {
    Node* curr = head;

    while (curr != nullptr) {
        cout << curr->data << " ";
        curr = curr->next;
    }
}
```

### Complexity

```text
Time:  O(n)
Space: O(1)
```

---

# 4. Insert at Beginning ⭐

Before:

```text
10 → 20 → 30
```

Insert `5`:

```text
5 → 10 → 20 → 30
```

```cpp
void insertAtBeginning(Node*& head, int x) {
    Node* newNode = new Node(x);

    newNode->next = head;
    head = newNode;
}
```

### Complexity

```text
Time:  O(1)
Space: O(1) extra
```

---

# 5. Insert at End

Without a `tail` pointer:

```cpp
void insertAtEnd(Node*& head, int x) {
    Node* newNode = new Node(x);

    if (head == nullptr) {
        head = newNode;
        return;
    }

    Node* curr = head;

    while (curr->next != nullptr) {
        curr = curr->next;
    }

    curr->next = newNode;
}
```

### Complexity

```text
Time:  O(n)
Space: O(1) extra
```

With a maintained `tail` pointer:

```text
Time: O(1)
```

---

# 6. Insert at Given Position

Example:

```text
10 → 20 → 30 → 40
```

Insert `25` at position 3:

```text
10 → 20 → 25 → 30 → 40
```

```cpp
void insertAtPosition(Node*& head, int x, int pos) {
    Node* newNode = new Node(x);

    if (pos == 1) {
        newNode->next = head;
        head = newNode;
        return;
    }

    Node* curr = head;

    for (int i = 1; i < pos - 1 && curr != nullptr; i++) {
        curr = curr->next;
    }

    if (curr == nullptr) {
        return;
    }

    newNode->next = curr->next;
    curr->next = newNode;
}
```

### Complexity

```text
Time:  O(n)
Space: O(1) extra
```

---

# 7. Delete From Beginning ⭐

```cpp
void deleteBeginning(Node*& head) {
    if (head == nullptr) {
        return;
    }

    Node* temp = head;
    head = head->next;

    delete temp;
}
```

### Complexity

```text
Time:  O(1)
Space: O(1)
```

---

# 8. Delete From End

```cpp
void deleteEnd(Node*& head) {
    if (head == nullptr) {
        return;
    }

    if (head->next == nullptr) {
        delete head;
        head = nullptr;
        return;
    }

    Node* curr = head;

    while (curr->next->next != nullptr) {
        curr = curr->next;
    }

    delete curr->next;
    curr->next = nullptr;
}
```

### Complexity

```text
Time:  O(n)
Space: O(1)
```

Why `O(n)`?

A singly linked list has no `prev` pointer, so we must find the second-last node.

---

# 9. Delete a Given Value

Example:

```text
10 → 20 → 30 → 40
```

Delete `30`:

```text
10 → 20 → 40
```

```cpp
void deleteValue(Node*& head, int x) {
    if (head == nullptr) {
        return;
    }

    if (head->data == x) {
        Node* temp = head;
        head = head->next;
        delete temp;
        return;
    }

    Node* curr = head;

    while (curr->next != nullptr &&
           curr->next->data != x) {
        curr = curr->next;
    }

    if (curr->next == nullptr) {
        return;
    }

    Node* temp = curr->next;
    curr->next = curr->next->next;

    delete temp;
}
```

### Complexity

```text
Time:  O(n)
Space: O(1)
```

---

# 10. Search

```cpp
bool search(Node* head, int x) {
    Node* curr = head;

    while (curr != nullptr) {
        if (curr->data == x) {
            return true;
        }

        curr = curr->next;
    }

    return false;
}
```

### Complexity

```text
Time:  O(n)
Space: O(1)
```

---

# 11. Count Nodes

```cpp
int countNodes(Node* head) {
    int count = 0;

    Node* curr = head;

    while (curr != nullptr) {
        count++;
        curr = curr->next;
    }

    return count;
}
```

### Complexity

```text
Time:  O(n)
Space: O(1)
```

---

# Phase 1 Complexity Summary

| Operation              |     Time | Extra Space |
| ---------------------- | -------: | ----------: |
| Create Node            |     O(1) |        O(1) |
| Create List            |     O(n) |        O(n) |
| Traverse               |     O(n) |        O(1) |
| Search                 |     O(n) |        O(1) |
| Count                  |     O(n) |        O(1) |
| Insert Beginning       | **O(1)** |        O(1) |
| Insert End — no tail   |     O(n) |        O(1) |
| Insert End — with tail | **O(1)** |        O(1) |
| Insert Position        |     O(n) |        O(1) |
| Delete Beginning       | **O(1)** |        O(1) |
| Delete End             |     O(n) |        O(1) |
| Delete Value           |     O(n) |        O(1) |

---

# 12. Phase 2 — Core Interview Patterns

These are the important problems to remember.

---

# 12.1 Reverse Linked List ⭐⭐⭐

Given:

```text
10 → 20 → 30 → 40 → NULL
```

Reverse:

```text
40 → 30 → 20 → 10 → NULL
```

Use three pointers:

```text
prev
curr
next
```

### Code

```cpp
Node* reverseList(Node* head) {
    Node* prev = nullptr;
    Node* curr = head;

    while (curr != nullptr) {
        Node* next = curr->next;

        curr->next = prev;

        prev = curr;
        curr = next;
    }

    return prev;
}
```

### Core pattern

```cpp
Node* next = curr->next;
curr->next = prev;
prev = curr;
curr = next;
```

### Complexity

```text
Time:  O(n)
Space: O(1)
```

### Must understand

The algorithm reverses the **links**, not the values.

---

# 12.2 Find Middle Node ⭐⭐⭐

Example:

```text
10 → 20 → 30 → 40 → 50
```

Answer:

```text
30
```

Use:

```text
slow → 1 step
fast → 2 steps
```

### Code

```cpp
Node* findMiddle(Node* head) {
    Node* slow = head;
    Node* fast = head;

    while (fast != nullptr && fast->next != nullptr) {
        slow = slow->next;
        fast = fast->next->next;
    }

    return slow;
}
```

### Complexity

```text
Time:  O(n)
Space: O(1)
```

For an even-sized list, this implementation returns the **second middle**.

Example:

```text
10 → 20 → 30 → 40
```

returns:

```text
30
```

---

# 12.3 Detect Cycle — Floyd's Algorithm ⭐⭐⭐

A cyclic list:

```text
10 → 20 → 30 → 40
          ↑     ↓
          └─────┘
```

Use:

```text
slow → 1 step
fast → 2 steps
```

If:

```text
slow == fast
```

a cycle exists.

### Code

```cpp
bool hasCycle(Node* head) {
    Node* slow = head;
    Node* fast = head;

    while (fast != nullptr && fast->next != nullptr) {
        slow = slow->next;
        fast = fast->next->next;

        if (slow == fast) {
            return true;
        }
    }

    return false;
}
```

### Complexity

```text
Time:  O(n)
Space: O(1)
```

### Why it works

Inside a cycle, `fast` gains one node per iteration relative to `slow`, so eventually they meet.

---

# 12.4 Nth Node From End ⭐⭐

Example:

```text
10 → 20 → 30 → 40 → 50
```

Find 2nd from end:

```text
40
```

Use two pointers with a gap of `n`.

### Code

```cpp
Node* nthFromEnd(Node* head, int n) {
    Node* first = head;
    Node* second = head;

    for (int i = 0; i < n; i++) {
        if (first == nullptr) {
            return nullptr;
        }

        first = first->next;
    }

    while (first != nullptr) {
        first = first->next;
        second = second->next;
    }

    return second;
}
```

### Complexity

```text
Time:  O(n)
Space: O(1)
```

### Pattern

```text
first
     ↓
     ←── n nodes ──→
                   second
```

Maintain the gap while moving both pointers.

---

# 12.5 Merge Two Sorted Linked Lists ⭐⭐

Input:

```text
List 1:
1 → 3 → 5

List 2:
2 → 4 → 6
```

Output:

```text
1 → 2 → 3 → 4 → 5 → 6
```

### Code

```cpp
Node* mergeSorted(Node* a, Node* b) {
    Node dummy(0);
    Node* tail = &dummy;

    while (a != nullptr && b != nullptr) {
        if (a->data <= b->data) {
            tail->next = a;
            a = a->next;
        } else {
            tail->next = b;
            b = b->next;
        }

        tail = tail->next;
    }

    if (a != nullptr) {
        tail->next = a;
    } else {
        tail->next = b;
    }

    return dummy.next;
}
```

### Complexity

```text
Time:  O(n + m)
Space: O(1)
```

---

# 13. Core Linked List Patterns

Don't memorize every problem independently.

Remember these patterns.

## Pattern 1 — Pointer Reversal

```cpp
Node* next = curr->next;
curr->next = prev;
prev = curr;
curr = next;
```

Used for:

```text
Reverse Linked List
```

---

## Pattern 2 — Slow / Fast Pointers

```cpp
slow = slow->next;
fast = fast->next->next;
```

Used for:

```text
Find Middle
Detect Cycle
```

---

## Pattern 3 — Two Pointers With a Gap

```text
first
 ↓
←── n nodes ──→
               ↓
             second
```

Used for:

```text
Nth Node From End
```

---

## Pattern 4 — Dummy Node + Tail

```cpp
Node dummy(0);
Node* tail = &dummy;
```

Useful when constructing/merging a list because it avoids repeatedly handling the special case of the first node.

Used for:

```text
Merge Two Sorted Lists
```

---

# 14. Final EY Linked List Set

## 🔴 MUST KNOW

### Basic

```text
✓ Node structure
✓ Create list
✓ Input handling
✓ Traversal
✓ Insert beginning
✓ Insert end
✓ Insert at position
✓ Delete beginning
✓ Delete end
✓ Delete value
✓ Search
✓ Count nodes
```

### Core Problems

```text
✓ Reverse Linked List
✓ Find Middle
✓ Detect Cycle
```

---

## 🟡 SHOULD KNOW

```text
✓ Nth Node From End
✓ Merge Two Sorted Lists
```

---

## 🟢 LOW PRIORITY

For the current EY preparation, don't spend significant time on:

```text
✗ Very hard Linked List problems
✗ Complex pointer tricks
✗ Multi-level linked lists
✗ Exotic Linked List variants
✗ Dozens of repetitive LeetCode problems
```

---

# 15. Final Revision Checklist

Before marking Linked List complete:

```text
[ ] I can create a Node without looking
[ ] I can create a Linked List from input
[ ] I can traverse it
[ ] I understand head and tail
[ ] I can insert at beginning
[ ] I can insert at end
[ ] I can insert at a position
[ ] I can delete from beginning
[ ] I can delete from end
[ ] I can delete a value
[ ] I can search
[ ] I can count nodes

[ ] I can reverse a Linked List
[ ] I understand slow/fast pointers
[ ] I can find the middle
[ ] I can detect a cycle
[ ] I understand nth-from-end
[ ] I can merge two sorted lists

[ ] I know the time/space complexity
[ ] I can explain Array vs Linked List
[ ] I know when Linked List is actually useful
```

---

# 16. Final Mental Model

```text
LINKED LIST

Basic operations
      ↓
Pointer manipulation
      ↓
Reverse
      ↓
Slow / Fast pointers
      ↓
Two-pointer techniques
```

### The three most important patterns

```text
Reverse
→ prev + curr + next

Middle
→ slow + fast

Cycle
→ slow + fast
```

---

# ✅ LINKED LIST STATUS

**Theory:** ✅ Done

**Basic implementation:** ✅ Done

**Core interview patterns:** ✅ Done

**Complexities:** ✅ Done

**EY-level preparation:** ✅ Done

> **Next:** Move on. Do not over-prepare Linked Lists. The remaining return on time is much higher in **Trees, Graphs, SQL, DBMS, OS, CN, and OOP**.
