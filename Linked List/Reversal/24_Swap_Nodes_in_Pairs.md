# 24. Swap Nodes in Pairs

## Problem

Given the head of a linked list, swap every two adjacent nodes.

You **cannot swap values**. Only the node connections (`next` pointers) may be changed.

---

# Example 1

Input

```text
1 -> 2 -> 3 -> 4
```

Output

```text
2 -> 1 -> 4 -> 3
```

---

# Example 2

Input

```text
1
```

Output

```text
1
```

---

# Example 3

Input

```text
1 -> 2 -> 3
```

Output

```text
2 -> 1 -> 3
```

The last node remains unchanged because it has no partner.

---

# Core Insight

We are not reversing the entire list.

We are simply swapping

```text
(first, second)
```

pair by pair.

Example

```text
1 -> 2 -> 3 -> 4

↓

(1,2) (3,4)

↓

2 -> 1 -> 4 -> 3
```

---

# Why Dummy Node?

Without dummy

```text
head

↓

1 -> 2 -> 3 -> 4
```

After swapping

```text
2 -> 1 -> 3 -> 4
```

Now

```text
head
```

must become

```text
2
```

The first pair becomes a special case.

---

With dummy

```text
dummy

↓

0 -> 1 -> 2 -> 3 -> 4
```

Now every pair has a previous node.

The same logic works for every iteration.

No special handling is required.

---

# Pointers Used

```text
prevPair

↓

dummy -> 1 -> 2 -> 3 -> 4

          ↑    ↑
        first second
```

---

# Swap Process

## Step 1

Connect first to node after second.

```cpp
first->next = second->next;
```

Result

```text
1 -> 3 -> 4

2 -> 3 -> 4
```

---

## Step 2

Connect second to first.

```cpp
second->next = first;
```

Result

```text
2 -> 1 -> 3 -> 4
```

---

## Step 3

Connect previous pair.

```cpp
prevPair->next = second;
```

Result

```text
dummy -> 2 -> 1 -> 3 -> 4
```

---

## Step 4

Move

```cpp
prevPair = first;
```

Next pair starts automatically.

---

# Dry Run

Input

```text
dummy

↓

0 -> 1 -> 2 -> 3 -> 4
```

---

## Iteration 1

```text
first  = 1

second = 2
```

Swap

```text
0 -> 2 -> 1 -> 3 -> 4
```

Move

```text
prevPair = 1
```

---

## Iteration 2

```text
0 -> 2 -> 1 -> 3 -> 4

          ↑
      prevPair

first  = 3

second = 4
```

Swap

```text
0 -> 2 -> 1 -> 4 -> 3
```

Return

```text
dummy->next
```

Output

```text
2 -> 1 -> 4 -> 3
```

---

# Odd Length Example

Input

```text
1 -> 2 -> 3 -> 4 -> 5
```

Swap

```text
2 -> 1 -> 4 -> 3 -> 5
```

The last node remains unchanged because it does not have a partner.

---

# Complexity

```text
Time  : O(n)

Space : O(1)
```

---

# Common Mistakes

### Mistake 1

```cpp
first = second->next;
```

Wrong.

After swapping

```text
second->next

↓

first
```

So this moves back to the previous node.

---

### Mistake 2

Forgetting

```cpp
prevPair->next = second;
```

Previous pair becomes disconnected.

---

### Mistake 3

Forgetting to update

```cpp
prevPair = first;
```

Loop cannot continue correctly.

---

# Why Dummy Makes Life Easy

Without dummy

```text
Need:

newHead

if(prevPair != NULL)

special case for first pair
```

With dummy

```text
Every pair has a previous node.

Same code works for every iteration.

No special cases.

Cleaner interview solution.
```

---

# Pattern Recognition

This problem belongs to

```text
Pointer Manipulation

+

Boundary Reconnection
```

Similar problems

```text
92 Reverse Linked List II

24 Swap Nodes in Pairs

25 Reverse Nodes in K Group
```

---

# Interview One-Liner

> Maintain a pointer to the previous pair and swap each adjacent pair by changing only the next pointers. A dummy node eliminates special handling for the first pair.

---

# Final Code

```cpp
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {

        if(head == nullptr || head->next == nullptr)
            return head;

        ListNode* dummy = new ListNode(0);
        dummy->next = head;

        ListNode* prevPair = dummy;

        while(prevPair->next && prevPair->next->next)
        {
            ListNode* first = prevPair->next;
            ListNode* second = first->next;

            first->next = second->next;
            second->next = first;
            prevPair->next = second;

            prevPair = first;
        }

        return dummy->next;
    }
};
```

---

# Notebook One-Liner

> Use a dummy node so every pair has a previous node. Swap using three pointer updates: `first->next`, `second->next`, and `prevPair->next`, then move `prevPair` to the tail of the swapped pair.
