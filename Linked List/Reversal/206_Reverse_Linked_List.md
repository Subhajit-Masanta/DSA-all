# 206. Reverse Linked List

## Problem

Given the head of a singly linked list, reverse the linked list and return the new head.

---

# Example

Input

```text
1 -> 2 -> 3 -> 4 -> 5
```

Output

```text
5 -> 4 -> 3 -> 2 -> 1
```

---

# Core Insight

A linked list stores connections using pointers.

Initially

```text
1 -> 2 -> 3 -> 4 -> 5
```

To reverse it, we must change every arrow.

```text
1 <- 2 <- 3 <- 4 <- 5
```

The challenge is:

When we reverse a link, we may lose access to the remaining list.

Example

```text
1 -> 2 -> 3 -> 4 -> 5
```

If we do

```cpp
2->next = 1;
```

without saving node 3 first,

we lose access to

```text
3 -> 4 -> 5
```

Therefore we must save the next node before reversing.

---

# Pattern Used

## Three Pointer Pattern

```text
prev
curr
next
```

### Meaning

```text
prev  = already reversed portion

curr  = current node being processed

next  = saved next node
```

---

# Initial Setup

Input

```text
1 -> 2 -> 3 -> 4 -> 5
```

Pointers

```text
prev = NULL

curr = 1

next = ?
```

Visualization

```text
NULL    1 -> 2 -> 3 -> 4 -> 5
 ^       ^
prev    curr
```

---

# Algorithm

For every node:

### Step 1

Save next node.

```cpp
next = curr->next;
```

---

### Step 2

Reverse link.

```cpp
curr->next = prev;
```

---

### Step 3

Move prev forward.

```cpp
prev = curr;
```

---

### Step 4

Move curr forward.

```cpp
curr = next;
```

Repeat until curr becomes NULL.

---

# Dry Run

## Iteration 1

Initial

```text
NULL    1 -> 2 -> 3 -> 4 -> 5
 ^       ^
prev    curr
```

Save

```text
next = 2
```

Reverse

```text
NULL <- 1     2 -> 3 -> 4 -> 5
```

Move

```text
prev = 1

curr = 2
```

Result

```text
NULL <- 1     2 -> 3 -> 4 -> 5
        ^     ^
      prev   curr
```

---

## Iteration 2

Save

```text
next = 3
```

Reverse

```text
NULL <- 1 <- 2     3 -> 4 -> 5
```

Move

```text
prev = 2

curr = 3
```

---

## Iteration 3

Save

```text
next = 4
```

Reverse

```text
NULL <- 1 <- 2 <- 3     4 -> 5
```

Move

```text
prev = 3

curr = 4
```

---

## Iteration 4

```text
NULL <- 1 <- 2 <- 3 <- 4     5
```

---

## Iteration 5

```text
NULL <- 1 <- 2 <- 3 <- 4 <- 5
```

Now

```text
curr = NULL
```

Loop ends.

---

# Important Observation

At the end

```text
prev = 5

curr = NULL
```

Who is the new head?

```text
5
```

which is stored in

```text
prev
```

Therefore

```cpp
return prev;
```

NOT

```cpp
return curr;
```

NOT

```cpp
return head;
```

---

# Why Do We Need next?

Without

```cpp
next = curr->next;
```

after

```cpp
curr->next = prev;
```

the original forward connection is lost.

Example

Before

```text
1 -> 2 -> 3
```

After

```cpp
2->next = 1;
```

becomes

```text
1 <- 2    3
```

Node 3 is disconnected unless saved earlier.

---

# Complexity

```text
Time  : O(n)

Space : O(1)
```

Only one traversal is performed.

---

# Common Mistakes

## Mistake 1

```cpp
next = next->next;
```

Wrong.

Eventually next becomes NULL.

Then

```cpp
NULL->next
```

causes a crash.

---

## Mistake 2

```cpp
return head;
```

Wrong.

head still points to the old first node.

---

## Mistake 3

```cpp
return curr;
```

Wrong.

At the end

```text
curr = NULL
```

---

## Correct

```cpp
return prev;
```

because prev points to the new head.

---

# Pattern Recognition

This is the foundation for many Linked List problems.

Used in:

```text
206 Reverse Linked List

92 Reverse Linked List II

25 Reverse Nodes in K Group

143 Reorder List

234 Palindrome Linked List
```

---

# Interview One-Liner

> Reverse a linked list by maintaining three pointers (prev, curr, next), reversing one link at a time while preserving access to the remaining nodes.

---

# Final Code

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {

        ListNode* prev = nullptr;
        ListNode* curr = head;

        while (curr)
        {
            ListNode* next = curr->next;

            curr->next = prev;

            prev = curr;

            curr = next;
        }

        return prev;
    }
};
```

---

# Notebook One-Liner

> Before reversing a link, always save the next node. Use the three-pointer pattern (prev, curr, next) to reverse the list in O(n) time and O(1) space.
