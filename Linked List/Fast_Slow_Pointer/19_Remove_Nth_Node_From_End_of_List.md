# 19. Remove Nth Node From End of List

## Problem

Given the head of a singly linked list and an integer `n`, remove the **nth node from the end** of the list and return the head.

---

# Example

## Example 1

Input

```text
1 -> 2 -> 3 -> 4 -> 5

n = 2
```

Output

```text
1 -> 2 -> 3 -> 5
```

---

## Example 2

Input

```text
1

n = 1
```

Output

```text
NULL
```

---

## Example 3

Input

```text
1 -> 2

n = 1
```

Output

```text
1
```

---

# Normal Approach

## Idea

1. Find length.
2. Convert

```text
nth from end

↓

(length - n + 1)th from beginning
```

3. Delete that node.

---

## Complexity

```text
Time  : O(n+n) = O(n)

Space : O(1)
```

---

# Interview Approach (Fast & Slow Pointer)

## Core Idea

Maintain a **fixed gap of n nodes**.

```text
slow ---------------- fast

Gap = n nodes
```

When fast reaches the last node,

slow automatically reaches **one node before the node to delete**.

---

# Algorithm

## Step 1

Initialize

```cpp
slow = head;
fast = head;
```

---

## Step 2

Move fast `n` steps.

Example

```text
1 -> 2 -> 3 -> 4 -> 5

slow

1 -> 2 -> 3 -> 4 -> 5
          ^
         fast
```

Gap

```text
slow -------- fast

2 nodes apart
```

---

## Step 3

Special Case

If

```text
fast == NULL
```

it means

```text
delete first node
```

Example

```text
1 -> 2

n = 2
```

Move fast two steps

```text
fast = NULL
```

Delete

```text
head
```

---

## Step 4

Move both together.

```text
slow += 1

fast += 1
```

until

```text
fast->next == NULL
```

---

## Dry Run

Input

```text
1 -> 2 -> 3 -> 4 -> 5

n = 2
```

Move fast 2 steps

```text
slow

1 -> 2 -> 3 -> 4 -> 5
          ^
         fast
```

Move together

```text
1 -> 2 -> 3 -> 4 -> 5

     S          F
```

Move

```text
1 -> 2 -> 3 -> 4 -> 5

          S          F(last)
```

Now

```text
slow = 3

slow->next = 4
```

Delete

```text
1 -> 2 -> 3 -> 5
```

---

# Why while(fast->next != NULL)?

We want

```text
slow

↓

1 -> 2 -> 3 -> 4 -> 5
          ^         ^
         slow      fast(last)
```

so that

```text
slow->next
```

is exactly the node to delete.

---

# Complexity

```text
Time  : O(n)

Space : O(1)
```

Only one traversal is required.

---

# Common Mistakes

### Wrong

Move fast less than `n` nodes.

This breaks the required gap.

---

### Wrong

```cpp
while(fast != NULL)
```

This moves slow one step too far.

Correct

```cpp
while(fast->next != NULL)
```

---

### Forgetting Head Deletion

If

```text
fast == NULL
```

after moving `n` steps,

the first node must be deleted.

---

# Pattern Recognition

Fast & Slow Pointer has three major patterns.

## Pattern 1

```text
fast = 2x slow
```

Used for

```text
876 Middle of Linked List
141 Linked List Cycle
```

---

## Pattern 2

```text
Meeting Point

↓

Move one pointer to head

↓

Move both one step
```

Used for

```text
142 Linked List Cycle II
```

---

## Pattern 3

```text
Maintain Fixed Gap
```

```text
slow -------- fast

Gap = n
```

Used for

```text
19 Remove Nth Node From End
```

---

# Interview One-Liner

> Maintain a fixed gap of `n` nodes between fast and slow pointers. When fast reaches the last node, slow automatically reaches the node before the one to be deleted.

---

# Final Code

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {

        ListNode* slow = head;
        ListNode* fast = head;

        // Move fast n steps
        for (int i = 0; i < n; i++)
        {
            fast = fast->next;
        }

        // Delete first node
        if (fast == nullptr)
        {
            ListNode* temp = head;
            head = head->next;
            delete temp;
            return head;
        }

        // Maintain gap
        while (fast->next != nullptr)
        {
            slow = slow->next;
            fast = fast->next;
        }

        // Delete node
        ListNode* del = slow->next;
        slow->next = del->next;
        delete del;

        return head;
    }
};
```

---

# Notebook One-Liner

> **Fixed Gap Pattern:** Move the fast pointer `n` nodes ahead, then move both pointers together. When the fast pointer reaches the last node, the slow pointer is exactly one node before the target node, allowing deletion in a single traversal.
