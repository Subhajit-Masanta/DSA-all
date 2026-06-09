# 24. Swap Nodes in Pairs

## Problem

Given the head of a linked list, swap every two adjacent nodes and return the modified list.

**Constraint:**

Do **not** swap node values.

Only change the node links.

---

# Example

Input

```text
1 -> 2 -> 3 -> 4
```

Output

```text
2 -> 1 -> 4 -> 3
```

---

## Odd Length Example

Input

```text
1 -> 2 -> 3 -> 4 -> 5
```

Output

```text
2 -> 1 -> 4 -> 3 -> 5
```

The last node has no partner, so it remains unchanged.

---

# Core Insight

We are **not reversing the entire list**.

We only swap

```text
(first , second)
```

for every pair.

Example

```text
1 -> 2 -> 3 -> 4

↓

2 -> 1 -> 4 -> 3
```

This is a **pointer reconnection problem**.

---

# Why Dummy Node?

Without dummy:

```text
head

↓

1 -> 2 -> 3 -> 4
```

After swapping,

```text
2 -> 1 -> 3 -> 4
```

Head changes from

```text
1

↓

2
```

So the first pair becomes a special case.

---

Using dummy:

```text
dummy -> 1 -> 2 -> 3 -> 4
```

Now every pair has a previous node.

The same logic works for every iteration.

No special cases.

---

# Visualization

Before

```text
dummy

↓

0 -> 1 -> 2 -> 3 -> 4

     ↑    ↑
   first second
```

---

## Step 1

```cpp
first->next = second->next;
```

Result

```text
0 -> 1 -> 3 -> 4

2 -> 3 -> 4
```

---

## Step 2

```cpp
second->next = first;
```

Result

```text
2 -> 1 -> 3 -> 4
```

---

## Step 3

```cpp
prevPair->next = second;
```

Result

```text
0 -> 2 -> 1 -> 3 -> 4
```

Swap completed.

---

# Algorithm

### Step 1

Handle edge cases.

```text
If list is empty or contains one node,
return head.
```

---

### Step 2

Create a dummy node.

```text
dummy -> head
```

---

### Step 3

Maintain

```text
prevPair

first

second
```

---

### Step 4

While two nodes exist,

perform

```cpp
first->next = second->next;
second->next = first;
prevPair->next = second;
```

---

### Step 5

Move

```cpp
prevPair = first;
```

to process the next pair.

---

### Step 6

Return

```cpp
dummy->next;
```

---

# Dry Run

Input

```text
1 -> 2 -> 3 -> 4
```

Dummy

```text
0 -> 1 -> 2 -> 3 -> 4
```

---

## Iteration 1

```text
first = 1

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
first = 3

second = 4
```

Swap

```text
0 -> 2 -> 1 -> 4 -> 3
```

Return

```text
2 -> 1 -> 4 -> 3
```

---

# Complexity

```text
Time  : O(n)

Space : O(1)
```

---

# Common Mistakes

### Mistake 1

Swapping node values.

Wrong.

Only pointers should be changed.

---

### Mistake 2

Forgetting

```cpp
prevPair->next = second;
```

Previous pair becomes disconnected.

---

### Mistake 3

Using

```cpp
first = second->next;
```

after swapping.

After swap,

```text
second->next = first
```

so it moves backwards.

Correct update:

```text
prevPair = first
```

and the next pair is accessed through

```cpp
prevPair->next
```

---

### Mistake 4

Not handling

```text
0 nodes

1 node
```

Always check

```cpp
if(head == nullptr || head->next == nullptr)
    return head;
```

---

# Pattern Recognition

This problem belongs to

```text
Pointer Manipulation
```

Pattern:

```text
Find pair

↓

Reconnect three links

↓

Move to next pair
```

Similar Problems

```text
92 Reverse Linked List II

25 Reverse Nodes in K Group

206 Reverse Linked List
```

---

# Interview One-Liner

> Use a dummy node and reconnect pointers pair by pair. Dummy removes the special case of swapping the head and keeps the logic uniform.

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

> **Dummy Node Pattern:** Add an imaginary node before head so every pair has a previous node. Swap using three pointer reconnections and move to the next pair in O(n) time and O(1) space.
