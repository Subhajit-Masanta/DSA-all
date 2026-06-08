# 92. Reverse Linked List II

## Problem

Given the head of a linked list and two positions `left` and `right`, reverse the nodes from position `left` to position `right`.

Return the modified linked list.

---

# Example

Input

```text
1 -> 2 -> 3 -> 4 -> 5

left = 2
right = 4
```

Output

```text
1 -> 4 -> 3 -> 2 -> 5
```

Only the sublist

```text
2 -> 3 -> 4
```

is reversed.

---

# Core Insight

This is not a completely new problem.

It is:

```text
206 Reverse Linked List

+

Reconnect the reversed section
```

The actual reversal logic is exactly the same as Problem 206.

The challenge is reconnecting the reversed portion back to the list.

---

# Visualization

Original List

```text
1 -> 2 -> 3 -> 4 -> 5
```

Mark the range

```text
1 -> [2 -> 3 -> 4] -> 5
```

Reverse only the marked section

```text
1 -> [4 -> 3 -> 2] -> 5
```

Final Answer

```text
1 -> 4 -> 3 -> 2 -> 5
```

---

# Important Pointers

For

```text
1 -> 2 -> 3 -> 4 -> 5

left = 2
right = 4
```

Store:

### prevleft

Node before reversal starts.

```text
1
```

---

### sublisthead

Original left node.

```text
2
```

After reversal this becomes the tail.

---

### prev

New head of reversed section.

```text
4
```

---

### curr

First node after reversed section.

```text
5
```

---

# Before Reversal

```text
prevleft

↓

1 -> 2 -> 3 -> 4 -> 5
     ^
 sublisthead
```

---

# Reverse Section

Reverse exactly

```text
right - left + 1
```

nodes.

For

```text
left = 2
right = 4
```

Reverse

```text
3 nodes
```

---

# After Reversal

```text
prev

↓

4 -> 3 -> 2 -> NULL

curr

↓

5
```

Notice:

```text
sublisthead = 2
```

has become the tail.

---

# Reconnecting

We need

```text
1 -> 4 -> 3 -> 2 -> 5
```

Two connections are required.

---

## Connection 1

```cpp
prevleft->next = prev;
```

Makes

```text
1 -> 4
```

---

## Connection 2

```cpp
sublisthead->next = curr;
```

Makes

```text
2 -> 5
```

---

# Edge Case

Suppose

```text
left = 1
```

Example

```text
1 -> 2 -> 3 -> 4 -> 5

left = 1
right = 3
```

Then

```text
prevleft = NULL
```

There is no node before the reversed section.

After reversing:

```text
3 -> 2 -> 1 -> 4 -> 5
```

The new head becomes

```text
prev
```

Therefore:

```cpp
if(prevleft)
{
    prevleft->next = prev;
}
else
{
    head = prev;
}
```

---

# Algorithm

### Step 1

Move to position `left`.

Store

```text
prevleft
sublisthead
```

---

### Step 2

Reverse exactly

```text
right-left+1
```

nodes using

```text
prev
curr
next
```

pattern.

---

### Step 3

Reconnect:

```cpp
prevleft->next = prev;
sublisthead->next = curr;
```

---

### Step 4

Return head.

---

# Dry Run

Input

```text
1 -> 2 -> 3 -> 4 -> 5

left = 2
right = 4
```

Before reverse

```text
prevleft = 1

sublisthead = 2
```

Reverse

```text
2 -> 3 -> 4

↓

4 -> 3 -> 2
```

After reverse

```text
prev = 4

curr = 5
```

Reconnect

```text
1 -> 4

2 -> 5
```

Final

```text
1 -> 4 -> 3 -> 2 -> 5
```

---

# Complexity

```text
Time  : O(n)

Space : O(1)
```

Only one traversal is required.

---

# Common Mistakes

### Mistake 1

```cpp
curr = prev->next;
```

Wrong.

After reversing, links have changed.

Correct:

```cpp
curr = next;
```

---

### Mistake 2

Forgetting

```cpp
sublisthead->next = curr;
```

Tail becomes disconnected.

---

### Mistake 3

Ignoring

```text
left = 1
```

This requires updating head.

---

# Pattern Recognition

This problem extends:

```text
206 Reverse Linked List
```

New idea:

```text
Reverse a portion

+

Reconnect boundaries
```

---

# Interview One-Liner

> Reverse only the nodes between left and right using the standard three-pointer reversal technique, then reconnect the reversed section with the unreversed parts of the list.

---

# Final Code

```cpp
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int left, int right) {

        if(head == nullptr || left == right)
            return head;

        ListNode* prevleft = nullptr;
        ListNode* sublisthead = head;

        for(int i = 1; i < left; i++)
        {
            prevleft = sublisthead;
            sublisthead = sublisthead->next;
        }

        ListNode* prev = nullptr;
        ListNode* curr = sublisthead;
        ListNode* next = nullptr;

        for(int i = 0; i < right - left + 1; i++)
        {
            next = curr->next;

            curr->next = prev;

            prev = curr;

            curr = next;
        }

        if(prevleft)
        {
            prevleft->next = prev;
        }
        else
        {
            head = prev;
        }

        sublisthead->next = curr;

        return head;
    }
};
```

---

# Notebook One-Liner

> Reverse only the required range using the normal linked list reversal pattern, then reconnect the node before the range and the node after the range to the reversed section.
