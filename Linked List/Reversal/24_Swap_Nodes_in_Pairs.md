# 24. Swap Nodes in Pairs

## Problem

Given the head of a linked list, swap every two adjacent nodes and return the modified list.

**You cannot swap node values.**

Only the node connections (`next` pointers) may be changed.

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
1 -> 2 -> 3 -> 4 -> 5
```

Output

```text
2 -> 1 -> 4 -> 3 -> 5
```

The last node remains unchanged because it has no partner.

---

# Core Insight

We are not reversing the entire list.

We only reverse **every pair**.

```text
1 -> 2 -> 3 -> 4

↓

(1,2) swap

↓

2 -> 1 -> 3 -> 4

↓

(3,4) swap

↓

2 -> 1 -> 4 -> 3
```

---

# Pattern

```text
Pointer Manipulation

+

Reconnect Previous Pair
```

---

# Algorithm (Dummy Node)

### Step 1

Create a dummy node.

```text
dummy -> head
```

---

### Step 2

Maintain

```text
prevPair
first
second
```

---

### Step 3

For every pair

```text
first = prevPair->next

second = first->next
```

perform

```text
first->next = second->next

second->next = first

prevPair->next = second
```

---

### Step 4

Move

```text
prevPair = first
```

and repeat.

---

# Dry Run

Input

```text
dummy

↓

0 -> 1 -> 2 -> 3 -> 4
```

---

Iteration 1

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

Iteration 2

```text
0 -> 2 -> 1 -> 3 -> 4

          ↑
      prevPair
```

Swap

```text
0 -> 2 -> 1 -> 4 -> 3
```

Return

```text
dummy->next

↓

2 -> 1 -> 4 -> 3
```

---

# Why Dummy Node?

Without dummy

```text
head

↓

1 -> 2
```

After swapping

```text
2 -> 1
```

Head must change.

This becomes a special case.

---

With dummy

```text
dummy -> 1 -> 2
```

Every pair has a previous node.

The same logic works for the first pair and all remaining pairs.

No special cases.

---

# Dummy Solution

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

# Algorithm (Without Dummy)

### Step 1

Handle the first pair separately.

```text
newHead = head->next
```

because after first swap

```text
2 -> 1 -> ...
```

the new head becomes node 2.

---

### Step 2

Maintain

```text
prevPair
first
second
```

---

### Step 3

Swap

```text
first->next = second->next

second->next = first
```

---

### Step 4

If previous pair exists

```text
prevPair->next = second
```

---

### Step 5

Move

```text
prevPair = first

first = first->next
```

Repeat.

---

# Dry Run (Without Dummy)

Input

```text
1 -> 2 -> 3 -> 4
```

First swap

```text
2 -> 1 -> 3 -> 4
```

Store

```text
newHead = 2
```

Second swap

```text
2 -> 1 -> 4 -> 3
```

Return

```text
newHead
```

---

# Non-Dummy Solution

```cpp
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {

        if(head == nullptr || head->next == nullptr)
            return head;

        ListNode* newHead = head->next;

        ListNode* prevPair = nullptr;
        ListNode* first = head;

        while(first && first->next)
        {
            ListNode* second = first->next;

            first->next = second->next;
            second->next = first;

            if(prevPair != nullptr)
            {
                prevPair->next = second;
            }

            prevPair = first;
            first = first->next;
        }

        return newHead;
    }
};
```

---

# Even Length

Input

```text
1 -> 2 -> 3 -> 4
```

Output

```text
2 -> 1 -> 4 -> 3
```

Every node is swapped.

---

# Odd Length

Input

```text
1 -> 2 -> 3 -> 4 -> 5
```

Output

```text
2 -> 1 -> 4 -> 3 -> 5
```

Last node remains unchanged because it has no adjacent partner.

---

# Complexity

```text
Time  : O(n)

Space : O(1)
```

---

# Common Mistakes

### Mistake 1

Updating

```cpp
first = second->next;
```

Wrong.

After swapping

```text
second->next = first
```

so this points backward.

---

### Mistake 2

Forgetting

```cpp
prevPair->next = second;
```

Previous pair becomes disconnected.

---

### Mistake 3

Ignoring

```text
head change
```

when not using a dummy node.

---

# Pattern Recognition

This problem belongs to

```text
Linked List Pointer Manipulation
```

Pattern:

```text
Take a small block

↓

Change next pointers

↓

Reconnect block

↓

Move to next block
```

---

# Interview One-Liner

> Swap adjacent nodes by manipulating pointers instead of values, reconnect each swapped pair to the previous pair, and continue until fewer than two nodes remain.

---

# Notebook One-Liner

> Dummy nodes eliminate head special cases and make pointer manipulation problems cleaner, safer, and easier to implement.
