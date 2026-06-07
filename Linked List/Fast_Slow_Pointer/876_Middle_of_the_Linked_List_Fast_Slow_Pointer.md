# 876. Middle of the Linked List

## Problem

Given the head of a singly linked list, return the **middle node**.

If there are **two middle nodes**, return the **second middle node**.

---

# Example

## Example 1

Input

```text
1 -> 2 -> 3 -> 4 -> 5
```

Output

```text
3 -> 4 -> 5
```

---

## Example 2

Input

```text
1 -> 2 -> 3 -> 4 -> 5 -> 6
```

Output

```text
4 -> 5 -> 6
```

---

# Approach 1 : Count Length

## Core Idea

1. Traverse the list and count the number of nodes.
2. Calculate the middle position.
3. Traverse again until the middle node.
4. Return it.

---

## Formula

```text
middle = length / 2 + 1
```

Works for both odd and even length.

---

## Dry Run

Input

```text
1 -> 2 -> 3 -> 4 -> 5 -> 6
```

Length

```text
6
```

Middle

```text
6 / 2 + 1 = 4
```

Traverse

```text
1

↓

2

↓

3

↓

4

Return
```

---

## Complexity

```text
Time  : O(n)

Space : O(1)
```

---

# Approach 2 : Fast & Slow Pointer (Interview Pattern)

## Core Idea

Maintain two pointers.

```text
slow -> moves 1 node

fast -> moves 2 nodes
```

When fast reaches the end,

slow automatically reaches the middle.

---

# Why Does It Work?

Imagine two people running.

```text
Slow Person

1 step

Fast Person

2 steps
```

When the fast person finishes the race,

the slow person has covered exactly half the distance.

The same idea is applied to Linked Lists.

---

# Visualization (Odd Length)

Input

```text
1 -> 2 -> 3 -> 4 -> 5
```

Initially

```text
S
F

1 -> 2 -> 3 -> 4 -> 5
```

Move

```text
1 -> 2 -> 3 -> 4 -> 5

     S

          F
```

Move

```text
1 -> 2 -> 3 -> 4 -> 5

          S

                    F(NULL)
```

Return

```text
3
```

---

# Visualization (Even Length)

Input

```text
1 -> 2 -> 3 -> 4 -> 5 -> 6
```

Initially

```text
S
F

1 -> 2 -> 3 -> 4 -> 5 -> 6
```

Move

```text
1 -> 2 -> 3 -> 4 -> 5 -> 6

     S

          F
```

Move

```text
1 -> 2 -> 3 -> 4 -> 5 -> 6

          S

                    F
```

Move

```text
1 -> 2 -> 3 -> 4 -> 5 -> 6

               S

                          F(NULL)
```

Return

```text
4
```

Automatically returns the **second middle node**.

---

# Why This While Condition?

```cpp
while(fast != nullptr && fast->next != nullptr)
```

means

```text
while(

fast exists

AND

fast->next exists

)
```

---

## Why?

Inside the loop we do

```cpp
fast = fast->next->next;
```

To jump two nodes,

we must guarantee

```text
fast exists

AND

fast->next exists
```

Otherwise,

```text
NULL->next
```

causes

```text
Segmentation Fault
```

---

# Pattern Recognition

Whenever a Linked List problem says

```text
Middle

Half

Cycle

Palindrome

Nth From End

Split List
```

Immediately think

```text
Fast & Slow Pointer
```

---

# Problems Using This Pattern

```text
876  Middle of Linked List

141  Linked List Cycle

142  Linked List Cycle II

234  Palindrome Linked List

19   Remove Nth Node From End

143  Reorder List
```

---

# Complexity

```text
Time  : O(n)

Space : O(1)
```

Only one traversal.

---

# Common Mistakes

### Wrong

```cpp
while(fast != nullptr)
```

Inside loop,

```cpp
fast->next->next
```

may access NULL.

---

### Correct

```cpp
while(fast != nullptr && fast->next != nullptr)
```

---

# Core Pattern

```text
slow += 1 step

fast += 2 steps

↓

fast reaches end

↓

slow reaches middle
```

---

# Interview One-Liner

> Use two pointers where `slow` moves one step and `fast` moves two steps. When `fast` reaches the end, `slow` is positioned at the middle node.

---

# Final Code

```cpp
class Solution {
public:
    ListNode* middleNode(ListNode* head) {

        ListNode* slow = head;
        ListNode* fast = head;

        while (fast != nullptr && fast->next != nullptr)
        {
            slow = slow->next;
            fast = fast->next->next;
        }

        return slow;
    }
};
```

---

# Notebook One-Liner

> **Fast & Slow Pointer:** Move one pointer by one step and another by two steps. When the fast pointer reaches the end of the linked list, the slow pointer automatically reaches the middle. This pattern is widely used for middle node, cycle detection, palindrome, and nth-from-end problems.
