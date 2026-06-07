# 141. Linked List Cycle

## Problem

Given the head of a singly linked list, determine whether the linked list contains a cycle.

Return

```text
true  -> cycle exists

false -> no cycle
```

---

# Example

## Example 1

```text
3 -> 2 -> 0 -> -4
     ^          |
     |          |
     +----------+
```

Output

```text
true
```

---

## Example 2

```text
1 -> 2
^    |
|____|
```

Output

```text
true
```

---

## Example 3

```text
1 -> NULL
```

Output

```text
false
```

---

# Core Insight

If there is **no cycle**

```text
1 -> 2 -> 3 -> 4 -> NULL
```

eventually we reach

```text
NULL
```

and traversal stops.

---

If there **is a cycle**

```text
1 -> 2 -> 3 -> 4
     ^         |
     |         |
     +---------+
```

we will keep visiting nodes forever.

---

# Pattern

## Fast & Slow Pointer (Floyd Cycle Detection)

Maintain two pointers.

```text
slow

moves 1 step

fast

moves 2 steps
```

---

# Why Does It Work?

Imagine two runners on a circular track.

```text
Slow Runner

1 step

Fast Runner

2 steps
```

If they keep running,

the faster runner will eventually catch the slower runner.

The same idea applies to a linked list cycle.

---

# Visualization

Initial

```text
1 -> 2 -> 3 -> 4
     ^         |
     |         |
     +---------+

S,F
```

---

Move 1

```text
1 -> 2 -> 3 -> 4
     ^         |
     |         |
     +---------+

     S

          F
```

---

Move 2

```text
1 -> 2 -> 3 -> 4
     ^         |
     |         |
     +---------+

          S

     F
```

---

Move 3

```text
1 -> 2 -> 3 -> 4
     ^         |
     |         |
     +---------+

               S,F
```

Both pointers meet.

Cycle detected.

Return

```text
true
```

---

# Why While Condition?

```cpp
while(fast && fast->next)
```

Equivalent to

```cpp
while(fast != nullptr && fast->next != nullptr)
```

---

# Why?

Inside loop we do

```cpp
fast = fast->next->next;
```

To move two steps,

we must guarantee

```text
fast exists

AND

fast->next exists
```

Otherwise

```text
NULL->next
```

causes a segmentation fault.

---

# Dry Run

Input

```text
1 -> 2 -> 3 -> 4
     ^         |
     |         |
     +---------+
```

Initially

```text
slow = 1

fast = 1
```

---

Iteration 1

```text
slow = 2

fast = 3
```

---

Iteration 2

```text
slow = 3

fast = 2
```

---

Iteration 3

```text
slow = 4

fast = 4
```

Pointers become equal.

Return

```text
true
```

---

# Why Compare Addresses?

We write

```cpp
if(slow == fast)
```

NOT

```cpp
if(slow->val == fast->val)
```

because

```text
Different nodes

can have

same value.
```

We want to know whether both pointers point to the **same node in memory**.

---

# Complexity

```text
Time  : O(n)

Space : O(1)
```

Only two pointers are used.

---

# Common Mistakes

### Wrong

```cpp
while(fast)
```

Inside loop,

```cpp
fast->next->next
```

may crash.

---

### Wrong

```cpp
if(slow->val == fast->val)
```

Values can repeat.

Always compare pointers.

---

### Correct

```cpp
if(slow == fast)
```

Compare addresses.

---

# Pattern Recognition

Whenever a Linked List problem mentions

```text
Cycle

Loop

Repeated node

Infinite traversal
```

Immediately think

```text
Fast & Slow Pointer

(Floyd Cycle Detection)
```

---

# Similar Problems

```text
876  Middle of Linked List

141  Linked List Cycle

142  Linked List Cycle II

234  Palindrome Linked List

19   Remove Nth Node From End

143  Reorder List
```

---

# Interview One-Liner

> Move one pointer one step and another pointer two steps. If a cycle exists, the faster pointer will eventually meet the slower pointer; otherwise, the fast pointer reaches NULL.

---

# Final Code

```cpp
class Solution {
public:

    bool hasCycle(ListNode *head) {

        ListNode* slow = head;
        ListNode* fast = head;

        while (fast && fast->next)
        {
            slow = slow->next;
            fast = fast->next->next;

            if (slow == fast)
                return true;
        }

        return false;
    }
};
```

---

# Notebook One-Liner

> **Fast & Slow Pointer (Floyd Cycle Detection):** If one pointer moves one step and another moves two steps, they will always meet inside a cycle. If no cycle exists, the fast pointer reaches NULL.
