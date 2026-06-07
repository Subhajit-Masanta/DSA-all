# 142. Linked List Cycle II

## Problem

Given the head of a linked list, return the **node where the cycle begins**.

If there is **no cycle**, return

```text
nullptr
```

**Do not modify the linked list.**

---

# Difference from 141

## 141. Linked List Cycle

Question

```text
Does a cycle exist?
```

Return

```text
true

or

false
```

---

## 142. Linked List Cycle II

Question

```text
Where does the cycle begin?
```

Return

```text
ListNode*
```

(the actual starting node of the cycle)

---

# Example

Input

```text
3 -> 2 -> 0 -> -4
     ^          |
     |          |
     +----------+
```

Output

```text
Node containing 2
```

LeetCode internally prints

```text
tail connects to node index 1
```

---

# Core Insight

The meeting point of slow and fast pointers is **NOT always the cycle start**.

Example

```text
A -> B -> C -> D -> E -> F
          ^              |
          |              |
          +--------------+
```

Cycle starts at

```text
C
```

Slow and fast may meet at

```text
E
```

So

```text
Meeting Point

≠

Cycle Starting Point
```

---

# Floyd Cycle Detection

## Phase 1

Use Fast & Slow pointers.

```text
slow

moves 1 step

fast

moves 2 steps
```

If

```text
slow == fast
```

then a cycle exists.

---

# Phase 2

Create two pointers.

```text
p1 = head

p2 = meeting point
```

Now move both

```text
1 step

1 step

1 step
```

They will meet exactly at the cycle starting node.

---

# Visualization

Input

```text
A -> B -> C -> D -> E -> F
          ^              |
          |              |
          +--------------+
```

Suppose

Fast & Slow meet at

```text
E
```

---

Create

```text
p1 = A

p2 = E
```

Move once

```text
p1 = B

p2 = F
```

Move again

```text
p1 = C

p2 = C
```

Pointers meet.

Return

```text
C
```

---

# Algorithm

### Step 1

Initialize

```cpp
slow = head;
fast = head;
```

---

### Step 2

Move

```text
slow -> 1 step

fast -> 2 steps
```

until

```text
slow == fast
```

or

```text
fast reaches NULL
```

---

### Step 3

If no meeting occurs

```text
return nullptr
```

---

### Step 4

Create

```cpp
p1 = head;

p2 = slow;
```

---

### Step 5

Move both one step.

```cpp
p1 = p1->next;

p2 = p2->next;
```

---

### Step 6

When

```text
p1 == p2
```

return

```cpp
p1
```

---

# Dry Run

Input

```text
3 -> 2 -> 0 -> -4
     ^          |
     |          |
     +----------+
```

---

## Phase 1

Initially

```text
slow = 3

fast = 3
```

Move

```text
slow = 2

fast = 0
```

Move

```text
slow = 0

fast = 2
```

Move

```text
slow = -4

fast = -4
```

Meeting found.

---

## Phase 2

Create

```text
p1 = head = 3

p2 = meeting = -4
```

Move

```text
p1 = 2

p2 = 2
```

Pointers meet.

Return

```text
Node containing 2
```

---

# Why Compare Pointers?

Correct

```cpp
if(slow == fast)
```

Compare addresses.

---

Wrong

```cpp
if(slow->val == fast->val)
```

Different nodes can have the same value.

Always compare node addresses.

---

# Complexity

```text
Time  : O(n)

Space : O(1)
```

Only constant extra memory is used.

---

# Common Mistakes

### Returning Meeting Point

Wrong

```cpp
return slow;
```

Meeting point is NOT necessarily the cycle start.

---

### Comparing Values

Wrong

```cpp
slow->val == fast->val
```

Compare pointers instead.

---

### Forgetting No Cycle Case

Always

```cpp
return nullptr;
```

if fast reaches NULL.

---

# Pattern Recognition

Whenever a Linked List problem mentions

```text
Cycle

Loop

Find starting point

Repeated traversal
```

Think

```text
Floyd Cycle Detection

Phase 1 : Detect meeting

Phase 2 : Move one pointer to head

Move both one step

Meeting point = Cycle Start
```

---

# Interview One-Liner

> Detect the cycle using Fast & Slow pointers. After they meet, place one pointer at the head and move both one step at a time. Their next meeting point is the starting node of the cycle.

---

# Final Code

```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {

        ListNode* slow = head;
        ListNode* fast = head;

        while (fast && fast->next)
        {
            slow = slow->next;
            fast = fast->next->next;

            if (slow == fast)
            {
                ListNode* p1 = head;
                ListNode* p2 = slow;

                while (p1 != p2)
                {
                    p1 = p1->next;
                    p2 = p2->next;
                }

                return p1;
            }
        }

        return nullptr;
    }
};
```

---

# Notebook One-Liner

> **141 checks whether a cycle exists. 142 finds where the cycle starts. After Fast & Slow meet, move one pointer to the head and move both one step at a time—their meeting point is the start of the cycle.**
