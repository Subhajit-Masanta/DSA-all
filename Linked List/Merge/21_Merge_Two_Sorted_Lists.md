# 21. Merge Two Sorted Lists

## Problem

You are given the heads of two sorted linked lists.

Merge them into one sorted linked list by **reusing the existing nodes**.

Do **not** create new nodes (except an optional dummy node).

Return the head of the merged linked list.

---

# Example

## Input

```text
list1

1 -> 2 -> 4

list2

1 -> 3 -> 4
```

## Output

```text
1 -> 1 -> 2 -> 3 -> 4 -> 4
```

---

# Core Insight

This problem is exactly the **Merge step of Merge Sort**.

Instead of comparing array elements,

we compare linked list nodes.

Every iteration:

```text
Compare

↓

Take smaller node

↓

Attach to merged list

↓

Move corresponding pointer
```

---

# Pattern

```text
Dummy Node

+

Tail Pointer

+

Pointer Manipulation
```

---

# Why Dummy Node?

Without dummy

```text
head

↓

1 -> 2

1 -> 3
```

After selecting the first node,

we must decide the new head.

This creates a special case.

---

With dummy

```text
dummy

↓

0

tail

↓

0
```

Every selected node is attached using the same logic.

No special handling is required.

---

# Important Pointers

```text
dummy

Imaginary node before merged list.

tail

Always points to the last node of merged list.

list1

Current node of first list.

list2

Current node of second list.
```

---

# Algorithm

### Step 1

Create a dummy node.

```cpp
dummy -> NULL
```

Initialize

```cpp
tail = dummy
```

---

### Step 2

While both lists exist

```text
Compare

list1->val

and

list2->val
```

Choose the smaller node.

---

### Step 3

Attach it.

```cpp
tail->next = chosenNode;
```

Move

```cpp
tail = tail->next;
```

Move only the pointer of the chosen list.

---

### Step 4

When one list becomes empty,

attach the remaining list.

```cpp
tail->next = list1 ? list1 : list2;
```

---

### Step 5

Return

```cpp
dummy->next;
```

---

# Dry Run

Input

```text
list1

1 -> 2 -> 4

list2

1 -> 3 -> 4
```

---

## Initial

```text
dummy

↓

0

tail
```

---

## Compare

```text
1

1
```

Choose list1

```text
0 -> 1

tail
```

Move

```text
list1

2 -> 4
```

---

## Compare

```text
2

1
```

Choose list2

```text
0 -> 1 -> 1

tail
```

Move

```text
list2

3 -> 4
```

---

## Compare

```text
2

3
```

Choose list1

```text
0 -> 1 -> 1 -> 2
```

---

## Compare

```text
4

3
```

Choose list2

```text
0 -> 1 -> 1 -> 2 -> 3
```

---

## Compare

```text
4

4
```

Choose list1

```text
0 -> 1 -> 1 -> 2 -> 3 -> 4
```

---

Now

```text
list1 = NULL

list2

4
```

Attach remaining list

```text
0 -> 1 -> 1 -> 2 -> 3 -> 4 -> 4
```

Return

```text
dummy->next
```

---

# Pointer Visualization

Initially

```text
dummy

↓

0

tail
```

Attach node

```cpp
tail->next = list1;
```

Result

```text
0 -> 1
```

Move

```cpp
tail = tail->next;
```

Now

```text
0 -> 1
     ↑
    tail
```

Tail always stays at the last node of the merged list.

---

# Why Do We Add Only One Node?

Suppose

```text
list1

2 -> 2

list2

100 -> 101
```

If we attach both nodes in one iteration

```text
2 -> 100
```

the next

```text
2
```

from list1 should actually come before

```text
100
```

The merged list becomes unsorted.

Therefore,

every iteration must attach exactly **one node**.

---

# Complexity

```text
Time  : O(n + m)

Space : O(1)
```

where

```text
n = length of list1

m = length of list2
```

---

# Common Mistakes

### Mistake 1

Adding both nodes in one iteration.

Wrong

```cpp
tail->next = list1;
tail = list1;

tail->next = list2;
tail = list2;
```

Always attach only one node.

---

### Mistake 2

Moving both pointers.

Wrong

```cpp
list1 = list1->next;
list2 = list2->next;
```

Move only the pointer whose node was selected.

---

### Mistake 3

Creating new nodes.

Not required.

Reuse existing nodes.

---

### Mistake 4

Forgetting to attach the remaining list.

Always add

```cpp
tail->next = list1 ? list1 : list2;
```

---

# Pattern Recognition

This problem is the linked list version of

```text
Merge Sort Merge Step
```

Pattern:

```text
Two sorted structures

↓

Compare current elements

↓

Take smaller

↓

Move one pointer

↓

Repeat
```

---

# Interview One-Liner

> Merge two sorted linked lists by maintaining a tail pointer, always attaching the smaller current node and advancing only that list pointer.

---

# Final Code

```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {

        ListNode* dummy = new ListNode(0);
        ListNode* tail = dummy;

        while(list1 && list2)
        {
            if(list1->val <= list2->val)
            {
                tail->next = list1;
                tail = tail->next;
                list1 = list1->next;
            }
            else
            {
                tail->next = list2;
                tail = tail->next;
                list2 = list2->next;
            }
        }

        tail->next = (list1 != nullptr) ? list1 : list2;

        return dummy->next;
    }
};
```

---

# Notebook One-Liner

> Treat this as the Merge Sort merge step: compare current nodes, attach the smaller one to the tail, move only that list pointer, and finally attach the remaining list.
