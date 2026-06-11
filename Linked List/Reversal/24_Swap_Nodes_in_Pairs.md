# Swap Nodes in Pairs

| Topic       | Difficulty | Pattern                           |
| :---------- | :--------- | :-------------------------------- |
| Linked List | Medium     | Pointer Manipulation / Dummy Node |

---

# 📌 Problem Description

Given the head of a linked list, swap every two adjacent nodes and return the modified list.

**Node values cannot be changed.**

Only the node connections (`next` pointers) may be modified.

Example:

```text
Input:
1 -> 2 -> 3 -> 4

Output:
2 -> 1 -> 4 -> 3
```

---

# 🧠 Core Insight

We are **not reversing the entire linked list**.

We only reverse **one pair at a time**.

```text
1 -> 2 -> 3 -> 4

↓

Swap (1,2)

↓

2 -> 1 -> 3 -> 4

↓

Swap (3,4)

↓

2 -> 1 -> 4 -> 3
```

Using a **dummy node** removes the special case of changing the head.

---

# ⚙️ Algorithm

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

where

```text
prevPair = node before current pair

first = first node of pair

second = second node of pair
```

---

### Step 3

Swap the pair.

```cpp
first->next = second->next;

second->next = first;

prevPair->next = second;
```

---

### Step 4

Move

```cpp
prevPair = first;
```

and repeat until fewer than two nodes remain.

---

# 🧪 Example / Dry Run

Input

```text
dummy

↓

0 -> 1 -> 2 -> 3 -> 4
```

---

### Iteration 1

```text
prevPair = 0

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

### Iteration 2

```text
0 -> 2 -> 1 -> 3 -> 4
          ↑
      prevPair
```

Now

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
dummy->next

↓

2 -> 1 -> 4 -> 3
```

---

# 🧪 Odd Length Example

Input

```text
1 -> 2 -> 3 -> 4 -> 5
```

Swap first pair

```text
2 -> 1 -> 3 -> 4 -> 5
```

Swap second pair

```text
2 -> 1 -> 4 -> 3 -> 5
```

Node

```text
5
```

remains unchanged because it has no partner.

---

# ⏱ Time Complexity

```text
O(n)
```

Every node is visited once.

---

# 📦 Space Complexity

```text
O(1)
```

Only pointer variables are used.

---

# ⚠️ Common Mistakes

### Mistake 1

Updating

```cpp
first = second->next;
```

Wrong.

After swapping,

```cpp
second->next = first;
```

so this moves backward.

Correct

```cpp
prevPair = first;
```

---

### Mistake 2

Forgetting

```cpp
prevPair->next = second;
```

The swapped pair becomes disconnected from the previous part of the list.

---

### Mistake 3

Not using a dummy node.

Without a dummy node,

```text
1 -> 2
```

becomes

```text
2 -> 1
```

and the head changes.

A dummy node avoids this special case and keeps the logic identical for every pair.

---

# 🔥 Key Insight

Think in **small blocks of two nodes**.

```text
Take Pair

↓

Swap Pair

↓

Reconnect

↓

Move to Next Pair
```

instead of thinking about the entire linked list.

---

# 🎯 Interview One-Liner

Use a dummy node and swap every adjacent pair by changing pointers, reconnecting each swapped pair with the previous one in **O(n)** time and **O(1)** space.

---

# 💻 Code (C++)

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

# 📚 Pattern Recognition

If the problem says:

```text
Swap Adjacent Nodes

Reverse Every Small Block

Pairwise Reordering

Pointer Rearrangement
```

Think:

```text
Dummy Node

↓

Take Two Nodes

↓

Swap

↓

Reconnect

↓

Move Forward
```

This pattern is reused in:

```text
24 Swap Nodes in Pairs

25 Reverse Nodes in K Group

92 Reverse Linked List II

143 Reorder List
```
